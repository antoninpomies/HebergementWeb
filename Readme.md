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
## Nous modifirons les valeur de ipsrv2 une fois la second machine crée. Je vous laisse donc crée la seconde machine qui doit être identique a la première, Mais nous faisons uniquement l'ajout du phpinfo.

Vous trouverez ci-dessous des images qui montre le rendu final du TP

### Tout d'abord le contenu du fichier hosts ou on voit bien que les deux nom pointe vers la meme IP et que le Server RP fait le reste
![Texte alternatif](/hosts.png "Titre de l'image")

### Et ensuite les deux phpinfo.php qui on peut le voir n'ont pas la meme ip ce qui montre que c'est bien deux fichiers différents
![Texte alternatif](/Vérifphpinfo.png "Titre de l'image")

# Mettre en place un Reverse Proxy unique

Pour la suite du tp nous allons devoir monter une nouvelle machine virtuelle Debian qui va servir uniquement de reverse proxy pour éviter de la surcharger etant donné que c'est un SPOF. Pour ce faire nous allons dans un premier temps restaurer les fichier par defaut de nos deux serveur. 

## Une fois la mise en place de la nouvelle VM, on se rend ici

```cd /etc/apache2/sites-enabled/```

## Dans ce fichier nous allons supprimé l'ensemble des fichier et garder uniquement celui qui se nomme "000-default.conf"

```rm * && touch 000-default.conf```

## On va donc modifié ce fichier et lui ajouter la configuration par défaut suivante

```nano 000-default.conf```

```
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

```
## On va ensuite vérifié que le site est activé dans apache2

```sudo a2ensite 000-default.conf```

## On redémarre ensuite apache

```systemctl restart apache2```

## On va ensuite passé a la configuration du Reverse Proxy

Il va falloir créer deux fichiers de configuration car il nous faut autant de fichier que de serveur web.

## On se rend dans le bon répertoire

```cd /etc/apache2/sites-available/```

## On ajouter deux fichier de configuration

```touch srv1.conf srv2.conf```

## Dans les deux fichiers nous allons ajouter le fichier suivant

Je vous laisserait apporter les modification nécéssaire au niveau des lien et des ip qui sont renseigné.

```
<VirtualHost *:80>
    ServerName srv1.anto.fr
    ProxyPass / http://192.168.1.46/
    ProxyPassReverse / http://192.168.1.46/
</VirtualHost>
```

Une fois les deux conf dans leur fichier respectif nous allons les activés

## Activation des configurations

```
sudo a2ensite srv1.conf
sudo a2ensite srv2.conf
```

## On va ensuite redémarrer apache2

```systemctl restart apache2```

## On va remodifié notre fichier hosts pour que l'ip ne pointe plus vers un des serveur web

on va donc aller dans le répertoire suivant et modifié le fichier "hosts"

```C:\Windows\System32\drivers\etc```

## Je vous conseille de la modifié avec Notepad ++ pour avoir une meilleur gestion des droits

Je vous laisse vous inspiré de ce qui a été fait [ ici ](https://github.com/antoninpomies/HebergementWeb#tout-dabord-le-contenu-du-fichier-hosts-ou-on-voit-bien-que-les-deux-nom-pointe-vers-la-meme-ip-et-que-le-server-rp-fait-le-reste) 























