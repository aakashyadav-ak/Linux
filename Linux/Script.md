# 🎥 SCENE 1: INTRODUCTION

**⏱ Duration: 0:00 – 0:50**

**🖥 Screen:** Zabbix Official Website / Video Title Screen

**🎙️ Voiceover:**

"Hello everyone, welcome back to the channel!

Aaj ki is video mein hum install aur configure karne wale hain Zabbix version 8.0, Red Hat Enterprise Linux 10 par, with MySQL version 8.4.

Is installation ke liye hum Zabbix ki official website aur official documentation ko follow karenge.

Aur sabse important baat, hum ye complete setup ek hi machine par configure karenge, jahan Zabbix Server, MySQL Database, Web Frontend aur Zabbix Agent sabhi ek hi server par run karenge.

Toh chaliye, bina time waste kiye installation start karte hain."

---

# 🎥 SCENE 2: WHAT IS ZABBIX?

**⏱ Duration: 0:50 – 1:40**

**🖥 Screen:** Zabbix Official Website — Overview

**🎙️ Voiceover:**

"Installation start karne se pehle briefly samajh lete hain ki Zabbix actually hai kya.

Zabbix ek open-source enterprise monitoring solution hai, jiska use hum servers, networks, applications, databases aur complete IT infrastructure ko monitor karne ke liye karte hain.

Zabbix ki help se hum CPU utilization, memory usage, disk space, network traffic aur system availability jaise important metrics ko real time mein monitor kar sakte hain.

Iske alawa Zabbix hume alerts, triggers, graphs aur customizable dashboards bhi provide karta hai.

Agar infrastructure mein koi problem detect hoti hai, toh Zabbix configured notification channels ke through hume alert kar sakta hai.

Is video mein hum Zabbix 8.0 ke installation aur basic monitoring setup par focus karenge."

---

# 🎥 SCENE 3: ENVIRONMENT OVERVIEW

**⏱ Duration: 1:40 – 2:30**

**🖥 Screen:** Terminal / Environment Details

**🎙️ Voiceover:**

"Ab ek baar apna environment check kar lete hain.

Yahan main Red Hat Enterprise Linux version 10 use kar raha hoon.

Database ke liye hum MySQL 8.4 use karenge.

Zabbix ka version rahega 8.0, aur web frontend ke liye hum Apache web server use karenge.

Yahan ek important point hai.

Normally production environments mein Zabbix Server aur Database ko separate machines par configure kiya ja sakta hai, depending on infrastructure requirements.

Lekin is tutorial mein hum single-server architecture follow kar rahe hain.

Matlab hamara database, Zabbix Server, frontend aur monitoring agent, sabhi isi machine par installed honge.

Isse installation process ko samajhna aur demonstrate karna kaafi easy ho jayega."

### Terminal Commands

```bash
cat /etc/redhat-release
```

```bash
hostnamectl
```

```bash
ip a
```

**🎙️ Voiceover:**

"Sabse pehle hum apne operating system ka version verify karenge.

Uske baad hostname aur IP address check karenge, taaki aage frontend access karte waqt hume apne server ka IP address pata ho."

---

# 🎥 SCENE 4: OFFICIAL ZABBIX WEBSITE

**⏱ Duration: 2:30 – 3:30**

**🖥 Screen:** Browser → Zabbix Official Website

**🎙️ Voiceover:**

"Ab hum apna browser open karenge aur Zabbix ki official website par jayenge.

Yahan se hum Download section open karenge.

Zabbix hume different operating systems, databases aur web servers ke according installation instructions provide karta hai.

Hum yahan apne environment ke according options select karenge.

Zabbix version 8.0 select karenge.

Operating system mein Red Hat Enterprise Linux select karenge.

OS version mein 10 select karenge.

Database ke liye MySQL select karenge.

Aur web server ke liye Apache choose karenge.

Ab yahan Zabbix hume installation ke liye required commands provide karega.

Hum inhi official commands ko step by step follow karenge.

Ek baat dhyan rakhiye, Zabbix 8.0 abhi pre-release stage mein hai, isliye production deployment se pehle stable release aur compatibility requirements zaroor verify karein."

---

# 🎥 SCENE 5: INSTALL ZABBIX REPOSITORY

**⏱ Duration: 3:30 – 4:30**

**🖥 Screen:** Official Download Page → Terminal

**🎙️ Voiceover:**

