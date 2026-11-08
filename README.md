# Hi, I'm <a href="https://linkedin.com/in/brian-russell-297828442">Brian</a>👋</h1>
**Information Technology Professional**

Welcome to my IT Project Portfolio. This repository showcases hands-on experience in cloud infrastructure, identity and access management, network security, and helpdesk operations using enterprise tools like **Microsoft Azure** and **osTicket**.

---

## 🛠️ Technical Skill Matrix

| Category | Skills & Tools |
| :--- | :--- |
| **Cloud & Infrastructure** | Microsoft Azure, Azure VMs, Virtual Networks (VNets), Resource Groups |
| **Identity & Access Management** | Active Directory Domain Services (AD DS), Domain Controllers, DNS |
| **Networking & Security** | Network Security Groups (NSGs), RDP, ICMP, WireShark protocol inspection |
| **ITSM & Operations** | osTicket (Deployment, SLA Configuration, User/Agent Roles, Ticket Routing) |
| **Operating Systems** | Windows Server, Windows 10/11, Linux (Ubuntu/Debian) |

---

## 🚀 Projects Overview

### 1. Microsoft Azure: Cloud Architecture, AD DS & Network Security
> Hands-on deployment of cloud infrastructure, domain controllers, and protocol inspection in Microsoft Azure.

* **[Configuring On-Premises Active Directory within Azure VMs](./azure-active-directory)**
  * Provisioned Windows Server Virtual Machines in Azure to simulate an enterprise domain environment.
  * Installed and configured **Active Directory Domain Services (AD DS)**, promoting the VM to a Primary Domain Controller.
  * Configured DNS settings, created Organizational Units (OUs), and automated bulk user creation to test environment readiness.
  * Joined client endpoints (Windows 10 VMs) to the domain and verified domain authentication and Group Policies.

* **[Network Security Groups (NSGs) & Inspecting Network Protocols](./azure-network-security)**
  * Configured Azure **Network Security Groups (NSGs)** to control inbound and outbound traffic rules for virtual networks.
  * Utilized **Wireshark** to capture and analyze network packets across multiple protocols, including **ICMP, SSH, RDP, and DNS**.
  * Demonstrated traffic filtering by toggling NSG rules to block/allow ping requests and remote access live in the capture tool.

---

### 2. osTicket: Enterprise Help Desk Ticketing System
> End-to-end setup and administration of an open-source IT Service Management (ITSM) ticketing workflow.

* **[osTicket: Prerequisites and Installation](./osticket-installation)**
  * Deployed IIS (Internet Information Services), PHP, and MySQL/MariaDB environment setup.
  * Configured database connections, PHP extensions, and security settings required for osTicket deployment.
  * Successfully initialized the osTicket web interface installer and completed initial configuration.

* **[osTicket: Post-Installation Configuration](./osticket-configuration)**
  * Configured **Roles, Departments, and Teams** to reflect real-world IT organizational structures (e.g., Tier 1 Support, System Admin, Networking).
  * Setup **SLA Plans** (Emergency vs. Standard) and **Help Topics** for automatic ticket routing and priority assignment.
  * Created Agent accounts and user access permissions.

* **[osTicket: Ticket Lifecycle Examples](./osticket-ticket-lifecycle)**
  * Simulated real-world helpdesk scenarios: account lockouts, printer failures, and system upgrades.
  * Managed ticket progression through **Creation ➔ Assignment ➔ Escalation ➔ Resolution ➔ Closure**.
  * Verified SLA tracking, priority reassignment, and ticket history logs.


<h2>🤳Connect with me:</h2>

[<img align="left" alt="Brian | LinkedIn" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/linkedin.svg" />][linkedin]

[linkedin]: https://linkedin.com/in/brian-russell-297828442
