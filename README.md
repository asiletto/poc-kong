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