"Ab installation ka first step hai Zabbix official repository ko configure karna.

Repository basically ek package source hota hai, jahan se hum required software packages download aur install kar sakte hain.

Yahan hum Zabbix ki official repository ka RPM package install karenge.

Iske baad DNF cache clean karenge, taaki system updated repository metadata fetch kar sake.

Repository configure hone ke baad hum Zabbix ke required packages install kar payenge."

**🖥 Screen Action:**

Official Zabbix website se repository installation command copy karke terminal mein execute karein.

```bash
dnf clean all
```

**🎙️ Voiceover:**

"Ab hamari Zabbix repository configure ho chuki hai.

Next step mein hum required Zabbix components install karenge."

---

# 🎥 SCENE 6: INSTALL ZABBIX COMPONENTS

**⏱ Duration: 4:30 – 5:45**

**🖥 Screen:** Terminal — Package Installation

**🎙️ Voiceover:**

"Ab hum Zabbix Server, frontend, Apache configuration, SQL scripts aur Zabbix Agent 2 install karenge.

Yahan Zabbix Server hamara main monitoring component hai.

Frontend ki help se hum browser ke through monitoring dashboard access karenge.

SQL scripts ka use initial database structure create karne ke liye hoga.

Aur Zabbix Agent 2 ki help se hum apne local Linux server ke performance metrics collect karenge.

Installation complete hone ke baad hum database configuration ki taraf move karenge."

**🖥 Screen Action:**

Official documentation mein diye gaye package installation commands execute karein.

**🎙️ Voiceover:**

"Installation process complete hone mein thoda time lag sakta hai, depending on internet speed aur system performance.

Jaise hi packages successfully install ho jayenge, hum next step continue karenge."

---

# 🎥 SCENE 7: MYSQL 8.4 DATABASE CONFIGURATION

**⏱ Duration: 5:45 – 7:15**

**🖥 Screen:** Terminal — MySQL Configuration

**🎙️ Voiceover:**

"Ab hum installation ke ek important part par aa gaye hain, jo hai database configuration.

Zabbix apni monitoring configuration, historical data, events aur collected metrics ko database mein store karta hai.

Is tutorial mein hum MySQL version 8.4 use kar rahe hain.

Sabse pehle verify karenge ki MySQL service properly running hai.

Uske baad MySQL mein login karke Zabbix ke liye ek dedicated database create karenge.

Database create karne ke baad hum ek separate database user configure karenge.

Is user ko sirf required database permissions provide karenge.

Yahan hum Zabbix ke liye dedicated database credentials use kar rahe hain, jo security aur database management ke perspective se better approach hai."

### Terminal Commands

```bash
mysql --version
```

```bash
systemctl status mysqld
```

```bash
mysql -u root -p
```

**🎙️ Voiceover:**

"Ab hum MySQL ke andar Zabbix database create karenge.

Database create karte waqt character set aur collation ko official documentation ke according configure karenge.

Uske baad Zabbix database user create karenge aur required privileges assign karenge.

Database aur user create hone ke baad hum MySQL se exit karenge."

---

# 🎥 SCENE 8: IMPORT INITIAL ZABBIX DATABASE

**⏱ Duration: 7:15 – 8:15**

**🖥 Screen:** Terminal — Database Import

**🎙️ Voiceover:**

"Ab hamara database ready hai, lekin abhi iske andar Zabbix ka required database structure available nahi hai.

Isliye next step mein hum Zabbix ke initial SQL schema ko import karenge.

Zabbix installation ke saath SQL scripts provide karta hai, jinmein required tables aur initial configuration data available hota hai.

Hum official documentation mein provided command ka use karke ye data apne Zabbix database mein import karenge.

Database import hone mein kuch time lag sakta hai.

Import successfully complete hone ke baad hamara database Zabbix Server ke saath connect hone ke liye ready ho jayega."

**🖥 Screen Action:**

Official documentation se SQL import command execute karein.

**🎙️ Voiceover:**

"Ab database import successfully complete ho chuka hai.

Next step mein hum Zabbix Server ki configuration file update karenge."

---

# 🎥 SCENE 9: CONFIGURE ZABBIX SERVER

**⏱ Duration: 8:15 – 9:15**

**🖥 Screen:** Terminal — Zabbix Configuration File

**🎙️ Voiceover:**

"Ab hum Zabbix Server ko apne MySQL database ke saath connect karenge.

