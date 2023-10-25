# TP1 : Maîtrise réseau du poste
Pour ce TP, on va utiliser uniquement votre poste (pas de VM, rien, quedal, quetchi).
Le but du TP : se remettre dans le bain tranquillement en manipulant pas mal de concepts qu'on a vu l'an dernier.
C'est un premier TP chill, qui vous(ré)apprend à maîtriser votre poste en ce qui concerne le réseau. Faites le seul ou avec votre mate préféré bien sûr, mais jouez le jeu, faites vos propres recherches.
La "difficulté" va crescendo au fil du TP, mais la solution tombe très vite avec une ptite recherche Google si vos connaissances de l'an dernier deviennent floues.

## TP1 : Maîtrise réseau du poste
I. Basics
II. Go further
III. Le requin


### I. Basics

Tout est à faire en ligne de commande, sauf si précision contraire.

☀️ Carte réseau WiFi
Déterminer...

l'adresse MAC de votre carte WiFi

```
Adresse physique . . . . . . . . . . . : F8-B5-4D-43-18-4A
```

l'adresse IP de votre carte WiFi

```
Adresse IPv4. . . . . . . . . . . . . .: 10.33.76.233/20(préféré)
```

le masque de sous-réseau du réseau LAN auquel vous êtes connectés en WiFi

```
Masque de sous-réseau. . . . . . . . . : 255.255.240.0/20
```
en notation CIDR, par exemple /16

ET en notation décimale, par exemple 255.255.0.0





☀️ Déso pas déso
Pas besoin d'un terminal là, juste une feuille, ou votre tête, ou un tool qui calcule tout hihi. Déterminer...

l'adresse de réseau du LAN auquel vous êtes connectés en WiFi
```
10.33.64.0
```
l'adresse de broadcast
```
10.33.79.255
```

le nombre d'adresses IP disponibles dans ce réseau

```
4094
```
☀️ Hostname

déterminer le hostname de votre PC

```
PS C:\Users\ozoux\TP-reseaux-B2> hostname Guillaume 
```
☀️ Passerelle du réseau
Déterminer...

l'adresse IP de la passerelle du réseau
```
PS C:\Users\ozoux\TP-reseaux-B2> ipconfig /all
Passerelle par défaut. . . . . . . . . : 10.33.79.254
```
l'adresse MAC de la passerelle du réseau
```
PS C:\Users\ozoux\TP-reseaux-B2> arp -a

Interface : 10.33.76.233 --- 0x9
  Adresse Internet      Adresse physique      Type
    10.33.79.254          7c-5a-1c-d3-d8-76 dynamique
```

☀️ Serveur DHCP et DNS
Déterminer...

l'adresse IP du serveur DHCP qui vous a filé une IP
```
PS C:\Users\ozoux\TP-reseaux-B2> ipconfig /all
Carte réseau sans fil Wi-Fi :
Adresse IPv4. . . . . . . . . . . . . .: 10.33.76.233(préféré)
```
l'adresse IP du serveur DNS que vous utilisez quand vous allez sur internet
```
PS C:\Users\ozoux\TP-reseaux-B2> ipconfig /all
Carte réseau sans fil Wi-Fi :
Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
```

☀️ Table de routage
Déterminer...

dans votre table de routage, laquelle est la route par défaut
```
PS C:\Users\ozoux\TP-reseaux-B2> route print
IPv4 Table de routage
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
          0.0.0.0          0.0.0.0     10.33.79.254     10.33.76.233     45
```

### II. Go further

Toujours tout en ligne de commande.


☀️ Hosts ?

faites en sorte que pour votre PC, le nom b2.hello.vous corresponde à l'IP 1.1.1.1

prouvez avec un ping b2.hello.vous que ça ping bien 1.1.1.1
```
PS C:\Users\ozoux\TP-reseaux-B2> ping b2.hello.vous

Envoi d’une requête 'ping' sur b2.hello.vous [1.1.1.1] avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=10 ms TTL=57
Réponse de 1.1.1.1 : octets=32 temps=11 ms TTL=57
Réponse de 1.1.1.1 : octets=32 temps=10 ms TTL=57
Réponse de 1.1.1.1 : octets=32 temps=10 ms TTL=57

Statistiques Ping pour 1.1.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 10ms, Maximum = 11ms, Moyenne = 10ms
```

☀️ Go mater une vidéo youtube et déterminer, pendant qu'elle tourne...

