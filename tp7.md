# II. SSH

## 1. Fingerprint

### 🌞 Effectuez une connexion SSH en vérifiant le fingerprint

- en rendu je veux voir le message du serveur à la première connexion

  - une commande ssh pour se connecter vers john
```
PS C:\Users\ghass> ssh dany@10.7.1.11
The authenticity of host '10.7.1.11 (10.7.1.11)' can't be established.
ED25519 key fingerprint is SHA256:AEtJxrKXGz1+aH+EHgalAhCRFUFx/83LYCM306PkW0I.
This host key is known by the following other names/addresses:
    C:\Users\ghass/.ssh/known_hosts:4: 10.3.1.11
    C:\Users\ghass/.ssh/known_hosts:7: 10.3.1.12
    C:\Users\ghass/.ssh/known_hosts:8: 10.3.1.254
    C:\Users\ghass/.ssh/known_hosts:9: 10.3.2.12
    C:\Users\ghass/.ssh/known_hosts:10: 10.5.1.11
    C:\Users\ghass/.ssh/known_hosts:11: 10.5.1.3
    C:\Users\ghass/.ssh/known_hosts:15: 10.5.1.254
    C:\Users\ghass/.ssh/known_hosts:16: 10.5.1.12
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.7.1.11' (ED25519) to the list of known hosts.
Last login: Fri Nov 24 09:38:44 2023
[dany@john ~]$
```
- et je veux aussi une commande qui me montre l'empreinte du serveur
```
[dany@john ~]$ sudo ssh-keygen -l -f /etc/ssh/ssh_host_ed25519_k
ey
[sudo] password for dany:
256 SHA256:AEtJxrKXGz1+aH+EHgalAhCRFUFx/83LYCM306PkW0I /etc/ssh/ssh_host_ed25519_key.pub (ED25519)
```

## 2. Conf serveur SSH

### 🌞 Consulter l'état actuel

- vérifiez que le serveur SSH tourne actuellement sur le port 22/tcp

```
[dany@router ~]$ ss -tn
State Recv-Q Send-Q Local Address:Port Peer Address:Port Process
ESTAB 0      0         10.7.1.254:22       10.7.1.1:21739
```

- vérifiez que le serveur SSH est disponible actuellement sur TOUTES les IPs de la machine

```
[dany@router ~]$ ss -lt
State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess
LISTEN 0      128          0.0.0.0:ssh       0.0.0.0:*
LISTEN 0      128             [::]:ssh          [::]:*
```

### 🌞 Modifier la configuration du serveur SSH

```
Port 22222
#AddressFamily any
ListenAddress 10.7.1.254
#ListenAddress ::
```
### 🌞 Prouvez que le changement a pris effet

```
[dany@router ~]$ ss -tln
State          Recv-Q         Send-Q                 Local Address:Port                    Peer Address:Port         Process
LISTEN         0              128                       10.7.1.254:22222                        0.0.0.0:*
```

### 🌞 N'oubliez pas d'ouvrir ce nouveau port dans le firewall

```
[dany@router ~]$ sudo firewall-cmd --add-port=22222/tcp --permanent
success
[dany@router ~]$ sudo firewall-cmd --reload
success
```

### 🌞 Effectuer une connexion SSH sur le nouveau port

```
PS C:\Users\ghass> ssh dany@10.7.1.254 -p 22222
dany@10.7.1.254's password:
Last login: Fri Nov 24 16:26:08 2023 from 10.7.1.1
[dany@router ~]$
```

## 3. Connexion par clé

### 🌞 Générer une paire de clés

- j'ai déjà généré une clé : 
```
PS C:\Users\ghass> ls .ssh


    Répertoire : C:\Users\ghass\.ssh


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        23/11/2023     10:10           3389 id_rsa
-a----        23/11/2023     10:10            748 id_rsa.pub
```

### 🌞 Déposer la clé publique sur une VM

- j'ai clone une machine qui possède déjà ma clé c'est donc pour ça que John connait déjà ma clé

```
ghass@LAPTOP-64JKSH1D MINGW64 ~
$ ssh-copy-id dany@10.7.1.11
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/c/Users/ghass/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed

/usr/bin/ssh-copy-id: WARNING: All keys were skipped because they already exist on the remote system.
                (if you think this is a mistake, you may want to use -f option)
```

### 🌞 Connectez-vous en SSH à la machine

```
ghass@LAPTOP-64JKSH1D MINGW64 ~
$ ssh dany@10.7.1.11
Last login: Fri Nov 24 16:20:34 2023 from 10.7.1.1
[dany@john ~]$
```

### 🌞 Supprimer les clés sur la machine router.tp7.b1

```
[dany@router ssh]$ sudo rm ssh_host_*
[sudo] password for dany:
[dany@router ssh]$ ls
moduli  ssh_config  ssh_config.d  sshd_config  sshd_config.d
```

