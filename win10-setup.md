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

Extract it in C:\php8.1

Add it to system path, for that open run cmd by doing win+R then type 

```
sysdm.cpl
```
then 
Click Advanced
Click Environment Variables
Edit PATH
Under System Variables:
Select Path
Click Edit
Click New
Add
```
C:\php8.1
```

Check php.ini is present. If it is not present then copy php.ini-development to php.ini
Make usre below extension does not have ; before them in php.ini

extension=mbstring

extension=gd

extension=exif

extension=fileinfo

extension=pdo_mysql

extension=curl

extension=openssl

extension=zip ; ignore if not present

Make sure this line also does not have ; at start 

extension_dir = "ext"

---

## 2. Composer

* Version: **2.10+**
* Download:
  [https://getcomposer.org/Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)


During setup install, make sure PHP path is C:\php8.1\php.exe

---

## 3. Node.js

* Version: **20.19.2**
* Download:
  [https://nodejs.org/en/download/releases/](https://nodejs.org/en/download/releases/)

Choose:

```
node-v20.19.2-x64.msi
```

install in default location

---

## 4. XAMPP (Apache + MySQL)

* Version: 8.1.25 / PHP 8.1.25
* Download:
  [https://www.apachefriends.org/download.html](https://www.apachefriends.org/download.html)

Install path MUST be:

```
C:\xampp
```
Select ONLY:

✔ Apache

✔ MySQL

✔ phpMyAdmin (optional but useful)

Open Xampp control panel and start Apache and mysql

Verify Apache works

http://localhost

Verify mysql work

http://localhost/phpmyadmin

In Xampp control panel, make sure to check the service column tick

---

# ⚙️ 2. POST-INSTALL CONFIGURATION check

---

make sure below is done

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

Clone the repo in

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
composer install --no-interaction --prefer-dist --ignore-platform-reqs
```

Patch the package.json to look like below

```
{
    "private": true,
    "scripts": {
        "dev": "npm run development",
        "development": "mix",
        "watch": "mix watch",
        "watch-poll": "mix watch -- --watch-options-poll=1000",
        "hot": "mix watch --hot",
        "prod": "npm run production",
        "production": "mix --production"
    },
    "devDependencies": {
        "axios": "^0.21",
	"webpack": "5.74.0",
  	"webpack-cli": "4.10.0",
        "laravel-mix": "^6.0.6",
        "lodash": "^4.17.19",
        "postcss": "^8.1.14"
    }
}

```

then

```
npm install  && npm run dev
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

## STEP 7 — Apache setup

Make sure file `C:\xampp\apache\conf\httpd.conf` has uncommented

`LoadModule rewrite_module modules/mod_rewrite.so`

and also inside block `<Directory "C:/xampp/htdocs">` it has

`AllowOverride All`

Then Edit:

```
C:\xampp\apache\conf\extra\httpd-vhosts.conf
```

Add at end of file:

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

**And system is live now**

---

# DATABASE BACKUP (CRITICAL)

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

# 🔁 FULL RECOVERY CHECKLIST (AFTER CRASH)

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
