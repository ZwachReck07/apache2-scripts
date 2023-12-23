# enable virtual hosting

- sudo mkdir -p /var/www/your_domain/public_html
- sudo chown -R $USER:$USER /var/www/your_domain/public_html
- sudo chmod -R 755 /var/www
- nano /var/www/your_domain/public_html/index.html
- sudo nano /etc/apache2/sites-available/your_domain.conf

```
<VirtualHost *:80>
  ...
    ServerAdmin admin@your_domain
    ServerName your_domain
    ServerAlias www.your_domain
    DocumentRoot /var/www/your_domain/public_html
    ...
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    ...
</VirtualHost>
```

- sudo a2ensite your_domain.conf
- sudo a2dissite 000-default.conf
- sudo apache2ctl configtest
- sudo systemctl restart apache2

# Get ssl certificates

- sudo snap install --classic certbot
- sudo ln -s /snap/bin/certbot /usr/bin/certbot
- sudo certbot certonly --standalone -d your_domain