### 🌞 Regénérez les clés sur la machine router.tp7.b1

```
[dany@router ~]$ sudo sudo ssh-keygen -A
ssh-keygen: generating new host keys: RSA DSA ECDSA ED25519
[dany@router ~]$ sudo systemctl restart sshd
```

### 🌞 Tentez une nouvelle connexion au serveur

- le message nous indique que la clé enregistré pour le router n'est pas la même que celle de la nouvelle clé du router

```
$ ssh dany@10.7.1.254
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:Jr5i/2pOJn4uf5ZPXee7nuznh3jLYlDHoGtkxbSO5VI.
Please contact your system administrator.
Add correct host key in /c/Users/ghass/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in /c/Users/ghass/.ssh/known_hosts:17
Host key for 10.7.1.254 has changed and you have requested strict checking.
Host key verification failed.
```
- pour remedier a ce problème je suis aller directement modifier la clé du router en mettant la nouvelle donné dans le message d'erreur : SHA256:Jr5i/2pOJn4uf5ZPXee7nuznh3jLYlDHoGtkxbSO5VI.
Et ensuite j'ai tenté une nouvelle connexion avec ssh et j'ai réussi.

```
$ ssh dany@10.7.1.254
The authenticity of host '10.7.1.254 (10.7.1.254)' can't be established.
ED25519 key fingerprint is SHA256:Jr5i/2pOJn4uf5ZPXee7nuznh3jLYlDHoGtkxbSO5VI.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.7.1.254' (ED25519) to the list of known hosts.
dany@10.7.1.254's password:
Last login: Fri Nov 24 16:48:48 2023 from 10.7.1.1
[dany@router ~]$
```
# III. Web sécurisé

### 🌞 Montrer sur quel port est disponible le serveur web

```
[dany@web ~]$ ss -tnl
State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*
```

### 🌞 Générer une clé et un certificat sur web.tp7.b1

```
[dany@web ~]$ openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout server.key -out server.crt
....+.....+.......+..+...+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*......+.......+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.......+..+....+...............+......+...........+....+......+...........................+.....+...+...+......+.+............+........+....+........+......+.......+.....+....+.....+....+.....+................+...+...+........+......+....+..+.........+.............+..+....+.....+.+...+........+....+.....+.+........+...................+..+.+...............+............+..+.............+.....+.+......+........+...+...+..........+...........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
...+.......+..+.+......+...+..+.+.........+...........+....+.....+.+........+....+......+..+.......+..+...+.+.........+............+........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.....+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.+.....+.......+.....+.........+.+...+..+.......+......+......+.........+..+...+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:dd
State or Province Name (full name) []:dany
Locality Name (eg, city) [Default City]:bordeaux
Organization Name (eg, company) [Default Company Ltd]:idk
Organizational Unit Name (eg, section) []:dany
Common Name (eg, your name or your server's hostname) []:dany
Email Address []:dany
[dany@web ~]$ sudo mv server.key /etc/pki/tls/private/web.tp7.b1.key
[sudo] password for dany:
[dany@web ~]$ sudo mv server.crt /etc/pki/tls/certs/web.tp7.b1.crt
[dany@web ~]$ sudo chown nginx:nginx /etc/pki/tls/private/web.tp7.b1.key
[dany@web ~]$ sudo chown nginx:nginx /etc/pki/tls/certs/web.tp7.b1.crt
[dany@web ~]$ sudo chmod 0400 /etc/pki/tls/private/web.tp7.b1.key
[dany@web ~]$ sudo chmod 0444 /etc/pki/tls/certs/web.tp7.b1.crt
```

### 🌞 Modification de la conf de NGINX

- c'est bon

### 🌞 Conf firewall

```
[dany@web ~]$ sudo firewall-cmd --add-port=443/tcp --permanent
success
[dany@web ~]$ sudo firewall-cmd --reload
success
```
### 🌞 Redémarrez NGINX

```
[dany@web ~]$ sudo systemctl restart nginx
```

### 🌞 Prouvez que NGINX écoute sur le port 443/tcp

- on peut voir le port 443

```
[dany@web ~]$ ss -tln
State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*
LISTEN 0      511        10.7.1.12:443       0.0.0.0:*
```

- et ici que c'est bien le https

```
[dany@web ~]$ ss -tl
State  Recv-Q  Send-Q   Local Address:Port    Peer Address:Port Process
LISTEN 0       511            0.0.0.0:http         0.0.0.0:*

LISTEN 0       128            0.0.0.0:ssh          0.0.0.0:*

LISTEN 0       511          10.7.1.12:https        0.0.0.0:*
```

### 🌞 Visitez le site web en https

- ça pas l'air de très bien marcher et lorsque j'ouvre avec le navigateur le https apparait mais il me dit que le site n'est pas sécurisé 

```
[dany@john ~]$ curl -k https://10.7.1.12
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.20.1</center>
</body>
</html>
```
