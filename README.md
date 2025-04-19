# osTicket Installation & Configuration Lab (Azure VM)

## Overview
This lab walks through the installation and configuration of the open-source support ticketing system, **osTicket**, on a Windows 10 virtual machine hosted in **Microsoft Azure**. It covers environment setup, server configuration, and simulated ticket workflow to demonstrate IT service management in practice.

## Technologies Used
- Microsoft Azure (VM deployment)
- Windows 10 Virtual Machine
- IIS (Internet Information Services)
- PHP 7.3.8
- MySQL 5.5.62
- osTicket v1.15.8
- HeidiSQL
- DNS and URL routing via localhost

---

## Environment Setup

### Create Azure VM
- **Name:** osticket-vm  
- **OS:** Windows 10  
- **vCPUs:** 4  
- **Authentication:** Secure admin credentials  
- Accessed via Remote Desktop (RDP)

### Software Installation

#### Prepare Installation Files
- Downloaded and unzipped `osTicket-Installation-Files.zip` to Desktop

#### Enable IIS with CGI Support
- Enabled via Windows Features:
  - World Wide Web Services → Application Development Features → CGI

#### Install Prerequisites
- PHP Manager for IIS  
- URL Rewrite Module  
- Visual C++ Redistributable (VC_redist.x86.exe)  
- Created folder `C:\PHP` and extracted PHP 7.3.8  
- Installed MySQL 5.5.62 (Typical > Standard Configuration)

---

## IIS + PHP Configuration
- Opened IIS Manager
- Registered PHP with PHP Manager (`php-cgi.exe`)
- Restarted IIS

---

## Deploy osTicket

### Application Setup
- Extracted `osTicket-v1.15.8.zip`
- Copied `upload` folder to `C:\inetpub\wwwroot` and renamed it to `osTicket`
- Verified site loads at `http://localhost/osTicket`

### Enable Required PHP Extensions
- Enabled in PHP Manager:
  - `php_imap.dll`
  - `php_intl.dll`
  - `php_opcache.dll`

### Configuration File
- Renamed `ost-sampleconfig.php` → `ost-config.php`
- Set permissions:
  - Disabled inheritance
  - Removed existing users
  - Granted `Everyone` full control

---

## MySQL Setup
- Installed HeidiSQL  
- Connected as root  
- Created new database: `osTicket`

---

## Finalize osTicket Setup
- Navigated to `http://localhost/osTicket`
- Completed installer with:
  - Helpdesk Name
  - Admin Email
  - MySQL DB: `osTicket`
  - Root credentials
- Clicked **Install Now**

### Access Points
- Admin Panel: `http://localhost/osTicket/scp/login.php`
- User Portal: `http://localhost/osTicket`

---

## Post-Installation Configuration

### Secure Setup
- Deleted `C:\inetpub\wwwroot\osTicket\setup`
- Set `ost-config.php` to **read-only**

### System Configuration

#### Roles
- Supreme Admin

#### Departments
- SysAdmins

#### Teams
- Online Banking

#### User Access Settings
- Disabled: "Require registration and login to create tickets"

#### Agents
- Jane (SysAdmins)
- John (Support)

#### Users
- Karen
- Ken

#### SLA Policies
- **Sev-A:** 1 hour (24/7)
- **Sev-B:** 4 hours (24/7)
- **Sev-C:** 8 hours (Business Hours)

#### Help Topics
- Business Critical Outage
- Personal Computer Issues
- Equipment Request
- Password Reset
- Other

---

## Ticket Simulation Scenarios

### Ticket 1: Online Banking Outage
- **End User:** Reports full online banking outage  
- **Agent John:**
  - SLA: Sev-A
  - Dept: Online Banking  
- **Agent Jane:** Resolves ticket

### Ticket 2: Adobe Upgrade Issue
- **End User:** Reports broken Adobe install in accounting  
- **Agent John:**
  - SLA: Sev-B
  - Dept: Support
  - Resolves ticket

### Ticket 3: CFO Laptop Failure
- **End User:** CFO laptop won’t boot  
- **Agent John:**
  - SLA: Sev-B
  - Dept: Support
  - Resolves ticket

---

## Access Control Workflow
- Promoted SysAdmins to top-level department  
- Deleted Maintenance department  
- Assigned ticket to SysAdmins (Sev-A)  
- Verified Agent John could not view the ticket  
- Granted John access to SysAdmins  
- Confirmed restored visibility

---

## Skills & Experience Gained
- **Cloud VM Deployment:** Built and connected to an Azure-hosted Windows 10 machine
- **Web Stack Configuration:** Installed and configured IIS, PHP, and MySQL for osTicket
- **ITSM Simulation:** Ran realistic help desk workflows with SLA prioritization and ticket escalation
- **User Role & Access Control:** Practiced department/team-based visibility and permissions
- **Database Administration:** Used HeidiSQL to create and manage MySQL databases
- **System Hardening:** Secured installation through permission adjustments and cleanup

## Screenshots

All screenshots related to this lab can be found in the [`/screenshots`](./screenshots) folder.
