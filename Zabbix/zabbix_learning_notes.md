# Zabbix Monitoring - Detailed Learning Notes from Basics


---
**Zabbix is an open-source monitoring tool used to monitor:**

- Servers
- Applications
- Databases
- Network devices
- Websites
- Cloud resources
## 1. First Things: Understanding Organizational Infrastructure

Before diving into tools, the very first thing I learned was about the organization's setup. This is important because monitoring tools like Zabbix need to know where everything is located to work properly.

### What is Infrastructure in Simple Terms?
Infrastructure means all the computers, servers, networks, and locations where the company runs its systems. It includes physical or virtual machines in different places (data centers or cloud zones). Knowing this helps set up monitoring connections without issues.

### Zones in the Organization
The organization uses different "zones" (like separate areas or data centers). These zones are named:
- **az1**
- **az2**
- **az3**
- **az4**
- **caz** (possibly Central Availability Zone)
- **can** (possibly Canada region)
- **mum** (possibly Mumbai region)

Each zone has its own servers, networks, and IP addresses (Internet Protocol addresses - unique numbers that identify devices on a network, like 192.168.1.10).

**Why learn zones and IPs?**
- To plan where to place monitoring components (like proxies - explained later).
- To ensure Zabbix can reach hosts (monitored computers) from different locations.
- For connectivity, security rules (firewalls), and efficient data collection.

**Respective IPs:** During initial learning, I noted the specific IP addresses or IP ranges for each zone (e.g., az1 might use 10.0.1.x range, az2 10.0.2.x, etc.). These are internal details for connectivity planning. Always check with the team for exact current IPs, as they can change. Proxies were created at different locations matching these zones to collect data locally and send it to the central server.

**Proxies at Different Locations:**
Zabbix proxies (explained in detail later) were set up in various zones. This reduces load on the main server and works even if the network between zones is slow or down temporarily. For example:
- One proxy in az1 zone.
- Another in mum zone, etc.

**Tip:** Draw a simple diagram of zones + IPs + proxy locations when starting. This was my starting point for all monitoring work.

---

## 2. Introduction to Zabbix - The Monitoring Tool Used at I2K2

The second major thing I learned is **Zabbix**. It is the main monitoring tool used in I2K2.

### What is Zabbix? (Basic Explanation)
Zabbix is an **enterprise-class open-source distributed monitoring solution**.

- **Open-source**: Free to use, download, and modify. The code is public. No license fees.
- **Enterprise-class**: Powerful enough for big companies, with professional features.
- **Distributed**: Can monitor thousands of devices across many locations using extra components like proxies.
- **Monitoring solution**: It watches (monitors) computers, networks, websites, databases, and more. It collects data (like CPU usage, disk space, temperature), checks if everything is okay, and alerts people when there is a problem.


### Why is Zabbix Used?
- To know immediately if a server is down or slow.
- To prevent big problems by catching small issues early (e.g., disk almost full).
- To see trends over time (e.g., "CPU usage is increasing every week").
- To generate reports for clients or management (how reliable was our service last month?).
- To monitor everything from one place, even if systems are in different cities or clouds.
- Saves time: No need to log into every server manually to check.

At I2K2, Zabbix is used for monitoring servers, proxies, hypervisors (software that runs virtual machines), databases, websites, and more.

### Key Features We Use in Zabbix for Monitoring
Zabbix has many features. Here are the main ones we actively use and learned:

