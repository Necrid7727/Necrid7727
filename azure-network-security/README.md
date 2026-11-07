
# 🛡️ Azure Network Security Groups (NSGs) & Traffic Protocol Inspection

## 🎯 Objective
This project demonstrates how to configure Azure Network Security Groups (NSGs) to enforce access control lists (ACLs) and monitor network traffic live using Wireshark. The objective was to observe traffic patterns, inspect key application/transport protocols, and analyze how real-time firewall rule adjustments impact network communication.

---

## 📐 Network Architecture & Tools

| Component / Tool | Details |
| :--- | :--- |
| **Target Subnet** | `Subnet-Internal` (`10.0.0.0/24`) inside `VNet-AD-Project` |
| **Network Security Group** | `NSG-Internal` |
| **Target Virtual Machine** | Windows 10 / Windows Server Endpoint (`10.0.0.5`) |
| **Inspection Tool** | Wireshark Packet Analyzer |
| **Inspected Protocols** | ICMP, RDP (3389), SSH (22), DNS (53), HTTP/HTTPS |

---

## 🛠️ Step-by-Step Implementation & Inspection Guide

### Step 1: Network Infrastructure & Environment Preparation
1. Ensure `Client-01` (Windows 10 endpoint) and `DC-01` are provisioned inside `Subnet-Internal` (`10.0.0.0/24`).
2. Verify `NSG-Internal` is associated with `Subnet-Internal` or attached directly to `Client-01`'s Network Interface Card (NIC).
3. Connect to `Client-01` via Remote Desktop Protocol (RDP).

---

### Step 2: Wireshark Setup & Baseline Capture
1. Download and install **Wireshark** on `Client-01`.
2. Launch Wireshark as Administrator and select the primary Ethernet network adapter (`eth0`).
3. Click the green shark fin icon to start capturing packets.
4. Apply an initial display filter in the Wireshark filter bar to isolate relevant traffic:
   ```text
   icmp or dns or rdp

```

---

### Step 3: ICMP Traffic & NSG Firewall Rule Testing

1. Open Command Prompt on `Client-01` and initiate a continuous ping to a public IP address (e.g., Google DNS `8.8.8.8`):
```cmd
ping 8.8.8.8 -t

```


2. Verify active `Echo (ping) request` and `Echo (ping) reply` packets appear in real time inside Wireshark.
3. Open the **Azure Portal** in a web browser $\rightarrow$ Navigate to **Network Security Groups** $\rightarrow$ Select `NSG-Internal`.
4. Navigate to **Inbound security rules** $\rightarrow$ Click **+ Add** and configure a rule to block ping requests:
* **Source:** Any
* **Source port ranges:** `*`
* **Destination:** Any
* **Service:** Custom
* **Protocol:** ICMP
* **Action:** Deny
* **Priority:** 100
* **Name:** `Deny-ICMP-Inbound`


5. Click **Add** and observe the Command Prompt window on `Client-01`:
* The continuous ping output changes from replies to `Request timed out`.


6. Examine Wireshark:
* Outbound `Echo (ping) request` packets are still sent by `Client-01`, but no `Echo (ping) reply` packets return because the inbound reply traffic is dropped by the Azure NSG rule.


7. Return to Azure Portal, change the rule action to **Allow** (or delete the rule), and verify in Command Prompt that ping replies resume immediately.

---

### Step 4: Remote Access Protocol Analysis (RDP & SSH)

1. **Inspecting RDP (3389):**
* Change the Wireshark display filter to:
```text
tcp.port == 3389

```


* Move your mouse or open windows inside the RDP session.
* Observe the continuous stream of TCP traffic maintaining the encrypted graphical interface session between your local management workstation and the Azure VM.


2. **Inspecting SSH (22):**
* Provision or locate a secondary Linux Virtual Machine (`10.0.0.6`) within the VNet.
* Open PowerShell or Command Prompt on `Client-01` and initiate an SSH session:
```bash
ssh azureuser@10.0.0.6

```


* Update the Wireshark filter to:
```text
tcp.port == 22

```


* Observe the initial TCP 3-way handshake (`SYN`, `SYN-ACK`, `ACK`), SSH version exchange, key exchange protocol (`Diffie-Hellman`), and subsequent encrypted payload packets.



---

### Step 5: DNS Query Inspection

1. Open Command Prompt on `Client-01` and flush the local DNS resolver cache:
```cmd
ipconfig /flushdns

```


2. Update the Wireshark display filter to capture standard DNS traffic:
```text
dns

```


3. Execute domain lookups using `nslookup`:
```cmd
nslookup [www.microsoft.com](https://www.microsoft.com)
nslookup [www.google.com](https://www.google.com)

```


4. Examine the captured UDP packets:
* **Standard Query:** Source port (random high port) $\rightarrow$ Destination port 53 (Domain Controller `10.0.0.4` or configured DNS server).
* **Standard Query Response:** Details the A/AAAA records returned by the DNS server.



---

## ✅ Verification & Security Insights

* **Firewall Enforce Latency:** NSG rule changes propagate across Azure network interfaces within seconds, instantly interrupting or restoring traffic flows.
* **Stateful Filtering:** Observed that Azure NSGs function as stateful firewalls—allowing outbound requests automatically permits matching inbound responses unless explicitly blocked by incoming inspection rules.
* **Network Visibility:** Wireshark confirmed cleartext protocol behavior vs. encrypted channels (e.g., unencrypted ICMP echo requests vs. encrypted RDP/SSH payload data).

---

## 📌 Key Takeaways

* **Defense-in-Depth:** Azure Network Security Groups provide essential Layer 3/4 network filtration for cloud infrastructure.
* **Diagnostic Capability:** Protocol analysis using Wireshark enables precise troubleshooting of dropped connections, misconfigured firewall priorities, and unauthorized network traffic.
