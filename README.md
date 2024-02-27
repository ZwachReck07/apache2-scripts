# enable virtual hosting

- your_domain=""
- sudo mkdir -p /var/www/${your_domain}/public_html
- sudo chown -R $USER:$USER /var/www/${your_domain}/public_html
- sudo chmod -R 755 /var/www
- nano /var/www/${your_domain}/public_html/index.html
- sudo nano /etc/apache2/sites-available/${your_domain}.conf

```
<VirtualHost *:80>

    ServerAdmin admin@your_domain
    ServerName your_domain
    ServerAlias www.your_domain
    DocumentRoot /var/www/your_domain/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

- sudo a2ensite ${your_domain}.conf
- sudo a2dissite 000-default.conf
- sudo apache2ctl configtest
- sudo systemctl restart apache2

# Get ssl certificates

## Auto

- sudo apt install certbot python3-certbot-apache
- sudo certbot --apache

## Manual

- sudo snap install --classic certbot
- sudo ln -s /snap/bin/certbot /usr/bin/certbot
- sudo certbot certonly --standalone -d ${your_domain}
- sudo nano /etc/apache2/sites-available ${your_domain}-le-ssl.conf
```
<IfModule mod_ssl.c>
<VirtualHost *:443>

	ServerAdmin admin@your_domain
	DocumentRoot /var/www/your_domain/public_html
	ServerName your_domain
	ServerAlias www.your_domain

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined


SSLCertificateFile /etc/letsencrypt/live/your_domain/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/your_domain/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
</IfModule>
```
-  sudo a2ensite ${your_domain}-le-ssl.conf
-  sudo systemctl restart apache2
