# I-Exploration locale en solo

## 1 Affichage d'informations sur la pile TCP/IP locale

🌞 Affichez les infos des cartes réseau de votre PC

```
PS C:\Users\ghass> ipconfig

Configuration IP de Windows

Carte Ethernet Ethernet 2 :

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . :

Carte Ethernet Ethernet 3 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::973:c0ad:c079:122d%53
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.56.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
```
🌞 Affichez votre gateway

```
PS C:\Users\ghass> ipconfig

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::bea3:6d81:698c:40d5%4
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.48.22
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Passerelle par défaut. . . . . . . . . : 10.33.51.254
```
🌞 Déterminer la MAC de la passerelle
```
 C:\Users\ghass> arp -a

Interface : 10.33.48.22 --- 0x4
  Adresse Internet      Adresse physique      Type
  10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique
```


🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)
 ```
 panneau de configuration > réseau et internet > centre réseau et partage > Wifi > Details >
 ipV4 : 10.33.48.22
 MAC : 48-E7-DA-A7-C7-5F
 gateway : 10.33.51.254
 ```

# 2/ Modifications des informations 
### 2.Modification d'adresse IP(part 1)
🌞 Utilisez l'interface graphique de votre OS pour changer d'adresse IP :

```
panneau de configuration > réseau et internet > centre réseau et partage > Wifi > Propriété > Protocole internet version 4 :
Adresse ip : 10.33.48.21
masque de sous réseau : 255.255.252.0
passerelle par defaut : 10.33.51.254
 ```

🌞 Il est possible que vous perdiez l'accès internet. Que ce soit le cas ou non, expliquez pourquoi c'est possible de perdre son accès internet en faisant cette opération

```Je pers l'accès a mon wifi car je me connecte a une ip qui est déjà prise par un autre ordinateur,  je peux doonc pas me connecter sur l'adresse manuelle.```

# II. Exploration locale en duo

## 3/ Modification d'adresse IP

🌞 Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau
```
panneau de configuration > réseau et internet > centre réseau et partage > Wifi > Propriété > Protocole internet version 4 :
Adresse ip : 10.10.10.2
masque de sous réseau : 255.255.255.0
```
  
🌞 Vérifier à l'aide d'une commande que votre IP a bien été changée
```PS C:\Users\abatm> IPCONFIG

Configuration IP de Windows


Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::60ed:14a4:aa20:18a6%14
   Adresse IPv4. . . . . . . . . . . . . .: 10.10.10.2
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
```

🌞 Vérifier que les deux machines se joignent
```PS C:\Users\abatm> ping 10.10.10.1

Envoi d’une requête 'Ping'  10.10.10.1 avec 32 octets de données :
Réponse de 10.10.10.1 : octets=32 temps=1 ms TTL=128
Réponse de 10.10.10.1 : octets=32 temps=2 ms TTL=128
Réponse de 10.10.10.1 : octets=32 temps=3 ms TTL=128
Réponse de 10.10.10.1 : octets=32 temps=2 ms TTL=128

Statistiques Ping pour 10.10.10.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 3ms, Moyenne = 2ms
```

🌞 Déterminer l'adresse MAC de votre correspondant

```PS C:\Users\abatm> arp -a
Interface : 10.10.10.2 --- 0xe
  Adresse Internet      Adresse physique      Type
  10.10.10.1            04-7c-16-a1-bd-25     dynamique
  10.10.10.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```

## 4/ Petit chat privé

🌞 sur le PC client

```
./nc.exe 10.10.10.09 8888

wesh
ptnrffsdf
dc
```

🌞 Visualiser la connexion en cours

```
netstat -a -n -b | Select-String "8888" -Context 0,1

>   TCP    10.10.10.15:51674      10.10.10.9:8888           ESTABLISHED
[nc.exe]
```

🌞Pour aller un peu plus loin

```
.\nc.exe -l -p 8888 -s 10.10.10.15
;jbkhff
??
ça marche ?
```

# 5. Firewall

