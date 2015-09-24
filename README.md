# ocmirror
OpenContrail mirrored packet reader/forwarder

This can be used with OpenContrail when setting a Service Instance with a mirroring policy. This tool aims to parse encapsulated packets and forward inner packets to another interface so that external analyzer tool can be use.

Usage
=====

```
$ ocmirror -h
Usage: ocmirror --port <udp port> --intf <forward interface> --verbose
```

Just printing packets

```
$ ocmirror -v

09:23:21.47207 default-domain:demo2:net1 > default-domain:default-project:default-virtual-network captured from: 10.43.91.10, action: Pass Mirror
        02:54:26:06:db:86 > 00:00:5e:00:01:00, ethertype IPv4 (0x0800), length: 98
        10.0.0.4 > 20.0.0.3 length: 84, protocol: ICMP
```

Printing and forwarding
```
$ ocmirror -i capture0 -v
09:26:40.200839 default-domain:demo2:net1 > default-domain:default-project:default-virtual-network captured from: 10.43.91.10, action: Pass, Mirror
        02:54:26:06:db:86 > 00:00:5e:00:01:00, ethertype IPv4 (0x0800), length: 98
        10.0.0.4 > 20.0.0.3 length: 84, protocol: ICMP

$ ocmirror -j
{"timestamp": "09:26:40.200839", "vn_src": "default-domain:demo2:net1", "vn_dst": "default-domain:default-project:default-virtual-network", "host": "10.43.91.10", "actions": ["Pass", "Mirror"], "hw_src": "02:54:26:06:db:86", "hw_dst": "00:00:5e:00:01:00", "ethertype": "IPv4 (0x0800)", "length": 98, "ip_src": "10.0.0.4", "ip_dst": "20.0.0.3", "length": 84, "protocol": "ICMP"}


$ tcpdump -n -i capture0
09:26:40.209230 02:54:26:06:db:86 > 00:00:5e:00:01:00, ethertype IPv4 (0x0800), length 98: 10.0.0.4 > 20.0.0.3: ICMP echo request, id 26625, seq 36986, length 64
```
