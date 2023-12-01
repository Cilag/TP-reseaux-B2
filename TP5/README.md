# I. Tester


🌞 **Récupérer l'application dans la VM `hosting.tp5.b1`**

```
[c1@localhost TP5-r-seau]$ ls
client.py  server.py
```

🌞 **Essayer de lancer l'app**

```
[c1@localhost TP5-r-seau]$ python3 server.py
13337
Le serveur tourne sur 127.0.0.1:13337
```
- il faut :
  - déterminer dans quel langage est codé l'application
  ```
  Python
  ```
  - juste lancer avec une commande adaptée !
  ```
  [c1@localhost TP5-r-seau]$ python3 server.py
  ```
- une commande `ss` qui me montre que le programme écoute derrière IP:port
 ```
  [c1@localhost TP5-r-seau]$ sudo ss -l | grep 127.0.0.1:13337
[sudo] password for c1:
tcp   LISTEN 0      1                               127.0.0.1:13337                   0.0.0.0:*
[c1@localhost TP5-r-seau]$
 ```

🌞 **Tester l'app depuis `hosting.tp5.b1`**

- maintenant, on lance le **client**
```
[c1@localhost TP5-r-seau]$ python3 client.py
Veuillez saisir une opération arithmétique :
```

# II. Intégrer

## 1. Environnement

Petite section pour préparer un environnement correct. On va faire le strict minimum.

C'est une application installée à la main, on va la positionner un peu à l'arrache dans `/opt`.

🌞 **Créer un dossier /opt/calculatrice**

- il doit contenir le code de l'application

## 2. systemd service

Dans cette section, on va créer un fichier `calculatrice.service` qui nous permettra de saisir des commandes comme :

```bash
$ sudo systemctl start calculatrice
$ sudo systemctl enable calculatrice

$ sudo journalctl -xe -u calculatrice
```

### B. Service basique

🌞 **Créer le fichier `/etc/systemd/system/calculatrice.service`**

- vous devrez apposer des permissions correctes sur ce fichier
  - c'est quoi "correctes" ?
  - go faire un `ls -al` dans le dossier pour 
```
[c1@localhost /]$ ls -al /etc/systemd/system/
total 12
drwxr-xr-x. 9 root root 4096 Nov 28 16:05 .
drwxr-xr-x. 4 root root  169 Sep 19 18:59 ..
drwxr-xr-x. 2 root root   31 Oct 24 12:09 basic.target.wants
-rw-r--r--. 1 root root  151 Nov 28 16:18 calculatrice.service
lrwxrwxrwx. 1 root root   37 Oct 24 12:09 ctrl-alt-del.target -> /usr/lib/systemd/system/reboot.target
lrwxrwxrwx. 1 root root   41 Oct 24 12:09 dbus-org.fedoraproject.FirewallD1.service -> /usr/lib/systemd/system/firewalld.service
lrwxrwxrwx. 1 root root   57 Oct 24 12:09 dbus-org.freedesktop.nm-dispatcher.service -> /usr/lib/systemd/system/NetworkManager-dispatcher.service
lrwxrwxrwx. 1 root root   41 Oct 24 12:09 dbus-org.freedesktop.timedate1.service -> /usr/lib/systemd/system/timedatex.service
lrwxrwxrwx. 1 root root   41 Oct 24 12:11 default.target -> /usr/lib/systemd/system/multi-user.target
drwxr-xr-x. 2 root root   32 Oct 24 12:09 getty.target.wants
drwxr-xr-x. 2 root root 4096 Nov 28 16:06 multi-user.target.wants
drwxr-xr-x. 2 root root   48 Oct 24 12:09 network-online.target.wants
drwxr-xr-x. 2 root root   52 Oct 24 12:09 sockets.target.wants
drwxr-xr-x. 2 root root  187 Oct 24 12:10 sysinit.target.wants
lrwxrwxrwx. 1 root root   39 Oct 24 12:09 syslog.service -> /usr/lib/systemd/system/rsyslog.service
lrwxrwxrwx. 1 root root    9 Apr 12  2021 systemd-timedated.service -> /dev/null
drwxr-xr-x. 2 root root   33 Oct 24 12:09 timers.target.wants
```

🌞 **Démarrer le service**
- prouvez que...
  - le service est actif avec un `systemctl status`
```
  [c1@localhost /]$ sudo systemctl status calculatrice
● calculatrice.service - Super calculatrice réseau
   Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2023-11-28 16:20:20 CET; 6s ago
 Main PID: 8119 (python3)
    Tasks: 2 (limit: 11148)
   Memory: 5.7M
   CGroup: /system.slice/calculatrice.service
           └─8119 /usr/bin/python3 /opt/calculatrice/server.py

Nov 28 16:20:20 localhost.localdomain systemd[1]: Started Super calculatrice réseau.
Nov 28 16:20:20 localhost.localdomain python3[8119]: Le serveur tourne sur 127.0.0.1:13337
```
  - le service tourne derrière un port donné avec un `ss`
```
[c1@localhost /]$ ss -ltnp | grep ":13337"
LISTEN 0      1          127.0.0.1:13337      0.0.0.0:*
```
  - c'est fonctionnel : vous pouvez utiliser l'app en lançant le client
```
[c1@localhost calculatrice]$ python3 client.py 127.0.0.1 13337
Veuillez saisir une opération arithmétique : 2+2
```

