
# Active Directory Home Lab
 
A comprehensive hands-on lab project demonstrating core Active Directory administration, group policy management, and network troubleshooting skills—essential for IT Help Desk and system administration roles.
 
---
 
## Project Overview
 
This lab simulates a small corporate network environment using Windows Server 2022 as a Domain Controller and Client OS | Windows 11 Enterprise (VM - 90 day evaluation) as a client machine. The project covers fundamental AD operations that IT Help Desk professionals handle daily: user management, access control, policy deployment, and basic network diagnostics.
 
**Why this project matters:** IT Help Desk roles heavily rely on Active Directory skills. This lab demonstrates you understand user provisioning, account troubleshooting, and group policy application—key responsibilities in any Windows environment.
 
---
 ## Block 1 — Active Directory Setup 

**Skills demonstrated:**
- Promoted Windows Server 2022 to Domain Controller
- Created domain corp.local with OU structure (IT, HR, Finance)
- Provisioned users and assigned them to department OUs
- Performed Help Desk account management: password reset, disable account, move users between OUs
- Joined Windows 11 Enterprise VM to the domain and verified end-to-end authentication

 [View Lab Write-up](ITLabs/LAB001%20Active%20Directory%20Setup.md)

---

## Block 2 — Group Policy Management (GPO) 

**Skills demonstrated:**
- Created and linked GPOs to specific OUs and domain level
- Configured user restrictions using Administrative Templates (Block Control Panel)
- Enforced corporate desktop wallpaper across domain users
- Applied password security policies domain-wide
- Verified GPO application using gpupdate /force on client machine

 [View Lab Write-up](ITLabs/LAB002%20Group%20Policy%20Management%20(GPO).md)

---

## Block 3 — Network Troubleshooting 

**Skills demonstrated:**
- Network configuration review using ipconfig /all
- Connectivity testing using ping by IP and by domain name
- DNS resolution verification using nslookup
- Internet connectivity and latency analysis using tracert
- Identifying acceptable vs problematic latency values
- Understanding of ICMP timeout behavior in tracert results

 [View Lab Write-up](ITLabs/LAB003%20Network%20Troubleshooting.md)

 ## Block 4 — Printer Management 

**Skills demonstrated:**
- Installation of Print and Document Services role on Windows Server 2022
- Adding and configuring a network printer using Print Management Console
- Selecting appropriate printer drivers for network environments
- Sharing a printer across a Windows domain
- Restricting printer access to specific domain users
- Verifying printer connectivity from a domain-joined client machine

[View Lab Write-up](ITLabs/LAB004%20Printer%20Management.md)
 
  ## Technologies & Tools that will be use
 
| Component | Technology |
|-----------|-----------|
| **Virtualization** | VirtualBox |
| **Server OS** | Windows Server 2022 |
| **Client OS** | Client OS | Windows 11 Enterprise (VM - 90 day evaluation) |
| **Directory Service** | Active Directory Domain Services (AD DS) |
| **Management Tools** | Group Policy Editor, Active Directory Users & Computers | Print Management Console |
| **Scripting** | PowerShell |
| **Network Tools** | ping, nslookup, ipconfig, tracert |
| Printer Management | Microsoft Print to PDF (virtual printer), Windows Print Server |