- **Data Collection (Metrics/Items)**: Collects numbers and text from hosts using agents, SNMP, web checks, etc. Examples: CPU load, memory used, website response time.
- **Problem Detection (Triggers)**: Rules that say "If CPU > 90% for 5 minutes, it is a problem." Triggers turn green/yellow/red.
- **Alerts and Notifications**: Sends emails, SMS, or other messages when problems happen. Supports media types.
- **Dashboards and Visualization**: Custom screens with graphs, maps, lists of problems. Easy to see everything at a glance.
- **Templates**: Ready-made packages of items, triggers, and graphs for common things (Linux server, Windows server, database, proxy, hypervisor). Saves huge time - no need to create everything from scratch.
- **Host Groups**: Organize hosts into logical groups (e.g., "Production Servers", "Database Servers").
- **Users and Permissions**: Create users, put them in groups, give different access levels (who can see what).
- **Proxies**: For remote locations - collect data locally and forward to main server.
- **Discovery**: Automatically find new hosts or items.
- **Web Monitoring**: Check if websites are up and working (response time, content checks).
- **SNMP Monitoring**: Special way to monitor network devices (routers, switches) without installing anything on them.
- **Reports**: Utilization reports, SLA (Service Level Agreement) reports, scheduled reports.
- **Encryption**: Secure communication using PSK keys.
- **Problem Acknowledgment**: Mark problems as "I am working on it" so others know.
- **Automation**: Actions that run when problems occur (e.g., restart a service).
- **Scalability**: Works for small setups or huge ones with millions of metrics.

**Zabbix Version History (What I Worked On):**
- Zabbix 3.0 (older version - basic features)
- Zabbix 5.4.3 (improved UI, better templates, security)
- Zabbix 6.0 (current focus - still learning; added more dashboards, better proxy features, etc.)

**Note:** As of 2026, Zabbix 7.0 LTS is the latest major version with even more improvements (faster polling, better web monitoring with real browsers, proxy high availability). But my hands-on was up to 6.0. Always check the official site for upgrades.

**Under Guidance:** With support from my mentor, I successfully installed the Zabbix server on the local network. This was a big milestone.

---

## 3. Installation - Step by Step (From Basics)

In the last 6 months, I learned and performed many installations. Zabbix has several parts:
- **Zabbix Server**: The brain. Collects all data, processes triggers, stores in database.
- **Zabbix Frontend (Web UI)**: The website where you log in to see dashboards, configure everything.
- **Zabbix Agent**: Small program installed on every computer you want to monitor. Collects detailed data from inside the OS.
- **Zabbix Proxy**: Optional but powerful - sits between remote agents and the server.
- **Database**: Usually MySQL/MariaDB or PostgreSQL to store all history.

**Important Basics Before Installing:**
- Need a Linux server (or Windows in some cases) with enough CPU, RAM (at least 4-8 GB recommended for start), and disk space.
- Internet for downloading packages.
- Root/sudo access.
- Firewall rules open for ports: 10051 (server/proxy), 10050 (agent), 80/443 (web), database ports.
- Time sync (NTP) important for all components.
- Choose database type early.

### 3.1 Zabbix Server Installation
**Basic Steps (Example on Ubuntu/Debian - similar on others):**

1. Update system: `sudo apt update && sudo apt upgrade`
2. Install Zabbix repository (official packages):
   - For Zabbix 6.0 on Ubuntu: Download and install the release package from zabbix.com.
   - Example: `wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu$(lsb_release -rs)_all.deb`
   - Then `sudo dpkg -i zabbix-release...deb` and `sudo apt update`
3. Install server and related: `sudo apt install zabbix-server-mysql zabbix-sql-scripts`
4. Install and setup database (MySQL example):
   - `sudo apt install mysql-server`
   - Create database and user for Zabbix.
   - Import initial schema: Use the SQL scripts provided.
5. Edit config: `/etc/zabbix/zabbix_server.conf` - set DB password, etc.
6. Start and enable: `sudo systemctl start zabbix-server` and `enable` for boot.
7. Check logs: `sudo journalctl -u zabbix-server` or tail the log file.

**On CentOS/RHEL:**
Use `dnf` or `yum` with similar repo setup from zabbix.com (rpm package).

**On Windows:**
Zabbix server is mainly for Linux. Not common on Windows for production.

**Local Network Install Note:** I did this successfully on our internal network with guidance. Started with basic DB setup, then server.

### 3.2 Zabbix Frontend Installation
The frontend is the web part (PHP-based).

1. Install web server (Apache or Nginx) and PHP.
   - Ubuntu: `sudo apt install apache2 php php-mysql ...` (many PHP modules needed).