🌞 **Configurer une politique de redémarrage** dans le fichier `calculatrice.service`

- un `cat` du fichier pour le compte-rendu
```
[c1@localhost /]$ cat etc/systemd/system/calculatrice.service
[Unit]
Description=Super calculatrice réseau

[Service]
Restart=on-failure
RestartSec=30
ExecStart=/usr/bin/python3 /opt/calculatrice/server.py

[Install]
WantedBy=multi-user.target
[c1@localhost /]$
```

🌞 **Tester que la politique de redémarrage fonctionne**

- lancez le service, repérer son PID, et tuer le process avec un `kill -9 <PID>`
```
[c1@localhost /]$ systemctl status calculatrice.service
● calculatrice.service - Super calculatrice réseau
   Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2023-11-30 13:51:35 CET; 20s ago
 Main PID: 1669 (python3)
    Tasks: 2 (limit: 11148)
   Memory: 7.5M
   CGroup: /system.slice/calculatrice.service
           └─1669 /usr/bin/python3 /opt/calculatrice/server.py

Nov 30 13:51:35 localhost.localdomain systemd[1]: calculatrice.service: Succeeded.
Nov 30 13:51:35 localhost.localdomain systemd[1]: Stopped Super calculatrice réseau.
Nov 30 13:51:35 localhost.localdomain systemd[1]: Started Super calculatrice réseau.
Nov 30 13:51:35 localhost.localdomain python3[1669]: Le serveur tourne sur 127.0.0.1:13337
[c1@localhost /]$ sudo kill -9 1669
[c1@localhost /]$ systemctl status calculatrice.service
● calculatrice.service - Super calculatrice réseau
   Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2023-11-30 13:55:14 CET; 13s ago
 Main PID: 1687 (python3)
    Tasks: 2 (limit: 11148)
   Memory: 5.7M
   CGroup: /system.slice/calculatrice.service
           └─1687 /usr/bin/python3 /opt/calculatrice/server.py

Nov 30 13:55:14 localhost.localdomain systemd[1]: Started Super calculatrice réseau.
Nov 30 13:55:14 localhost.localdomain python3[1687]: Le serveur tourne sur 127.0.0.1:13337
```

🌞 **Ouverture automatique du firewall** dans le fichier `calculatrice.service`

- un `cat` du fichier pour le compte-rendu
```
[c1@localhost /]$ cat etc/systemd/system/calculatrice.service
[Unit]
Description=Super calculatrice réseau

[Service]
Restart=on-failure
RestartSec=30
ExecStartPre=/sbin/iptables -A INPUT -p tcp --dport 13337 -j ACCEPT
ExecStart=/usr/bin/python3 /opt/calculatrice/server.py
ExecStopPost=/sbin/iptables -D INPUT -p tcp --dport 13337 -j ACCEPT

[Install]
WantedBy=multi-user.target
```

🌞 **Vérifier l'ouverture automatique du firewall**

- lancer/stopper le service et constater avec un `firewall-cmd --list-all` que le port s'ouvre/se ferme bien automatiquement
```
[c1@localhost /]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 13337/tcp
  protocols:
  forward: no
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```
- tester une connexion depuis **votre PC**
  - accéder au service
```
PS C:\Users\ozoux\TP5-r-seau> python client.py 0.0.0.0 13337
Veuillez saisir une opération arithmétique (ou 'exit' pour quitter) : 2+2
```
## 3. Monitoring

🌞 **Installer Netdata** sur `hosting.tp5.b1`

- en suivant la doc officielle
- assurez-vous que le dashboard Web est fonctionnel une fois l'install terminée

🌞 **Configurer une sonde TCP**

![Alt text](<Capture d'écran 2023-11-30 234142.png>)

```
jobs:
  - name: calculatrice
    host: 127.0.0.1
    ports:
      - 33337
```
🌞 **Alerting Discord**

![Alt text](<Capture d'écran 2023-11-30 235618.png>)

# III. Héberger

## 1. Interface bridge

➜ Conf VM `hosting.tp5.b1`

- éteindre la VM
- la carte NAT ne sera plus utile
- ajoutez lui une nouvelle carte réseau en bridge
- allumez la VM
- récupérez une IP en DHCP sur cette interface
  - vous devriez récup une IP dans la range du LAN de l'école
  - vous devriez avoir un accès internet

## 2. Firewall

🌞 **Assurez-vous qu'aucun port est inutilement ouvert**

## 3. Serveur SSH

🌞 **Conf serveur SSH**

- prouvez avec un `ss` que ça a pris effet

```
[c1@Host-005 ~]$ sudo ss -tulnp | grep sshd
tcp   LISTEN 0      128         10.1.1.3:22         0.0.0.0:*    users:(("sshd",pid=2270,fd=3))
```

## 4. Serveur Calculatrice

🌞 **Conf serveur Calculatrice**

- prouvez avec un `ss` que le changement a bien pris effet
```
[c1@Host-005 calculatrice]$ sudo ss -tulnp | grep "13337"
tcp   LISTEN 0      1       192.168.1.69:13337      0.0.0.0:*    users:(("python3",pid=2380,fd=4))
```
teste
```
PS C:\Users\ozoux> python C:\Users\ozoux\TP5-r-seau\client.py 192.168.1.69 13337
Veuillez saisir une opération arithmétique (ou 'exit' pour quitter) : 2+2
Veuillez saisir une opération arithmétique (ou 'exit' pour quitter) :
```