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

<p>
<img width="2548" height="1462" alt="Screenshot 2026-02-12 144848" src="https://github.com/user-attachments/assets/80bb37a8-ce69-44e6-bcd5-b841f01fd2f9" />
</p>

<p>
<img width="2551" height="1464" alt="Screenshot 2026-02-12 145224" src="https://github.com/user-attachments/assets/95a06208-63d2-444f-839b-74988f33a8e3" />
</p>

* Create a Windows Server 2022 VM named "dc-1".
* Configure its private IP address as "static".
* Create a Windows 10 VM named "client-1".
* Place both VMs in the same Resource Group and Virtual Network (VNet).
* Confirm network visibility between machines.

---

## Validate Network Connectivity

<p>
<img width="1279" height="759" alt="Screenshot RD2" src="https://github.com/user-attachments/assets/4a69f6a2-c2b2-41cc-bbb5-a8bb24040461" />
</p>

<p>
<img width="850" height="512" alt="Screenshot RD1" src="https://github.com/user-attachments/assets/68e68a0a-cc6a-498e-983b-c8757a9e3a49" />
</p>

<p>
<img width="1279" height="759" alt="Screenshot RD3" src="https://github.com/user-attachments/assets/1e561389-765b-4cfb-b15c-77a7a74125b6" />
</p>

* Use Remote Desktop to access Client-1 and dc-1.
* Disable Windows Firewall on DC-1.
* Ping to DC-1’s private IP.
* Confirm successful communication.

This ensures proper internal routing before domain configuration.

---

## Install and Configure Active Directory

<p>
<img width="1280" height="763" alt="Screenshot RD4" src="https://github.com/user-attachments/assets/ac53651b-df16-4a8f-a53f-6f8b52907a3c" />
</p>

<p>
<img width="1280" height="763" alt="Screenshot RD5" src="https://github.com/user-attachments/assets/86914c85-23c6-4516-b7a0-085ad290325e" />
</p>

* Install the AD DS role on dc-1.
* Promote dc-1 to a Domain Controller.
* Create a new forest (example: mydomain.com).
* Restart and log in using domain credentials.

At this point, dc-1 functions as the central authentication server.

---

## Create Organizational Units & Admin Account

<p>
<img width="1280" height="770" alt="Screenshot RD6" src="https://github.com/user-attachments/assets/b00a358c-7438-41eb-8e72-222e41a59138" />
</p>

Using Active Directory Users and Computers:

Create three OUs:

* "_EMPLOYEES"
* "_ADMINS"
* "_CLIENTS"

<p>
<img width="1280" height="770" alt="Screenshot RD7" src="https://github.com/user-attachments/assets/6ebfce3e-696e-48b7-83f6-53e1c54cc557" />
</p>

<p>
<img width="1280" height="772" alt="Screenshot RD8" src="https://github.com/user-attachments/assets/48f784e6-622e-479f-a084-1f08b630f8dc" />
</p>

Create an administrative account:

* Username: "jane_admin"
* Add to the "Domain Admins" group

Switch to this account for ongoing administrative tasks.

---

## Join Client Machine to Domain

<p>
<img width="1280" height="772" alt="Screenshot RD9" src="https://github.com/user-attachments/assets/df7a4404-f770-4343-8120-82bc236c6934" />
</p>

<p>
<img width="1280" height="772" alt="Screenshot RD10" src="https://github.com/user-attachments/assets/b151d051-34a3-4bc2-9b2d-a7e16d294489" />
</p>

* Join the machine to "mydomain.com".
* Restart again.
* Verify it appears in Active Directory.
* Move it into the "_CLIENTS" OU.

---

## Automate User Creation with PowerShell

<p>
<img width="1280" height="772" alt="Screenshot RD11" src="https://github.com/user-attachments/assets/98740df0-d293-4de5-bd3c-fb8e36f8a4b4" />
</p>

<p>
<img width="1280" height="772" alt="Screenshot RD12" src="https://github.com/user-attachments/assets/f60b9bb3-c982-4193-bb90-aa11e99bdb92" />
</p>

<p>
<img width="2535" height="1458" alt="Screenshot 2026-02-12 170941" src="https://github.com/user-attachments/assets/b460fdb3-36cb-4df6-9f70-a041b678aacc" />
</p>

* Log into DC-1 as "jane_admin".
* Open PowerShell ISE as Administrator.
* Execute a script to bulk-create user accounts.
* Confirm new users appear in the correct OU.
* Test login from Client-1 with a newly created user.

---

## Configure Remote Desktop via Group Policy

<p>
<img width="1280" height="772" alt="Screenshot RD13" src="https://github.com/user-attachments/assets/b3e2a1fd-a207-4118-8a76-e59a0788df9f" />
</p>

<p>
<img width="1280" height="772" alt="Screenshot RD14" src="https://github.com/user-attachments/assets/d29922ae-01ca-4975-afb9-e761c9951300" />
</p>

Create and link a Group Policy Object (GPO) to:

* "_CLIENTS"
* "_EMPLOYEES"

Configure the policy to lockout users after a certain amount of login attempts.

Force policy update with:

 - "gpupdate /force"


This centralizes the account lockout policy.

---


---

# Skills Demonstrated

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
