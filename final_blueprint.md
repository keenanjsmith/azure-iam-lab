# Azure Entra ID – Cloud Only Lab

This lab gives you a complete blueprint to build your own Azure Entra ID lab for identity and access management.  It’s written so anyone can follow along, even if they’ve never set up an Entra tenant before.  Each section explains **why** you’re doing something, **where** to click, what to **do**, what to **check**, and which **screenshot** corresponds to that step.

## What this lab shows

- **Cloud resources** – Create a dedicated resource group and a virtual network with a subnet for any client virtual machines you may need.
- **Entra ID identities** – Add lab users and groups and delegate a directory role using least privilege.
- **Device join** – Register a Windows 11 device to Entra ID and verify it’s joined.
- **Identity Protection** – Configure risk‑based sign‑in and user policies.
- **Conditional Access** – Create baseline policies: block legacy authentication, require MFA, and define trusted locations.

## What you need

- An Azure subscription with a Microsoft Entra tenant
- Global Administrator rights on that tenant
- A Windows 11 Pro device or virtual machine to join and test sign‑ins
- Browser access to the Entra admin center

## Screenshot naming and repo layout

Use the following convention so files sort in order:

```
PhaseX‑StepY.Z_<short‑title>.png
```

Expected repository layout:

```
README.md
/assets
  Phase1‑Step1_resource‑group_overview.png
  Phase1‑Step2.2_vnet_subnet_config.png
  Phase2‑Step1_users_created_list.png
  Phase2‑Step1B_reset‑password.png
  Phase2‑Step2_groups_members_admins.png
  Phase2‑Step2_groups_members_users.png
  Phase2‑Step3_role_useradmin_assigned.png
  phase03_ca‑list.png
  phase03_named‑locations.png
  Phase3‑Step1_device_join_wizard.png
  Phase3‑Step1_device_list.png
  Phase3‑Step1_dsregcmd_status.png
  Phase3‑Step2_CA02_BlockLegacyAuth.png
  Phase3‑Step2_CA03_MFAforAdmins.png
  Phase3‑Step2_ID‑Protection_Dashboard.png
  Phase3‑Step2_Sign‑in‑Risk‑Policy_On.png
  Phase3‑Step2_User‑Risk‑Policy_On.png
  CA – Require MFA for admin roles (everywhere).png
```

---

## Phase 1 – Azure core

### Step 1.1 – Create a resource group

**Why:** Keep all lab resources in one place and make cleanup easy.

**Where:** Azure Portal → **Resource groups** → **Create**

**Do this:**

- Name the group **IAM‑Lab‑RG**.
- Choose a region close to you.
- Select your subscription, then choose **Review + Create**, and **Create**.

**Check:** The new resource group appears with the correct name, subscription, and region.

**Screenshot:** `assets/Phase1‑Step1_resource‑group_overview.png`

---

### Step 1.2 – Create a virtual network and subnet

**Why:** Provide a private address space for any lab client VMs.

**Where:** Resource group **IAM‑Lab‑RG** → **Create** → **Virtual network**

**Do this:**

- Name the VNet **IAM‑VNet**.
- Set the address space, for example `10.10.0.0/16`.
- Define a subnet named **IAM‑Subnet**, for example `10.10.1.0/24`.
- Choose **Review + Create**, then **Create**.

**Check:** The VNet and subnet exist and show the ranges you set.

**Screenshot:** `assets/Phase1‑Step2.2_vnet_subnet_config.png`

---

## Phase 2 – Entra identity

### Step 2.1 – Create test users

**Why:** You need non‑admin accounts to target with policies and to use for sign‑in tests.

**Where:** Entra admin center → **Identity** → **Users** → **New user**

**Do this:**

- Create **user1** through **user5** in your tenant’s `onmicrosoft.com` domain.
- Assign each a temporary password for the lab.

**Check:** `user1` through `user5` appear in the Users list with the expected UPNs.

**Screenshot:** `assets/Phase2‑Step1_users_created_list.png`

