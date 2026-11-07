
# ⚙️ osTicket: Post-Installation Configuration

## 🎯 Objective
This project demonstrates administrative configuration of the osTicket ITSM system following initial setup. The goal was to establish organizational structure, roles, SLA tiers, agent accounts, and ticket routing rules to mirror a functioning enterprise IT department.

---

## 🛠️ Step-by-Step Implementation Guide

### Step 1: Admin Panel Navigation & System Settings
1. Log into the osTicket staff portal (`http://localhost/osticket/scp/`).
2. Switch to the **Admin Panel** using the top-right toggle.
3. Under **Settings** $\rightarrow$ **Company**, customize site name, system emails, and default landing page rules.

---

### Step 2: Configure Roles, Departments & Teams
1. **Roles:**
   * Navigate to **Agents** $\rightarrow$ **Roles**.
   * Created **Supreme Admin** (full permissions) and **Tier 1 Support** (limited to ticket viewing, editing, and replying).
2. **Departments:**
   * Navigate to **Agents** $\rightarrow$ **Departments**.
   * Created functional departments:
     * `System Administrators`
     * `Networking`
     * `Tier 1 Helpdesk`
3. **Teams:**
   * Navigate to **Agents** $\rightarrow$ **Teams**.
   * Created cross-departmental working teams (e.g., `Hardware Escalations`).

---

### Step 3: Service Level Agreements (SLAs) & Help Topics
1. **SLA Plans:**
   * Navigate to **Manage** $\rightarrow$ **SLA Plans**.
   * Configured response time windows:
     * **Emergency:** 1-hour grace period, 24/7 schedule.
     * **Standard:** 8-hour grace period, business hours schedule.
2. **Help Topics:**
   * Navigate to **Manage** $\rightarrow$ **Help Topics**.
   * Created user-selectable ticket categories mapped to default departments and SLAs:
     * `Business Systems / ERP` $\rightarrow$ System Administrators (Emergency SLA)
     * `Hardware / Printer Issue` $\rightarrow$ Tier 1 Helpdesk (Standard SLA)
     * `Network Connectivity` $\rightarrow$ Networking (Emergency SLA)

---

### Step 4: Provisioning Agents & End Users
1. **Agent Provisioning:**
   * Navigate to **Agents** $\rightarrow$ **Add New Agent**.
   * Created staff accounts:
     * `Jane Doe` (Department: Tier 1 Helpdesk, Role: Tier 1 Support)
     * `John Smith` (Department: System Administrators, Role: Supreme Admin)
   * Sent password reset/activation emails to establish credentials.
2. **User Registration Settings:**
   * Navigate to **Settings** $\rightarrow$ **Users**.
   * Set user registration preferences to require authentication or allow guest ticket creation.

---

## ✅ Verification

* Confirmed that Help Topics properly route incoming requests to designated departments upon submission.
* Verified SLA escalation timers start upon ticket creation based on category selection.

---

## 📌 Key Takeaways
* **ITSM Structure:** Proper assignment of roles and departments prevents ticket bottlenecks and limits security exposure.
* **Automated Routing:** Pre-configured Help Topics reduce manual ticket sorting time for support staff.
