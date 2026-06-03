# Zabbix Installation Notes (RHEL 9 & MySQL)

These notes will help you install Zabbix on Red Hat Enterprise Linux 9 (RHEL 9). The notes are written in simple English to make it easy to understand what we are doing and why.

---

## 1. How Zabbix Works in the Background (The Structure)

![Zabbix 3D Architecture Diagram](zabbix_architecture.png)



Imagine Zabbix as a **large company monitoring team**:
* **The Zabbix Agent (The Reporter):** This is a small program installed on the computers you want to monitor. It checks the health of the computer (like CPU usage and disk space) and reports back.
* **The Zabbix Server (The Manager):** This is the brain of the operation. It receives all the reports from the Agents, checks if anything is wrong, and sends out alerts if something is broken.
* **The MySQL Database (The Filing Cabinet):** The Zabbix Server has a terrible memory. It needs a place to permanently store all the data, settings, and user information. We use MySQL for this.
* **The Zabbix Web Interface (The Dashboard):** This is the website you log into (using Apache/PHP) to look at beautiful graphs, read the reports, and configure new settings.

**The Flow:** Agent -> Server -> Database -> Web Interface

---

## 2. Installation

### Step A: Getting the System Ready
Before installing Zabbix, we must make sure our server is updated and knows where to download Zabbix.

**1. Update the Server**
```bash
sudo dnf update -y
```
* **What it does:** Downloads and installs the latest updates for your RHEL 9 operating system. The `-y` means "say yes to all prompts".
* **Why we choose it:** It is a best practice to start with a fully updated and secure system to prevent bugs and security issues.

**2. Add the Official Zabbix Repository**
```bash
sudo rpm -Uvh https://repo.zabbix.com/zabbix/7.0/rhel/9/x86_64/zabbix-release-7.0-1.el9.noarch.rpm
```
* **What it does:** Installs a small package that adds Zabbix's official software store (repository) to your system.
* **Why we choose it:** RHEL 9 does not include Zabbix in its default store. We have to tell the server exactly where to find the official Zabbix files so we get the correct, safe version.

**3. Clean the Download Cache**
```bash
sudo dnf clean all
```
* **What it does:** Clears out old software lists.
* **Why we choose it:** This forces the system to download the freshest list of software, ensuring it sees the new Zabbix repository we just added.

---

### Step B: Installing Everything
Now we will install the Server, Database, Web Interface, and Agent in one go.

**1. Install Zabbix and MySQL**
```bash
sudo dnf install zabbix-server-mysql zabbix-web-mysql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent mysql-server -y
```
* **What it does:** Installs all the necessary software packages.
* **Why we choose it:** We chose `zabbix-server-mysql` because we want to use MySQL as our filing cabinet. `zabbix-web-mysql` and `zabbix-apache-conf` install the web dashboard. `zabbix-selinux-policy` ensures Zabbix works nicely with RHEL's strict security rules. `mysql-server` is the database itself.

---

### Step C: Setting Up and Securing the Database (MySQL)

**1. Start the Database Service**
```bash
sudo systemctl start mysqld
sudo systemctl enable mysqld
```
* **What it does:** `start` turns the database on right now. `enable` tells the database to turn on automatically every time the server reboots.
* **Why we choose it:** The database must be running before we can secure it or create the Zabbix filing cabinet.

**2. Secure the Database (CRITICAL)**
```bash
sudo mysql_secure_installation
```
* **What it does:** Runs a built-in security script for MySQL. It will ask you a series of questions:
  * Set a root (admin) password? **Yes** (Create a strong password).
  * Remove anonymous users? **Yes**.
  * Disallow root login remotely? **Yes**.
  * Remove test database? **Yes**.
  * Reload privilege tables now? **Yes**.
* **Why we choose it:** Out of the box, MySQL is insecure. Anyone could access it. This tool locks the database doors so hackers cannot easily steal or delete your data.

**3. Create the Zabbix Database & User**
```bash
sudo mysql -uroot -p
```
* **What it does:** Logs you into the MySQL command line as the "root" (admin) user. It will ask for the password you just made.

*(Once inside MySQL, type these commands one by one):*
```sql
create database zabbix character set utf8mb4 collate utf8mb4_bin;
```
* **What and Why:** Creates an empty filing cabinet named "zabbix". We use `utf8mb4` so it can properly read all languages and special characters.

```sql
create user 'zabbix'@'localhost' identified by 'YOUR_STRONG_PASSWORD';
```
* **What and Why:** Creates a specific employee (user) named "zabbix" with a password. It is much safer to give Zabbix its own user rather than using the master "root" user.

```sql
grant all privileges on zabbix.* to 'zabbix'@'localhost';
```
* **What and Why:** Gives this new user full control *only* over the "zabbix" filing cabinet.

```sql
set global log_bin_trust_function_creators = 1;
quit;
```
* **What and Why:** The first command briefly lowers a strict MySQL security setting so we can import the Zabbix blueprints in the next step. `quit` simply exits MySQL.

---

### Step D: Connecting Zabbix to the Database

**1. Import the Zabbix Database Blueprints**
```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```
* **What it does:** Takes the default Zabbix database tables (which are stored in a zipped file), unzips them (`zcat`), and pipes (`|`) them straight into the MySQL database using the Zabbix user.
* **Why we choose it:** Zabbix needs specific tables (like 'Users', 'Alerts', 'Hosts') to function. This command builds all of them automatically. (It will ask for the password you created for the 'zabbix' database user).

**2. Turn the strict security setting back on**
```bash
sudo mysql -uroot -p -e "set global log_bin_trust_function_creators = 0;"
```
* **What and Why:** We turned this setting off temporarily to import the database. We turn it back on (to `0`) to keep the database secure.

**3. Tell Zabbix the Database Password**
```bash
sudo nano /etc/zabbix/zabbix_server.conf
```
* **What it does:** Opens the main configuration file for the Zabbix Server in a text editor called `nano`.
* **Why we choose it:** The Zabbix Server needs to know the password to log into the MySQL database. 
* **Action:** Find the line that says `# DBPassword=`. Delete the `#` to uncomment it, and type your password so it looks like this: `DBPassword=YOUR_STRONG_PASSWORD`. Save and exit (Ctrl+O, Enter, Ctrl+X).

---

### Step E: Starting Everything Up

**1. Start and Enable Zabbix Services**
```bash
sudo systemctl restart zabbix-server zabbix-agent httpd php-fpm
sudo systemctl enable zabbix-server zabbix-agent httpd php-fpm
```
* **What it does:** Restarts the Zabbix Server, the Zabbix Agent (to monitor this server itself), the Web server (`httpd`), and the PHP processor (`php-fpm`). `enable` makes sure they all start automatically if the server loses power and reboots.
* **Why we choose it:** Because we just changed configuration files and imported data, restarting all services ensures they load the new, correct settings.

**2. Open the Firewall for the Web Interface**
```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```
* **What it does:** Tells the RHEL 9 built-in security wall to allow normal web traffic (Ports 80 and 443).
* **Why we choose it:** Without this, your computer's browser would be blocked when you try to visit the Zabbix web dashboard.

---

## 3. The Final Step
Now, open a web browser on your computer and type your server's IP address:
`http://YOUR_SERVER_IP/zabbix`

Follow the setup wizard on the screen. The default login is usually:
* **Username:** Admin (Case-sensitive, capital 'A')
* **Password:** zabbix