🌞 Activez et configurez votre firewall

```
ping 10.10.10.9

Envoi d’une requête 'Ping'  10.10.10.9 avec 32 octets de données :
Réponse de 10.10.10.9 : octets=32 temps=3 ms TTL=128
Réponse de 10.10.10.9 : octets=32 temps=3 ms TTL=128
Réponse de 10.10.10.9 : octets=32 temps=2 ms TTL=128
Réponse de 10.10.10.9 : octets=32 temps=3 ms TTL=128
Statistiques Ping pour 10.10.10.9:
Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
Minimum = 2ms, Maximum = 3ms, Moyenne = 2ms
```

# 6. Utilisation d'un des deux comme gateway

🌞Tester l'accès internet

```
ping 1.1.1.1

Envoi d’une requête 'Ping'  1.1.1.1 avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=11 ms TTL=57
Réponse de 1.1.1.1 : octets=32 temps=12 ms TTL=57
Réponse de 1.1.1.1 : octets=32 temps=11 ms TTL=57
Réponse de 1.1.1.1 : octets=32 temps=13 ms TTL=57

Statistiques Ping pour 1.1.1.1:
Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
Minimum = 11ms, Maximum = 13ms, Moyenne = 11ms
```

🌞 Prouver que la connexion Internet passe bien par l'autre PC

``` 
tracert 10.10.10.9

Détermination de l’itinéraire vers 10.10.10.9 avec un maximum de 30 sauts.

1     4 ms     3 ms     5 ms  10.33.51.254
2     3 ms     3 ms     2 ms  237.252.159.77.rev.sfr.net [77.159.252.237]
 3     6 ms     4 ms     5 ms  108.97.30.212.rev.sfr.net [212.30.97.108]
 4     4 ms     4 ms     4 ms  205.212.192.77.rev.sfr.net [77.192.212.205]
 5    24 ms    24 ms    23 ms  2.144.6.194.rev.sfr.net [194.6.144.2]
 6    21 ms    20 ms    21 ms  122.220.96.84.rev.sfr.net [84.96.220.122]
 7    22 ms    22 ms    21 ms  154.224.63.86.rev.sfr.net [86.63.224.154]
 8    26 ms    34 ms    25 ms  185.225.63.86.rev.sfr.net [86.63.225.185]
 9    27 ms    25 ms    28 ms  174.225.63.86.rev.sfr.net [86.63.225.174]
10    27 ms    26 ms    26 ms  169.225.63.86.rev.sfr.net [86.63.225.169]
```

# III. Manipulations d'autres outils/protocoles côté client

### 1. DHCP

🌞Exploration du DHCP, depuis votre PC

```
PS C:\Users\ghass> ipconfig /all

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : MediaTek Wi-Fi 6 MT7921 Wireless LAN Card
   Adresse physique . . . . . . . . . . . : 48-E7-DA-A7-C7-5F
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::bea3:6d81:698c:40d5%4(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.71.18(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
   Bail obtenu. . . . . . . . . . . . . . : jeudi 19 octobre 2023 09:02:58
   Bail expirant. . . . . . . . . . . . . : vendredi 20 octobre 2023 09:02:58
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
   IAID DHCPv6 . . . . . . . . . . . : 55109594
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2A-D2-48-C3-48-E7-DA-A7-C7-5F
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
   ```

### 2. DNS

🌞 Trouver l'adresse IP du serveur DNS que connaît votre ordinateur

```
PS C:\Users\ghass> ipconfig /all

Configuration IP de Windows

   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
```

🌞 Utiliser, en ligne de commande l'outil nslookup (Windows, MacOS) ou dig (GNU/Linux, MacOS) pour faire des requêtes DNS à la main

```
PS C:\Users\ghass> nslookup ynov.com 8.8.8.8
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::681a:ae9
          2606:4700:20::681a:be9
          2606:4700:20::ac43:4ae2
          172.67.74.226
          104.26.10.233
          104.26.11.233
```

# IV. Wireshark

🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

```
dispo sur github
```