l'adresse IP du serveur auquel vous êtes connectés pour regarder la vidéo
le port du serveur auquel vous êtes connectés
le port que votre PC a ouvert en local pour se connecter au port du serveur distant
```
PS C:\Users\ozoux> netstat -anb  | Select-String 91.68.245.76 -Context 0,1

>   UDP    0.0.0.0:59446          91.68.245.76:443
   [msedge.exe]
```

☀️ Requêtes DNS
Déterminer...

à quelle adresse IP correspond le nom de domaine www.ynov.com
```
PS C:\Users\ozoux> nslookup ynov.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::681a:be9
          2606:4700:20::681a:ae9
          2606:4700:20::ac43:4ae2
          104.26.10.233
          104.26.11.233
          172.67.74.226

```

à quel nom de domaine correspond l'IP 174.43.238.89
```
PS C:\Users\ozoux> ping -a 174.43.238.89

Envoi d’une requête 'ping' sur 89.sub-174-43-238.myvzw.com [174.43.238.89] avec 32 octets de données :
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.

Statistiques Ping pour 174.43.238.89:
    Paquets : envoyés = 4, reçus = 0, perdus = 4 (perte 100%),
```

☀️ Hop hop hop
Déterminer...

par combien de machines vos paquets passent quand vous essayez de joindre www.ynov.com
```
PS C:\Users\ozoux> tracert ynov.com

Détermination de l’itinéraire vers ynov.com [172.67.74.226]
avec un maximum de 30 sauts :

  1     6 ms     1 ms     1 ms  10.33.79.254
  2    13 ms     3 ms     2 ms  145.117.7.195.rev.sfr.net [195.7.117.145]
  3     4 ms     4 ms     4 ms  237.195.79.86.rev.sfr.net [86.79.195.237]
  4     4 ms     5 ms     6 ms  196.224.65.86.rev.sfr.net [86.65.224.196]
  5    12 ms    13 ms    13 ms  12.148.6.194.rev.sfr.net [194.6.148.12]
  6    12 ms    12 ms    11 ms  12.148.6.194.rev.sfr.net [194.6.148.12]
  7    11 ms    12 ms    12 ms  141.101.67.48
  8    13 ms    13 ms    12 ms  172.71.124.4
  9    13 ms    12 ms    14 ms  172.67.74.226

Itinéraire déterminé.
```

☀️ IP publique
Déterminer...

l'adresse IP publique de la passerelle du réseau (le routeur d'YNOV donc si vous êtes dans les locaux d'YNOV quand vous faites le TP)
```

PS C:\Users\ozoux> curl ipconfig.me


StatusCode        : 200
StatusDescription : OK
Content           : 195.7.117.146
RawContent        : HTTP/1.1 200 OK
                    access-control-allow-origin: *
                    x-envoy-upstream-service-time: 1
                    strict-transport-security: max-age=2592000; includeSubDomains
                    Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=259200...
Forms             : {}
Headers           : {[access-control-allow-origin, *], [x-envoy-upstream-service-time, 1], [strict-transport-security,
                    max-age=2592000; includeSubDomains], [Alt-Svc, h3=":443"; ma=2592000,h3-29=":443"; ma=2592000]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 13
```

☀️ Scan réseau
Déterminer...

combien il y a de machines dans le LAN auquel vous êtes connectés
```
PS C:\Users\ozoux> nmap -sP 10.33.64.0/20
Nmap done: 4096 IP addresses (852 hosts up) scanned in 205.14 seconds
```



### III. Le requin

☀️ Capture ARP


📁 fichier arp.pcap

capturez un échange ARP entre votre PC et la passerelle du réseau


Si vous utilisez un filtre Wireshark pour mieux voir ce trafic, précisez-le moi dans le compte-rendu.

[Lien vers capture ARP](./captures_arp.pcapng)

☀️ Capture DNS


📁 fichier dns.pcap

capturez une requête DNS vers le domaine de votre choix et la réponse
vous effectuerez la requête DNS en ligne de commande

[Lien vers capture DNS](./captures_dns.pcapng)


☀️ Capture TCP


📁 fichier tcp.pcap

effectuez une connexion qui sollicite le protocole TCP
je veux voir dans la capture :

un 3-way handshake
un peu de trafic
la fin de la connexion TCP

[Lien vers capture DNS](./captures_tcp.pcapng)


Si vous utilisez un filtre Wireshark pour mieux voir ce trafic, précisez-le moi dans le compte-rendu.




Je sais que je vous l'ai déjà servi l'an dernier lui, mais j'aime trop ce meme hihi 🐈
