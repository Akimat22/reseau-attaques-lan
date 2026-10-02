# Annexe : captures tcpdump depuis la machine attaquante

Captures brutes réalisées avec `tcpdump` sur la machine Kali, branchée dans le LAN1.

## Capture 1 : trafic ARP et DNS du LAN

On y voit les requêtes ARP des clients qui cherchent la passerelle `10.2.1.254`, et les requêtes DNS envoyées à `1.1.1.1` (`one.one.one.one`).

```
┌──(kali㉿kali)-[~]
└─$ sudo tcpdump      
[sudo] password for kali: 
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
17:13:04.144478 DTPv1, length 26
17:13:04.144478 aa:bb:cc:00:01:11 (oui Unknown) > 01:00:0c:00:00:00 (oui Unknown) SNAP, oui Cisco (0x00000c), pid Unknown (0x0003), length 68: 
        0x0000:  aaaa 0300 000c 0003 0000 0000 0100 0ccc  ................
        0x0010:  cccc aabb cc00 0111 0022 aaaa 0300 000c  ........."......
        0x0020:  2004 0100 0100 0500 0002 0005 0400 0300  ................
        0x0030:  0540 0004 000a aabb cc00 0111 1000 0014  .@..............
        0x0040:  0002 000f 0000 0000 0ffd 527f            ..........R.
17:13:04.780511 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:06.785054 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:08.787781 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:10.789513 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:12.794274 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:14.800208 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:16.288263 ARP, Request who-has 10.2.1.254 (Broadcast) tell 10.2.1.11, length 50
17:13:16.288263 ARP, Request who-has 10.2.1.254 (Broadcast) tell 10.2.1.11, length 50
17:13:16.368883 ARP, Request who-has 10.2.1.254 tell 10.2.1.125, length 28
17:13:16.380969 ARP, Reply 10.2.1.254 is-at ca:01:08:aa:00:00 (oui Unknown), length 46
17:13:16.380992 IP 10.2.1.125.42548 > one.one.one.one.domain: 62406+ PTR? 254.1.2.10.in-addr.arpa. (41)
17:13:16.380970 ARP, Reply 10.2.1.254 is-at ca:01:08:aa:00:00 (oui Unknown), length 46
17:13:16.415839 IP one.one.one.one.domain > 10.2.1.125.42548: 62406 NXDomain 0/0/0 (41)
17:13:16.416164 IP 10.2.1.125.51823 > one.one.one.one.domain: 21372+ PTR? 11.1.2.10.in-addr.arpa. (40)
17:13:16.458580 IP one.one.one.one.domain > 10.2.1.125.51823: 21372 NXDomain 0/0/0 (40)
17:13:16.459227 IP 10.2.1.125.54475 > one.one.one.one.domain: 14042+ PTR? 125.1.2.10.in-addr.arpa. (41)
17:13:16.493660 IP one.one.one.one.domain > 10.2.1.125.54475: 14042 NXDomain 0/0/0 (41)
17:13:16.495666 IP 10.2.1.125.58084 > one.one.one.one.domain: 54160+ PTR? 1.1.1.1.in-addr.arpa. (38)
17:13:16.527321 IP one.one.one.one.domain > 10.2.1.125.58084: 54160 1/0/0 PTR one.one.one.one. (67)
17:13:16.801182 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:18.808894 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:20.822127 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:22.825421 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:13:24.823437 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
^C
26 packets captured
26 packets received by filter
0 packets dropped by kernel
```

## Capture 2 : échange DHCP pendant l'attaque

À 17:49:45, un client (`00:50:79:66:68:01`) envoie une requête DHCP, et la machine `10.2.1.125` lui répond (*BOOTP/DHCP, Reply*).

