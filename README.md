# poc-kong
kong enterprise proof of concept

## install OS

Ubuntu 18.04 LTS http://releases.ubuntu.com/releases/18.04/ on VirtualBox with Bridged network adapter.

## install java

 $ sudo apt-get install openjdk-8-jdk-headless

## install cassandra

 $ echo "deb http://debian.datastax.com/community stable main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
 $ curl -L http://debian.datastax.com/debian/repo_key | sudo apt-key add -
 $ sudo apt-get update
 $ sudo apt-get install cassandra
 
## install kong

## configure kong
