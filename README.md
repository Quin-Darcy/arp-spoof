# arp-spoof


This is a minimal ARP-Spoofing tool written in **Rust language** using pcap.

This tool allows intercepting Ipv4 traffic between two hosts on the same network.
Typically, between one machine and the internet gateway.

Please note, this tool was created to get comfortable with Rust, so the code isn't necessarily optimal nor idiomatic.

**This tool is for educational purposes only.**

## Features:

* 1 to 1 route poisoning
* save intercepted traffic as pcap file
* automatic Ipv4 forwarding

# Usage
```
Usage:
    ./arp-spoof [OPTIONS]

Minimal ARP spoofing tool written in Rust.

optional arguments:
  -h,--help             show this help message and exit
  -i,--interface INTERFACE
                        interface name
  --target TARGET       target ipv4 address
  --gateway GATEWAY     gateway ipv4 address
  --log-traffic         logs all target traffic to `save.pcap`
  -n,--no-forward       leave `/proc/sys/net/ipv4/ip_forward` untouched
  -V,--version          show version
```
A typical invocation would look like this. The arguments are pretty self-describing.
```
# ./arp-spoof --interface eth0 --own 192.168.0.100 --target 192.168.0.16 --gateway 192.168.0.1 --log-traffic
[*] Using device eth0 ...
 -> ip address: 192.168.0.100
 -> mac address: 95:8F:6F:17:36:71
 -> connection_status: Connected
[+] forwarding ipv4 traffic: true
[*] Resolving hosts (this can take a bit) ...
 -> found 7E:FA:8B:B2:F5:8A at 192.168.0.16
 -> found 7C:0B:B6:94:11:6B at 192.168.0.1
[*] Saving captured packets as save.pcap ...
[+] Poisoning traffic between 192.168.0.16 <==> 192.168.0.1
[*] Received: 274, dropped: 0, if_dropped: 0
[*] Received: 518, dropped: 0, if_dropped: 0
```
# Building

```
# cargo build --release
# ./target/release/arp-spoof --help
```
Tested with rust 1.68.0.


## Linux

On Arch based Linux, install `community/rust`, `community/cargo` and `core/libpcap`. If not running as root, you need to set capabilities like so: ```sudo setcap cap_net_raw,cap_net_admin=eip path/to/bin```

# TODO:

* implement `n` to `m` route poisoning
