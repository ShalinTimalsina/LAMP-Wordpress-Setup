# AWS LAMP Stack Deployment on Ubuntu Linux
**A step-by-step LAMP stack deployment to host a WordPress website on AWS EC2 using Ubuntu, Apache, MariaDB, and PHP.**

---
<p align="center">
  <img src="https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws&style=for-the-badge" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Apache-red?logo=apache&style=for-the-badge" />
  <img src="https://img.shields.io/badge/MariaDB-blue?logo=mariadb&style=for-the-badge" />
  <img src="https://img.shields.io/badge/PHP-777BB4?logo=php&style=for-the-badge" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Ubuntu-E95420?logo=ubuntu&style=for-the-badge" />
  <img src="https://img.shields.io/badge/WordPress-21759B?logo=wordpress&style=for-the-badge" />
  <img src="https://img.shields.io/badge/DevOps-Practices-success?style=for-the-badge" />
</p>


## Table of Contents




---
## 🎯 Objectives
- Deploy **Apache + MariaDB + PHP** on **Ubuntu EC2**
- Host **WordPress** under a proper web root
- Validate **Apache**, **PHP runtime**, and **database connectivity**
---
---
### Traffic Flow
1. Users access the site via **HTTP (80)** (optional **HTTPS (443)**)
2. **Apache** serves WordPress files from the web root
3. **PHP** executes WordPress code
4. **MariaDB** stores WordPress data locally on the same instance.

## Architecture

**User Browser → EC2 Public IP → Apache (port 80/443) → PHP → MariaDB (localhost:3306) → WordPress files in `/var/www/wordpress` (Apache `DocumentRoot` points here)**

---
---
### Architecture Diagram (ASCII)
```
                +-------------------------+
                |     Internet Users      |
                +-----------+-------------+
                            |
                            |  HTTP/HTTPS
                            v
                +-------------------------+
                |   AWS Security Group    |
                |  80, (443), 22 (My IP)  |
                +-----------+-------------+
                            |
                            v
          +----------------------------------------+
          |      EC2 Instance (Ubuntu 22.04)       |
          |                                        |
          |  +-------------+     +---------------+ |
          |  |   Apache    | --> |      PHP      | |
          |  |   (2.4)     |     |     (8.x)     | |
          |  +------+------+     +-------+-------+ |
          |         |                    |         |
          |         v                    v         |
          |     WordPress            MariaDB        |
          |                          (10.x)         |
          +----------------------------------------+

```
---

---

## 🏗 Architecture Overview

📂 **Architecture Diagram:**  
<p align="center">
  <img src="Architecture%20Diagram/.png" width="700">
</p>

---

## Prerequisites

- An AWS account with access to EC2  
- SSH keypair to connect to the instance  
- Ubuntu 24.04 EC2 instance  
- Open inbound ports:
  - **HTTP (80)**
  - **HTTPS (443)**
  - **SSH (22)** *(restricted to your IP only)*



## Step 1 — Create EC2 Instance

1. Launch an **Ubuntu 24.04** EC2 instance.
2. Choose an instance type `As you like`.
3. Configure a **Security Group** with inbound rules:

| Type  | Port | Source |
|------|------|--------|
| SSH  | 22   | Your IP only |
| HTTP | 80   | 0.0.0.0/0 |
| HTTPS| 443  | 0.0.0.0/0 |

✅ **Verify:** Instance is running and you have its **Public IPv4 address**.
<details> <summary>📸 <b>Click to view screenshot</b></summary>


<img src="Screenshots/AWS-EC2-Details.png" alt="EC2 Instance Details" /> 
<img src="Screenshots/Security-Group-AWS.png" alt="Security Group Details" />

</details>

---

## Step 2 — Connect to EC2

From your local machine:

```bash
ssh -i /path/to/your-key.pem ubuntu@<EC2_PUBLIC_IP>
```
✅ Verify: You land on the EC2 shell as ubuntu@...

---
## Step 3 — Update Packages
```
sudo apt update -y

```
✅ Verify: “All packages are up to date.” or updates complete.


---

## Step 4 — Install Apache

```
sudo apt install apache2 -y
sudo systemctl enable --now apache2
```
✅ Verify (Browser): Open:

```
http://<EC2_PUBLIC_IP>/
```
You should see the Apache2 Default Page (“It works!”).

<details> <summary>📸 <b>Click to view screenshot</b></summary>
<img src="Screenshots/apache-default-page.png" alt="Apache Default Welcome Page" />
</details>

---
## Step 5 — Install PHP

Install PHP + Apache integration + MySQL driver:

```
sudo apt install php libapache2-mod-php php-mysql -y
php -v
```
✅ Verify: php -v prints PHP version (example: PHP 8.3.6)

<details> <summary>📸 <b>Click to view screenshot</b></summary>
<img src="Screenshots/php-version-terminal.png" alt="PHP Version Check in Terminal" />
</details>

---

Create `info.php` to verify PHP in browser

```
sudo nano /var/www/html/info.php
```

Paste:
```
<?php
phpinfo();
```

Restart Apache:
```
sudo systemctl restart apache2
```

✅ Verify (Browser):
```
http://<EC2_PUBLIC_IP>/info.php
```
You should see the PHP info page confirming PHP runs via Apache.
- After verification, you can remove `info.php` later for security.

<details> <summary>📸 <b>Click to view screenshot</b></summary>
<img src="Screenshots/php-info-browser.png" alt="PHP Info Page in Browser" />
</details>

---
## Step 6 — Install MariaDB

