# xflow collector Dockerfile

## Description

This repository containes Dockerfiles to kickstart xflow collector with:

* Fluentd
  * fluent-plugin-neflow
  * fluent-plugin-sflow
* Elasticsearch
* Kibana

:bulb: If you are interested in sflow collector with using wireshark dissector, please try [wireshark-dissector](https://github.com/codeout/docker-xflow-collector/tree/wireshark-dissector) branch.

## How to start

### Linux

```
$ git clone https://github.com/codeout/docker-xflow-collector.git
$ cd docker-xflow-collector
```

#### 1. Start gobgpd as a origin AS resolver

Start `gobgpd` anywhere docker is available.

:warning: DO NOT USE newer gobgpd than:

```
v1.32 (a6e0d00a705146e6b7f72a4d58c61b063980cc65)
```

#### 2. Configure fluentd

```shell
$ vi fluentd/fluent.conf
```

Update gRPC server where your gobgpd is listening.

```diff
--- a/fluentd/fluent.conf
+++ b/fluentd/fluent.conf
@@ -14,13 +14,13 @@
 <filter netflow.event>
   @type asresolver

-  grpc 192.168.0.2:50051
+  grpc <your fluentd host>:50051
 </filter>

 <filter sflow.event>
   @type asresolver

-  grpc 192.168.0.2:50051
+  grpc <your fluentd host>:50051
 </filter>
```

:bulb: **TIPS:**

If you don't need AS resolver, just remove `<filter></filter>` element for speed.

#### 3. Start containers

```
$ sudo sysctl -w vm.max_map_count=262144
```

or write `vm.max_map_count=262144` to `/etc/sysctl.conf`  
See https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html for details.

Then,

```shell
$ sudo docoker-compose up
```

## Listen ports

|service|ports|
|--|--|
|Kibana console|5601/tcp (http)|
|Elasticsearch|9200/tcp|
|netflow collector| 2055/udp|
|sflow collector|6343/udp|
