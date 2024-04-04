🚨 Dans un premier temps faire une snapshot de votre machine en cas de problème.

# Commande d'installation complète

### Utilisation de la commande : 
modifié uniquement les valeur des variables : ```$nomconf``` (nom du fichier de conf) ```$site``` (ex. srv1.fr) ```$ip``` (192.168.1.1 ip vers le server web)

```apt install sudo && sudo apt-get update -y && sudo apt-get upgrade -y && nomconf=srv1.fr.conf && site=srv1.fr && ip=127.0.0.1 && apt-get install apache2 -y && systemctl enable apache2 && sudo a2enmod proxy && sudo a2enmod proxy_http && systemctl restart apache2 && cd /etc/apache2/sites-available/ && touch $nomconf && echo -e "<VirtualHost *:80>\n    ServerName $site\n    ProxyPass / http://$ip/\n    ProxyPassReverse / http://$ip/\n</VirtualHost>" > $nomconf && sudo a2ensite $nomconf && systemctl reload apache2```

# Installation Server Web

```apt install sudo && sudo apt-get update -y && sudo apt-get upgrade -y && apt-get install apache2 php mariadb-server -y && sudo a2enmod rewrite && sudo a2enmod deflate && sudo a2enmod headers && sudo a2enmod ssl && sudo apt-get install -y php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath && sudo systemctl enable apache2 && touch /var/www/html/phpinfo.php && echo "<?php phpinfo();?>" > /var/www/html/phpinfo.php && systemctl reload apache2```
