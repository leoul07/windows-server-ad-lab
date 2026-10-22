# 🏢 Windows Server 2022 Active Directory Lab

<div align="center">

![Windows Server 2022 Active Directory Lab](./assets/windows-server-ad-lab-banner.svg)

[![Windows Server 2022][b-server]](docs/01-domain-controller-build.md)
[![Windows 11][b-client]](docs/02-windows11-domain-join.md)
[![Active Directory][b-ad]](docs/01-domain-controller-build.md)
[![AD CS][b-pki]](docs/01-domain-controller-build.md)

[![VirtualBox][b-vbox]](#-lab-architecture)
[![PowerShell][b-ps]](docs/03-creating-domain-users.md)


</div>

[b-server]: https://img.shields.io/badge/Windows_Server-2022-0078D4?style=for-the-badge&labelColor=6E7681&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI%2BPHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0wIDMuNDQ5IDkuNzUgMi4xdjkuNDUxSDBtMTAuOTQ5LTkuNjAyTDI0IDB2MTEuNEgxMC45NDlNMCAxMi42aDkuNzV2OS40NTFMMCAyMC42OTlNMTAuOTQ5IDEyLjZIMjRWMjRsLTEyLjktMS44MDEiLz48L3N2Zz4%3D
[b-client]: https://img.shields.io/badge/Windows_11-Enterprise-0078D4?style=for-the-badge&labelColor=6E7681&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI%2BPHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0wIDMuNDQ5IDkuNzUgMi4xdjkuNDUxSDBtMTAuOTQ5LTkuNjAyTDI0IDB2MTEuNEgxMC45NDlNMCAxMi42aDkuNzV2OS40NTFMMCAyMC42OTlNMTAuOTQ5IDEyLjZIMjRWMjRsLTEyLjktMS44MDEiLz48L3N2Zz4%3D
[b-ad]: https://img.shields.io/badge/Active_Directory-LAB.local-005A9E?style=for-the-badge&labelColor=6E7681
[b-pki]: https://img.shields.io/badge/AD_CS-Enterprise_Root_CA-8E24AA?style=for-the-badge&labelColor=6E7681
[b-vbox]: https://img.shields.io/badge/VirtualBox-Lab_Host-2F61B4?style=for-the-badge&labelColor=6E7681&logo=virtualbox&logoColor=white
[b-ps]: https://img.shields.io/badge/PowerShell-Automation-5391FE?style=for-the-badge&labelColor=6E7681


> **Summary:** A hands-on Active Directory home lab built in Oracle VirtualBox. It starts with a Windows Server 2022 domain controller (AD DS, DNS, AD CS) and grows phase by phase into a managed Windows environment with domain users, Organizational Units, security groups, and Group Policy. Each phase is documented step by step with screenshots, design decisions, and security considerations.

> **Short on time?** Read [Phase 1](docs/01-domain-controller-build.md) to see the build and verification, then [Phase 5](docs/05-group-policy.md) to see how the structure is used to manage users.

<details>
<summary>📌 <b>Table of Contents</b> (Click to expand)</summary>

- [📊 Lab at a Glance](#-lab-at-a-glance)
- [🧭 Lab Architecture](#-lab-architecture)
- [📚 Phases](#-phases)
- [🧰 Skills Demonstrated](#-skills-demonstrated)
- [🔎 Lessons Learned](#-lessons-learned)
- [📁 Repository Structure](#-repository-structure)
- [🚀 Roadmap](#-roadmap)

</details>

---

## 📊 Lab at a Glance

| | |
|---|---|
| **Domain / forest** | `LAB.local` (new forest, Windows Server 2016 functional level) |
| **Hosts** | 2 virtual machines: `DC01` (server) and `WS01` (client) |
| **Server roles** | AD DS, DNS, Global Catalog, AD CS Enterprise Root CA (`LAB-DC01-CA-1`) |
| **Identity objects** | Domain users (created with ADUC and PowerShell), 3 OUs plus 1 nested OU, 1 global security group |
| **Policy** | 1 Group Policy Object linked to the `Engineering` OU |
| **Documentation** | 5 completed phase write-ups |

---

## 🧭 Lab Architecture

| Host | Role | IP Address | Details |
|---|---|---|---|
| `DC01` | Domain Controller | `10.0.2.15` | Windows Server 2022, forest `LAB.local`, AD DS, DNS, Global Catalog, AD CS Enterprise Root CA |
| `WS01` | Domain Client | `10.0.2.16` | Windows 11 Enterprise Evaluation, joined to `LAB.local` |


## **Hypervisor:** Oracle VirtualBox

### **Network view**

<img width="1024" height="350" alt="image" src="https://github.com/user-attachments/assets/8a75b338-5b32-4606-8103-f7681d52103d" />



### **Directory view**

<img width="1024" height="350" alt="image" src="https://github.com/user-attachments/assets/0b3e3d53-5685-447d-849a-e64b795667c8" />


---

## 📚 Phases

| Phase | Project | Skills Demonstrated |
|---|---|---|
| 1 | [Domain Controller Build](docs/01-domain-controller-build.md) | AD DS, DNS, Global Catalog, AD CS, verification (dcdiag, certutil) |
| 2 | [Windows 11 Domain Join](docs/02-windows11-domain-join.md) | Static IP, DNS design, client join, ADUC verification |
| 3 | [Creating Domain Users](docs/03-creating-domain-users.md) | ADUC, PowerShell `New-ADUser`, account design, credential hygiene |
| 4 | [Organizational Units and Groups](docs/04-organizational-units-and-groups.md) | OU design, nested OUs, security groups, group scope |
| 5 | [Group Policy](docs/05-group-policy.md) | GPO creation and linking, Administrative Templates, policy verification |

---

## 🧰 Skills Demonstrated

| Area | What I did | Where |
|---|---|---|
| **Windows Server administration** | Installed and configured AD DS, DNS, and AD CS roles on Windows Server 2022 | [Phase 1](docs/01-domain-controller-build.md) |
| **Networking and DNS** | Designed static addressing and pointed the client at the DC for DNS so the domain join works | [Phase 2](docs/02-windows11-domain-join.md) |
| **Identity and access management** | Created user accounts with consistent naming and forced password change at first logon | [Phase 3](docs/03-creating-domain-users.md) |
| **Directory design** | Built an OU hierarchy and security groups, and kept group membership independent of OU placement | [Phase 4](docs/04-organizational-units-and-groups.md) |
| **Group Policy** | Created, linked, edited, and verified a GPO scoped to a single OU | [Phase 5](docs/05-group-policy.md) |
| **PKI** | Deployed an AD-integrated Enterprise Root CA and verified it with `certutil` | [Phase 1](docs/01-domain-controller-build.md) |
| **Automation** | Created accounts with PowerShell `New-ADUser`, entering the password through a secure prompt | [Phase 3](docs/03-creating-domain-users.md) |
| **Security mindset and documentation** | Documented limitations, hardening notes, and next steps for every phase | All phases |

---

## 🔎 Lessons Learned

- **Active Directory depends on DNS.** The Windows 11 client could only find the domain because its DNS pointed to the domain controller.
- **OUs and groups solve different problems.** OUs organize and govern accounts (policy, delegation), while groups grant access, and a group can include members from several OUs.
- **User and computer settings target different objects.** A User Configuration policy linked to an OU applies to the users in it, not to the computers.
- **Verify, don't assume.** Each phase ends with a check from the server side, such as `dcdiag`, `certutil`, ADUC, or the GPO Settings report.
- **Defaults deserve attention.** Accounts landing in the default `Users` container, and policy filtering to Authenticated Users, are starting points to refine, not final designs.

---

## 📁 Repository Structure

```
windows-server-ad-lab/
├── README.md
├── assets/
│   ├── windows-server-ad-lab-banner.svg
│   ├── windows-server-ad-pki-lab.svg
│   ├── windows11-domain-join-banner.svg
│   ├── creating-domain-users-banner.svg
│   ├── organizational-units-groups-banner.svg
│   └── group-policy-banner.svg
└── docs/
    ├── 01-domain-controller-build.md
    ├── 02-windows11-domain-join.md
    ├── 03-creating-domain-users.md
    ├── 04-organizational-units-and-groups.md
    └── 05-group-policy.md
```

---

## 🚀 Roadmap

- [x] Build and promote the domain controller
- [x] Deploy AD CS as an Enterprise Root CA
- [x] Join a Windows 11 client to the domain
- [x] Create domain users with ADUC and PowerShell
- [x] Build an OU structure and a security group
- [x] Create and link a Group Policy Object to an OU


---
