# 🏢 Windows Server 2022 Active Directory Lab

![Windows Server AD](./assets/windows-server-ad-lab-banner.svg)

<div align="center">

[![Windows Server 2022][b-server]](docs/01-domain-controller-build.md)
[![Windows 11][b-client]](docs/02-windows11-domain-join.md)
[![Active Directory][b-ad]](docs/01-domain-controller-build.md)
[![AD CS][b-pki]](docs/01-domain-controller-build.md)

[![VirtualBox][b-vbox]](#-lab-architecture)
[![PowerShell][b-ps]](docs/01-domain-controller-build.md#-verification)

</div>

[b-server]: https://img.shields.io/badge/Windows_Server-2022-0078D4?style=for-the-badge&labelColor=0D1117&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI%2BPHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0wIDMuNDQ5IDkuNzUgMi4xdjkuNDUxSDBtMTAuOTQ5LTkuNjAyTDI0IDB2MTEuNEgxMC45NDlNMCAxMi42aDkuNzV2OS40NTFMMCAyMC42OTlNMTAuOTQ5IDEyLjZIMjRWMjRsLTEyLjktMS44MDEiLz48L3N2Zz4%3D
[b-client]: https://img.shields.io/badge/Windows_11-Enterprise-0078D4?style=for-the-badge&labelColor=0D1117&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI%2BPHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0wIDMuNDQ5IDkuNzUgMi4xdjkuNDUxSDBtMTAuOTQ5LTkuNjAyTDI0IDB2MTEuNEgxMC45NDlNMCAxMi42aDkuNzV2OS40NTFMMCAyMC42OTlNMTAuOTQ5IDEyLjZIMjRWMjRsLTEyLjktMS44MDEiLz48L3N2Zz4%3D
[b-ad]: https://img.shields.io/badge/Active_Directory-LAB.local-005A9E?style=for-the-badge&labelColor=0D1117
[b-pki]: https://img.shields.io/badge/AD_CS-Enterprise_Root_CA-8E24AA?style=for-the-badge&labelColor=0D1117
[b-vbox]: https://img.shields.io/badge/VirtualBox-Lab_Host-2F61B4?style=for-the-badge&labelColor=0D1117&logo=virtualbox&logoColor=white
[b-ps]: https://img.shields.io/badge/PowerShell-Verified-5391FE?style=for-the-badge&labelColor=0D1117

> **Summary:** A hands-on Active Directory home lab built in Oracle VirtualBox. It starts with a Windows Server 2022 domain controller (AD DS, DNS, AD CS) and grows phase by phase into a managed, monitored, and eventually attackable Windows environment.


<details>
<summary>📌 <b>Table of Contents</b> (Click to expand)</summary>

- [🧭 Lab Architecture](#-lab-architecture)
- [📚 Phases](#-phases)
- [📁 Repository Structure](#-repository-structure)
- [🚀 Roadmap](#-roadmap)

</details>

</details>

---

## 🧭 Lab Architecture

| Host | Role | IP Address | Details |
|---|---|---|---|
| `DC01` | Domain Controller | `10.0.2.15` | Windows Server 2022, forest `LAB.local`, AD DS, DNS, Global Catalog, AD CS Enterprise Root CA |
| `WS01` | Domain Client | `10.0.2.16` | Windows 11 Enterprise Evaluation, joined to `LAB.local` |

**Hypervisor:** Oracle VirtualBox

---

## 📚 Phases

| Phase | Project | Status | Skills Demonstrated |
|---|---|---|---|
| 1 | [Domain Controller Build](docs/01-domain-controller-build.md) | Completed | AD DS, DNS, Global Catalog, AD CS, verification (dcdiag, certutil) |
| 2 | [Windows 11 Domain Join](docs/02-windows11-domain-join.md) | Completed | Static IP, DNS design, client join, ADUC verification |
| 3 | Organizational Units and Users | Completed | AD structure, account management |


---

## 🗂 Repository Structure

```
windows-server-ad-lab/
├── README.md
├── docs/
    ├── 01-domain-controller-build.md
    └── 02-windows11-domain-join.md

```

---

## 🚀 Roadmap

- [x] Build and promote the domain controller
- [x] Deploy AD CS as an Enterprise Root CA
- [x] Join a Windows 11 client to the domain
- [x] Create an OU structure and domain users

---
