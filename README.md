# 📦 Nextcloud Installation on a 32-bit Computer (Without Docker)

## 🧾 Overview

This guide documents how I successfully installed and configured **Nextcloud** on a **32-bit system (antiX Linux)** without using Docker. Since many modern solutions rely on Docker (which often doesn’t support 32-bit systems well), this approach uses a traditional **LAMP stack (Linux, Apache, MySQL, PHP)**.

---

## ⚙️ System Requirements

* 32-bit Linux system (antiX in my case)
* Internet connection
* Root or sudo access
* At least 1GB RAM (recommended)

---

## 🔧 Step 1: Update System

First, update all packages:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🌐 Step 2: Install Apache Web Server

```bash
sudo apt install apache2 -y
```

Start Apache:

```bash
sudo service apache2 start
```

Test in browser:

```
http://localhost
```

---

## 🗄️ Step 3: Install MySQL (MariaDB)

```bash
sudo apt install mariadb-server -y
```

Start the database:

```bash
sudo service mysql start
```

Secure installation:

```bash
sudo mysql_secure_installation
```

---

## 🧠 Step 4: Install PHP and Required Modules

Nextcloud requires several PHP modules:

```bash
sudo apt install php php-mysql php-xml php-gd php-curl php-zip php-mbstring php-intl php-bz2 php-imagick -y
```

Restart Apache:

```bash
sudo service apache2 restart
```

---

## 📥 Step 5: Download Nextcloud

Move to web directory:

```bash
cd /var/www/
```

Download:

```bash
sudo wget https://download.nextcloud.com/server/releases/latest.zip
```

Extract files:

```bash
sudo unzip latest.zip
```

---

## 🔐 Step 6: Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/nextcloud
sudo chmod -R 755 /var/www/nextcloud
```

---

## 🛠️ Step 7: Create Database

Login to MySQL:

```bash
sudo mysql -u root -p
```

Run:

```sql
CREATE DATABASE nextcloud;
CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## 🌍 Step 8: Configure Apache

Enable required modules:

```bash
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod env
sudo a2enmod dir
sudo a2enmod mime
```

Restart Apache:

```bash
sudo service apache2 restart
```

---

## 🚀 Step 9: Access Nextcloud

Open browser:

```
http://localhost/nextcloud
```

Fill in:

* Admin username & password
* Database name: `nextcloud`
* Database user: `nextclouduser`
* Database password: (your password)

---

## ⚠️ Challenges Faced (32-bit Specific)

* Docker not supported → had to use manual installation
* Some PHP modules missing → installed manually
* Apache service issues (`systemctl` not available in antiX) → used `service`
* File permission errors → fixed with `chown` and `chmod`
* MySQL authentication errors → required proper user creation

---

## ✅ Final Result

Successfully hosted Nextcloud locally on a 32-bit machine using a lightweight setup without Docker.

---

## 📌 Notes

* Use `http://localhost/nextcloud` when accessing from the same machine
* Ensure Apache and MySQL are always running
* This setup is ideal for learning and small personal cloud storage

---

## 🙌 Conclusion

Even with older 32-bit hardware, it is still possible to run modern self-hosted applications like Nextcloud by avoiding Docker and using traditional server tools.

---
