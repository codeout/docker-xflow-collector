# xflow collector Dockerfile

## Description

This repository containes Dockerfiles to kickstart xflow collector with:

* Fluentd
  * fluent-plugin-neflow
  * fluent-plugin-sflow
* Elasticsearch
* Kibana

## How to start

### Linux

```
$ sysctl -w vm.max_map_count=262144
```

or write `vm.max_map_count=262144` to `/etc/sysctl.conf`  
See https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html for details.

Then,

```shell
$ docoker-compose up
```

## Listen ports

|service|ports|
|--|--|
|Kibana console|5601/tcp (http)|
|Elasticsearch|9200/tcp|
|netflow collector| 2055/udp|
|sflow collector|6343/udp|