---

### Step 2.1b – Reset a user password

**Why:** Demonstrate basic onboarding and password recovery.

**Where:** **Users** → select **user1** → **Reset password**

**Do this:**

- Generate a temporary password.

**Check:** A success banner displays a new temporary password.

**Screenshot:** `assets/Phase2‑Step1B_reset‑password.png`

---

### Step 2.2 – Create groups and add members

**Why:** Group‑based scope keeps policies clear and avoids one‑off assignments.

**Where:** **Identity** → **Groups** → **New group**

**Do this:**

- Create two security groups: **IAM‑Admins** and **IAM‑Users**.
- Add your admin account to **IAM‑Admins**.
- Add `user1` through `user5` to **IAM‑Users**.

**Check:** Memberships look correct in both groups.

**Screenshot:**

- `assets/Phase2‑Step2_groups_members_admins.png`
- `assets/Phase2‑Step2_groups_members_users.png`

---

### Step 2.3 – Assign a directory role

**Why:** Apply least privilege by delegating only the rights needed for user management.

**Where:** **Identity** → **Roles and administrators** → **User Administrator** → **Assignments** → **Add assignment**

**Do this:**

- Assign the **User Administrator** role to your admin account, or to the **IAM‑Admins** group if role‑assignable groups are available in your tenant.

**Check:** The assignment appears under the role with the correct principal and scope.

**Screenshot:** `assets/Phase2‑Step3_role_useradmin_assigned.png`

---

## Phase 3 – Device enrollment

### Step 3.1 – Join a Windows 11 device to Entra ID

**Why:** Device trust is a requirement for many Conditional Access rules.

**Where:** Windows 11 → **Settings** → **Accounts** → **Access work or school** → **Connect** → **Join this device to Microsoft Entra**

**Do this:**

- Sign in with an account that’s allowed to join devices in your tenant.
- Complete the join wizard and restart if prompted.

**Check:**

- In Entra, go to **Identity** → **Devices** and ensure the device is listed as **Microsoft Entra joined**.
- Open an elevated Command Prompt on the device and run `dsregcmd /status`.  Look for `AzureAdJoined : YES` and a successful device authentication status.

**Screenshot:**

- `assets/Phase3‑Step1_device_join_wizard.png`
- `assets/Phase3‑Step1_device_list.png`
- `assets/Phase3‑Step1_dsregcmd_status.png`

---

## Phase 3 – Identity Protection

### Step 3.2 – Open the Identity Protection dashboard

**Why:** This is where Microsoft surfaces risk detections and lets you configure risk policies.

**Where:** **Protection** → **Identity Protection** → **Dashboard**

**Check:** Tiles for detections, risky users, and policies are present.

**Screenshot:** `assets/Phase3‑Step2_ID‑Protection_Dashboard.png`

---

### Step 3.2 – Configure risk policies

**Why:** Use risk signals to add MFA or force a password change when appropriate.

**Where:** **Identity Protection** → **Sign‑in risk policy** and **User risk policy**

**Do this:**

- For the **Sign‑in risk policy**, include the **IAM‑Users** group, set the control to **Require MFA**, and choose **On** or **Report‑only** to test.
- For the **User risk policy**, include the **IAM‑Users** group, set the control to **Require password change**, and choose **On** or **Report‑only** to test.

**Check:** Each policy shows the correct target group and the control you selected.

**Screenshot:**

- `assets/Phase3‑Step2_Sign‑in‑Risk‑Policy_On.png`
- `assets/Phase3‑Step2_User‑Risk‑Policy_On.png`

---

## Phase 3 – Conditional Access

### Named locations and policy list

**Why:** Trusted locations allow safe exceptions, and the policy list gives a high‑level view of what’s enabled.

**Where:** **Protection** → **Conditional Access** → **Named locations**, then **Policies**

**Do this:**

- Add a Named location for your home IP range and mark it as trusted.
- Open the **Policies** blade to view the full list of Conditional Access policies.

