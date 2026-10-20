# 🏢🔐 Windows Server 2022 Domain Controller and Enterprise Root CA Lab

![Windows Server 2022 DC](./assets/windows-server-ad-pki-lab.svg) 

[![Windows Server 2022](https://img.shields.io/badge/Windows_Server-2022-0078D4?style=for-the-badge&logo=windows&logoColor=white)](#-architecture-and-environment)
[![Domain LAB.local](https://img.shields.io/badge/Domain-LAB.local-6E7681?style=for-the-badge)](#-phase-2-promote-the-server-to-a-domain-controller)
[![Active Directory AD DS](https://img.shields.io/badge/Active_Directory-AD_DS-005A9E?style=for-the-badge)](#-phase-2-promote-the-server-to-a-domain-controller)
[![AD CS Enterprise Root CA](https://img.shields.io/badge/PKI-AD_CS_Enterprise_Root_CA-00A88E?style=for-the-badge)](#-phase-4-configure-the-enterprise-root-ca)
[![PowerShell Verified](https://img.shields.io/badge/PowerShell-Verified-5391FE?style=for-the-badge&logo=powershell&logoColor=white)](#-verification)
[![Oracle VirtualBox](https://img.shields.io/badge/VirtualBox-Lab_Host-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)](#-architecture-and-environment)



> 🏗️ A hands-on lab that builds the identity and trust foundation of a Windows enterprise network: a new Active Directory forest with integrated DNS, plus an Enterprise Root Certification Authority for internal PKI.

🔍 **Focus:** Windows Server | Active Directory | PKI | Systems Administration

👤 **Role:** Systems Administrator (Home Lab Build)

---


<details>
  <summary>📌 <b>Table of Contents</b> (Click to expand)</summary>

- [📋 Executive Summary](#-executive-summary)
- [💻 Architecture and Environment](#-architecture-and-environment)
- [🔧 Step-by-Step Implementation Guide](#-step-by-step-implementation-guide)
  - [📦 Phase 1: Install the AD DS Role](#-phase-1-install-the-ad-ds-role)
  - [🏢 Phase 2: Promote the Server to a Domain Controller](#-phase-2-promote-the-server-to-a-domain-controller)
  - [🔑 Phase 3: Install the AD CS Role](#-phase-3-install-the-ad-cs-role)
  - [📜 Phase 4: Configure the Enterprise Root CA](#-phase-4-configure-the-enterprise-root-ca)
- [✅ Verification](#-verification)
- [🔒 Design Decisions, Limitations and Hardening Roadmap](#-design-decisions-limitations-and-hardening-roadmap)
- [🎯 Key Technical Competencies Demonstrated](#-key-technical-competencies-demonstrated)
- [🚀 Next Steps](#-next-steps)

</details>

---

## 📋 Executive Summary

Nearly every enterprise Windows environment depends on two services: **Active Directory** to answer "who is this user or machine, and what are they allowed to do?", and **Public Key Infrastructure (PKI)** to answer "can this identity and this connection be trusted?"

This project builds both from scratch on a single Windows Server 2022 host:

- 🔐 **Identity layer:** Active Directory Domain Services (AD DS) with integrated DNS, deployed as a brand-new forest (`LAB.local`) on a server named `DC01`.
- 🔑 **Trust layer:** Active Directory Certificate Services (AD CS) configured as an **Enterprise Root CA**, so certificates can be issued and managed in step with AD identities and Group Policy.

The result is a working foundation for later labs: user and group management, Group Policy, certificate templates and autoenrollment, LDAPS, and AD attack and defense exercises.

---

## 💻 Architecture and Environment

| Component | Configuration |
|---|---|
| 🖥️ Hostname | `DC01` |
| 💿 Operating system | Microsoft Windows Server 2022 Standard (Evaluation) |
| 🌐 IP address | `10.0.2.15` |
| 📦 Virtualization platform | Oracle VirtualBox (VM) |
| 🏢 Domain / forest | `LAB.local` (new forest, root domain) |
| 📶 Forest functional level | Windows Server 2016 |
| 📶 Domain functional level | Windows Server 2016 |
| ⚙️ Domain controller capabilities | DNS Server, Global Catalog (GC); RODC not selected |
| 🧩 Installed roles | AD DS, DNS Server, AD CS, File and Storage Services |
| 🧰 Management tools | Group Policy Management, RSAT (AD Administrative Center, AD DS snap-ins and command-line tools, AD module for Windows PowerShell, CA Management tools) |
| 📜 CA type | Enterprise Root CA |
| 🏷️ CA common name | `LAB-DC01-CA-1` |
| 🏷️ CA distinguished name | `CN=LAB-DC01-CA-1,DC=LAB,DC=local` |
| 🔏 Cryptographic provider | RSA#Microsoft Software Key Storage Provider |
| 🔢 Key length / hash | 2048-bit RSA / SHA-256 |
| ⏳ CA validity period | 5 years (default) |
| 🗄️ CA database and log | `C:\Windows\system32\CertLog` |
| 👤 Administrative account for CA setup | `LAB\Administrator` |

---

## 🔧 Step-by-Step Implementation Guide

### 📦 Phase 1: Install the AD DS Role
---

Installing the role places the Active Directory binaries and management tools on the server. It does not yet make the machine a domain controller.

1. In **Server Manager**, open **Manage > Add Roles and Features**.
   
   <img width="966" height="390" alt="chrome_vweGFppdcM" src="https://github.com/user-attachments/assets/7b286eba-8eec-414c-9216-01cb902f9548" />

2. Choose **Role-based or feature-based installation** and select `DC01` from the server pool as the destination.

   <img width="977" height="415" alt="chrome_OHRXXWC7lB" src="https://github.com/user-attachments/assets/9b366122-f7d0-4253-a76f-043f34ef10e0" />

3. On **Select server roles**, check **Active Directory Domain Services**. The wizard prompts to add the required features (Group Policy Management, AD DS and AD LDS tools, AD Administrative Center, the AD module for PowerShell, and the snap-ins). Leave **Include management tools** checked and click **Add Features**.

   <img width="960" height="396" alt="VirtualBoxVM_LANRJ38kld" src="https://github.com/user-attachments/assets/5bc3fd99-030f-4eb8-b76f-af17eb76180a" />
   

4. On **Confirm installation selections**, check **Restart the destination server automatically if required** and click **Install**.

   <img width="957" height="424" alt="4VirtualBoxVM_BJVxzMbTl0" src="https://github.com/user-attachments/assets/0e2211c2-4a47-42f8-8da2-97936f5ea321" />

---

## 🏢 Phase 2: Promote the Server to a Domain Controller

When the role install finishes, Server Manager shows a warning flag: *"Configuration required for Active Directory Domain Services."* This is the post-deployment step that creates the directory.

1. Click **Promote this server to a domain controller** (from the installation results or the notifications flag).

   * Installation Results

      <img width="959" height="420" alt="6 Promote server to DC VirtualBoxVM_BBoEPPT0fm" src="https://github.com/user-attachments/assets/725f02df-59f2-48c6-b315-89ff52942b4f" />
  
    * Notifications flag

      <img width="962" height="424" alt="6chrome_jGgLoIbA8K" src="https://github.com/user-attachments/assets/86ce6abe-e2b6-4315-8e13-92bdd7662f89" />

2. **Deployment Configuration:** select **Add a new forest** and set the root domain name to `LAB.local`.

   <img width="958" height="422" alt="7Add a new forest" src="https://github.com/user-attachments/assets/87381b4c-769b-4ef8-b468-af71c23a7ee9" />


4. **Domain Controller Options:**
   - Forest and domain functional levels: **Windows Server 2016**.
   - Capabilities: **DNS server** and **Global Catalog** enabled, **RODC** disabled.
   - Set the **Directory Services Restore Mode (DSRM)** password. This is the break-glass credential used to repair or restore the directory offline.
   
   <img width="955" height="391" alt="3VirtualBoxVM_9h9b5ZrGpB" src="https://github.com/user-attachments/assets/32ea3d9e-3e06-43b0-ab39-69a4b831c70a" />

5. **Prerequisites Check:** the wizard validated the server and passed with warnings (see [Design Decisions](#-design-decisions-limitations-and-hardening-roadmap)). Click **Install**.

 
   <img width="959" height="417" alt="4VirtualBoxVM_41pbq75YB3" src="https://github.com/user-attachments/assets/e7cd57f7-ef5e-4040-95d6-324d36731ebf" />


6. The server reboots automatically at the end of promotion and comes back as `DC01.LAB.local`.

> 💡 **Why a new forest?** It creates a clean, isolated security boundary for the lab, which makes it safe to experiment with delegation, Group Policy, and attack-and-defense scenarios without touching any production directory.
---
### 🔑 Phase 3: Install the AD CS Role
---
With the domain in place, the server can host an **Enterprise** CA, which requires AD DS to publish templates and trust information. The CA name and domain membership cannot be changed after installation, so the domain is built first.

1. Reopen **Add Roles and Features** and again choose **Role-based or feature-based installation**.

   <img width="957" height="514" alt="9kPiySqWyp" src="https://github.com/user-attachments/assets/a611a0fb-7c81-421f-835b-f8e754c8552d" />

3. Confirm `DC01.LAB.local` as the destination server.

   <img width="960" height="419" alt="8VirtualBoxVM_VHUglwVl9i" src="https://github.com/user-attachments/assets/ec202776-ccfc-426f-b56a-a8309256c9c2" />

5. On **Select server roles**, check **Active Directory Certificate Services** and accept the required management tools.

   <img width="959" height="517" alt="6YxUkd05YK" src="https://github.com/user-attachments/assets/23031cbd-7ed1-42e8-9ea9-1d93ec76ff7b" />


4. Click through **Features** without changes and read the AD CS notice about the server name and domain settings being locked after CA installation.
5. On **Select role services**, select only **Certification Authority**.

   <img width="959" height="512" alt="HXrGTmgWgv" src="https://github.com/user-attachments/assets/0b56f54b-bd3b-4cb9-beb2-ed2b69b62b72" />


6. On **Confirm installation selections**, click **Install**.

   <img width="958" height="386" alt="VirtualBoxVM_7D7OeW4Kae" src="https://github.com/user-attachments/assets/8d30f8df-36a9-4577-8396-179fe43d3b98" />

---
### 📜 Phase 4: Configure the Enterprise Root CA
---
As with AD DS, installing the role is separate from configuring it. Click **Configure Active Directory Certificate Services on the destination server** from the installation results.

| Wizard page | Selection |
|---|---|
| Credentials | `LAB\Administrator` (an Enterprise Admin is required for an Enterprise CA) |
| Role Services | Certification Authority |
| Setup Type | **Enterprise CA** (AD-integrated; issues certificates to domain members using templates) |
| CA Type | **Root CA** (top of the trust hierarchy; issues its own self-signed certificate) |
| Private Key | Create a new private key |
| Cryptography | RSA, Microsoft Software Key Storage Provider, 2048-bit key, SHA-256 |
| CA Name | `LAB-DC01-CA-1` (DN suffix `DC=LAB,DC=local`) |
| Validity Period | 5 years |
| Certificate Database | `C:\Windows\system32\CertLog` |

<img width="957" height="516" alt="C2B2mbxSsz" src="https://github.com/user-attachments/assets/f7cf16a0-fb79-4639-ae0f-977d969e0cc0" />

<img width="956" height="504" alt="7lgUDwRnph" src="https://github.com/user-attachments/assets/d52ea12b-dc4b-4c45-8956-0d15bce720d0" />

<img width="955" height="518" alt="fxGxQW99fN" src="https://github.com/user-attachments/assets/2a33e1c4-168d-4389-a38a-7cadc2a7617c" />

<img width="957" height="500" alt="Mu0XSTEGoF" src="https://github.com/user-attachments/assets/0f94ad23-4709-4560-9c9d-bcb181d99d0a" />

<img width="957" height="473" alt="sgiEnvTyp0" src="https://github.com/user-attachments/assets/9f76a044-1e35-4cb2-8edc-b5b21672a873" />

<img width="958" height="490" alt="Screenshot 2026-10-17 195631" src="https://github.com/user-attachments/assets/988304ce-c1cd-4ac1-b0ad-9f7c99b79ea2" />




Review the **Confirmation** page and click **Configure**.

<img width="957" height="492" alt="UtY6PDTl0D" src="https://github.com/user-attachments/assets/f86921c7-9492-4fa5-bb16-a64d5cd675ce" />


The **Results** page reports **Configuration succeeded** for the Certification Authority.

<img width="956" height="460" alt="Screenshot 2026-10-17 195848" src="https://github.com/user-attachments/assets/d547bfc5-2905-41e9-b621-4ccf1ec5e353" />


---

## ✅ Verification

After the build, I validated both services from an elevated PowerShell session on `DC01` (with `Start-Transcript` logging the session) and confirmed the results in the AD and CA management consoles.

| Check | Tool | Result |
|---|---|---|
| 🏢 Domain details | `Get-ADDomain` | ✅ `LAB.local`, NetBIOS `LAB`, `Windows2016Domain` |
| ⚙️ Core AD services | `Get-Service NTDS, DNS, Netlogon, kdc` | ✅ All four `Running` |
| 🩺 Domain controller diagnostics | `dcdiag /q` | ⚠️ One failed test (`SystemLog`); no other failures reported. See [findings](#-findings-from-verification) |
| 🔑 CA service | `Get-Service certsvc` | ✅ `Running` |
| 📡 CA responding | `certutil -ping` | ✅ `ICertRequest2` interface alive (16 ms) |
| 🏷️ CA identity | `certutil -CAInfo name` | ✅ `LAB-DC01-CA-1` |
| 🗂️ DC registered in the directory | Active Directory Users and Computers | ✅ `DC01` under **Domain Controllers**, type GC, site `Default-First-Site-Name` |
| 📜 CA registered and visible | Certification Authority console (`certsrv.msc`) | ✅ `LAB-DC01-CA-1` listed |

---
### 📁 Active Directory
---

```powershell
Get-ADDomain | Select-Object DNSRoot, NetBIOSName, DomainMode
```

<img width="961" height="420" alt="ShareX_2tpHOwvKbd" src="https://github.com/user-attachments/assets/8265d8bc-83a8-4e67-b433-8af8e431a5ea" />

The domain reports `LAB.local` at the Windows Server 2016 domain functional level, matching the deployment settings.

```powershell
dcdiag /q
Get-Service NTDS, DNS, Netlogon, kdc | Select-Object Name, Status
```

<img width="1907" height="835" alt="image" src="https://github.com/user-attachments/assets/22a0f5d0-80c0-4ce4-bc88-1f6da274cbc0" />

`/q` prints failures only, so every test except `SystemLog` passed. The directory database (NTDS), DNS, Netlogon, and Kerberos (kdc) services are all running.

---
### 🔏 Certificate Services
---

```powershell
certutil -ping
Get-Service certsvc | Select-Object Name, Status
certutil -CAInfo name
```

<img width="1909" height="819" alt="image" src="https://github.com/user-attachments/assets/6a5cd1f8-7393-495b-ad74-014d3065d135" />


The CA answered the ping, the `certsvc` service is running, and the CA reports its name as `LAB-DC01-CA-1`.

---
### 📊 Management Console Confirmation
---

The command-line results were cross-checked in the graphical consoles.

**Active Directory Users and Computers** shows `DC01` under **Domain Controllers** in the `LAB.local` tree. Its DC type is GC, so it is acting as a Global Catalog server, and it sits in `Default-First-Site-Name`.

<img width="957" height="416" alt="S2FFQucXDq" src="https://github.com/user-attachments/assets/ee962a09-f80c-4ee9-822f-ae7b6d04b441" />

**Certification Authority console** (`certsrv.msc`) lists `LAB-DC01-CA-1` as the local certification authority, confirming the CA is registered and manageable on this server.


<img width="958" height="424" alt="lsGcyJhQ3R" src="https://github.com/user-attachments/assets/f08fb909-8fb4-4d19-a422-a5d907f7f899" />


---
### 🔍 Findings from Verification
---

`dcdiag` flagged **DC01 failed test SystemLog**, which means recent error events were found in the System log. The events were:

- An unexpected-shutdown event: the VM was not shut down cleanly before it was last started.
- Two events reporting that an OCSP response could not be retrieved, which relate to certificate revocation checking.


> ⚠️ Neither points to a failure of AD DS or the CA, since every other `dcdiag` test passed and the services respond normally. They are consistent with a lab VM that was powered off without a clean shutdown and a new CA that has no revocation (CDP/AIA/OCSP) endpoints configured yet. The follow-ups are listed in the table below.

---

## 🎯 Key Technical Competencies Demonstrated

**🖥️ Systems Administration**
- Deploying and configuring Windows Server 2022 roles and features through Server Manager
- Separating role installation from post-deployment configuration, and handling the required reboot sequencing
- Interpreting prerequisite-check warnings and assessing their real-world impact
- Verifying the build with PowerShell, `dcdiag`, `certutil`, and the AD and CA consoles, then tracing a `dcdiag` SystemLog failure back to its cause in the event log

**🔐 Identity and Access Management (IAM)**
- Building a new AD forest and root domain, including functional level selection
- Configuring a domain controller with Global Catalog and integrated DNS
- Managing the DSRM credential as a recovery control
- Using Group Policy Management and RSAT tooling as the base for centralized policy enforcement

**🔑 Public Key Infrastructure (PKI)**
- Understanding the difference between Enterprise and Standalone CAs, and Root and Subordinate CAs
- Deploying an AD-integrated Enterprise Root CA with a new private key (RSA 2048, SHA-256)
- Applying correct dependency ordering (domain first, then CA, since CA name and domain membership become fixed)
- Recognizing production PKI best practice (offline root, tiered hierarchy) versus lab shortcuts

**📝 Security Mindset and Documentation**
- Identifying and documenting limitations and a hardening roadmap rather than only the happy path
- Producing clear technical documentation suitable for handoff and peer review

---