2. Install Zabbix web package: `sudo apt install zabbix-frontend-php`
3. Configure web server virtual host for Zabbix (usually /zabbix).
4. Edit PHP config for timezone, etc.: `/etc/php/*/apache2/php.ini` or similar.
5. Restart web server.
6. Open browser: http://your-server-ip/zabbix
7. Follow the web setup wizard:
   - Check prerequisites.
   - Enter database details.
   - Create admin user (default zabbix/zabbix - change password immediately!).
   - Finish.

**Common Issues:** Missing PHP extensions (like gd, mbstring, xml), wrong timezone, permissions on config files.

### 3.3 Zabbix Agent Installation
The agent is lightweight and installed on every host you monitor.

**On Linux (Ubuntu/Debian/CentOS):**
1. Install repo same as server.
2. `sudo apt install zabbix-agent` (or zabbix-agent2 for newer).
3. Edit `/etc/zabbix/zabbix_agentd.conf` (or zabbix_agent2.conf):
   - `Server=IP-of-Zabbix-Server-or-Proxy`
   - `ServerActive=IP-of-Zabbix-Server-or-Proxy` (for active checks)
   - `Hostname=exact-name-of-this-host` (must match what you put in frontend)
4. Start: `sudo systemctl start zabbix-agent` and enable.
5. Test: Use `zabbix_get` or check port 10050.

**On Windows:**
- Download MSI installer from zabbix.com or use zip.
- Run installer, choose components.
- Configure via UI or edit zabbix_agentd.conf (usually in C:\Program Files\Zabbix Agent).
- Set Server and ServerActive.
- Install as service.
- Start the service.
- For firewall: Allow port 10050 inbound if passive checks.

**Agent Installation Which Connects Through Proxies:**
- In agent config: Point Server and ServerActive to the **proxy's IP**, not the main server.
- In Zabbix frontend (when adding the host): Set "Monitored by proxy" to the correct proxy.
- This way, the agent talks only to the nearby proxy. Proxy forwards everything to server.
- Great for remote zones.

### 3.4 Zabbix Proxy Installation
Proxies are installed on servers in remote zones.

**Basic Steps (Ubuntu example):**
1. Install repo.
2. `sudo apt install zabbix-proxy-mysql zabbix-sql-scripts`
3. Setup local database for proxy (smaller than server DB).
4. Edit `/etc/zabbix/zabbix_proxy.conf`:
   - `Server=IP-of-main-Zabbix-Server`
   - `Hostname=unique-proxy-name` (must match frontend)
   - DB settings.
   - Mode: Active (proxy connects to server) or Passive (server connects to proxy). Active is often preferred.
5. Start proxy service.
6. **Register in Frontend:** Go to Administration > Proxies > Create proxy. Enter exact Hostname, mode, IP. Add interface.
7. Monitor the proxy itself as a host using "Template App Zabbix Proxy".

**Benefits:** Data buffering if link to server is down. Less traffic to central server. Local processing.

**Proxies at Different Locations:** I learned to deploy one per major zone (az1, mum, etc.) for best performance.

### 3.5 Installed Zabbix in Different Operating Systems
- **CentOS**: Used yum/dnf repos. Good for older setups.
- **Debian/Ubuntu**: apt repos. Clean and easy. Most common in our env.
- **Windows**: Mainly for agents (MSI). Server/frontend rare on Windows.
- Always match Zabbix version across server, proxy, agent, frontend for best compatibility (within same major version like 6.0.x).

**General Tip:** Use official Zabbix repositories. Test in a lab first. Document every step with screenshots.

---

## 4. Basic Configuration After Installation

Once installed, log in and start configuring.

### Login and Configure the User
- Default: http://server/zabbix → user: Admin, password: zabbix
- **First action:** Change the Admin password immediately.
- Go to Administration > Users to create more users.
- For each user: Set alias (login name), name, password, language, theme.
- Assign to groups and set media (email, etc.) for notifications.

### Adding Host in Zabbix
A "host" is anything you monitor (server, switch, website, etc.).

