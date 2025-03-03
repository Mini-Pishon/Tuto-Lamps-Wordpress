# Linux
## System update
`sudo apt -y update && \
  sudo apt -y full-upgrade` 


# Installing basic tools
```
sudo apt -y install \
ca-certificates \
ccze \
curl \
htop \
iptables-persistent \
man \
neofetch \
net-tools \
tcpdump \
unzip \
vim \
wget \
zip
```


# Apache
## Installing Apache2
`sudo apt -y install apache2`


> Check from public IP or FQDN to make sure everything works from now

## Creating vHost for wordpress
```
sudo vim /etc/apache2/sites-available/website.conf
<VirtualHost *:80>
    ServerName site.inter.net
    DocumentRoot /var/www/html/wordpress
    <Directory /var/www/html/wordpress>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    # Logs
    ErrorLog ${APACHE_LOG_DIR}/wordpress-error.log
    CustomLog ${APACHE_LOG_DIR}/wordpress-access.log combined
</VirtualHost>
```

```
sudo a2ensite yoursite.conf && \
  sudo systemctl reload apache2.service && \
  sudo systemctl status apache2.service -l --no-pager
[14:17]
Mariadb
Installing Mariadb
sudo apt -y install mariadb-server
```


## Securing Mariadb
`sudo mysql_secure_installation`
```
Switch to unix_socket authentication [Y/n] Y
Change the root password? [Y/n] n
Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access to it? [Y/n] y
Reload privilege tables now? [Y/n] y
```

> Creating user for wordpress
`sudo mysql`

```
CREATE DATABASE wordpress;
CREATE USER 'youruser'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON wordpress.* TO 'youruser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

```
SHOW Databases;
SELECT User, Host, Password FROM mysql.user;
EXIT;
```

## PHP
> Installing PHP and its dependences
```
sudo apt install -y \
  php \
  php-mysql \
  php-curl \
  php-gd \
  php-mbstring \
  php-xml \
  php-xmlrpc \
  php-zip \
  php-intl \
  php-bcmath
```

`php -v`


# Wordpress
## Installing Wordpress
```
sudo curl -O https://wordpress.org/latest.tar.gz && \
sudo tar -xzf latest.tar.gz -C /var/www/html/ && \
rm latest.tar.gz
```

```
sudo chown -R www-data:www-data /var/www/html/wordpress && \
sudo chmod -R 755 /var/www/html/wordpress
```

## Setting up wordpress
> Connecting to local Mariadb Database
> Using the previously defined credentials

Username
`youruser`
Password
`yourpassword`

> Welcome
> Creating a user for Wordpress
Username
`youruser`
Password
`yourpassword`
