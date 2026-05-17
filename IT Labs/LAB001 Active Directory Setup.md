# LAB-001: Active directory setup

**Date:** 2026-05-17
**Difficulty:** Beginner
**Estimated Time:** 2 hours
**Status:** Completed

---

## Objective

Set up a functional Active Directory environment by promoting Windows Server 2022 to a Domain Controller. This lab simulates a small corporate network and covers the core AD skills required in IT Help Desk roles.

---

## Environment
| Component      | Details                                      |
| -------------- | -------------------------------------------- |
| Server OS      | Windows Server 2022                          |
| Client OS      | Windows 11 Enterprise (VM - 90 day evaluation) |
| Virtualization | VirtualBox                                   |
| Network Mode   | Bridge                                       |
| Server IP      | 192.168.1.10                                 |
| Domain Name    | corp.local                                   |

---

## Steps Performed

### 1. Static IP Configuration

A static IP is required on the server because if the IP changes after 
configuration, all services depending on it will stop working. Active Directory 
and DNS are tied to this address — a change would break the entire domain.

**Configuration applied:**

| Setting         | Value               |
| --------------- | ------------------- |
| IP Address      | 192.168.1.10        |
| Subnet Mask     | 255.255.255.0 (/24) |
| Default Gateway | 192.168.1.1         |
| Preferred DNS   | 192.168.1.10        |

![configuracion de red](IT%20Labs/screenshots/configuracion%20de%20red.png)

The DNS was set to the server's own IP because after promoting it to Domain 
Controller, AD installs its own DNS service. The server needs to point to 
itself to resolve domain queries correctly.

After this configuration ping was executed to confirm network connectivity.

---

### 2. AD DS Role Installation

Active Directory Domain Services (AD DS) is the role that enables the server 
to manage users, groups, and policies across the network. Without this role, 
the server is just a regular Windows Server with no directory capabilities.

The role was installed through Server Manager using the Add Roles and Features 
wizard. During installation, the wizard prompted to add additional required 
features — these were accepted and installed alongside AD DS.

![wizard select](IT%20Labs/assets/screenshots/wizard%20select.png)

The service AD DS was added correctly.

![complete AD DS](IT%20Labs/assets/screenshots/complete%20AD%20DS.png)

---

### 3. Domain Controller Configuration

After the AD DS role was installed, Server Manager displayed a notification 
requiring additional configuration. The AD DS Configuration Wizard was launched 
to promote the server to a Domain Controller.

**Deployment Configuration**

Add a new forest was selected since this is a fresh environment with no existing 
domain infrastructure. The root domain name was set to corp.local

![new forest](IT%20Labs/assets/screenshots/new%20forest.png)

**Domain Controller Options**

The forest and domain functional levels were set to Windows Server 2016. 
DNS Server and Global Catalog were enabled automatically. A DSRM password 
was configured. This is an emergency recovery password used to restore 
Active Directory if the domain becomes unavailable.

![functional level and DSRM](IT%20Labs/assets/screenshots/functional%20level%20and%20DSRM.png)

**DNS Options**

A warning appeared stating that a DNS delegation could not be created because 
no authoritative parent zone was found. This is expected behavior in a lab 
environment and was ignored.

![Dns options](IT%20Labs/assets/screenshots/Dns%20options.png)

**Additional Options**

The NetBIOS name was automatically assigned as CORP based on the domain name 
corp.local. This is the short name used for legacy authentication — users can 
log in as either CORP\username or username@corp.local.

![NETBios domain name](IT%20Labs/assets/screenshots/NETBios%20domain%20name.png)

**Paths**

All AD DS database paths were left at their default locations:

- Database folder: C:\Windows\NTDS
- Log files folder: C:\Windows\NTDS
- SYSVOL folder: C:\Windows\SYSVOL

![path](IT%20Labs/assets/screenshots/path.png)

**Review Options**

A full summary of the configuration was reviewed before proceeding with 
the installation. Key settings confirmed:
- First Domain Controller in a new forest
- Domain name: corp.local
- NetBIOS name: CORP
- Forest and Domain Functional Level: Windows Server 2016
- Global Catalog: Yes
- DNS Server: Yes

![review options](IT%20Labs/assets/screenshots/review%20options.png)

After installation completed the server restarted automatically. Upon login, 
the screen displayed CORP\Administrator confirming the server was successfully 
promoted to Domain Controller. Server Manager now shows AD DS and DNS as 
active roles.

![AD DS and DNS set](IT%20Labs/assets/screenshots/AD%20DS%20and%20DNS%20set.png)

---