Iske liye hum Zabbix Server ki main configuration file open karenge.

Yahan database connection ke liye required parameters configure karne honge.

Jaise database name, database user aur database password.

Kyuki humara MySQL database aur Zabbix Server same machine par installed hain, isliye hum local database connection use karenge.

Configuration update karne ke baad file ko save kar denge."

### Terminal Command

```bash
vi /etc/zabbix/zabbix_server.conf
```

**Configuration:**

```ini
DBName=zabbix
DBUser=zabbix
DBPassword=YOUR_DATABASE_PASSWORD
```

**🎙️ Voiceover:**

"Yahan database password wahi enter karna hai jo humne database user create karte waqt configure kiya tha.

Ab hum configuration file save karke exit karenge."

---

# 🎥 SCENE 10: START ZABBIX SERVICES

**⏱ Duration: 9:15 – 10:30**

**🖥 Screen:** Terminal — Systemd Services

**🎙️ Voiceover:**

"Ab hamari basic installation aur database configuration complete ho chuki hai.

Next step mein hum required services ko start aur enable karenge.

Yahan hum Zabbix Server, Zabbix Agent 2, Apache aur PHP-FPM services start karenge.

Services ko enable bhi karenge, taaki system reboot hone ke baad ye automatically start ho sakein.

Uske baad hum verify karenge ki services properly running hain ya nahi."

### Terminal Commands

```bash
systemctl restart zabbix-server zabbix-agent2 httpd php-fpm
```

```bash
systemctl enable zabbix-server zabbix-agent2 httpd php-fpm
```

```bash
systemctl status zabbix-server
```

```bash
systemctl status zabbix-agent2
```

```bash
systemctl status httpd
```

**🎙️ Voiceover:**

"Yahan aap dekh sakte hain ki hamari services active aur running state mein hain.

Agar kisi service mein error aata hai, toh hum system logs aur Zabbix logs check karke issue troubleshoot kar sakte hain.

Ab hum firewall configuration verify karenge."

---

# 🎥 SCENE 11: FIREWALL CONFIGURATION

**⏱ Duration: 10:30 – 11:15**

**🖥 Screen:** Terminal — Firewall Configuration

**🎙️ Voiceover:**

"Ab Zabbix frontend ko browser se access karne ke liye hume firewall mein HTTP service allow karni hogi.

Iske alawa Zabbix Server aur Agent ke communication ke liye required ports bhi configure karenge.

Zabbix Server normally port 10051 use karta hai.

Aur Zabbix Agent port 10050 use karta hai.

Yahan firewall rules ko apne environment ki requirements ke according configure karna important hai.

Production environment mein unnecessary ports ko publicly expose nahi karna chahiye."

### Terminal Commands

```bash
firewall-cmd --permanent --add-service=http
```

```bash
firewall-cmd --permanent --add-port=10051/tcp
```

```bash
firewall-cmd --permanent --add-port=10050/tcp
```

```bash
firewall-cmd --reload
```

```bash
firewall-cmd --list-all
```

**🎙️ Voiceover:**

"Ab required firewall configuration complete ho chuki hai.

Next step mein hum Zabbix web frontend access karenge."

---

# 🎥 SCENE 12: ZABBIX WEB FRONTEND SETUP

**⏱ Duration: 11:15 – 13:00**

**🖥 Screen:** Browser — Zabbix Installation Wizard

**🎙️ Voiceover:**

"Ab hum apna browser open karenge aur server ke IP address ke through Zabbix frontend access karenge.

Yahan hum server ka IP address enter karenge, followed by slash Zabbix.

Jaise hi page open hoga, hume Zabbix ka installation wizard दिखाई देगा.

Sabse pehle hum language select karenge.

Uske baad Zabbix required prerequisites check karega.

Agar saari requirements successfully fulfill ho rahi hain, toh hum next step par proceed karenge.

Ab database connection details enter karenge.

Database type mein MySQL select karenge.

Database host mein localhost use karenge.

Database name mein Zabbix enter karenge.

Aur database user aur password wahi provide karenge jo humne MySQL configuration ke time create kiya tha.

Uske baad next par click karenge.

Ab hum Zabbix Server ka name configure karenge.

Timezone select karenge aur installation summary verify karenge.

Sabhi details correct hone ke baad installation finish karenge."

---

# 🎥 SCENE 13: LOGIN TO ZABBIX DASHBOARD

**⏱ Duration: 13:00 – 14:00**