Steps:
1. Configuration > Hosts > Create host.
2. Host tab: Name (visible), Hostname (technical, often same), Groups (add to at least one), Templates (add later), Interfaces (IP + port, e.g., Agent interface on port 10050).
3. For proxy: Select "Monitored by proxy".
4. Save. Zabbix will start collecting data if agent/template is correct.

**Test connection:** Use "Check now" or look at Latest data.

### Create a Host Group as per Preference
- Configuration > Host groups > Create host group.
- Give name like "Linux Servers - Production" or "Database - Mumbai Zone".
- Add hosts to groups for easy filtering and permissions.

### Set Templates for Server, Proxies, Hypervisor, Database
Templates are the best feature!

- Many built-in: "Linux by Zabbix agent", "Windows by Zabbix agent", "Template App Zabbix Proxy", database-specific (MySQL, PostgreSQL), hypervisor (VMware, etc.).
- When adding host: Click "Select" under Templates and add 1-3 relevant ones.
- Templates bring:
  - Items (what to monitor)
  - Triggers (when to alert)
  - Graphs
  - Dashboards widgets sometimes

**Learn about setting templates on different threshold:**
- After linking template, you can override items/triggers per host.
- Example: For a critical database server, change CPU trigger from 80% to 70%.
- Use "Mass update" or edit individual items.
- Thresholds = the numbers that decide "warning" or "problem" (e.g., disk free < 10% = problem).

### Adding Items for Hosts
Items are individual metrics.
- In host or template: Items > Create item.
- Key: The magic code, e.g., `system.cpu.load[all,avg1]` for CPU.
- Type: Zabbix agent, SNMP, etc.
- Interval: How often to check (30s, 1m, 5m).
- History and Trends storage periods.
- Units: %, MB, etc.

Templates already have hundreds of items. Add custom ones as needed.

### Adding New Triggers on Different Thresholds
Triggers define problems.
- In host/template: Triggers > Create trigger.
- Expression: Example `last(/host/system.cpu.load[all,avg1])>5` means "last value > 5".
- Severity: Information, Warning, Average, High, Disaster.
- Description, recovery expression.
- Can depend on other triggers.

We learned to set different thresholds per environment (test vs production).

### Create User
- Administration > Users > Create user.
- Basic info + password.
- Groups: Add to groups for permissions.
- Media: Add email, etc. for alerts.

### Create User Groups
- Administration > User groups > Create.
- Name, e.g., "Monitoring Team", "Read-Only Viewers".
- Add users to the group.

### Give Permission to the User Groups
Permissions are key for security.
- In user group: Permissions tab.
- For Host groups: Read-only, Read-write, or Deny.
- Can set on Templates too.
- Also, Frontend sections access (Configuration, Monitoring, etc.).
- Use roles (in newer versions) for more control.
- Best practice: Least privilege - give only what is needed.

**Problem Acknowledgment:**
- When a problem appears (red icon), click it.
- Acknowledge: Add comment "Working on it - John".
- This updates the problem status so team knows it's being handled.
- Can close or change severity.

---

## 5. Monitoring Methods

### Using SNMP for Host Monitoring
SNMP = Simple Network Management Protocol.
- Used for devices that don't have agents easily (routers, switches, printers, UPS, some servers).
- No agent install needed.
- Zabbix acts as SNMP manager, device is agent.
- Versions: v1, v2c (community string like password), v3 (more secure with users/passwords).
- In Zabbix:
  - Add host with SNMP interface (IP, port 161, community).
  - Use SNMP templates or create items with OIDs (object identifiers - like .1.3.6.1.2.1.1.3.0 for uptime).
- Pros: Agentless.
- Cons: Less detailed than agent, security concerns if not using v3.

We used it for network equipment and some legacy systems.

### Webpage Monitoring
- Also called Web scenarios or HTTP checks.
- Monitor websites: Is it up? How fast? Does it contain certain text?
- Configuration > Web monitoring > Create scenario.
- Add steps: GET/POST URL, required string, variables, etc.
- Can check login flows, multi-step.
- Results appear as items (response time, status code, etc.).
- Triggers on slow pages or errors.
- In Zabbix 7+ even better with real browser simulation.

Great for client-facing sites.

