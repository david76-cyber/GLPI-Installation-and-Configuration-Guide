# GLPI-Installation-and-Configuration-Guide



## Overview
GLPI (Gestionnaire Libre de Parc Informatique) is an open-source IT Asset Management and Service Desk software.

This repository documents the installation, configuration, and basic security setup of GLPI on Ubuntu with Apache and MariaDB/MySQL.

---

## Prerequisites

- Ubuntu 20.04 or later
- Apache 2.4+
- PHP 7.4 or later with required extensions (e.g., gd, intl, mysql, curl)
- MariaDB or MySQL server
- wget, tar utilities

---

## Installation Steps

### 1. Download GLPI

```bash
wget https://github.com/glpi-project/glpi/releases/download/10.0.14/glpi-10.0.14.tgz

2. Extract and move files

tar -xvzf glpi-10.0.14.tgz
sudo mv glpi /var/www/html/

3. Set permissions

sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 755 /var/www/html/glpi

4. Configure Apache Virtual Host

<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/glpi/public

    <Directory /var/www/html/glpi/public>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Enable Apache rewrite module and reload Apache:

sudo a2enmod rewrite
sudo systemctl reload apache2


5. Create GLPI Database and User in MariaDB/MySQL

sudo mysql -u root -p
Run:
CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
EXIT;

6. Run the web installer
Open your browser and go to:
http://your-server-ip/

Post-Installation Security Steps
Change passwords for default users (glpi, post-only, tech, normal) through Administration → Users.

Remove the installer directory for security:
sudo rm -rf /var/www/html/glpi/install
Ensure Apache DocumentRoot points to /var/www/html/glpi/public to prevent access to non-public files.

Troubleshooting
Permission issues: Check and adjust ownership/permissions on /var/www/html/glpi.

Apache errors: Run sudo apachectl configtest and inspect /var/log/apache2/error.log.

Database errors: Verify user credentials and database privileges.

References
GLPI Official Website

GLPI Documentation

GLPI GitHub Releases