```
┌──(kali㉿kali)-[/etc]
└─$ sudo tcpdump
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
17:49:39.178530 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:41.182231 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:43.184855 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:44.716201 IP6 fe80::a00:27ff:fed0:9b71 > ip6-allrouters: ICMP6, router solicitation, length 8
17:49:44.780042 IP 10.2.1.125.57339 > one.one.one.one.domain: 32655+ PTR? 1.7.b.9.0.d.e.f.f.f.7.2.0.0.a.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.e.f.ip6.arpa. (90)
17:49:44.826688 IP one.one.one.one.domain > 10.2.1.125.57339: 32655 NXDomain 0/0/0 (90)
17:49:44.879532 IP 10.2.1.125.54047 > one.one.one.one.domain: 51732+ PTR? 1.1.1.1.in-addr.arpa. (38)
17:49:44.919119 IP one.one.one.one.domain > 10.2.1.125.54047: 51732 1/0/0 PTR one.one.one.one. (67)
17:49:44.919390 IP 10.2.1.125.35672 > one.one.one.one.domain: 63597+ PTR? 125.1.2.10.in-addr.arpa. (41)
17:49:44.963746 IP one.one.one.one.domain > 10.2.1.125.35672: 63597 NXDomain 0/0/0 (41)
17:49:45.186828 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:45.739566 IP 0.0.0.0.bootpc > 255.255.255.255.bootps: BOOTP/DHCP, Request from 00:50:79:66:68:01 (oui Unknown), length 364
17:49:45.739809 IP 10.2.1.125.bootps > 10.2.1.229.bootpc: BOOTP/DHCP, Reply, length 300
17:49:45.823027 IP 10.2.1.125.37041 > one.one.one.one.domain: 43799+ PTR? 255.255.255.255.in-addr.arpa. (46)
17:49:45.881400 IP one.one.one.one.domain > 10.2.1.125.37041: 43799 NXDomain 0/0/0 (46)
17:49:45.881680 IP 10.2.1.125.45717 > one.one.one.one.domain: 13984+ PTR? 0.0.0.0.in-addr.arpa. (38)
17:49:45.928645 IP one.one.one.one.domain > 10.2.1.125.45717: 13984 NXDomain 0/0/0 (38)
17:49:45.929351 IP 10.2.1.125.50493 > one.one.one.one.domain: 37549+ PTR? 229.1.2.10.in-addr.arpa. (41)
17:49:45.962096 IP one.one.one.one.domain > 10.2.1.125.50493: 37549 NXDomain 0/0/0 (41)
17:49:46.971268 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:47.740806 ARP, Request who-has 10.2.1.161 (Broadcast) tell 10.2.1.161, length 50
17:49:47.740968 ARP, Request who-has 10.2.1.161 (Broadcast) tell 10.2.1.161, length 50
17:49:47.806735 IP 10.2.1.125.45851 > one.one.one.one.domain: 34394+ PTR? 161.1.2.10.in-addr.arpa. (41)
17:49:47.867102 IP one.one.one.one.domain > 10.2.1.125.45851: 34394 NXDomain 0/0/0 (41)
17:49:48.740382 ARP, Request who-has 10.2.1.161 (Broadcast) tell 10.2.1.161, length 50
17:49:48.740382 ARP, Request who-has 10.2.1.161 (Broadcast) tell 10.2.1.161, length 50
17:49:48.969752 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:49.741085 ARP, Request who-has 10.2.1.161 (Broadcast) tell 10.2.1.161, length 50
17:49:49.741085 ARP, Request who-has 10.2.1.161 (Broadcast) tell 10.2.1.161, length 50
17:49:49.855241 ARP, Request who-has 10.2.1.254 tell 10.2.1.125, length 28
17:49:49.861658 ARP, Reply 10.2.1.254 is-at ca:01:08:aa:00:00 (oui Unknown), length 46
17:49:49.899581 IP 10.2.1.125.56571 > one.one.one.one.domain: 52483+ PTR? 254.1.2.10.in-addr.arpa. (41)
17:49:49.929154 IP one.one.one.one.domain > 10.2.1.125.56571: 52483 NXDomain 0/0/0 (41)
17:49:50.878447 ARP, Request who-has 10.2.1.229 tell 10.2.1.125, length 28
17:49:50.980670 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:51.902981 ARP, Request who-has 10.2.1.229 tell 10.2.1.125, length 28
17:49:52.926329 ARP, Request who-has 10.2.1.229 tell 10.2.1.125, length 28
17:49:52.984322 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:54.981765 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:56.998170 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:49:58.999821 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:50:01.007917 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:50:03.021575 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:50:05.029412 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:50:05.232250 DTPv1, length 26
17:50:05.232250 aa:bb:cc:00:01:11 (oui Unknown) > 01:00:0c:00:00:00 (oui Unknown) SNAP, oui Cisco (0x00000c), pid Unknown (0x0003), length 68: 
        0x0000:  aaaa 0300 000c 0003 0000 0000 0100 0ccc  ................
        0x0010:  cccc aabb cc00 0111 0022 aaaa 0300 000c  ........."......
        0x0020:  2004 0100 0100 0500 0002 0005 0400 0300  ................
        0x0030:  0540 0004 000a aabb cc00 0111 01a1 0000  .@..............
        0x0040:  0000 0000 0000 0000 91ac ff3c            ...........<
```
