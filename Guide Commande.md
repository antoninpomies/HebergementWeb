🚨 Dans un premier temps faire une snapshot de votre machine en cas de problème.

# Commande d'installation complète

### Utilisation de la commande : 
modifié uniquement les valeur des variables : ```$nomconf``` (nom du fichier de conf) ```$site``` (ex. srv1.fr) ```$ip``` (192.168.1.1 ip vers le server web)

```apt install sudo && sudo apt-get update -y && sudo apt-get upgrade -y && nomconf=srv1.fr.conf && site=srv1.fr && ip=127.0.0.1 && apt-get install apache2 -y && systemctl enable apache2 && sudo a2enmod proxy && sudo a2enmod proxy_http && systemctl restart apache2 && cd /etc/apache2/sites-available/ && touch $nomconf && echo -e "<VirtualHost *:80>\n    ServerName $site\n    ProxyPass / http://$ip/\n    ProxyPassReverse / http://$ip/\n</VirtualHost>" > $nomconf && sudo a2ensite $nomconf && systemctl reload apache2```

# Installation Server Web

```apt install sudo && sudo apt-get update -y && sudo apt-get upgrade -y && apt-get install apache2 php mariadb-server -y && sudo a2enmod rewrite && sudo a2enmod deflate && sudo a2enmod headers && sudo a2enmod ssl && sudo apt-get install -y php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath && sudo systemctl enable apache2 && touch /var/www/html/phpinfo.php && echo "<?php phpinfo();?>" > /var/www/html/phpinfo.php && systemctl reload apache2```

# Modification des paramètres IP
```
#This file describes the network interfaces available on your system
#and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

#The loopback network interface
auto lo
iface lo inet loopback

#The primary network interface
allow-hotplug ens33
iface ens33 inet static
  address 192.168.1.1
  netmask 255.255.255.0
  gateway 192.168.1.254
```
# Avoir plusieurs version de PHP sur un seul serveur Web
installation des dépendances et des paquets nécéssaires
```
sudo apt update && sudo apt install -y wget gnupg2 lsb-release ca-certificates apt-transport-https
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
sudo apt update
```

Une fois cela fait il va falloir installer les version de PHP qu'il nous faut.
```
sudo apt install php8.0
sudo apt install php8.1
```
On installe ce qu'il faut pour que PHP tourne correctement et sans erreurs
```
sudo apt install php8.0-cli php8.0-fpm php8.0-mysql
sudo apt install php8.1-cli php8.1-fpm php8.1-mysql
```
Enfin on active les modules nécéssaire pour que apache surpport differente version de php
```sudo a2enmod actions fcgid alias proxy_fcgi```

# Modification du VHOST du SERVEUR WEB
```
<VirtualHost *:80>
    ServerName srv1.fr
    ServerAdmin webmaster@example.com
    DocumentRoot /var/www/html

    <FilesMatch \.php$>
      # For Apache version 2.4.10 and above, use SetHandler to run PHP as a fastCGI process server
      SetHandler "proxy:unix:/run/php/php7.0-fpm.sock|fcgi://localhost"
    </FilesMatch>

</VirtualHost>
```
On pense bien evidemment a faire les changement au niveau de la version et de l'url du site (srv1.fr).

Une fois cela fait on peut redemarrer le service apache2 et constaté le bon fonctionnement du site. pour la configuration du fichier hosts (DNS Corrompu) voir dans l'autre procédure. [ici](https://github.com/antoninpomies/HebergementWeb/tree/master?tab=readme-ov-file#tout-dabord-le-contenu-du-fichier-hosts-ou-on-voit-bien-que-les-deux-nom-pointe-vers-la-meme-ip-et-que-le-server-rp-fait-le-reste)