---

## 6. Security: Encrypting the Connection Through the PSK Key

**PSK = Pre-Shared Key** (like a secret password shared between components).

Why? To encrypt traffic between Zabbix server <-> proxy <-> agent so no one can read the data in between.

**How to set up (basic steps):**
1. Generate a PSK key (long random string, e.g., using `openssl rand -hex 32`).
2. On agent config: `TLSConnect=psk`, `TLSAccept=psk`, `TLSPSKIdentity=unique-name`, `TLSPSKFile=/path/to/key`.
3. Same on proxy and server configs.
4. In Zabbix frontend:
   - For host: Go to Encryption tab.
   - Set Connections to host: PSK, select identity and paste key (or use file).
   - For proxy: Similar in proxy config.
5. Restart services.
6. Test: Data should flow encrypted. Look for "TLS" in logs or status.

**Benefits:** Secure, especially over public internet or between zones.
We also learned certificate-based encryption as alternative.

Always use encryption in production!

---

## 7. Alerts, Notifications, and Problem Handling

### Receiving Problem Notification
- When trigger fires → Action runs → Notification sent via media type to users.
- You receive email/SMS/whatever when problem starts and when it recovers.
- Can have different actions for different severities or host groups.

### Adding Media Type for Users
Media types = how to send messages.

- Administration > Media types > Create.
- Types: Email (SMTP settings), SMS (via script or gateway), Webhook (for Slack, Teams, Jira, etc.), Custom scripts.
- For Email: Set SMTP server, port, authentication, from address.
- Then, for each user: In user profile > Media > Add media type (e.g., Email to john@company.com).
- Test it!

We added several: Email for everyone, and integrations for teams.

### Manage Ace Pelican and Nagios Alerts
- "Ace Pelican" and Nagios are other monitoring or alerting tools/systems (possibly internal or third-party).
- In Zabbix, we learned to integrate or manage alerts coming from/going to these.
- Examples: Forward Zabbix problems to Nagios, or receive external alerts into Zabbix.
- Often done via webhooks, scripts, or API.
- Goal: Single pane of glass - see all alerts in one place or route them properly.
- We worked on disabling duplicate alerts and proper routing.

### Worked on Disabling Servers
- When a server is being maintained or decommissioned:
  - In host: Set "Status" to Disabled (or use maintenance periods).
  - This stops data collection and alerts for that host.
  - Or put host in maintenance window (recurring or one-time) so no alerts during planned downtime.
- Always communicate with team before disabling.

---

## 8. Dashboards, Reports, and SLA

### Manage Dashboards
- Dashboards are custom home screens.
- Monitoring > Dashboards > Create.
- Add widgets: Problems (list of current issues), Graphs (CPU over time), Latest data, Maps, Clock, etc.
- Can make public or private.
- Share with user groups.
- We created zone-specific dashboards (e.g., Mumbai Zone Overview) and global ones.
- Widgets can be resized, arranged nicely.

### Generate Utilization Reports as per the Client
- Use dashboards + scheduled reports or export.
- Or create custom reports via API/scripts.
- Show CPU, memory, disk, network usage over time for specific hosts/groups.
- Export to PDF or email.
- Clients get regular reports on how their infrastructure is performing.

### SLA & Schedule Report
**SLA (Service Level Agreement) Reports:**
- Zabbix has built-in SLA functionality (under Services).
- Define Services (business view of your infrastructure).
- Create SLA: Name, SLO (e.g. 99.5% uptime), Reporting period (monthly), Schedule (24x7 or business hours), Effective date.
- Link to triggers/problems that affect the service.
- Zabbix calculates actual availability vs target.
- View SLA reports showing percentage uptime, downtime periods.

**Scheduled Reports:**
- Administration > Reports > Scheduled reports (or similar in your version).
- Create report based on a Dashboard.
- Set period (last month), cycle (monthly), start time, recipients (users with email media).
- Zabbix automatically generates PDF of the dashboard and emails it on schedule.
- Perfect for management and clients.

We learned to set these up for regular client updates.

