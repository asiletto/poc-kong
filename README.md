# poc-kong
kong enterprise proof of concept

## install OS

Ubuntu 18.04 LTS http://releases.ubuntu.com/releases/18.04/ on VirtualBox with Bridged network adapter.

## install java

```
$ sudo apt-get install openjdk-8-jdk-headless
```

## install cassandra

```
$ echo "deb http://debian.datastax.com/community stable main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
$ curl -L http://debian.datastax.com/debian/repo_key | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install cassandra
$ echo "/bin/bash"|chsh cassandra
```

## install kong enterprise

Download kong-enterprise-edition-0.32.trusty.all.deb and license.json

```
 $ sudo dpkg -i kong-enterprise-edition-0.32.trusty.all.deb
 $ sudo cp license-xxxxxxxxxxxxxxxx_xxxxxxxxxxxxxxx.json /etc/kong/license.json
 $ sudo cp /etc/kong/kong.conf.default /etc/kong/kong.conf
```

## configure kong

/etc/kong/kong.conf
```
database = cassandra
admin_listen = 0.0.0.0:8001, 0.0.0.0:8444 ssl
vitals = on
portal = on
portal_auth = basic-auth
```
## run kong

```
 $ kong migrations up
 $ kong start
```
admin console available at http://localhost:8002/dashboard

## test apis

define api
```
$ curl -i -X POST \
  --url http://localhost:8001/apis/ \
  --data 'name=example-api' \
  --data 'hosts=example.com' \
  --data 'upstream_url=http://my-internal-apache/demo/'
```

call api
```
$ curl -i -X GET \
  --url http://localhost:8000/v1.json \
  --header 'Host: example.com'
```

## enable key-auth plugin

https://docs.konghq.com/enterprise/0.32-x/getting-started/adding-consumers/

```
curl -i -X POST \
  --url http://localhost:8001/consumers/ale/key-auth/ \
  --data 'key=123-456-789'
  
{"id":"764d890a-4f44-4dc4-a996-27a7aeca221a","created_at":1531303819637,"key":"123-456-789","consumer_id":"f1c27f11-b584-43d5-ba84-62c4f845d8ef"}
```

```
curl -i -X GET --url http://localhost:8000/v1.json --header 'Host: example.com' --header 'apikey: 123-456-789' -v
```
