# 4. Attaque : faux serveur DHCP (rogue DHCP)

**Objectif :** depuis une machine **Kali Linux** branchée sur le LAN, monter un **faux serveur DHCP** qui répond aux clients à la place du serveur légitime.

**Pourquoi c'est dangereux :** le serveur DHCP indique au client sa passerelle et son DNS. Un attaquant qui contrôle ces informations peut rediriger tout le trafic de la victime (voir le [TP2](../tp2-routage-nat-spoofing/)).

## Configuration du rogue DHCP sur Kali

L'attaquant distribue une plage différente : `10.1.1.210` → `10.1.1.250`.

```
┌──(kali㉿kali)-[/etc]
└─$ cat dnsmasq.conf
interface=eth1
dhcp-range=10.1.1.210,10.1.1.250,12h
```

```
┌──(kali㉿kali)-[~]
└─$ sudo systemctl status dnsmasq
● dnsmasq.service - dnsmasq - A lightweight DHCP and caching DNS server
     Active: active (running) since Sat 2026-01-17 20:04:51 EST; 1min 13s ago
Jan 17 20:04:51 kali dnsmasq-dhcp[1821]: DHCP, IP range 10.1.1.210 -- 10.1.1.250, lease time 12h
```

## Test : le client tombe sur l'attaquant

Le serveur légitime est arrêté, seul le rogue DHCP répond :

```
[aki69@localhost ~]$ sudo systemctl status dnsmasq
○ dnsmasq.service - DNS caching server.
     Active: inactive (dead)
```

```
VPCS> dhcp
DDORA IP 10.1.1.246/24 GW 10.1.1.251
```

Le client a reçu une IP dans la plage de l'attaquant (`10.1.1.246`), et la passerelle annoncée est la machine Kali (`10.1.1.251`).

## La « course » entre les deux serveurs

📁 Capture : [`captures/tp1-04-rogue-dhcp-race.pcapng`](../captures/tp1-04-rogue-dhcp-race.pcapng)

Quand les deux serveurs sont actifs en même temps, c'est **le premier qui répond** qui gagne. Dans la capture :

1. le client envoie un *Discover* ;
2. le rogue DHCP (`10.1.1.251`) répond en premier avec son *Offer*, puis valide avec un *Ack* ;
3. l'*Offer* du serveur légitime (`10.1.1.253`) arrive trop tard et est ignorée ;
4. le client s'annonce avec l'adresse `10.1.1.248`, prise dans la plage de l'attaquant.

## Contre-mesure

Sur un vrai réseau, on bloque ce type d'attaque avec le **DHCP Snooping** sur les switchs : seuls les ports déclarés « de confiance » ont le droit d'envoyer des réponses DHCP.
