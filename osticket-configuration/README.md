
# ⚙️ osTicket: Prerequisites and Installation

## 🎯 Objective
This project covers the baseline environment configuration and installation of **osTicket**, an open-source IT Service Management (ITSM) ticketing system. The objective was to set up a web server (IIS), database (MySQL), and PHP environment on a Windows virtual machine and successfully initialize the osTicket web application.

---

## 📐 Environment & Prerequisites

| Component | Software / Details |
| :--- | :--- |
| **Operating System** | Windows 10 / Windows Server 2022 VM |
| **Web Server** | Internet Information Services (IIS) |
| **Database** | MySQL Server / MariaDB |
| **Scripting Engine** | PHP (v8.x) |
| **Application** | osTicket (v1.17+ core files) |

---

## 🛠️ Step-by-Step Implementation Guide

### Step 1: Web Server & IIS Configuration
1. Open **Control Panel** $\rightarrow$ **Programs and Features** $\rightarrow$ **Turn Windows features on or off**.
2. Enable **Internet Information Services (IIS)** along with the following features:
   * World Wide Web Services $\rightarrow$ Application Development Features $\rightarrow$ **CGI**.
   * Web Management Tools $\rightarrow$ **IIS Management Console**.
3. Install the **PHP Manager for IIS** and the **URL Rewrite Module**.

---

### Step 2: PHP Setup & Extension Mapping
1. Download and extract the non-thread-safe version of **PHP** into `C:\php`.
2. Open IIS Manager, select the server root, and open **Handler Mappings**.
3. Add a Module Mapping for PHP:
   * **Executable Path:** `C:\php\php-cgi.exe`
   * **Extension:** `*.php`
   * **Name:** `PHP_via_FastCGI`
4. Rename `C:\php\php.ini-production` to `php.ini`.
5. Open `php.ini` in a text editor and enable required extensions by removing the semicolon `;`:
   ```ini
   extension=php_mysqli.dll
   extension=php_mbstring.dll
   extension=php_openssl.dll
   extension=php_intl.dll
   extension=php_gd.dll
   extension=php_imap.dll

```

---

### Step 3: MySQL Database Setup

1. Download and install **MySQL Server**.
2. Open MySQL Command Line Client or PowerShell and connect to MySQL:
```sql
mysql -u root -p

```


3. Create the dedicated database and user account for osTicket:
```sql
CREATE DATABASE osticket_db;
CREATE USER 'osticket_user'@'localhost' IDENTIFIED BY 'Password2026!';
GRANT ALL PRIVILEGES ON osticket_db.* TO 'osticket_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;

```



---

### Step 4: osTicket File Deployment & Web Installation

1. Download the osTicket core files and extract the `upload/` folder contents into `C:\inetpub\wwwroot\osticket`.
2. Navigate to `C:\inetpub\wwwroot\osticket\include\`.
3. Locate `ost-sampleconfig.php` and copy/rename it to `ost-config.php`.
4. Adjust NTFS permissions on `ost-config.php` to allow IIS (`IUSR`) write permissions during setup.
5. Open a web browser on the server and navigate to `http://localhost/osticket/setup/`.
6. Verify all system prerequisite checks (PHP extensions, web server) pass green.
7. Fill in the installer form:
* **System Settings:** Site Name, Default Email address.
* **Admin User:** System Administrator credentials.
* **Database Settings:** Database (`osticket_db`), Username (`osticket_user`), Password (`Password2026!`), Host (`localhost`).


8. Click **Install Now** to populate the database tables and finalize installation.
9. Post-Installation Cleanup:
* Delete the `C:\inetpub\wwwroot\osticket\setup/` directory.
* Change `ost-config.php` permissions back to read-only.



---

## ✅ Verification

* Accessed the public ticketing portal at `http://localhost/osticket/`.
* Accessed the staff control panel at `http://localhost/osticket/scp/login.php` and verified successful login using administrator credentials.

---

## 📌 Key Takeaways

* **Web Server Integration:** FastCGI mapping bridges IIS and PHP execution for Windows-hosted web apps.
* **Database Management:** Proper user permission isolation enhances database security for enterprise applications.

