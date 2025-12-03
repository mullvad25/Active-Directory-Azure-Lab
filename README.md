# 🛠 Active Directory Lab in Azure (Windows Server 2025)

![Azure](https://img.shields.io/badge/Azure-0089D6?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Windows Server](https://img.shields.io/badge/Windows%20Server-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active%20Directory-0067C5?style=for-the-badge&logo=microsoft&logoColor=white)
![Help Desk](https://img.shields.io/badge/Help%20Desk%20Skills-4CAF50?style=for-the-badge)

This project is a hands-on Active Directory lab built entirely in Microsoft Azure.  
It recreates a real enterprise IT environment with a Windows Server 2025 Domain Controller.  
The goal is to demonstrate core IT support skills including domain creation, DNS, user management, RDP, and Azure VM administration.

---

# 🎯 Project Overview

This lab simulates a real on-premises IT environment using cloud-hosted Azure VMs.

**Completed so far:**

- Deployed a Windows Server 2025 Azure VM  
- Assigned static private IP for DC reliability  
- Installed Active Directory Domain Services  
- Promoted server to a Domain Controller  
- Created the forest/domain `lab.local`  
- Logged in using domain credentials (`lab\labadmin`)  

This environment is ideal for practising IT Support, Desktop Support, Service Desk, and junior Sysadmin tasks.

---

# ☁️ Azure Infrastructure Setup

## 1️⃣ Resource Group  
- **Name:** `AD-Lab-RG`  
- **Region:** West Europe  

![Resource Group](Screenshots/Resource%20Group.PNG)

---

## 2️⃣ Virtual Network  
- **VNet:** `AD-Lab-VNet`  
- **Subnet:** `default`  
- **Address Space:** `10.0.0.0/16`
- **Subnet Space:** `10.0.0.0/24`

![VN](Screenshots/AD-LAB-VN.PNG)

---

## 3️⃣ Windows Server 2025 Virtual Machine  
- **VM Name:** `AD-Lab-VM`  
- **Size:** `B2ls_v2`  
- **OS:** Windows Server 2025 Datacenter  
- **Inbound Ports:** RDP (3389)

![VM Overview](Screenshots/VM.PNG)

---

## 4️⃣ Network Interface + Static IP  
- Enabled static private IP to support domain controller functions  
- Ensures DNS and authentication stability  

**NIC settings:**

![NIC](Screenshots/Network%20Interface.PNG)

**Static Private IP:**

![Static IP](Screenshots/static.PNG)

---

# 🛠 Active Directory Domain Setup

## 1️⃣ Installed AD DS Role  
Using Server Manager → Add Roles and Features → Active Directory Domain Services

---

## 2️⃣ Promoted to Domain Controller  
Created a new forest:

- **Domain:** `lab.local`  
- DNS configured automatically  
- Server rebooted after installation  

**Domain Controller Confirmation:**

![Domain Controller](Screenshots/Domain%20Controller.PNG)

---

## 3️⃣ Logged in Using Domain Credentials  
Signed in as:
labadmin@LAB.LOCAL

---

# 🧠 Skills Demonstrated So Far

### 🔹 Azure Skills
- Deploying virtual machines  
- Network Interface configuration  
- Static IP assignment  
- Managing VNets and subnets  
- RDP connectivity troubleshooting  

### 🔹 Windows Server Skills
- Installing server roles  
- Managing Server Manager  
- Understanding domain controllers  
- DNS configuration  

### 🔹 Active Directory Skills
- Creating a new forest and domain  
- Promoting a server to Domain Controller  
- Logging in with domain credentials  
- Understanding domain structure  

This already demonstrates Tier 1 and Tier 2 support capabilities.

---

# 📸 Screenshots

| Description | Image |
|------------|--------|
| Azure Resource Group | ![Resource Group](Screenshots/Resource%20Group.PNG) |
| Azure VM Overview | ![VM](Screenshots/VM.PNG) |
| Network Interface | ![NIC](Screenshots/Network%20Interface.PNG) |
| Static Private IP Configuration | ![Static IP](Screenshots/static.PNG) |
| Domain Controller Confirmation | ![Domain Controller](Screenshots/Domain%20Controller.PNG) |

# 📌 Phase 2 — Active Directory Foundation Completed 

Phase 2 establishes the full Active Directory environment inside `lab.local`.  
This includes departments, users, admin accounts, service accounts, onboarding accounts, RDP permissions, and domain-level security settings.

---

## 🗂️ Organizational Units (OUs) Created

A clean, enterprise-style OU structure was created under `lab.local`:

- **_Admins**
- **_Users**
- **_Groups**
- **_Computers**
- **_Departments**
  - IT
  - HR
  - Finance
  - Sales
  - Operations

📸 *OU Structure:*  
![OU Structure](Screenshots/Phase2/OU-Structure.png)

---

## 👥 Standard User Accounts (10 Users)

10 realistic employee accounts were added into `_Users`, then moved into their respective department OUs.

| Name | Username | Department |
|------|----------|------------|
| Adam Reid | areid | IT |
| Daniel Grant | dgrant | IT |
| Sarah Khan | skhan | HR |
| Laura Ibrahim | librahim | HR |
| Michael Brown | mbrown | Finance |
| Martin Stone | mstone | Finance |
| Emma Patel | epatel | Sales |
| Aisha Rahman | arahman | Sales |
| Jason Lee | jlee | Operations |
| Chloe Adams | cadams | Operations |

All users were created with:
- No forced password change at next logon  
- Password never expires: **disabled**  
- Accounts enabled  

📸 *Users in Department OUs:*  
![Departments](Screenshots/Phase2/Departments.png)

---

## 🔐 Privileged & Special Accounts

### 🛠️ IT Admin (Domain Administrator)
- Username: `itadmin`
- Added to:
  - Domain Admins  
  - Enterprise Admins  
  - Schema Admins  
- Password never expires: **enabled**

📸 *IT Admin Group Membership:*  
![IT Admin](Screenshots/Phase2/ITAdmin-GroupMembership.png)

---

### ⚙️ Service Account (SQL Service)
- Username: `sqlsvc`
- Password never expires: **enabled**  
- User cannot change password: **enabled**

📸 *Service Account Settings:*  
![Service Account](Screenshots/Phase2/ServiceAccount.png)

---

### 🚫 Onboarding Account
- Username: `new.starter`
- Account disabled (greyed-out icon)  
- Simulates HR pre-staging

📸 *Disabled Account:*  
![Disabled Account](Screenshots/Phase2/DisabledAccount.png)

---

### 🔒 Locked-Out User (Help Desk Simulation)
- Username: `locktest`
- Lockout triggered via incorrect password attempts  
- Unlocked through ADUC using Account tab  

📸 *Locked-Out Account:*  
![Locktest Locked Out](Screenshots/Phase2/LocktestLockedOut.png)

---

## 🔐 RDP & User Logon Rights Configuration

To allow non-admin users (like `locktest`) to sign into the server via RDP:

### ✔ Added to Remote Desktop Users
- `locktest` was added to:
  - **Remote Desktop Users** (local group)

### ✔ Updated Local Security Policy
- Confirmed `Remote Desktop Users` has RDP logon rights  

---

## 🔄 Account Lockout Policy (Domain Security)

Configured via **Default Domain Policy**:

- **Lockout threshold:** 3 attempts  
- **Lockout duration:** 30 minutes  
- **Reset counter:** 30 minutes  

Lockout was successfully tested and confirmed using the `locktest` account.

---

This completes Phase 2 of the Active Directory setup.  
Next phase: **Phase 3 — Security Groups & Role-Based Access (RBAC)**.

---

# 🧑‍💻 Author  
**Mohammad Masood (mullvad25)**  
Aspiring IT Support & Systems Engineer  
Focused on infrastructure, cloud, networking, and enterprise IT tools.

---

# 🌐 Purpose of This Lab  
This project is meant to:

- Build real job-ready IT skills  
- Strengthen a professional portfolio  
- Prove hands-on technical ability to recruiters  
- Prepare for IT Support, Help Desk, Desktop Support, and Sysadmin roles  

---

