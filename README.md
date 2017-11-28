# CloudInf Lab 10 - Template
This Python script generates a CLOS environment (leaf, spine switches and a few test hosts). The size of this environment can be specified dynamically via 2 script parameters.

## Prerequirements & installation
- `mininet` must be installed (from distribution package repository)

## Usage
Start environment:
`sudo mn --custom mininetClosStartup.py --topo clos,LEAFS,SPINE --controller remote`

Replace "LEAFS" with the number of leaf switches you would like to generate and "SPINE" with the number of spine switches. For every leaf switch there will be a host created and attached with it. 

**Important:** Your OpenFlow controller instance (ryu) must be running before you start the mininet environment!

Start the ryu controller:
```
ryu-manager superSimpleHub.py --verbose
```

Start the mininet setup:
```
(lab10_ryu) user@lnx01:~/code/cloudinf/lab10_ryu$ sudo mn --custom mininetClosStartup.py --topo clos,3,2 --controller remote
*** Configuring hosts
h0 h1 h2 
*** Starting controller
c0
*** Starting 5 switches
l0 l1 l2 s0 s1 ...
*** Starting CLI:
```

## Testing
To test the OpenFlow13 controller behaviour, use the following commands:

**Important:** Before you test your setup, ensure the topology is loop free..

### Check connectivity
```
mininet> h1 ping h2
PING 10.0.0.3 (10.0.0.3) 56(84) bytes of data.
64 bytes from 10.0.0.3: icmp_seq=1 ttl=64 time=3.27 ms
64 bytes from 10.0.0.3: icmp_seq=2 ttl=64 time=0.353 ms
64 bytes from 10.0.0.3: icmp_seq=3 ttl=64 time=0.038 ms
64 bytes from 10.0.0.3: icmp_seq=4 ttl=64 time=0.058 ms
64 bytes from 10.0.0.3: icmp_seq=5 ttl=64 time=0.041 ms
^C
--- 10.0.0.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 3998ms
rtt min/avg/max/mdev = 0.038/0.752/3.272/1.265 ms
```
### Check if there are loops
```
mininet> sh tcpdump -i s0
tcpdump: WARNING: s0: no IPv4 address assigned
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on s0, link-type EN10MB (Ethernet), capture size 65535 bytes
```