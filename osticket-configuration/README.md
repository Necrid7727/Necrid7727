# ⚙️ osTicket: Prerequisites and Installation Walkthrough

## 📝 Project Summary

### Description
This project is a detailed technical walkthrough covering the baseline environment configuration and installation of **osTicket**, an open-source IT Service Management (ITSM) ticketing system. It details the step-by-step setup of Internet Information Services (IIS), PHP extension mapping, MySQL database creation, and web application initialization on a Windows environment.

### Languages Used
* **PowerShell / Batch:** Command execution and environment setup.
* **SQL:** Database initialization and user privilege allocation.
* **PHP:** Scripting engine configuration (`php.ini`).

### Environments Used
* **Windows 10 / Windows Server 2022** (Virtual Machine)
* **Internet Information Services (IIS 10.0)**

### Technologies & Applications Used
* **osTicket** (v1.17+ Core Files)
* **MySQL Server** (v8.0)
* **PHP** (v8.x Non-Thread-Safe)
* **PHP Manager for IIS**
* **URL Rewrite Module for IIS**

---

## 🛠️ Step-by-Step Written Demonstration

### Step 1: Web Server (IIS) & Feature Activation
1. On the Windows host machine, click **Start**, search for **Control Panel**, and press `Enter`.
2. Navigate to **Programs** $\rightarrow$ Click **Turn Windows features on or off**.
3. In the Windows Features wizard, scroll down to **Internet Information Services** and expand the selection.
4. Check **World Wide Web Services** $\rightarrow$ Expand **Application Development Features** $\rightarrow$ Select **CGI**.
5. Expand **Web Management Tools** $\rightarrow$ Check **IIS Management Console**.
6. Click **OK** and wait for Windows to apply changes.
7. Download and install **PHP Manager for IIS** and the **URL Rewrite Module** using default installation prompts.
8. **Expected Result:** Opening IIS Manager (`inetmgr`) reveals the server homepage with CGI capabilities enabled.

---

### Step 2: PHP Setup & FastCGI Handler Mapping
1. Download the latest non-thread-safe PHP zip archive and extract contents to `C:\php`.
2. Open **IIS Manager** (`inetmgr`), click on the root Server node in the left Connections pane, and double-click **Handler Mappings** in the center feature panel.
3. Click **Add Module Mapping...** in the right-hand Actions menu.
4. Enter the following parameters:
   * **Request path:** `*.php`
   * **Module:** `FastCgiModule`
   * **Executable:** `C:\php\php-cgi.exe`
   * **Name:** `PHP_via_FastCGI`
5. Click **OK**, then click **Yes** when prompted to allow FastCGI permissions.

<img width="2752" height="1536" alt="Gemini_Generated_Image_wqn0y9wqn0y9wqn0" src="https://github.com/user-attachments/assets/373f2dfa-8075-4a78-ae54-e79fe9965d34" />

6. Open `C:\php` in File Explorer, copy `php.ini-production`, and rename it to `php.ini`.
7. Open `php.ini` in Notepad as Administrator. Locate the `Dynamic Extensions` section, remove the leading semicolon `;` from the following lines to enable required modules, and save the file:
   ```ini
   extension=php_mysqli.dll
   extension=php_mbstring.dll
   extension=php_openssl.dll
   extension=php_intl.dll
   extension=php_gd.dll
   extension=php_imap.dll

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

<img width="2752" height="1536" alt="Gemini_Generated_Image_bvzbqxbvzbqxbvzb" src="https://github.com/user-attachments/assets/f182491f-ebe9-42ad-aece-4d9d2e56a320" />


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

