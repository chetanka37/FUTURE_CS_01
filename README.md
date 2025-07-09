# Task 1 – Web Application Security Testing (Future Interns Cybersecurity Internship)

**Internship Track:** Cyber Security (Track Code: CS)
**Task Name:** Web Application Security Testing
**Platform Used:** Kali Linux (VirtualBox)
**Target Application:** DVWA (Damn Vulnerable Web Application)
**Duration:** 3 Phases

---

## 🌟 Phase 1 – DVWA Setup and Configuration

### ✅ Objective:

To install and configure DVWA (Damn Vulnerable Web Application) locally on Kali Linux using Apache2, PHP, and MariaDB.

### 🔧 Tools Used:

* **Apache2**: Web server for hosting DVWA
* **MariaDB**: MySQL-compatible database server
* **PHP**: For dynamic backend processing
* **Git**: To clone the DVWA repo from GitHub
* **Nano**: For editing config files
* **Terminal (Kali Linux)**: All installations and setups

### 📃 Steps Performed:

1. **Install Apache2, PHP, and MariaDB**:

   ```bash
   sudo apt update
   sudo apt install apache2 mariadb-server php php-mysqli php-gd php-xml php-cli php-common php-curl libapache2-mod-php git unzip -y
   ```

2. **Enable PHP Module and Restart Apache**:

   ```bash
   sudo a2enmod php8.2
   sudo systemctl restart apache2
   ```

3. **Clone DVWA from GitHub**:

   ```bash
   cd /var/www/html
   sudo rm index.html
   sudo git clone https://github.com/digininja/DVWA.git
   sudo chown -R www-data:www-data DVWA
   ```

4. **Configure DVWA**:

   ```bash
   cd /var/www/html/DVWA/config
   sudo cp config.inc.php.dist config.inc.php
   sudo nano config.inc.php
   ```

   * Set `db_user` to `dvwauser`
   * Set `db_password` to `dvwapass`
   * Set `default_security_level` to `low`

5. **Create Database and User**:

   ```bash
   sudo mysql
   CREATE DATABASE dvwa;
   CREATE USER 'dvwauser'@'localhost' IDENTIFIED BY 'dvwapass';
   GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwauser'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

6. **Enable PHP Error Display**:

   ```bash
   sudo nano /etc/php/*/apache2/php.ini
   ```

   * Set `display_errors = On`
   * Set `error_reporting = E_ALL`

   ```bash
   sudo systemctl restart apache2
   ```

7. **Launch DVWA**:

   * In Firefox: `http://localhost/DVWA/setup.php` → Click "Create/Reset Database"
   * Go to `http://localhost/DVWA/login.php` and login with `admin / password`

### 🌐 Outcome:

* Successfully hosted and accessed DVWA on local Apache server
* DVWA login screen functional

### 🔧 Suggestion:

* Ensure Apache and MariaDB are running before accessing DVWA
* Enable PHP error reporting to debug blank screens

---

## 🌟 Phase 2 – Automated Vulnerability Scanning with OWASP ZAP

### ✅ Objective:

Perform vulnerability scanning on DVWA using OWASP ZAP to identify common OWASP Top 10 issues.

### 🔧 Tools Used:

* **OWASP ZAP**: Automated scanner
* **Firefox**: Manual proxy config for capturing DVWA traffic
* **Kali Linux Terminal**: Start/stop services, launch ZAP

### 📃 Steps Performed:

1. **Install ZAP**:

   ```bash
   sudo apt update
   sudo apt install zaproxy -y
   zaproxy &
   ```

2. **Start Apache and MariaDB**:

   ```bash
   sudo systemctl start apache2
   sudo systemctl start mariadb
   ```

3. **Set DVWA Security Level to Low**:

   * Login to DVWA and go to `DVWA Security`
   * Set to "Low"

4. **Configure Firefox Proxy**:

   * Go to Firefox → Settings → Network Settings → Manual Proxy

     * HTTP Proxy: `127.0.0.1`, Port: `8080`
     * Enable "Use this proxy for all protocols"
   * Also visit `about:config` and set:

     * `network.proxy.allow_hijacking_localhost` = `true`

5. **Browse DVWA Pages to Let ZAP Discover Endpoints**:

   * Visit pages like SQLi, XSS, Brute Force, CSRF, etc.

6. **Start Active Scan in ZAP**:

   * Right-click `http://localhost/DVWA/` → Attack → Active Scan → Start

7. **View Alerts Tab**:

   * XSS, SQLi, missing headers, cookies, etc. will appear

### 🌐 Outcome:

* Identified reflected XSS, SQL Injection, insecure cookies, and header-based flaws using ZAP.

### 🔧 Suggestion:

* Always browse target endpoints before scanning
* Ensure Firefox proxy is properly configured to allow localhost traffic

---

## 🌟 Phase 3 – Manual Vulnerability Testing

### ✅ Objective:

Manually test DVWA for SQL Injection, Cross-site Scripting (XSS), and Authentication Bypass vulnerabilities.

### 🔧 Tools Used:

* **Firefox**: Web interface for DVWA
* **DVWA**: Low security mode
* **Terminal (for screenshots)**

### 📃 Steps Performed:

1. **SQL Injection**:

   * Go to `DVWA → SQL Injection`
   * Input:

     ```
     1' OR '1'='1
     ```
   * Result: Login bypassed or admin info shown

2. **XSS (Reflected)**:

   * Go to `DVWA → XSS (Reflected)`
   * Input:

     ```html
     <script>alert('XSS')</script>
     ```
   * Result: Alert popup executed

3. **Authentication Bypass**:

   * Go to `DVWA → Brute Force`
   * Input:

     * Username: `admin' OR '1'='1`
     * Password: any random string
   * Result: Logged in due to logic flaw

### 🌐 Outcome:

* Successfully demonstrated manual exploitation of DVWA's weak login and input validation.

### 🔧 Suggestion:

* Change DVWA security to "Medium" or "High" and retry to test mitigation
* Use developer tools to monitor input requests

---

## 🔖 Overall Learning & Reflection

* Hands-on experience with OWASP Top 10 vulnerabilities
* Learned both automated and manual testing
* Understood how proxy tools like ZAP intercept traffic
* Improved command-line usage and web app setup in Kali

### 📚 Commands Used Across All Phases:

```bash
# General setup
sudo apt update
sudo apt install apache2 mariadb-server php git -y

# Clone and configure DVWA
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
sudo chown -R www-data:www-data DVWA

# ZAP Installation
sudo apt install zaproxy -y
zaproxy &

# Apache and MySQL controls
sudo systemctl start apache2
sudo systemctl start mariadb
```

---

## 🌐 Used OS & Environment

* **Kali Linux (main OS in VirtualBox)** ✅
* **No Windows used for Task 1** ❌

---

## ✨ Final Outcome

Task 1 successfully completed with full understanding of web app security testing techniques, tool setup, vulnerability scanning, manual exploitation, and structured documentation.