**🖥 Screen:** Zabbix Login Page → Dashboard

**🎙️ Voiceover:**

"Ab hamara Zabbix web frontend successfully configure ho chuka hai.

Yahan hume Zabbix ka login page दिखाई दे raha hai.

Ab hum initial administrator credentials ke through login karenge.

Login successful hone ke baad Zabbix ka main dashboard open ho jayega.

Yahan se hum hosts, monitoring data, problems, alerts aur dashboards manage kar sakte hain.

Security ke liye initial administrator password change karna important hai.

Ab hum verify karenge ki Zabbix Server properly running hai aur monitoring data collect kar raha hai."

---

# 🎥 SCENE 14: ADD LOCAL LINUX HOST

**⏱ Duration: 14:00 – 15:30**

**🖥 Screen:** Zabbix Dashboard → Hosts

**🎙️ Voiceover:**

"Ab hum Zabbix installation ko practically verify karenge.

Iske liye hum apne local RHEL 10 server ko monitor karenge.

Kyuki humne Zabbix Agent 2 already install kar liya hai, toh ab hume host configuration verify karni hai.

Zabbix frontend mein hum Hosts section open karenge.

Yahan hum apne Linux server ko add ya existing host configuration ko update karenge.

Host ke liye ek meaningful name provide karenge.

Uske baad required host group select karenge.

Agent interface mein apne server ka IP address configure karenge.

Aur monitoring template mein Linux by Zabbix agent select karenge.

Template basically predefined monitoring items aur triggers ka collection hota hai.

Iski help se hum manually har monitoring item create kiye bina CPU, memory, disk aur network metrics monitor kar sakte hain.

Host configuration save karne ke baad hum monitoring data check karenge."

---

# 🎥 SCENE 15: VERIFY MONITORING DATA

**⏱ Duration: 15:30 – 16:30**

**🖥 Screen:** Monitoring → Latest Data / Dashboards

**🎙️ Voiceover:**

"Ab hum verify karenge ki Zabbix hamare Linux server se monitoring data receive kar raha hai ya nahi.

Iske liye hum Latest Data section open karenge.

Yahan apna configured host select karenge.

Ab aap dekh sakte hain ki Zabbix different system metrics collect kar raha hai.

Jaise CPU utilization, available memory, disk usage aur network-related statistics.

Kuch monitoring items ko initial data collect karne mein thoda time lag sakta hai.

Jaise hi data available hota hai, hum graphs ke through server performance analyze kar sakte hain.

Is tarah Zabbix hume apne infrastructure ki health aur performance monitor karne mein help karta hai."

---

# 🎥 SCENE 16: FINAL VERIFICATION

**⏱ Duration: 16:30 – 17:15**

**🖥 Screen:** Terminal + Zabbix Dashboard

**🎙️ Voiceover:**

"Ab ek baar apne complete installation ko verify kar lete hain.

Humne Red Hat Enterprise Linux 10 par Zabbix 8.0 install kiya.

Database ke liye MySQL 8.4 configure kiya.

Zabbix Server ko database ke saath connect kiya.

Apache web server ke through Zabbix frontend access kiya.

Aur Zabbix Agent 2 ki help se apne local Linux server ki monitoring bhi configure ki.

Yahan humara complete setup single-machine architecture par running hai.

Iska matlab Zabbix Server, Database, Frontend aur Agent sabhi same machine par configured hain."

---

# 🎥 SCENE 17: OUTRO

**⏱ Duration: 17:15 – 18:00**

**🖥 Screen:** Zabbix Dashboard / End Screen

**🎙️ Voiceover:**

"So guys, is video mein humne successfully Zabbix 8.0 ko Red Hat Enterprise Linux 10 par MySQL 8.4 ke saath install aur configure kiya.

Humne complete installation ke liye Zabbix ki official website aur documentation follow ki.

Saath hi humne apne local Linux server ki basic monitoring bhi configure ki.

Agar aap Zabbix seekh rahe hain ya apne infrastructure ke liye monitoring solution explore kar rahe hain, toh hopefully ye video aapke liye helpful rahi hogi.

Aane wali videos mein hum Zabbix ke monitoring templates, triggers, alerts, dashboards aur advanced configuration ko explore karenge.

Agar video helpful lagi ho toh like karein, channel ko subscribe karein, aur apne questions comment section mein zaroor share karein.

Thanks for watching, and see you in the next video!"
