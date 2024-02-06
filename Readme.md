# TP Hebergement Web Reverse Proxy

## Installation de tout ce qui nous faut pour le TP

```apt update -y && apt upgrade -y && apt install sudo apache2 mariadb-server php libapache2-mod-php php-mysql openssh-server -y && systemctl enable apache2```

## Maintenant que la machine est installé et bien configurer, nous allons nous connecter a celle-ci en SSH nous allons donc devoir récupéré l'adresse IP de la machine

```ip a | grep inet | grep ens33```

## Une fois la connexion effectué avec Putty, nous allons nous rendre dans le répertoire :

```cd /var/www/html/```

## Une fois dans ce répertoire nous allons crée le fichier phpinfo qui va nous donnée des information sur notre machine

```touch phpinfo.php && nano phpinfo.php```

## Une fois dans le fichier nous allons copier le contenu suivant
```
<?php

phpinfo( );

?>
```
## On peut ensuite enregistrer les modifications grace a 

```^X + y + Enter```

## On peut directememnt aller voir que tout fonctionne en se rendant ici 

http://ipmachine/phpinfo.php

## Une fois cela fait et vérifié, nous allons activé le module proxy / reverseproxy de apache. Pour ce faire 
```
sudo a2enmod proxy
sudo a2enmod proxy_http
```
## Suite a ça nous allons devoir modifié la configuration de base qui va nous permettre de redirigé les demandes. Pour ce faire il faut modifié ce fichier : 

```sudo nano /etc/apache2/sites-available/000-default.conf```

## Dans notre cas il va falloir lui apporter quelques modifications

```
<VirtualHost *:80>
    ServerName srv1.anto.fr
    ProxyPass / http://127.0.0.1/
    ProxyPassReverse / http://127.0.0.1/
</VirtualHost>
```

## Il va falloir maintenant qu'on ajoute une règle de manière a résoudre le nom de domaine mais localement. Pour ce faire nous allons devoir télécharger Notepad ++ car il est possible de l'ouvrir avec les droits administrateur. Nous nous rendons dans un premier temps dans le dossier.

Windows + R 
```C:\Windows\System32\drivers\etc```
Entrer

## Nous allons ensuite modifié le fichier pour ce faire il faut se rendre a la fin et entrer cette ligne.

ipmachine srv1.anto.fr

## On enregistre le fichier. Après l'ensemble de ces étapes si vous ouvrer un naviguateur et que vous tapez srv1.anto.fr/phpinfo.php vous devrier tombé sur le phpinfo qui a été crée tout a l'heure.

## Maintenant que ceci est fait nous allons préparer le fichier du second serveur. Pour ce faire nous allons crée un autre fichier

```touch srv2.anto.fr```

Et nous allons l'éditer

```nano srv2.anto.fr```

Copier Coller ces lignes
```
<VirtualHost *:80>
    ServerName srv2.anto.fr
    ProxyPass / http://ipsrv2/
    ProxyPassReverse / http://ipsrv2/
</VirtualHost>
```
## Nous modifirons les valeur de ipsrv2 une fois la second machine crée. Je vous laisse donc crée la seconde machine qui doit être identique a la première.


















