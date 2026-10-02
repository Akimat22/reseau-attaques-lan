# 3. Attaque : DHCP spoofing

**Objectif :** reprendre l'attaque rogue DHCP du TP1 dans une infrastructure plus réaliste (routeur, NAT, DHCP légitime). La machine Kali (`10.2.1.252`) se fait passer pour le serveur DHCP et s'annonce comme **passerelle** des victimes.

## La victime récupère la passerelle de l'attaquant

Nouveau client, node6 :

```
node6> ip dhcp
DORA IP 10.2.1.201/24 GW 10.2.1.252

node6> ping efrei.fr
efrei.fr resolved to 51.210.229.203
84 bytes from 51.210.229.203 icmp_seq=1 ttl=53 time=59.426 ms
84 bytes from 51.210.229.203 icmp_seq=2 ttl=53 time=84.680 ms
84 bytes from 51.210.229.203 icmp_seq=3 ttl=53 time=61.269 ms
```

La passerelle annoncée est **`10.2.1.252`, la machine de l'attaquant**. La victime accède quand même à internet : elle ne remarque rien, alors que son trafic passe par la Kali. C'est une position de **Man-in-the-Middle**.

## Observation du réseau depuis l'attaquant

Écoute avec `tcpdump` depuis la Kali (extrait) :

```
┌──(kali㉿kali)-[/etc]
└─$ sudo tcpdump
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
17:46:10.550445 STP 802.1d, Config, Flags [none], bridge-id 8001.aa:bb:cc:00:01:00.8006, length 35
17:46:35.324925 DTPv1, length 26
17:46:40.520386 CDPv2, ttl: 180s, Device-ID 'sw1', length 404
```

On y voit les protocoles d'infrastructure diffusés par le switch Cisco (STP, DTP, CDP). Ils donnent des informations utiles à un attaquant sur le matériel réseau : nom du switch, topologie.

Captures `tcpdump` complètes : [`annexe-captures-tcpdump.md`](./annexe-captures-tcpdump.md).
