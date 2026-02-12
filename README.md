<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

* Deployed virtual machines in Azure
* Installed and configured "Active Directory Domain Services"
* Created Organizational Units (OUs) and user accounts
* Joined a Windows client to the domain
* Configured Remote Desktop access through Group Policy
* Automated user account creation with PowerShell

# Active Directory Domain Deployment Lab (Azure)

This repository outlines a complete walkthrough of deploying and configuring a Windows domain environment in the cloud using **Microsoft Azure**.

The project demonstrates how to build a Domain Controller, join a client machine to the domain, configure Group Policy, and automate user creation using PowerShell — simulating a real-world enterprise setup.

---

# Environment & Tools

* **Microsoft Azure** (Virtual Machines & Networking)
* Remote Desktop (RDP)
* **Windows Server 2022** (Domain Controller)
* **Windows 10** (Client – 21H2)

---

# Deployment Overview

## Provision Infrastructure in Azure

* Create a Windows Server 2022 VM named "DC-1".
* Configure its private IP address as "static".
* Create a Windows 10 VM named "Client-1".
* Place both VMs in the same Resource Group and Virtual Network (VNet).
* Confirm network visibility between machines.

---

## Validate Network Connectivity

* Use Remote Desktop to access Client-1.
* Run a continuous ping to DC-1’s private IP.
* Enable ICMP in Windows Firewall on DC-1.
* Confirm successful communication.

This ensures proper internal routing before domain configuration.

---

## 🏢 3️⃣ Install and Configure Active Directory

* Install the AD DS role on DC-1.
* Promote DC-1 to a Domain Controller.
* Create a new forest (example: `mydomain.com`).
* Restart and log in using domain credentials.

At this point, DC-1 functions as the central authentication server.

---

## 👥 4️⃣ Create Organizational Units & Admin Account

Using Active Directory Users and Computers:

Create three OUs:

* `_EMPLOYEES`
* `_ADMINS`
* `_CLIENTS`

Create an administrative account:

* Username: `jane_admin`
* Add to the **Domain Admins** group

Switch to this account for ongoing administrative tasks.

---

## 💻 5️⃣ Join Client Machine to Domain

* Configure Client-1’s DNS to point to DC-1.
* Restart the client VM.
* Join the machine to `mydomain.com`.
* Restart again.
* Verify it appears in Active Directory.
* (Optional) Move it into the `_CLIENTS` OU.

---

## 🛠 6️⃣ Configure Remote Desktop via Group Policy

Create and link a Group Policy Object (GPO) to:

* `_CLIENTS`
* `_EMPLOYEES`

Configure the policy to allow domain users Remote Desktop access.

Force policy update with:

```bash
gpupdate /force
```

This centralizes RDP permissions instead of configuring each device individually.

---

## ⚡ 7️⃣ Automate User Creation with PowerShell

* Log into DC-1 as `jane_admin`.
* Open PowerShell ISE as Administrator.
* Execute a script to bulk-create user accounts.
* Confirm new users appear in the correct OU.
* Test login from Client-1 with a newly created user.

---

# 🎯 Skills Demonstrated

* Cloud VM deployment in Azure
* Virtual network configuration
* Active Directory installation and promotion
* OU design and account management
* Domain joining process
* Group Policy management
* PowerShell scripting and automation
* Centralized authentication testing

---

# ✅ Outcome

By completing this project, you now have:

* A fully functional Domain Controller
* A domain-joined client machine
* Structured Organizational Units
* Admin and standard user accounts
* Centralized Remote Desktop configuration
* Automated user provisioning

This repository reflects practical system administration experience and demonstrates foundational skills required for IT support and entry-level system administrator roles.
