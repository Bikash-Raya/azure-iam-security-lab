<div align="center">

# 🔐 Azure IAM Security Lab – User, Group, RBAC, Access Testing & Sign-in Logs

![Domain](https://img.shields.io/badge/Platform-Microsoft%20Azure-blue?style=for-the-badge&logo=microsoftazure)
![Service](https://img.shields.io/badge/Service-Microsoft%20Entra%20ID-purple?style=for-the-badge)
![Type](https://img.shields.io/badge/Type-IAM%20%26%20RBAC%20Lab-green?style=for-the-badge)

<img src="https://img.shields.io/badge/Resource%20Group-RG--IAM--Security--Lab-blue?style=flat-square" />
<img src="https://img.shields.io/badge/RBAC%20Role-Reader-orange?style=flat-square" />
<img src="https://img.shields.io/badge/MFA-Microsoft%20Authenticator-green?style=flat-square" />
<img src="https://img.shields.io/badge/Access%20Read-PASS-brightgreen?style=flat-square" />
<img src="https://img.shields.io/badge/Access%20Delete-FAIL%20(expected)-red?style=flat-square" />
<img src="https://img.shields.io/badge/Sign--in%20Logs-Reviewed-lightgrey?style=flat-square" />

---

**Prepared by:** Bikash Raya  
**Project Type:** Azure Identity & Access Management Lab — User, Group, RBAC, Access Testing & Sign-in Log Review

</div>

---

## 📁 Repository Structure

| File | Description |
| --- | --- |
| [Azure_IAM_Security_Lab_Report.docx](./Azure_IAM_Security_Lab_Report.docx) | Complete lab report with all screenshots embedded |
| README.md | Project overview |

---

## 📋 Overview

This repository documents a hands-on **Azure Identity and Access Management (IAM) lab** performed in a live Microsoft Azure subscription using **Microsoft Entra ID** (formerly Azure Active Directory).

The lab covers the complete IAM lifecycle:

* 📦 Created a dedicated Resource Group — **RG-IAM-Security-Lab** (Australia East)
* 👤 Provisioned a test user — **IAM Test User** in Microsoft Entra ID
* 👥 Created a Security Group — **IAM-Lab-Readers** and added the test user as a member
* 🔐 Assigned the **Reader** RBAC role to the group at resource group scope
* ✅ Logged in as the test user and **validated access** — read access permitted, delete and create blocked
* 📱 Configured **MFA** using Microsoft Authenticator
* 📊 Reviewed **Sign-in logs** in Microsoft Entra ID Monitoring

---

## 🛠️ Technologies Used

* Microsoft Azure (Azure Portal)
* Microsoft Entra ID (Azure Active Directory)
* Azure Role-Based Access Control (RBAC)
* Microsoft Authenticator (MFA)
* Microsoft Entra ID Sign-in Logs & Monitoring

---

## 🧪 Lab Environment

| Component | Value |
| --- | --- |
| Platform | Microsoft Azure |
| Identity Service | Microsoft Entra ID |
| Subscription | Azure Subscription 1 |
| Resource Group | RG-IAM-Security-Lab |
| Region | Australia East |
| Test User | IAM Test User (iam.testuser@BiksecOpsLaboutlook.onmicrosoft.com) |
| Security Group | IAM-Lab-Readers (Security, Assigned membership) |
| RBAC Role | Reader (scoped to RG-IAM-Security-Lab) |
| MFA Method | Microsoft Authenticator App |

---

## 🌐 Lab Architecture

```
[Microsoft Entra ID]
        │
        ├── IAM Test User (iam.testuser@BiksecOpsLaboutlook.onmicrosoft.com)
        │       └── Member of IAM-Lab-Readers
        │
        └── IAM-Lab-Readers (Security Group)
                └── Reader Role Assigned
                        │
                        ▼
              [RG-IAM-Security-Lab]  ←  Scope of RBAC assignment
              (Resource Group — Australia East)

Access Control Results:
  ✅ View RG-IAM-Security-Lab     → PASS  (Reader permitted)
  ❌ Delete Resource Group         → FAIL  (AuthorizationFailed)
  ❌ Create new Resource Group     → FAIL  (No permissions)
```

---

## 🔬 Lab Steps

### Step 1 — Create Resource Group

**Navigation:** Azure Portal → Resource groups → Create

| Setting | Value |
| --- | --- |
| Resource Group Name | RG-IAM-Security-Lab |
| Subscription | Azure Subscription 1 |
| Region | Australia East |

Clicked **Review + create** → **Create**. Resource group successfully provisioned.

---

### Step 2 — Create Test User

**Navigation:** Microsoft Entra ID → Users → New user → Create new user

| Setting | Value |
| --- | --- |
| Display Name | IAM Test User |
| User Principal Name | iam.testuser@BiksecOpsLaboutlook.onmicrosoft.com |
| Password | Auto-generated (temporary) |
| Account Status | Enabled |
| User Type | Member |

Full username and temporary password copied for access testing.

---

### Step 3 — Create Security Group

**Navigation:** Microsoft Entra ID → Groups → New group

| Setting | Value |
| --- | --- |
| Group Type | Security |
| Group Name | IAM-Lab-Readers |
| Membership Type | Assigned |

After creating the group: opened it → **Members → Add members → IAM Test User**

---

### Step 4 — Assign Reader RBAC Role

**Navigation:** Resource groups → RG-IAM-Security-Lab → Access control (IAM) → Add → Add role assignment

| Setting | Value |
| --- | --- |
| Role | Reader |
| Assign Access To | User, group, or service principal |
| Member | IAM-Lab-Readers (Security Group) |
| Scope | RG-IAM-Security-Lab |

Clicked **Review + assign** to complete the role assignment.

---

### Step 5 — Login as Test User & Access Testing

Opened Incognito browser → navigated to https://portal.azure.com → logged in as IAM Test User → changed temporary password → configured MFA.

**Access Test Results:**

| Test Action | Expected | Result |
| --- | --- | --- |
| View RG-IAM-Security-Lab | Permitted (Reader) | ✅ PASS |
| Delete Resource Group | Denied — AuthorizationFailed | ❌ FAIL (as expected) |
| Create new Resource Group | Denied — no permissions | ❌ FAIL (as expected) |

> **Delete error:** `Failed to delete resource group RG-IAM-Security-Lab: The client 'iam.testuser@...' does not have authorization to perform action 'Microsoft.Resources/subscriptions/resourceGroups/delete'` — Code: AuthorizationFailed

> **Create error:** `You do not have permissions to create resource groups under subscription edc2aab4-e5b7-4e39-8577-71a6ade32785`

---

### Step 6 — Review Sign-in Logs

**Navigation:** Microsoft Entra ID → Monitoring & health → Sign-in logs → Filter: IAM Test User

| Field | Value |
| --- | --- |
| User | IAM Test User |
| Application | Azure Portal |
| Status | Success + Interrupted (MFA setup) |
| Authentication | Multifactor Authentication |
| Location | AU (Australia) |

Sign-in logs confirmed two entries for IAM Test User — Success and Interrupted (MFA registration) — both from Azure Portal, Australia.

---

## 🎯 Skills Demonstrated

* Microsoft Entra ID User Provisioning & Management
* Azure Resource Group Creation & Scoping
* Azure Role-Based Access Control (RBAC) — Role Assignment
* Security Group Creation & Group-Based Access Management
* MFA Configuration using Microsoft Authenticator
* Access Permission Testing & Validation
* Principle of Least Privilege — practical enforcement
* Azure IAM (Access Control) configuration
* Sign-in Log Review & Identity Monitoring
* Azure Portal Navigation

---

## 🎯 Key Takeaway

> This lab demonstrates practical, hands-on experience with Azure Identity and Access Management — covering the full IAM lifecycle from resource group and user provisioning through security group configuration, RBAC role assignment, live access testing (confirming both permitted and denied actions), MFA setup, and sign-in log review. All steps were performed in a live Azure subscription, validating that least-privilege access controls work correctly in a real cloud environment.

---

## 🔗 Connect With Me

<div align="center">

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/bikash-raya/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=for-the-badge&logo=github)](https://github.com/Bikash-Raya)

</div>

---

<div align="center">

⭐ If you find this project useful, feel free to star the repository ⭐

</div>
