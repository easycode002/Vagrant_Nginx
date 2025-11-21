Here you go — **full README.md content**, ready to paste directly into your project.

---

````md
# Vagrant Ubuntu 22.04 LEMP Setup (Nginx + PHP 8.2 + MySQL)

This README provides step-by-step instructions to set up a complete LEMP development environment using **Vagrant + Ubuntu 22.04**, including:

- Private Network  
- Nginx  
- PHP 8.2 + Extensions  
- MySQL  
- SSH tunnel for Navicat  
- Synced folders for project code  

---

## 1. Initialize Vagrant

```bash
vagrant init ubuntu/jammy64
````

---

## 2. Configure Vagrantfile

### Enable private network

Uncomment or add:

```ruby
config.vm.network "private_network", ip: "192.168.33.10"
```

### Enable synced folder

Add:

```ruby
config.vm.synced_folder "D:/z1_trading_code", "/var/www",
  owner: "www-data",
  group: "www-data",
  mount_options: ['dmode=777','fmode=777']
```

---

## 3. Start VM

```bash
vagrant up
vagrant ssh
```

---

## 4. Update System

```bash
sudo apt-get update
```

---

## 5. Install Nginx

```bash
sudo apt-get install nginx -y
sudo service nginx status
```

---

## 6. Install PHP 8.2 + Extensions

### Add PHP PPA

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt install software-properties-common -y
sudo apt update
```

### Install PHP packages

```bash
sudo apt-get install php8.2-fpm php8.2-cli php8.2-mcrypt php8.2-gd \
php8.2-mysql php8.2-imap php8.2-memcached php8.2-mbstring \
php8.2-xml php8.2-curl php8.2-bcmath php8.2-xdebug -y
```

### Check PHP

```bash
sudo service php8.2-fpm status
php -v
```

---

## 7. Install MySQL Server

```bash
sudo apt install mysql-server -y
sudo service mysql status
```

Secure MySQL:

```bash
sudo mysql_secure_installation
```

---

## 8. Configure MySQL Root Login

Login:

```bash
sudo mysql -u root -p
```

Run SQL commands:

```sql
SHOW DATABASES;
UPDATE mysql.user SET plugin = 'mysql_native_password' WHERE User='root';
FLUSH PRIVILEGES;
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456@Abc';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost';
FLUSH PRIVILEGES;
```

---

## 9. Connect MySQL via Navicat (SSH Tunnel)

### General Tab

| Field           | Value                 |
| --------------- | --------------------- |
| Connection Name | Any name              |
| Host            | localhost             |
| Port            | 3306                  |
| Username        | MySQL user (ex: root) |
| Password        | Your MySQL password   |

### SSH Tab

| Field    | Value         |
| -------- | ------------- |
| Host     | 192.168.33.10 |
| Port     | 22            |
| Username | vagrant       |
| Password | vagrant       |

---

## 10. Nginx Virtual Host Setup

Create config file:

```bash
sudo nano /etc/nginx/sites-available/test.conf
```

Add:

```nginx
server {
    listen 8080;
    listen [::]:8090;

    server_name 192.168.33.10;

    root /var/www/admin-template-main;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/test.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo service nginx restart
```

---

## 11. Access Your Website

Open browser:

```
http://192.168.33.10:8080
```

Or IPv6:

```
http://192.168.33.10:8090
```

---

## ✔ Done

You now have a fully working **LEMP stack** inside Vagrant.

If you want, I can also add:

* Provision script (auto-install everything on `vagrant up`)
* Laravel / WordPress auto setup
* Redis installation
* phpMyAdmin installation

Just ask!

```

---

Let me know if you want the README.md **auto-generated into the Vagrantfile provisioning script**.
```
