# BurpSweet

This repo continas a script for simplifying the use of BurpSuite
transparent proxying with dnsmasq and optionally an OpenWRT router.

## Configuration

Sample configs are in the `samples` directory. To get started, copy
`samples` to `configs` and modify as needed. Below I will explain how
each config works.

#### burp.dns

```
# Host           # IP
asdf.example.com 192.168.1.10
blah.example.com 192.168.1.11
```

This file contains entries to map hostnames to their real IP addresses
(in BurpSuite).

#### burp.listeners

```
# IP:port
192.168.1.2:8080
```

This file simply contians a list of IP:port entries for BurpSuite
listeners.

#### dnsmasq.mitm

```
# Domain         # IP
asdf.example.com eth0:192.168.2.1
.example.com     192.168.1.2
```

This file is for configuring dnsmasq to point domains to the MitM host
and optionally a virtual NIC on the MitM host. When only MitM-ing a
few hosts, I've found that virutal NICs make it easier to read packet
captures in Wireshark.

#### openwrt.iptables

```
# Original dest IP # New dest IP
192.168.1.10       192.168.1.3
192.168.1.11       192.168.2.1
192.168.1.12       192.168.1.2
192.168.1.13       192.168.1.2
```

This file is for configuring iptables on an OpenWRT router to add to
the `PREROUTING` table and to `DNAT` from the original destination IP
to a new destination IP (most likely one of the virtual IPs). This is
useful when the traffic you want to MitM is using IPs instead of
hostnames.

# TODO

- Fix bugs which I'm sure exist