### 4. OU Structure and User Creation

Three Organizational Units were created under corp.local to simulate a 
real corporate department structure. Each user was placed in their 
corresponding department OU.

**OU Structure created:**

| OU      | User          |
| ------- | ------------- |
| IT      | John J. Patel |
| HR      | Jane Alicante |
| Finance | Bob Hallen    |

![user IT](IT%20Labs/assets/screenshots/user%20IT.png)

![user HR](IT%20Labs/assets/screenshots/user%20HR.png)

![user Finance](IT%20Labs/assets/screenshots/user%20Finance.png)

---

### 5. Account Management — Simulated Help Desk Tickets

**Ticket #001 — Password Reset**

- **User:** Jane Alicante
- **Department:** HR
- **Issue:** User locked out after multiple failed login attempts
- **Action:** Located user in HR OU → Right-click → Reset Password → 
  Set temporary password → Enabled "User must change password at next logon"
- **Result:** Account unlocked, temporary password issued
- **Status:** Resolved

![reset password jane alicante](IT%20Labs/assets/screenshots/reset%20password%20jane%20alicante.png)

**Ticket #002 — Disable Account**

- **User:** Bob Hallen
- **Department:** Finance
- **Issue:** Employee left the company, account must be disabled immediately
- **Action:** Located user in Finance OU → Right-click → Disable Account
- **Result:** Account disabled, user can no longer authenticate to the domain
- **Status:** Resolved

![Bob disable account](IT%20Labs/assets/screenshots/Bob%20disable%20account.png)

**Ticket #003 — Move User Between OUs**

- **User:** John J. Patel
- **Department:** IT (transferred to Finance)
- **Issue:** Employee transferred departments, must be moved to correct OU
- **Action:** Located user in IT OU → Right-click → Move → Selected Finance OU
- **Result:** User moved successfully
- **Status:** Resolved

![john was moved of OU](IT%20Labs/assets/screenshots/john%20was%20moved%20of%20OU%20.png)

---

### 6. Joining Windows 11 Enterprise VM to the Domain

Before joining the domain, the DNS server on the Windows 11 VM was manually 
set to point to the Domain Controller at 192.168.1.10. This is required so 
the client can locate and resolve the corp.local domain.

![domain server set](IT%20Labs/assets/screenshots/domain%20server%20set.png)

This was confirmed by running ipconfig /all in the command prompt, verifying 
that the DNS Server field shows 192.168.1.10.

![Confirmation domain server set](IT%20Labs/assets/screenshots/Confirmation%20domain%20server%20set.png)

The computer was then joined to the domain through System Properties → 
Computer Name → Change. The computer name was set to Windowsclient and 
the domain corp.local was entered.

![set domain](IT%20Labs/assets/screenshots/set%20domain.png)

After entering the Administrator credentials, a confirmation message appeared 
welcoming the computer to the corp.local domain. The VM was restarted to 
apply the changes.

![domain connected](IT%20Labs/assets/screenshots/domain%20connected.png)

After the restart, login was performed using the domain user Jane Alicante, 
confirming that domain authentication is working correctly end-to-end.

![set jane alicante account](IT%20Labs/assets/screenshots/set%20jane%20alicante%20account%20.png)

---

## What I Learned

This lab was my first hands-on experience with Active Directory. I learned what 
Active Directory actually is and why it exists, what AD DS does as a role on a 
server, and how to install and configure it. Creating the OU structure made me 
understand how real companies organize their users and why that structure matters 
when applying security policies.

The account management part connected directly to my Security+ knowledge. 
Concepts like least privilege, account lifecycle, and access control are 
things I studied for the exam — but resetting passwords, disabling accounts, 
and moving users between OUs showed me what those concepts look like in 
practice on a daily Help Desk job.

I also got more comfortable working with virtual machines and applying my 
networking knowledge in a real scenario — configuring static IPs, setting 
DNS manually, and verifying connectivity between machines before joining 
the domain. Seeing how everything depends on the network being correctly 
configured before anything else works was a valuable lesson.

---

## References

- [How to configure static IP address - YouTube](https://www.youtube.com/watch?v=HL5gEHqHk4A)
- [Install and Configure AD DS - Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-a-new-windows-server-2012-active-directory-forest--level-200-)
- [Create OUs and Users - YouTube](https://www.youtube.com/watch?v=3Yfk5Wk3WU8)
- [Account Management - YouTube](https://www.youtube.com/watch?v=3ImfaiYpbBM)
- [How to join a computer to Windows Server - YouTube](https://www.youtube.com/watch?v=2_Vtv6FNusY)