# 🎫 osTicket: Ticket Lifecycle & Workflow Implementation

## 📝 Project Summary

### Description
This project is an operational walkthrough demonstrating the complete lifecycle management of IT support tickets inside osTicket. It covers ticket creation via client portal, SLA calculation, ticket assignment, department escalation via internal communications, customer status updates, and formal ticket closure.

### Languages Used
* **Markdown:** Documentation and scenario formatting.

### Environments Used
* **Windows 10 / Windows Server 2022** (Virtual Machine)
* **osTicket Staff Portal (`/scp`) & Customer Portal**

### Technologies & Applications Used
* **osTicket ITSM Workflow Engine**
* **Active Directory Users and Computers** (for backend simulation)

---

## 🛠️ Step-by-Step Written Demonstration

### Scenario 1: Account Lockout (Standard Tier 1 Ticket)

#### 1. End-User Ticket Creation
1. Open web browser and navigate to public portal `http://localhost/osticket/`.
2. Click **Open a New Ticket**.
3. Fill in ticket details:
   * **Email Address:** `alice.johnson@example.com`
   * **Full Name:** `Alice Johnson`
   * **Help Topic:** `Account Access / Password Reset`
   * **Issue Summary:** `Domain account locked out after returning from leave`
4. Click **Create Ticket**.

<img width="2752" height="1536" alt="Gemini_Generated_Image_3qyzqn3qyzqn3qyz" src="https://github.com/user-attachments/assets/be43b20e-e776-4fb3-a3d7-13e5655c7645" />


5. **Expected Result:** Ticket `#100492` is generated and assigned to **Tier 1 Helpdesk** under **Standard SLA**.

#### 2. Agent Processing & Resolution
1. Log into `/scp` as Agent `Jane Doe` (`jdoe`).
2. Click **Tickets** $\rightarrow$ Click **Unassigned** $\rightarrow$ Click Ticket `#100492`.
3. In the right pane, click **Claim** to assign the ticket to `Jane Doe`.

<img width="2752" height="1536" alt="Gemini_Generated_Image_pp1dhpp1dhpp1dhp" src="https://github.com/user-attachments/assets/955f8b05-d2e3-46c6-9c33-b87c59b33031" />


4. Open Active Directory on `DC-01`, locate user `alice.johnson`, right-click $\rightarrow$ **Reset Password**, and uncheck "Account is Locked Out".
5. Return to osTicket, select **Post Reply**, and enter response:
   > *"Hello Alice, your Active Directory account has been unlocked and your password reset. Please test logging in at your workstation."*
6. Set Ticket Status dropdown to **Closed**, then click **Post Reply**.
7. **Expected Result:** Ticket moves to Closed status; customer receives automated resolution email.

---

### Scenario 2: Network Outage (Emergency Escalation Ticket)

#### 1. Ticket Submission & Auto-Routing
1. Navigate to `http://localhost/osticket/` $\rightarrow$ Click **Open a New Ticket**.
2. Enter User details: `Bob Smith` (`bob.smith@example.com`).
3. Select Help Topic: `Network Connectivity`.
4. Enter Issue Summary: `Entire 3rd floor switch down - no internet or server access`. Click **Create Ticket**.
5. **Expected Result:** Ticket `#100493` is generated and automatically routed to **Networking** under **Emergency SLA (1-hour grace period)**.

#### 2. Triage & Department Escalation
1. Log into `/scp` as Agent `Jane Doe`. Open Ticket `#100493`.
2. Click **Post Internal Note**. Type message:
   > *"Preliminary check completed; 3rd floor switch unresponsive to ping. Escalating to System Administrators for physical inspection."* Click **Post Note**.
3. In the Ticket Actions toolbar, click **Transfer**.
4. Select New Department: `System Administrators`. Enter Transfer Reason: `Physical hardware troubleshooting required`. Click **Transfer**.

<img width="2752" height="1536" alt="Gemini_Generated_Image_msv5etmsv5etmsv5" src="https://github.com/user-attachments/assets/3ff87ae3-6b03-48cf-843e-6a08e2645dd2" />


5. Log out of `/scp` and log in as Lead Admin `John Smith` (`jsmith`).
6. Open Ticket `#100493` under `System Administrators` queue.
7. Click **Claim**, swap out failed switch power supply in datacenter, and post customer reply:
   > *"The core switch power unit on Floor 3 has been replaced. Network connectivity and local server share access are fully restored."*
8. Change Ticket Status to **Closed** and click **Post Reply**.

<p align="center">
  <img src="./screenshots/04-ticket-resolution-closed.png" alt="Ticket Closed with Audit Trail" width="700" />
</p>

9. **Expected Result:** Complete activity timeline (notes, transfers, status updates, timestamps) is permanently recorded in ticket history log.
---

## 📌 Key Takeaways
* **Clear Audit Trails:** Comprehensive ticket logs provide historical visibility into system infrastructure failures and agent response metrics.
* **Communication Segregation:** Internal notes ensure confidential technical discussion remains internal while providing transparent customer updates.