### Mediatype
Already covered above - it's the configuration for how alerts are delivered (Email, Webhook, etc.). Configure once, use everywhere.

---

## 9. Diff Agent & Agent2 (Zabbix Agent vs Zabbix Agent 2)

This is important - there are two agents.

**Zabbix Agent (Classic / "agent"):**
- Written in C language.
- Multiple separate processes (one for passive checks, etc.).
- Supports many items out of the box.
- Custom modules in C.
- Runs on almost all platforms (old Linux, Windows XP+, AIX, Solaris, etc.).
- Daemonizes easily.
- Good for basic monitoring.
- Configuration file: zabbix_agentd.conf

**Zabbix Agent 2 (Newer, recommended for most new installs):**
- Written mainly in Go language (some C parts).
- Single process with threads (more efficient).
- Built-in plugins for advanced monitoring: Docker, MySQL, PostgreSQL, Redis, systemd, Memcached, and more (native, no external scripts needed).
- On Windows: Better support for some checks (HTTPS, LDAP services).
- Plugins in Go (easier to extend sometimes).
- Uses systemd on Linux for daemon.
- Configuration: zabbix_agent2.conf
- Requires newer Go runtime on some systems.
- Faster development of new features.

**Key Differences Summary:**
- Performance: Agent2 often better (single process, async in newer Zabbix).
- Features: Agent2 has more built-in modern plugins.
- Compatibility: Classic agent supports more old/exotic OS.
- Resources: Agent2 might use slightly more memory but is modern.
- Encryption: Both support PSK/TLS, but details differ slightly (Agent2 has some limitations on crypto libs).

**Recommendation from learning:**
- Use Agent2 for new Linux/Windows servers (better plugins).
- Use classic Agent only if you need support for very old systems or specific modules.
- In config, the keys are similar, but some item keys are Agent2-only.
- Both can be used in same Zabbix environment.

In our setup, we used both depending on the host.

---

## 10. Other Important Things Learned

- **Encrypting connections** (already detailed with PSK).
- **Problem acknowledgment** (detailed earlier).
- **Webpage monitoring** (detailed).
- **Generate utilization reports** (detailed).
- **Manage dashboards** (detailed).
- **SLA & Schedule report** (detailed).
- **Mediatype** (detailed).
- **Diff Agent & Agent2** (detailed above).

**Additional Best Practices:**
- Always use templates instead of creating items manually.
- Use host groups and tags for organization.
- Set up maintenance periods for planned work.
- Regularly review triggers - avoid alert fatigue (too many false alerts).
- Backup Zabbix database and configs.
- Monitor the Zabbix server and proxies themselves!
- Use API for automation (scripts to add hosts in bulk).
- Keep versions consistent.
- Document everything (like these notes).

---

## 11. Summary of My 6-Month Learning at I2K2

- Started with infrastructure zones and IPs + proxies locations.
- Learned Zabbix from basics: what, why, features, versions (3.0 → 5.4.3 → 6.0).
- Installed full stack: server + frontend on local network (with guidance).
- Installed agents on Linux (CentOS, Debian, Ubuntu) and Windows.
- Installed and configured proxies for different zones, with agents routing through them.
- Mastered configuration: hosts, groups, users, permissions, templates, items, triggers, thresholds.
- Used SNMP for network devices.
- Set up secure PSK encryption.
- Web monitoring for sites.
- Dashboards, reports (utilization, SLA, scheduled).
- Alerts: media types, problem handling, integration with other tools (Ace Pelican, Nagios).
- Handled disabling servers and notifications.
- Understood differences between classic agent and agent 2.

This covers **maximum things** from the original list, explained from basic level in simple English with practical steps and tips.

**Next Steps Suggestion:**
- Practice in a test environment.
- Upgrade to Zabbix 7.0 when ready (new features like better HA proxies, synthetic browser monitoring).
- Explore Zabbix API and automation.
- Create your own custom templates and dashboards.

If you need more details on any section, specific config examples, or screenshots (in real life), just ask!

---

*These notes are comprehensive, self-contained, and ready for reference or sharing with new team members. All points from the original request have been captured and expanded.*