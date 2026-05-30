Good — this is exactly the right next step. What you are asking for is a **recovery + rebuild manual** so your system can be restored after Windows crash. I’ll make it clean, repeatable, and realistic for your exact stack.

---

# 📘 FULL RECOVERY MANUAL (Windows 10 + Laravel + XAMPP Setup)

This is your **one-time setup + disaster recovery guide**.

---

# 🧱 1. SOFTWARE STACK (LOCKED VERSIONS)

You MUST reinstall these exact versions for compatibility:

## 1. PHP (CLI)

* Version: **8.1.34 (ZTS)**
* Download:
  [https://windows.php.net/downloads/releases/archives/](https://windows.php.net/downloads/releases/archives/)

Choose:

```
php-8.1.34-Win32-vs16-x64.zip
```

---

## 2. Composer

* Version: **2.10+**
* Download:
  [https://getcomposer.org/Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)

---

## 3. Node.js

* Version: **20.19.2**
* Download:
  [https://nodejs.org/en/download/releases/](https://nodejs.org/en/download/releases/)

Choose:

```
node-v20.19.2-x64.msi
```

---

## 4. XAMPP (Apache + MySQL)

* Version: latest stable
* Download:
  [https://www.apachefriends.org/download.html](https://www.apachefriends.org/download.html)

Install path MUST be:

```
C:\xampp
```

---

# ⚙️ 2. POST-INSTALL CONFIGURATION

---

## A. PHP Setup (CLI)

1. Extract PHP to:

```
C:\php8.1
```

2. Add to PATH:

```
C:\php8.1
```

3. Confirm:

```cmd
php -v
```

---

## B. Enable PHP extensions

Edit:

```
C:\php8.1\php.ini
```

Enable:

```ini
extension=mbstring
extension=gd
extension=exif
extension=fileinfo
extension=pdo_mysql
extension=curl
extension=openssl
extension=zip
```

Set:

```ini
extension_dir = "ext"
```

---

## C. Composer link

During install set:

```
C:\php8.1\php.exe
```

Verify:

```cmd
composer -V
```

---

## D. Node verify

```cmd
node -v
npm -v
```

---

## E. XAMPP services

Run:

```
C:\xampp\xampp-control.exe
```

Start:

* Apache
* MySQL

Enable services (optional):

* Install Apache service
* Install MySQL service

---

# 📁 3. PROJECT RESTORE PROCESS (IMPORTANT)

---

## STEP 1 — Place project

After reinstall Windows:

```
C:\xampp\htdocs\pharmacy
```

OR restore from Git:

```cmd
cd C:\xampp\htdocs
git clone <your-repo-url>
```

---

## STEP 2 — Install dependencies

```cmd
cd C:\xampp\htdocs\pharmacy
composer install
```

---

## STEP 3 — Restore .env

If missing:

```cmd
copy .env.example .env
```

Edit:

```env
APP_URL=http://pharmacy.local

DB_CONNECTION=mysql
DB_DATABASE=pharmacy
DB_USERNAME=root
DB_PASSWORD=
```

---

## STEP 4 — Generate key

```cmd
php artisan key:generate
```

---

## STEP 5 — Restore database

(see backup section below)

---

## STEP 6 — Frontend build

```cmd
npm install
npm run build
```

---

## STEP 7 — Apache setup

Edit:

```
C:\xampp\apache\conf\extra\httpd-vhosts.conf
```

Add:

```apache
<VirtualHost *:80>
    DocumentRoot "C:/xampp/htdocs/pharmacy/public"
    ServerName pharmacy.local

    <Directory "C:/xampp/htdocs/pharmacy/public">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

---

Edit hosts file:

```
C:\Windows\System32\drivers\etc\hosts
```

Add:

```
127.0.0.1 pharmacy.local
```

---

Restart Apache.

---

# 🗄️ 4. DATABASE BACKUP (CRITICAL)

You have 3 safe methods:

---

## METHOD 1 (BEST): phpMyAdmin export

Go:

```
http://localhost/phpmyadmin
```

Steps:

* Select database: `pharmacy`
* Click **Export**
* Choose **Quick**
* Format: SQL
* Download file

Save as:

```
backup-pharmacy.sql
```

---

## METHOD 2: Command line backup

```cmd
mysqldump -u root pharmacy > C:\backup\pharmacy.sql
```

---

## RESTORE:

```cmd
mysql -u root pharmacy < C:\backup\pharmacy.sql
```

---

## METHOD 3: Full XAMPP backup (optional)

Copy:

```
C:\xampp\mysql\data\pharmacy
```

---

# 💾 5. PROJECT BACKUP (IMPORTANT)

Backup these folders:

```
/app
/bootstrap
/config
/database
/public
/resources
/routes
.env
composer.json
package.json
```

DO NOT forget:

```
vendor/
node_modules/
```

(but these can be regenerated)

---

# 🔁 6. FULL RECOVERY CHECKLIST (AFTER CRASH)

After reinstall:

### Step 1

✔ Install PHP 8.1
✔ Install Composer
✔ Install Node 20
✔ Install XAMPP

### Step 2

✔ Clone project

### Step 3

✔ composer install
✔ npm install

### Step 4

✔ restore .env

### Step 5

✔ restore DB backup

### Step 6

✔ setup Apache vhost

### Step 7

✔ start Apache + MySQL

### Step 8

✔ open:

```
http://pharmacy.local
```

---

# 🧠 FINAL NOTE (IMPORTANT)

Your setup is now a:

> ✔ reproducible Laravel local server environment

Meaning:

* You can rebuild it anytime
* No dependency guessing
* No Docker needed
* No manual troubleshooting chaos

---

# If you want next upgrade (optional)

I can next help you create:

### 🔥 “ONE CLICK RESTORE SYSTEM”

* batch file to rebuild environment
* auto database restore script
* auto start Apache/MySQL
* auto project launch

Just tell me 👍
