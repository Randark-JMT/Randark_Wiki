---
title: tshark介绍
description: 
published: true
date: 2022-09-13T14:49:52.850Z
tags: 
editor: markdown
dateCreated: 2022-09-13T14:22:33.904Z
---

# tshark
`tshark`作为一个Wireshark的CLI，具有多平台的兼容性，并且支持多种语法来对提取出来的`pcap`或者`pcapng`流量包文件进行数据的筛选和提取。
在Linux上，可以通过以下命令来安装tshark：
```shell
sudo apt install tshark
```
在Windows上，`tshark`包含在`wireshark`软件之中，安装`wireshark`的同时便会获取`tshark`。

# tshark的基本语法
通过`tshark`的`-h`参数，可以看到`tshark`的使用文档，但是实际做题中用不到那么多的参数，最常见的还是
- `-r`读取文件
- `-T`设置数据的导出格式
- `-e`目标格式的参数设置（配合`fields`）
- `-Y`设置筛选器

其中，如果对相关语法的参数没有一定把握的话，建议可以先使用`-T json`，将流量包的全部数据导出为json格式的数据，既便于查找参数，也可以将原本的流量数据用其他支持json数据分析的数据分析工具

# -T json
通过Linux的管道符，将输出的json数据写入文件中，便可以对json数据进行分析
（以下示例基于————TODO——————）
```shell
tshark -r tesst.pcapng -T json > out.json
```
![90360ea2-d70b-4f1a-991e-1050495e557d.png](/90360ea2-d70b-4f1a-991e-1050495e557d.png)

截取一个package作为例子：
```json
{
  "_index": "packets-2022-09-13", //包索引
  "_type": "doc",
  "_score": null,
  "_source": {
    "layers": {
      "frame": {
        "frame.interface_id": "0",
        "frame.interface_id_tree": {
          "frame.interface_name": "\\Device\\NPF_Loopback"
        },
        "frame.encap_type": "15",
        "frame.time": "Sep 13, 2022 22:20:54.811933000 CST", //包的捕捉时间
        "frame.offset_shift": "0.000000000",
        "frame.time_epoch": "1663078854.811933000",
        "frame.time_delta": "0.000000000",
        "frame.time_delta_displayed": "0.000000000",
        "frame.time_relative": "0.000000000",
        "frame.number": "1",
        "frame.len": "56", //包大小
        "frame.cap_len": "56",
        "frame.marked": "0",
        "frame.ignored": "0",
        "frame.protocols": "null:ip:tcp"
      },
      "null": {
        "null.family": "2"
      },
      "ip": {
        "ip.version": "4",
        "ip.hdr_len": "20",
        "ip.dsfield": "0x00",
        "ip.dsfield_tree": {
          "ip.dsfield.dscp": "0",
          "ip.dsfield.ecn": "0"
        },
        "ip.len": "52",
        "ip.id": "0xef44",
        "ip.flags": "0x40",
        "ip.flags_tree": {
          "ip.flags.rb": "0",
          "ip.flags.df": "1",
          "ip.flags.mf": "0"
        },
        "ip.frag_offset": "0",
        "ip.ttl": "128",
        "ip.proto": "6",
        "ip.checksum": "0x0000",
        "ip.checksum.status": "2",
        "ip.src": "127.0.0.1", //包的源地址
        "ip.addr": "127.0.0.1", 
        "ip.src_host": "127.0.0.1",
        "ip.host": "127.0.0.1",
        "ip.dst": "127.0.0.1", //包的目标地址
        "ip.addr": "127.0.0.1",
        "ip.dst_host": "127.0.0.1",
        "ip.host": "127.0.0.1"
      },
      "tcp": {
        "tcp.srcport": "35811",
        "tcp.dstport": "8974",
        "tcp.port": "35811",
        "tcp.port": "8974",
        "tcp.stream": "0",
        "tcp.completeness": "0",
        "tcp.len": "0",
        "tcp.seq": "0",
        "tcp.seq_raw": "2505424863",
        "tcp.nxtseq": "1",
        "tcp.ack": "0",
        "tcp.ack_raw": "0",
        "tcp.hdr_len": "32",
        "tcp.flags": "0x0002",
        "tcp.flags_tree": {
          "tcp.flags.res": "0",
          "tcp.flags.ns": "0",
          "tcp.flags.cwr": "0",
          "tcp.flags.ecn": "0",
          "tcp.flags.urg": "0",
          "tcp.flags.ack": "0",
          "tcp.flags.push": "0",
          "tcp.flags.reset": "0",
          "tcp.flags.syn": "1",
          "tcp.flags.syn_tree": {
            "_ws.expert": {
              "tcp.connection.syn": "",
              "_ws.expert.message": "Connection establish request (SYN): server port 8974",
              "_ws.expert.severity": "2097152",
              "_ws.expert.group": "33554432"
            }
          },
          "tcp.flags.fin": "0",
          "tcp.flags.str": "··········S·"
        },
        "tcp.window_size_value": "65535",
        "tcp.window_size": "65535",
        "tcp.checksum": "0x72c3",
        "tcp.checksum.status": "2",
        "tcp.urgent_pointer": "0",
        "tcp.options": "02:04:ff:d7:01:03:03:08:01:01:04:02",
        "tcp.options_tree": {
          "tcp.options.mss": "02:04:ff:d7",
          "tcp.options.mss_tree": {
            "tcp.option_kind": "2",
            "tcp.option_len": "4",
            "tcp.options.mss_val": "65495"
          },
          "tcp.options.nop": "01",
          "tcp.options.nop_tree": {
            "tcp.option_kind": "1"
          },
          "tcp.options.wscale": "03:03:08",
          "tcp.options.wscale_tree": {
            "tcp.option_kind": "3",
            "tcp.option_len": "3",
            "tcp.options.wscale.shift": "8",
            "tcp.options.wscale.multiplier": "256"
          },
          "tcp.options.nop": "01",
          "tcp.options.nop_tree": {
            "tcp.option_kind": "1"
          },
          "tcp.options.nop": "01",
          "tcp.options.nop_tree": {
            "tcp.option_kind": "1"
          },
          "tcp.options.sack_perm": "04:02",
          "tcp.options.sack_perm_tree": {
            "tcp.option_kind": "4",
            "tcp.option_len": "2"
          }
        },
        "Timestamps": {
          "tcp.time_relative": "0.000000000",
          "tcp.time_delta": "0.000000000"
        }
      }
    }
  }
}
```
***~~都是成年人了，就这么点英文，应该不用每个都解释过去吧~~***
# 对数据应用筛选器
一个流量包中，包含大量的数据包，那么进行流量分析的时候，就需要对流量包中的数据先做一个筛选
```shell
tshark -r ***.pcapng -Y "<筛选条件>"
```
其中`-Y`参数就是应用筛选条件，而条件可以参考json中的数据，也可以用`wireshark`对条件右键，选择“作为过滤器应用”，然后在过滤器框中就可以看到具体的筛选条件。
例如，以下参数就是筛选源地址是`127.0.0.1`的流量包：
```shell
tshark -r ***.pcapng -Y "ip.src == 127.0.0.1"
```
而下面一个参数则是筛选源地址是`127.0.0.1`，并且目标地址也是`127.0.0.1`的流量包：
```shell
tshark -r ***.pcapng -Y "(ip.src == 127.0.0.1) && (ip.dst == 127.0.0.1)"
```
由此可见，`-Y`参数是支持逻辑运算符的，所以筛选的姿势是可以灵活多变的。
支持的逻辑符有以下：
  - `&&`  且选中
  - `!`  非选中
  - `||` 或选中