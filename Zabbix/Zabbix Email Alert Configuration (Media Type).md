>Goal: Configure Zabbix from scratch to monitor a Linux host and receive Problem and Recovery email notifications.


______________________________________

#### Architecture

Linux Host (Zabbix Agent)
        │
        ▼
Zabbix Server
        │
        ▼
Trigger
        │
        ▼
Action
        │
        ▼
User Media
        │
        ▼
SMTP Email
        │
        ▼
Inbox


_________________________________

**Prerequisites**
1. Zabbix Server installed
2. Zabbix Frontend accessible
3. Ubuntu/Linux host
4. Zabbix Agent installed
5. SMTP server details
6. Email account for notifications
   
**Overall Workflow**

1. A[Create Host Group]
2. B[Add Host]
3. C[Install & Configure Agent]
4. D[Link Template]
5. E[Verify Latest Data]
6. F[Create Trigger]
7. G[Configure Email Media Type]
8. H[Create User Group]
9. I[Create User]
10. J[Add Email Media]
11. K[Create Action]
12. L[Test CPU Load]
13. M[Problem Generated]
14. N[Email Received]
15. O[Recovery Email]
___________________________________


## Step 1 - Create a Host Group
A Host Group in Zabbix is a logical container used to organize hosts (servers, VMs, network devices, applications, etc.). It does not monitor anything by itself; instead, it helps you manage and control multiple hosts efficiently.

**Navigation**
```
Configuration
    └── Host groups
            └── Create host group
```

**Fill the following fields**

Field                                                 	Value
Host group Name=	                       Linux Servers/Windows Server/(custom)

Click **Add**  

>Shown in SS1,02
>![[Cybersecurity-notes/Zabbix/SS1.png]]
>![[ss02 1.png]]



__________________________________________

## Step 2 - Add a Host

**Navigation**
```
Configuration
    └── Hosts
            └── Create host
```

| Field        | Example                                                                  |
| ------------ | ------------------------------------------------------------------------ |
| Host name    | `wsrv-445` ,`lserver229`/ *(Custom)*                                     |
| Visible name | `wsrv-445` / *(Custom)*                                                  |
| Host groups  | `Linux Servers`, `Windows Servers`, *(Custom)* (multiple groups allowed) |
| Templates    | `Linux by Zabbix Agent`, `Windows by Zabbix Agent`                       |

**On Interfaces Click Add**

| Field      | Value            |
| ---------- | ---------------- |
| Type       | `Agent`          |
| Connect to | `IP`             |
| IP Address | `192.168.44.130` |
| Port       | `10050`          |

Click **Add**

>Shown in SS2
![[Cybersecurity-notes/Zabbix/SS2.png]]


## Step 3 – Add Trigger
A trigger checks the collected data and decides whether the host is in a normal (OK) state or a problem (PROBLEM) state.
### Navigation
```
Configuration
    └── Hosts
            └── Triggers
                    └── Create trigger
```

 >[OR YOU CAN CLONE EXICTING TRIGGER]


>Shown in SS3
>![[Cybersecurity-notes/Zabbix/SS3.png]]
---

### Trigger Tab
>Shown in SS4
>![[Cybersecurity-notes/Zabbix/SS4.png]]


