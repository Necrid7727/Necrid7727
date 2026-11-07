# 🏛️ Configuring On-Premises Active Directory within Azure Virtual Machines

## 🎯 Objective
This project demonstrates the deployment and configuration of an on-premises Active Directory Domain Services (AD DS) environment hosted inside Microsoft Azure. The goal was to establish a fully functional domain ecosystem, join client virtual machines, implement Organizational Units (OUs), and automate bulk user creation using PowerShell.

---

## 📐 Architecture & Environment

| Resource | Configuration / Details |
| :--- | :--- |
| **Resource Group** | `RG-AD-Project` |
| **Virtual Network** | `VNet-AD-Project` (`10.0.0.0/16`) |
| **Subnet** | `Subnet-Internal` (`10.0.0.0/24`) |
| **Domain Controller (DC)** | Windows Server 2022 (`DC-01`, Private IP: `10.0.0.4`) |
| **Client Endpoint** | Windows 10 Pro (`Client-01`, Private IP: `10.0.0.5`) |
| **Target Domain** | `mydomain.com` (or your custom domain) |

---

## 🛠️ Implementation Steps

### Step 1: Network Infrastructure & Virtual Machines
1. Created Resource Group `RG-AD-Project` and deployed `VNet-AD-Project`.
2. Provisioned `DC-01` (Windows Server 2022) with a Static Private IP address (`10.0.0.4`).
3. Provisioned `Client-01` (Windows 10) on the same subnet.
4. Set `VNet-AD-Project` Custom DNS to point directly to `DC-01` (`10.0.0.4`) to enable domain lookup for client machines.

---

### Step 2: Active Directory Installation & Domain Promotion
1. RDP into `DC-01`.
2. Installed **Active Directory Domain Services (AD DS)** via Server Manager.
3. Promoted `DC-01` to a Domain Controller for a new forest named `mydomain.com`.
4. Configured DSRM password and rebooted the server to finalize promotion.

---

### Step 3: OU Hierarchy & Bulk User Creation
1. Opened **Active Directory Users and Computers** (`dsa.msc`).
2. Created custom Organizational Units:
   * `_EMPLOYEES`
     * `_ADMINS`
     * `_USERS`
3. Executed PowerShell script to generate sample accounts for testing domain authentication:

```powershell
# Create Bulk AD Users
$Password = ConvertTo-SecureString "P@ssword2026!" -AsPlainText -Force
$OUPath = "OU=_USERS,OU=_EMPLOYEES,DC=mydomain,DC=local"

1..10 | ForEach-Object {
    $Username = "testuser_$_"
    New-ADUser -Name $Username `
               -SamAccountName $Username `
               -UserPrincipalName "$Username@mydomain.local" `
               -Path $OUPath `
               -AccountPassword $Password `
               -Enabled $true `
               -ChangePasswordAtLogon $false
}
