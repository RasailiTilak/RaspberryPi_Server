# SSH SETUP

## ON UBUNTU SEREVER
```
- sudo apt update && sudo apt upgrade -y
- sudo apt install openssh-server
- sudo systemctl status ssh.service
- sudo lsof -i
- cd /etc/ssh/
- ls -la
- sudo cp ./sshd_config ./sshd_config.original
- ls
```
## ON ANOTHER DEVICE 
```
- ssh server@piserver
- ssh your_name@server_ip_address
	- yes
	- your_password 
  **server open here --- now you can access server**

- sudo nano /etc/ssh/sshd_config
	- Banner /etc/issue.net

- sudo nano /etc/issue.net

- sudo sshd -t -f /etc/ssh/sshd_config
- sudo systemctl restart ssh.service
- exit

```

# NGINX SETUP
## INSTALLATION
```
- sudo apt-get update
- sudo apt-get install nginx
- sudo systemctl start nginx
- sudo systemctl enable nginx
- sudo ufw allow 'Nginx HTTP'
 ** check with your ip address**
```
## BASIC CONFIGURATION OF THE NGINX
```
- sudo nano /etc/nginx/sites-available/default
 server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name your_domain_or_IP;

    location / {
        try_files $uri $uri/ =404;
    }
}


- sudo nginx -t
- sudo systemctl reload nginx
```
# MQTT BROKER SETUP
## BASIC INSTALLATION
```
- sudo apt-get update
- sudo apt-get install mosquitto mosquitto-clients
- sudo systemctl start mosquitto
- sudo systemctl enable mosquito
- sudo ufw allow 1883
- sudo ufw allow 8883

```

## MQTT CONFIGURATION
```
- sudo nano /etc/mosquitto/mosquitto.conf
	listener 1883
	allow_anonymous true
- sudo systemctl restart mosquitto

```

## VERIFICATION
```
- sudo systemctl status mosquito
- mosquitto_pub -h localhost -t test/topic -m "Hello MQTT"
- mosquitto_sub -h localhost -t test/topic
```

# INSTALL THE PHP 
```
sudo apt update
sudo apt install php
php -v
sudo apt install php-cli php-curl php-mbstring php-xml php-zip
```
# RESTART SERVER
```
sudo systemctl restart apache2  # Ubuntu/Debian

sudo apt install php-fpm  # Ubuntu/Debian
sudo systemctl restart nginx
systemctl list-units --type=service | grep php

sudo systemctl restart php-fpm
```

# MARIADB SETUP
```
sudo apt update
sudo apt install mariadb-server
sudo systemctl start mariadb
sudo systemctl start mariadb
sudo mysql_secure_installation
mysql -V
```
## Common Post-Installation Steps:
```
sudo mysql -u root -p
CREATE DATABASE mydatabase;
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
GRANT ALL PRIVILEGES ON mydatabase.* TO 'myuser'@'localhost';
FLUSH PRIVILEGES;
####OR 
sudo mysql -u root -p

CREATE USER 'user'@'localhost' IDENTIFIED BY 'user@2024';
GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;

**--------**
sudo mysql
USE mysql;
ALTER USER 'root'@'localhost' IDENTIFIED VIA mysql_native_password USING PASSWORD('server@555');
FLUSH PRIVILEGES;
EXIT;
```
**---------------------------- FILE TRANSFER -----------------------**
## IF NO PERMISSION CHANGE THE  PRESION TO THE USER
### CHECK 
	cd /path/to/directory
	ls -l
### CHANGE THE PERMISSION
```
sudo chmod -R 775 /var/www/html

sudo chown -R server:server /var/www/html

```
**-----------------------check site with ip ----------**
### if not work 
### setup
```
sudo apt update
sudo apt install nginx php-fpm

sudo systemctl start php-fpm
sudo systemctl enable php-fpm

sudo nano /etc/nginx/sites-available/example.com


server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.4-fpm.sock; # Adjust the PHP version accordingly
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}





sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/




sudo nginx -t


sudo systemctl restart nginx


sudo ufw allow 'Nginx Full'



 **----------------------------------- if 502  error --bad  Gateway---**

dpkg --list | grep php
```
### check and 
```
sudo systemctl status php8.1-fpm
sudo systemctl start php8.1-fpm
sudo systemctl enable php8.1-fpm
sudo systemctl enable php8.1-fpm
```
### Verify PHP-FPM Configuration
```
sudo nano /etc/php/8.1/fpm/pool.d/www.conf
```
#Look for the listen 
```
listen = /run/php/php8.1-fpm.sock
sudo nano /etc/nginx/sites-available/default


location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php8.1-fpm.sock; # Adjust to match your PHP-FPM version and configuration
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}
sudo nginx -t
sudo systemctl restart nginx



**--------------------if couldno't find the driver -----**

sudo apt update
sudo apt install php8.1-mysql

sudo systemctl restart php8.1-fpm
sudo systemctl restart nginx


 
**------------------------------------mqtt failed --- to connect how to fix** 

sudo nano /etc/mosquitto/mosquitto.conf
listener 1883
protocol mqtt

listener 9090
protocol websockets
```
 #### else if 
```

sudo tail -f /var/log/mosquitto/mosquitto.log

sudo netstat -tuln | grep mosquito
	sudo apt update
	sudo apt install iproute2


sudo ss -tuln | grep 1883
sudo ss -tuln | grep 9090
sudo ufw allow 9090/tcp
sudo ufw allow 1883/tcp
sudo ufw reload



sudo systemctl restart mosquitto
sudo ss -tuln | grep mosquitto




sudo ufw status
sudo ufw allow 9090/tcp
sudo ufw allow 1883/tcp
sudo ufw reload

**no need below this** 
sudo apt install websocat
websocat ws://192.168.0.46:9090/mqtt
```
### if not 
```
wget https://github.com/vi/websocat/releases/latest/download/websocat.x86_64-unknown-linux-musl -O websocat
chmod +x websocat


**------------------ local network with router------------**
 
sudo apt-get install network-manager
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager

 sudo nmcli device wifi list

**----------------- wifi off and enthernt enable----**
sudo ip link set wlan0 down
### ethernet
sudo ip link set eth0 up

sudo dhclient eth0
ip addr


**--------------------------------------   speed check ----------------**
```
### pir server
```
sudo apt update
sudo apt install iperf3
iperf3 -s
sudo ufw allow 5201
sudo lsof -i -P -n | grep LISTEN



sudo ethtool -s eth0 speed 1000 duplex full autoneg on
```

### windows
```
download iperf3 
cd C:\path\to\iperf3\
iperf3.exe -c <Raspberry_Pi_IP>

```






# for the ssh disconnection error 
- set the ufw rule to allow ssh
-
  ```
sudo chown server1:server1 /var/www/html
sudo chmod 700 /var/www/html
```

- give permission to the UPLOAD THE files to the serever from the  WinScp 
