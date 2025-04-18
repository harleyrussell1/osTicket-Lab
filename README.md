osTicket Installation & Configuration Lab (Azure VM)

Overview

This lab walks through installing and configuring the open-source support ticketing system, **osTicket**, on a Windows 10 virtual machine hosted in Azure. It includes the full environment setup, software configuration, and ticket workflow simulation.

Environment Setup

Create Azure Virtual Machine

- **Name:** `osticket-vm`
- **OS:** Windows 10
- **vCPUs:** 4
- **Login:** Use secure credentials for the administrator account

Access the VM

- Log into the VM via Remote Desktop (RDP)

Software Installation

Prepare Installation Files

- Download and unzip `osTicket-Installation-Files.zip` to your desktop as `osTicket-Installation-Files`

Enable IIS with CGI Support

- Enable via Windows Features:
  - `World Wide Web Services → Application Development Features → [✓] CGI`

Install Prerequisites

From the `osTicket-Installation-Files` folder:

1. **PHP Manager for IIS** (`PHPManagerForIIS_V1.5.0.msi`)
2. **URL Rewrite Module** (`rewrite_amd64_en-US.msi`)
3. Create directory: `C:\PHP`
4. Extract **PHP 7.3.8** (`php-7.3.8-nts-Win32-VC15-x86.zip`) into `C:\PHP`
5. Install **Visual C++ Redistributable** (`VC_redist.x86.exe`)
6. Install **MySQL 5.5.62** (`mysql-5.5.62-win32.msi`)
   - Setup: Typical → Standard Configuration

IIS + PHP Configuration

1. Open **IIS Manager** as Administrator
2. Register PHP with **PHP Manager**: `C:\PHP\php-cgi.exe`
3. Restart IIS

Deploy osTicket

Application Setup

1. Extract `osTicket-v1.15.8.zip`
2. Copy the `upload` folder into `C:\inetpub\wwwroot`
3. Rename `upload` to `osTicket`

Verify Installation Site

- Open IIS → Sites → Default Web Site → `osTicket`
- Select "Browse *:80"

Enable Required PHP Extensions

In IIS → PHP Manager → "Enable or disable an extension":

- `php_imap.dll`
- `php_intl.dll`
- `php_opcache.dll`

Configuration File

- Rename:
  - From: `C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php`
  - To: `C:\inetpub\wwwroot\osTicket\include\ost-config.php`
- Set Permissions:
  - Disable inheritance
  - Remove existing permissions
  - Add `Everyone` with Full Control

## MySQL Database Setup

1. Install **HeidiSQL**
2. Connect using root credentials
3. Create a database named: `osTicket`

Finalize osTicket Setup

1. Navigate to `http://localhost/osTicket`
2. Complete the web-based setup:
   - Helpdesk Name
   - Default Email
   - MySQL DB Name: `osTicket`
   - Root credentials
3. Click **Install Now**

Access Points

- **Admin Panel:** [http://localhost/osTicket/scp/login.php](http://localhost/osTicket/scp/login.php)
- **User Portal:** [http://localhost/osTicket](http://localhost/osTicket)

Post-Installation Configuration

- Delete: `C:\inetpub\wwwroot\osTicket\setup`
- Set config file to read-only:  
  `C:\inetpub\wwwroot\osTicket\include\ost-config.php`

Configure osTicket

Roles

- Admin Panel → Agents → Roles → Add:  
  `Supreme Admin`

Departments

- Admin Panel → Agents → Departments → Add:  
  `SysAdmins`

Teams

- Admin Panel → Agents → Teams → Add:  
  `Online Banking`

User Access Settings

- Admin Panel → Settings → User Settings  
  - Uncheck: "Require registration and login to create tickets"

Agents

- Admin Panel → Agents → Add:
  - `Jane` (Dept: SysAdmins)
  - `John` (Dept: Support)

Users

- Agent Panel → Users → Add:
  - `Karen`
  - `Ken`

SLA Configuration

- Admin Panel → Manage → SLA:
  - **Sev-A:** 1 hour, 24/7
  - **Sev-B:** 4 hours, 24/7
  - **Sev-C:** 8 hours, Business Hours

Help Topics

- Admin Panel → Manage → Help Topics:
  - Business Critical Outage
  - Personal Computer Issues
  - Equipment Request
  - Password Reset
  - Other

Ticket Simulation Scenarios

Ticket 1: Online Banking Outage

- **End User:** Create a ticket for "entire mobile/online banking system is down"
- **Agent (John):**
  - Review properties
  - Assign SLA: **Sev-A**
  - Assign Dept: **Online Banking**
- **Agent (Jane):** Resolve the ticket

Ticket 2: Adobe Upgrade Issue

- **End User:** "accounting department needs adobe upgrade, broken"
- **Agent (John):**
  - Assign SLA: **Sev-B**
  - Assign Dept: **Support**
  - Resolve the ticket

Ticket 3: CFO Laptop Failure

- **End User:** "CFO’s laptop will no longer turn on"
- **Agent (John):**
  - Assign SLA: **Sev-B**
  - Assign Dept: **Support**
  - Resolve the ticket

Access Control Workflow

- Change `SysAdmins` to a top-level department
- Delete `Maintenance` department
- Assign ticket to `SysAdmins` (Sev-A)
- Verify Agent `John` cannot view the ticket
- Grant John access to `SysAdmins` via Admin Panel
- Observe restored visibility and escalated access

Ticketing System Best Practices

- Most systems email users automatically on ticket updates
- Tickets may originate from email, chat, web forms, or in-person requests
- Always create tickets for tasks completed to ensure tracking and performance metrics
