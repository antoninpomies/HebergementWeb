# Installation Reverse Proxy

```sudo apt-get update -y && sudo apt-get upgrade -y && apt-get install apache2 -y && sudo a2enmod proxy && sudo a2enmod proxy_http && systemctl restart apache2 && cd /etc/apache2/sites-available/ && touch srv1.fr.conf && echo -e "<VirtualHost *:80>\n    ServerName srv1.anto.fr\n    ProxyPass / http://127.0.0.1/\n    ProxyPassReverse / http://127.0.0.1/\n</VirtualHost>" > srv1.fr.conf && a2ensite srv1.fr.conf```

# Installation Server Web

```sudo apt-get update -y && sudo apt-get upgrade -y && apt-get install apache2 -y && touch /var/www/html/phpinfo.php && echo "<?php phpinfo();?>"```