| Field                         | Value                                                                     |
| ----------------------------- | ------------------------------------------------------------------------- |
| Name                          | Memory utilization is higher than 70% - call to client                    |
| Event name                    | memory utilization is higher than 70% (directly inform to client on call) |
| Operational data              | Current memory utilization is {ITEM.LASTVALUE}                            |
| Severity                      | High                                                                      |
| Expression                    | last(/sr-44593-vm01/vm.memory.util)>70                                    |
| OK event generation           | Expression                                                                |
| Problem event generation mode | Single                                                                    |
| OK event closes               | All problems                                                              |
| Allow manual close            | No                                                                        |
| URL                           | (Not set)                                                                 |
| Description                   | The system is running out of free memory.                                 |
| Status | Enabled |

 > [after adding make a test for it as shown in  SS5,6,7
 > ![[SS5 1.png]]
>![[SS6 1.png]]
>![[SS7 1.png]]

## Trigger Notes –
#### 1. Memory Utilization

##### Configuration Summary

- Trigger monitors memory usage of VM
- Host: sr-44593-vm01
- Item used: vm.memory.util
- Threshold: > 70%

##### Expression Used
last(/sr-44593-vm01/vm.memory.util)>70

##### Operational Data (Important)
Current memory utilization is {ITEM.LASTVALUE}

✔ This helps show real-time memory value in alerts

##### Behavior
- Trigger goes to PROBLEM when memory > 70%
- Returns to OK when value goes below threshold
- Event type: Single
- Manual close: Disabled

______________________________

### Disk C: Drive Usage Trigger
| Field | Value |
|------|------|
| Name | C: Drive disk usage is higher than 80% |
| Event name | C: Drive storage usage critical |
| Operational data | Current disk usage is {ITEM.LASTVALUE} |
| Severity | High |
| Expression | last(/sr-44593-vm01/vfs.fs.size[C:,pused])>80 |
| OK event generation | Expression |
| Problem event generation mode | Single |
| OK event closes | All problems |
| Allow manual close | No |
| URL | Not set |
| Description | C drive storage is running low |
| Status | Enabled |


##### Purpose
Monitors Windows C: drive disk usage and triggers alert when storage exceeds threshold.

---

##### Key Configuration
- Metric: vfs.fs.size[C:,pused]
- Threshold: > 80%
- Host: sr-44593-vm01

---

##### Expression
last(/sr-44593-vm01/vfs.fs.size[C:,pused])>80

---

##### Operational Data
Current disk usage is {ITEM.LASTVALUE}

✔ Shows real-time disk usage in alerts

---

##### Behavior
- PROBLEM → when disk usage > 80%
- OK → when disk usage drops below threshold
- Event type: Single


### Memory Utilization Trigger

##### Trigger Fields

| Field | Value |
|------|------|
| Name | Memory utilization is higher than 70% |
| Event name | Memory utilization is higher than 70% (call to client) |
| Operational data | Current memory utilization is {ITEM.LASTVALUE} |
| Severity | High |
| Expression | last(/sr-44593-vm01/vm.memory.util)>70 |
| OK event generation | Expression |
| Problem event generation mode | Single |
| OK event closes | All problems |
| Allow manual close | No |
| URL | Not set |
| Description | The system is running out of free memory |
| Status | Enabled |

---
##### Expression Used
```
last(/sr-44593-vm01/vm.memory.util)>70
```

---

##### Operational Data Purpose
- Shows real-time memory usage in problem alerts
- Helps support team quickly understand severity

Example output in alert:
```
Current memory utilization is 82%
```

---

##### Behavior

- Trigger = PROBLEM when memory > 70%
- Trigger = OK when memory < 70%
- Event type = Single



>>> We now configure **alerting system (Users + Media + Actions)**


# 1. Create User Group

## Navigation
```
Administration
    └── User groups
            └── Create user group
```

---

#### Fields

| Field      | Value                                |
| ---------- | ------------------------------------ |
| Group name | Cahal computers/Jeena sikho / Custom |

#### Permissions Tab

| Field            | Value         |
| ---------------- | ------------- |
| Host group       | Linux Servers |
| Permission level | Read          |

#### Purpose
- Controls which hosts users can see
- Required for receiving alerts

> Shown in SS8,9
> ![[Cybersecurity-notes/Zabbix/SS8.png]]

![[Cybersecurity-notes/Zabbix/SS9.png]]

_________________________________


# 2. Create User

## Navigation
```
Administration
    └── Users
            └── Create user
```

## User Tab

| Field    | Value                          |
| -------- | ------------------------------ |
| Username | zabbix-admin / noc-user/Custom |
| Name     | Optional                       |
| Surname  | Optional                       |
| Password | Strong password                |

## Groups Tab

- Assign user to:
  - Linux Admins / NOC Team / Custom

## Role Tab

| Field | Value                                            |
| ----- | ------------------------------------------------ |
| Role  | Always `CLIENT ROLE` User (or Admin if required) |

## Purpose
- Receives alerts
- Linked with Media (Email)

> Shown in SS10,11,12
> ![[Cybersecurity-notes/Zabbix/SS10.png]]
![[Cybersecurity-notes/Zabbix/SS11.png]]
![[Cybersecurity-notes/Zabbix/SS12.png]]





_______________________

# 3. Configure Media Type (Email)

## Navigation
```
Alerts
    └── Media types
            └── Create media type
```

---

## Email Media Type

### Fields

| Field | Value |
|------|------|
| Name | Email SMTP |
| Type | Email |

### SMTP Settings

| Field | Value |
|------|------|
| SMTP server | smtp.gmail.com |
| Port | 587 |
| Security | STARTTLS |
| Username | yourmail@gmail.com |
| Password | App Password |

## Purpose
- Defines HOW alerts are sent (Email)

> Shown in SS13,14,S2
> ![[Cybersecurity-notes/Zabbix/s2.png]]
> ![[SS13 1.png]]

![[SS14 1.png]]

_______________________

# 4. Link Media to User

## Navigation
```
Administration
    └── Users
            └── Select User
                    └── Media tab
```

## Media Tab Fields

| Field | Value |
|------|------|
| Type | Email SMTP / Telegram |
| Send to | email@example.com |
| Active period | 1-7, 00:00-24:00 |


## Severity Selection

Enable all required:

- Not classified
- Information
- Warning
- Average
- High
- Disaster
## Purpose
- Connects USER → NOTIFICATION CHANNEL

Without this step:
❌ Alerts will NOT be sent

> Shown in SS11
> ![[Cybersecurity-notes/Zabbix/SS11.png]]



# 5. Trigger Action
A Trigger Action is an automation rule that tells Zabbix what to do when a trigger generates a Problem or Recovery event.

**Navigation**
```
Alerts
    └── Actions
            └── Trigger actions
                    └── Create action

```

### Action Components
#### 1. Action Name

Provide a meaningful name that describes the purpose of the action.

**Examples:**

- Linux Server Email Alerts
- Windows Server Notifications
- Production Critical Alerts
- Zimbra Monitoring Alerts
#### 2. Conditions

Conditions determine when the action should execute.

**Common conditions include:**

| Condition    | Example                    |
| ------------ | -------------------------- |
| Event Source | `Trigger`                  |
| Host Group   | `Linux Servers`            |
| Severity     | `High`                     |
| Trigger Name | Contains `"Memory"`        |
| Tag          | `Environment = Production` |

The action runs only when all configured conditions are satisfied.

#### 3. Operations

Operations define what should happen when a problem event occurs.

**Common operations:**

- Send Email
- Send Telegram Message
- Send Slack Notification
- Execute Remote Command
- Call a Webhook

**Example:**

Operation:
Send Message

Recipient:
Linux Admins

Media Type:
Email SMTP

#### 4. Recovery Operations

Recovery Operations define what happens after the trigger returns to the OK state.

**Example:**

Send Recovery Email

Recipient:
Linux Admins

This informs administrators that the issue has been resolved.

#### 5. Update Operations (Optional)

Update Operations are executed when an event is updated, such as:

- Problem acknowledged
- Severity changed
- Tags updated

Example:

Notify Team Lead when the problem is acknowledged.


#### Example Configuration
**Action Name**
	Linux Server Email Alerts

**Conditions**
	Event Source = Trigger
	Host Group = Linux Servers
	Severity >= High

**Operations**
	Send Email

**To:**
	Linux Admins

**Media:**
	Email SMTP

**Recovery Operations**
	Send Recovery Email

**To:**
	Linux Admins

> Shown in SS15,16,17
> ![[Cybersecurity-notes/Zabbix/SS15.png]]

![[Cybersecurity-notes/Zabbix/SS16.png]]

![[Cybersecurity-notes/Zabbix/SS17.png]]