```
sudo apt install mariadb-server -y
sudo systemctl enable --now mariadb
sudo systemctl status mariadb
```
✅ Verify: status shows active (running) and MariaDB is ready for connections.

<details> <summary>📸 <b>Click to view screenshot</b></summary>
<img src="Screenshots/mariadb-service-status.png" alt="MariaDB Service Status Output" />
</details>


### Secure Mariadb(`mysql_secure_installation`)

- Run the security wizard:
```
sudo mysql_secure_installation
```

| Prompt (Question) | Recommended Answer |
|------------------|-------------------|
| `Switch to unix_socket authentication [Y/n]` | `n` |
| `Change the root password? [Y/n]` | `n` |
| `Remove anonymous users? [Y/n]` | `Y` |
| `Disallow root login remotely? [Y/n]` | `Y` |
| `Remove test database and access to it? [Y/n]` | `Y` |
| `Reload privilege tables now? [Y/n]` | `Y` |

✅ Verify: The wizard finishes successfully and prints completion messages.

## Step 7 — Create Database + User
Enter MariaDB shell:
```
sudo mysql
```
Create DB + user (replace password with your own):
- DB --> `testdb` in this example.
- DB user --> `test` in this example
- `You may replace with your own`
  
```
CREATE DATABASE wordpress_db;

CREATE USER 'wordpress_user'@'localhost' IDENTIFIED BY 'CHANGE_THIS_PASSWORD';

GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress_user'@'localhost';

FLUSH PRIVILEGES;

EXIT;
```
Verify user login
```
mysql -u wordpress_user -p
```
<details> <summary>📸 <b>Click to view screenshot</b></summary>
<img src="Screenshots/mariadb-show-databases.png" alt="MariaDB Show Databases Command Output" />
</details>

Then run:
```
SHOW DATABASES;
EXIT;
```
✅ Verify: You can see your database listed.

---
## Step 8 — Test DB Connection (PHP)

Create a database connection test file:
```
sudo nano /var/www/html/dbtest.php
```
Paste:
```
<?php
$servername = "localhost"; 
$username = "wordpress_user_you_made";
$password = "CHANGE_THIS_PASSWORD";
$dbname = "wordpress_db";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

echo "Connected successfully to MariaDB!";
```

✅ Verify (Browser):

`http://<EC2_PUBLIC_IP>/dbtest.php`

You should see:
- `Connected successfully to MariaDB!`
<details> <summary>📸 <b>Click to view screenshot</b></summary>
<img src="Screenshots/mariadb-connection-success.png" alt="MariaDB Connection Success Message" />
</details>

## Now, Clean up test files

Remove test files after verification:
```
sudo rm /var/www/html/dbtest.php
sudo rm /var/www/html/info.php
```
Why to remove them??

- `Security`: Test files like phpinfo() reveal server configuration that attackers can exploit.

- `Cleanliness`: Removing temporary files keeps the web root organized and production-ready.

- `Prevent Accidental Access`: Test files may expose debug info or sample data to users.

---

## Step 9 — Install PHP Extensions for WordPress

Install required WordPress PHP extensions:
```
sudo apt install php-curl php-mbstring php-gd php-xml php-xmlrpc php-soap php-intl php-zip php-bcmath php-imagick -y
sudo systemctl restart apache2
```
✅ Verify: No errors; Apache restarts successfully.

---

## Step 10 — Download & Deploy WordPress

Go to `/tmp` and download WordPress:
```
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
```
<details> <summary>📸 <b>Click to view screenshot</b></summary>
<img src="Screenshots/wordpress-install.png" alt="WordPress Installation Screen" />
</details>

Move WordPress to /var/www:
```
sudo mv wordpress /var/www/
```
Remove Apache default index:
```
sudo rm /var/www/html/index.html
```
✅ Verify: WordPress files exist:
```
ls /var/www/wordpress
```
You should see folders like:

- `wp-admin`
- `wp-content`
- `wp-includes`

---


## Step 11 — Configure WordPress (`wp-config.php`)

Copy sample config file:
```
sudo cp /var/www/wordpress/wp-config-sample.php /var/www/wordpress/wp-config.php
```

Edit config:

```
sudo nano /var/www/wordpress/wp-config.php
```
Update these fields:
```
define( 'DB_NAME', 'wordpress_db' );
define( 'DB_USER', 'wordpress_user' );
define( 'DB_PASSWORD', 'CHANGE_THIS_PASSWORD' );
define( 'DB_HOST', 'localhost' );
```
✅ Verify: DB constants are updated correctly.

Step 12 — Point Apache to WordPress

Edit default Apache vhost:
```
sudo nano /etc/apache2/sites-available/000-default.conf
```

Find DocumentRoot and set it to:
```
DocumentRoot /var/www/wordpress
```

Restart Apache:
```
sudo systemctl restart apache2
```
✅ Verify: No restart errors.

## Step 13 — Run WordPress Installer

Open:
```
http://<EC2_PUBLIC_IP>/
```

Fill:

* Site title
* Admin username
* Admin password
* Admin email

<details> <summary>📸 <b>Click to view screenshot</b></summary>
<img src="Screenshots/wordpress-configuration.png" alt="MWordpress-Configuration-Screen" />
 <img src="Screenshots/wordpress-login-page.png" alt="WordPress Login Page" /> 
</details>

✅ Verify: After install you can log in:

```
http://<EC2_PUBLIC_IP>/wp-admin/
```
You should see:

* WordPress dashboard
* Homepage with "Hello world!" post
<details> <summary>📸 <b>Click to view screenshot</b></summary>
  
<img src="Screenshots/wordpress-site-page.png" alt="Live WordPress Site" />
</details>






























