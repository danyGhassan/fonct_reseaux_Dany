# I. DHCP Client

### 🌞 Déterminer

```
PS C:\WINDOWS\system32> ipconfig /all

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : MediaTek Wi-Fi 6 MT7921 Wireless LAN Card
   Adresse physique . . . . . . . . . . . : 48-E7-DA-A7-C7-5F
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::bea3:6d81:698c:40d5%4(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.71.18(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.240.0
   Bail obtenu. . . . . . . . . . . . . . : vendredi 27 octobre 2023 09:02:57
   Bail expirant. . . . . . . . . . . . . : samedi 28 octobre 2023 09:02:49
   Passerelle par défaut. . . . . . . . . : 10.33.79.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.79.254
   IAID DHCPv6 . . . . . . . . . . . : 55109594
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2A-D2-48-C3-48-E7-DA-A7-C7-5F
   Serveurs DNS. . .  . . . . . . . . . . : 8.8.8.8
                                       1.1.1.1
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
### 🌞 Capturer un échange DHCP

```
PS C:\WINDOWS\system32> ipconfig /release

Configuration IP de Windows

Aucune opération ne peut être effectuée sur Ethernet 2 lorsque
son média est déconnecté.
Aucune opération ne peut être effectuée sur Connexion au réseau local* 1 lorsque
son média est déconnecté.

Carte inconnue Connexion au réseau local :

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . :

Carte Ethernet Ethernet 2 :

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . :

Carte Ethernet Ethernet 3 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::2c62:a90d:4377:c6%10
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.56.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :

Carte Ethernet Ethernet 4 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::1c1:6b45:9b7e:293a%9
   Adresse IPv4. . . . . . . . . . . . . .: 10.3.1.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :

Carte Ethernet Ethernet 5 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::3754:39e6:1cd9:dc52%17
   Adresse IPv4. . . . . . . . . . . . . .: 10.3.2.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :

Carte réseau sans fil Connexion au réseau local* 1 :

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . :

Carte réseau sans fil Connexion au réseau local* 2 :

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . :

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::bea3:6d81:698c:40d5%4
   Passerelle par défaut. . . . . . . . . :

Carte Ethernet VMware Network Adapter VMnet1 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::5e93:4948:226:601e%22
   Passerelle par défaut. . . . . . . . . :

Carte Ethernet VMware Network Adapter VMnet8 :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::b0e0:21c:e45e:6219%24
   Passerelle par défaut. . . . . . . . . :
```

### 🌞 Analyser la capture Wireshark

```
c'est la 2eme tram qui contient tous les infos proposés au client
```

### 🦈 tp4_dhcp_client.pcapng

## II. Serveur DHCP