**Check:** The trusted location is present and the policies list loads.

**Screenshot:**

- `assets/phase03_named‑locations.png`
- `assets/phase03_ca‑list.png`

---

### CA02 – Block legacy authentication

**Why:** Legacy clients can’t perform MFA; blocking them reduces attack surface.

**Where:** **Conditional Access** → **Policies** → **New policy**

**Do this:**

- Name the policy **CA02 Block Legacy Auth**.
- Users: include the **IAM‑Users** group.
- Cloud apps: **All cloud apps**.
- Conditions: **Client apps**, select only **Legacy authentication clients**.
- Grant: **Block access**.
- Enable: **On**, or **Report‑only** if testing.

**Check:** The policy shows the legacy client condition and is enabled as configured.

**Screenshot:** `assets/Phase3‑Step2_CA02_BlockLegacyAuth.png`

---

### CA03 – Require MFA for Admins

**Why:** Admin accounts need stronger authentication to mitigate misuse.

**Where:** **Conditional Access** → **Policies**

**Do this:**

- Name the policy **CA03 Require MFA for Admins**.
- Users: include your admin account or the **IAM‑Admins** group.
- Cloud apps: **All cloud apps**.
- Grant: **Require multifactor authentication**.
- Enable: **On**.

**Check:** The assignments and grant control align with the intended design.

**Screenshot:** `assets/Phase3‑Step2_CA03_MFAforAdmins.png`

---

### Role‑based – Require MFA for admin roles

**Why:** Protects directory roles themselves so that any user assigned to those roles must satisfy MFA.

**Where:** **Conditional Access** → **Policies**

**Do this:**

- Create a policy targeting selected directory roles (for example **Global Administrator** and **User Administrator**).
- Grant: **Require MFA**.
- Enable: **On**.

**Check:** The policy shows role‑based targeting with the MFA requirement.

**Screenshot:** `assets/CA – Require MFA for admin roles (everywhere).png`

---

## Screenshot checklist

- `Phase1‑Step1_resource‑group_overview.png` – Resource group created with name, subscription and region
- `Phase1‑Step2.2_vnet_subnet_config.png` – VNet and subnet with CIDR ranges
- `Phase2‑Step1_users_created_list.png` – Test users (`user1`‑`user5`) created
- `Phase2‑Step1B_reset‑password.png` – Password reset success banner
- `Phase2‑Step2_groups_members_admins.png` – `IAM‑Admins` membership verified
- `Phase2‑Step2_groups_members_users.png` – `IAM‑Users` membership verified
- `Phase2‑Step3_role_useradmin_assigned.png` – User Administrator role assignment
- `phase03_ca‑list.png` – Inventory of Conditional Access policies
- `phase03_named‑locations.png` – Trusted Home IP configured
- `Phase3‑Step1_device_join_wizard.png` – Device join wizard
- `Phase3‑Step1_device_list.png` – Device object visible in Entra
- `Phase3‑Step1_dsregcmd_status.png` – `AzureAdJoined : YES` and device auth success
- `Phase3‑Step2_CA02_BlockLegacyAuth.png` – Legacy clients blocked
- `Phase3‑Step2_CA03_MFAforAdmins.png` – Admin MFA policy
- `Phase3‑Step2_ID‑Protection_Dashboard.png` – Identity Protection home
- `Phase3‑Step2_Sign‑in‑Risk‑Policy_On.png` – Sign‑in risk policy configured
- `Phase3‑Step2_User‑Risk‑Policy_On.png` – User risk policy configured
- `CA – Require MFA for admin roles (everywhere).png` – Role‑based MFA policy

---

## Tips

- Always confirm the tenant name displayed in the portal header before making changes.
- Start new Conditional Access and Identity Protection policies in **Report‑only** mode, review sign‑in logs, then switch to **On** once validated.
- Keep a single “break‑glass” admin account excluded from Conditional Access and risk policies and protect it with a strong, unique password stored offline.
