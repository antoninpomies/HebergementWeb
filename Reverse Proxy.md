# Installation Reverse Proxy

```sudo apt-get update -y && sudo apt-get upgrade -y && apt-get install apache2 -y && sudo a2enmod proxy && sudo a2enmod proxy_http && systemctl restart apache2 && cd /etc/apache2/sites-available/ && touch srv1.fr.conf && echo "<VirtualHost *:80>" >> srv1.fr.conf && echo "    ServerName srv1.anto.fr" >> srv1.fr.conf && echo "    ProxyPass / http://127.0.0.1/" >> srv1.fr.conf && echo "    ProxyPassReverse / http://127.0.0.1/" >> srv1.fr.conf && echo "</VirtualHost>" >> srv1.fr.conf && a2ensite srv1.fr.conf```


