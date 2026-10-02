# Réseau & attaques LAN : du ping au DNS spoofing

Labs réseau réalisés sous **GNS3**. J'ai construit des réseaux de A à Z (adressage, ARP, DHCP, routage, NAT), puis je les ai attaqués depuis une machine **Kali Linux** pour comprendre comment un attaquant se place en *Man-in-the-Middle* sur un réseau local.

> Contexte : travaux pratiques du Bachelor Cybersécurité & Ethical Hacking, EFREI (2026).

## Ce que montre ce projet

| Partie | Ce que j'ai fait | Compétences |
|---|---|---|
| Construire | LAN, adressage IP, table ARP, serveur DHCP `dnsmasq` | TCP/IP, ARP, DHCP (DORA) |
| Interconnecter | Routage entre deux LAN, NAT vers internet, DNS | Routeur Cisco, NAT, DNS |
| Attaquer | Rogue DHCP, DHCP spoofing, DNS spoofing | Kali Linux, Man-in-the-Middle |
| Analyser | Captures Wireshark et `tcpdump` à chaque étape | Wireshark, tcpdump |

## Les deux maquettes

- **TP1 — un LAN simple (`10.1.1.0/24`) :** trois clients, un serveur DHCP sous Rocky Linux et une machine attaquante sous Kali.
- **TP2 — deux LAN routés :** `LAN1 10.2.1.0/24` et `LAN2 10.2.2.0/24` reliés par un routeur Cisco qui fait le NAT vers internet, avec un serveur DHCP dans le LAN1 et l'attaquant Kali sur le même segment.

## Sommaire

**TP1 — LAN, ARP et DHCP**
1. [Adressage IP et premier ping](tp1-lan-arp-dhcp/01-adressage-ip-et-ping.md)
2. [Trois machines et table ARP](tp1-lan-arp-dhcp/02-table-arp.md)
3. [Serveur DHCP avec dnsmasq](tp1-lan-arp-dhcp/03-serveur-dhcp.md)
4. [Attaque : faux serveur DHCP](tp1-lan-arp-dhcp/04-attaque-rogue-dhcp.md)

**TP2 — routage, NAT et spoofing**
1. [Routage entre deux LAN](tp2-routage-nat-spoofing/01-routage-inter-lan.md)
2. [Accès internet (NAT) et DHCP](tp2-routage-nat-spoofing/02-nat-et-dhcp.md)
3. [Attaque : DHCP spoofing](tp2-routage-nat-spoofing/03-attaque-dhcp-spoofing.md)
4. [Attaque : DNS spoofing](tp2-routage-nat-spoofing/04-attaque-dns-spoofing.md)

## Captures

Toutes les captures réseau sont dans [`captures/`](captures/), ouvrables avec Wireshark. Chaque page y renvoie au bon moment.

## Ce que j'en retiens

Une attaque réseau ne demande pas forcément d'outil sophistiqué : un simple serveur `dnsmasq` mal placé suffit à détourner tout le trafic d'un LAN. La défense se joue surtout sur le switch (DHCP Snooping, Dynamic ARP Inspection) et sur le chiffrement de bout en bout (HTTPS).

## Outils

GNS3 · VPCS · Rocky Linux · Kali Linux · routeur Cisco · dnsmasq · Wireshark · tcpdump
