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


## Dissector output

Following `header_decoded` entry will be added to each record. Please pick anything you want to remove `header_decoded` itself for Kibana to accept.

```json
  "header_decoded": {
    "_index": "packets-2018-06-14",
    "_type": "pcap_file",
    "_score": null,
    "_source": {
      "layers": {
        "frame_raw": [
          "020586717403020586716403884700010140450000544b8100004001add4c0a80002c0a800010800d193400e004f5b1d85c900081a1d08090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f202122232425262728292a2b2c2d2e2f3031323334353637",
          0,
          102,
          0,
          1
        ],
        "frame": {
          "frame.encap_type": "1",
          "frame.time": "Sep 17, 4446980 08:55:20.1380504064 UTC",
          "_ws.expert": {
            "frame.time_invalid": "",
            "_ws.expert.message": "Arrival Time: Fractional second 1380504064 is invalid, the valid range is 0-1000000000",
            "_ws.expert.severity": "4194304",
            "_ws.expert.group": "33554432"
          },
          "frame.offset_shift": "0.000000000",
          "frame.time_epoch": "1652792056.1380504064",
          "frame.time_delta": "0.000000000",
          "frame.time_delta_displayed": "0.000000000",
          "frame.time_relative": "0.000000000",
          "frame.number": "577",
          "frame.len": "102",
          "frame.cap_len": "102",
          "frame.marked": "0",
          "frame.ignored": "0",
          "frame.protocols": "eth:ethertype:mpls:ip:icmp:data"
        },
        "eth_raw": [
          "0205867174030205867164038847",
          0,
          14,
          0,
          1
        ],
        "eth": {
          "eth.dst_raw": [
            "020586717403",
            0,
            6,
            0,
            29
          ],
          "eth.dst": "02:05:86:71:74:03",
          "eth.dst_tree": {
            "eth.dst_resolved_raw": [
              "020586717403",
              0,
              6,
              0,
              26
            ],
            "eth.dst_resolved": "MS-NLB-PhysServer-05_86:71:74:03",
            "eth.addr_raw": [
              "020586717403",
              0,
              6,
              0,
              29
            ],
            "eth.addr": "02:05:86:71:74:03",
            "eth.addr_resolved_raw": [
              "020586717403",
              0,
              6,
              0,
              26
            ],
            "eth.addr_resolved": "MS-NLB-PhysServer-05_86:71:74:03",
            "eth.lg_raw": [
              "1",
              0,
              3,
              131072,
              2
            ],
            "eth.lg": "1",
            "eth.ig_raw": [
              "0",
              0,
              3,
              65536,
              2
            ],
            "eth.ig": "0"
          },
          "eth.src_raw": [
            "020586716403",
            6,
            6,
            0,
            29
          ],
          "eth.src": "02:05:86:71:64:03",
          "eth.src_tree": {
            "eth.src_resolved_raw": [
              "020586716403",
              6,
              6,
              0,
              26
            ],
            "eth.src_resolved": "MS-NLB-PhysServer-05_86:71:64:03",
            "eth.addr_raw": [
              "020586716403",
              6,
              6,
              0,
              29
            ],
            "eth.addr": "02:05:86:71:64:03",
            "eth.addr_resolved_raw": [
              "020586716403",
              6,
              6,
              0,
              26
            ],
            "eth.addr_resolved": "MS-NLB-PhysServer-05_86:71:64:03",
            "eth.lg_raw": [
              "1",
              6,
              3,
              131072,
              2
            ],
            "eth.lg": "1",
            "eth.ig_raw": [
              "0",
              6,
              3,
              65536,
              2
            ],
            "eth.ig": "0"
          },
          "eth.type_raw": [
            "8847",
            12,
            2,
            0,
            5
          ],
          "eth.type": "0x00008847"
        },
        "mpls_raw": [
          "00010140",
          14,
          4,
          0,
          1
        ],
        "mpls": {
          "mpls.label_raw": [
            "10",
            14,
            4,
            4294963200,
            7
          ],
          "mpls.label": "16",
          "mpls.exp_raw": [
            "0",
            14,
            4,
            3584,
            7
          ],
          "mpls.exp": "0",
          "mpls.bottom_raw": [
            "1",
            14,
            4,
            256,
            7
          ],
          "mpls.bottom": "1",
          "mpls.ttl_raw": [
            "40",
            14,
            4,
            255,
            7
          ],
          "mpls.ttl": "64"
        },
        "ip_raw": [
          "450000544b8100004001add4c0a80002c0a80001",
          18,
          20,
          0,
          1
        ],
        "ip": {
          "ip.version_raw": [
            "4",
            18,
            1,
            240,
            4
          ],
          "ip.version": "4",
          "ip.hdr_len_raw": [
            "45",
            18,
            1,
            0,
            4
          ],
          "ip.hdr_len": "20",
          "ip.dsfield_raw": [
            "00",
            19,
            1,
            0,
            4
          ],
          "ip.dsfield": "0x00000000",
          "ip.dsfield_tree": {
            "ip.dsfield.dscp_raw": [
              "0",
              19,
              1,
              252,
              4
            ],
            "ip.dsfield.dscp": "0",
            "ip.dsfield.ecn_raw": [
              "0",
              19,
              1,
              3,
              4
            ],
            "ip.dsfield.ecn": "0"
          },
          "ip.len_raw": [
            "0054",
            20,
            2,
            0,
            5
          ],
          "ip.len": "84",
          "ip.id_raw": [
            "4b81",
            22,
            2,
            0,
            5
          ],
          "ip.id": "0x00004b81",
          "ip.flags_raw": [
            "00",
            24,
            1,
            0,
            4
          ],
          "ip.flags": "0x00000000",
          "ip.flags_tree": {
            "ip.flags.rb_raw": [
              "00",
              24,
              1,
              0,
              2
            ],
            "ip.flags.rb": "0",
            "ip.flags.df_raw": [
              "00",
              24,
              1,
              0,
              2
            ],
            "ip.flags.df": "0",
            "ip.flags.mf_raw": [
              "00",
              24,
              1,
              0,
              2
            ],
            "ip.flags.mf": "0"
          },
          "ip.frag_offset_raw": [
            "0000",
            24,
            2,
            0,
            5
          ],
          "ip.frag_offset": "0",
          "ip.ttl_raw": [
            "40",
            26,
            1,
            0,
            4
          ],
          "ip.ttl": "64",
          "ip.proto_raw": [
            "01",
            27,
            1,
            0,
            4
          ],
          "ip.proto": "1",
          "ip.checksum_raw": [
            "add4",
            28,
            2,
            0,
            5
          ],
          "ip.checksum": "0x0000add4",
          "ip.checksum.status": "2",
          "ip.src_raw": [
            "c0a80002",
            30,
            4,
            0,
            32
          ],
          "ip.src": "192.168.0.2",
          "ip.addr_raw": [
            "c0a80001",
            34,
            4,
            0,
            32
          ],
          "ip.addr": "192.168.0.1",
          "ip.src_host_raw": [
            "c0a80002",
            30,
            4,
            0,
            26
          ],
          "ip.src_host": "192.168.0.2",
          "ip.host_raw": [
            "c0a80001",
            34,
            4,
            0,
            26
          ],
          "ip.host": "192.168.0.1",
          "ip.dst_raw": [
            "c0a80001",
            34,
            4,
            0,
            32
          ],
          "ip.dst": "192.168.0.1",
          "ip.dst_host_raw": [
            "c0a80001",
            34,
            4,
            0,
            26
          ],
          "ip.dst_host": "192.168.0.1"
        },
        "icmp_raw": [
          "0800d193400e004f5b1d85c900081a1d08090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f202122232425262728292a2b2c2d2e2f3031323334353637",
          38,
          64,
          0,
          1
        ],
        "icmp": {
          "icmp.type_raw": [
            "08",
            38,
            1,
            0,
            4
          ],
          "icmp.type": "8",
          "icmp.code_raw": [
            "00",
            39,
            1,
            0,
            4
          ],
          "icmp.code": "0",
          "icmp.checksum_raw": [
            "d193",
            40,
            2,
            0,
            5
          ],
          "icmp.checksum": "0x0000d193",
          "icmp.checksum.status": "1",
          "icmp.ident_raw": [
            "400e",
            42,
            2,
            0,
            5
          ],
          "icmp.ident": "3648",
          "icmp.seq_raw": [
            "004f",
            44,
            2,
            0,
            5
          ],
          "icmp.seq": "79",
          "icmp.seq_le_raw": [
            "004f",
            44,
            2,
            0,
            5
          ],
          "icmp.seq_le": "20224",
          "data_raw": [
            "5b1d85c900081a1d08090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f202122232425262728292a2b2c2d2e2f3031323334353637",
            46,
            56,
            0,
            1
          ],
          "data": {
            "data.data_raw": [
              "5b1d85c900081a1d08090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f202122232425262728292a2b2c2d2e2f3031323334353637",
              46,
              56,
              0,
              30
            ],
            "data.data": "5b:1d:85:c9:00:08:1a:1d:08:09:0a:0b:0c:0d:0e:0f:10:11:12:13:14:15:16:17:18:19:1a:1b:1c:1d:1e:1f:20:21:22:23:24:25:26:27:28:29:2a:2b:2c:2d:2e:2f:30:31:32:33:34:35:36:37",
            "data.len": "56"
          }
        }
      }
    }
  },
  ```