## Phase 1 – Azure Network Setup

### Step 1 – Resource Group overview

In this step you create an Azure resource group to hold all of the project resources. The screenshot below shows the overview for the newly created resource group.

[![Phase 1 Step 1 – Resource Group overview](assets/Phase1-Step1_resource-group_overview.png)](assets/Phase1-Step1_resource-group_overview.png)

### Step 2 – VNet & Subnet configuration

Next, you configure the virtual network and subnet that the machines in this project will use. The image demonstrates the VNet and subnet settings as they were configured.

[![Phase 1 Step 2.2 – VNet & Subnet configuration](assets/Phase1-Step2.2_vnet_subnet_config.png)](assets/Phase1-Step2.2_vnet_subnet_config.png)

## Phase 2 – Identity Management

### Step 1 – Users created list

During Phase 2 you create a set of test users in Azure AD. Here is the list of users after creation:

[![Phase 2 Step 1 – Users created list](assets/Phase2-Step1_users_created_list.png)](assets/Phase2-Step1_users_created_list.png)

### Step 1B – Reset password

As part of onboarding these users, you reset one of the user passwords. The following screenshot shows the reset‑password dialog in Azure AD.

[![Phase 2 Step 1B – Reset password](assets/Phase2-Step1B_reset-password.png)](assets/Phase2-Step1B_reset-password.png)

### Step 2 – Group membership

You assign the new users to appropriate groups. Two separate screenshots document the membership lists for the admins and the general users:

[![Phase 2 Step 2 – Groups members (admins)](assets/Phase2-Step2_groups_members_admins.png)](assets/Phase2-Step2_groups_members_admins.png)

[![Phase 2 Step 2 – Groups members (users)](assets/Phase2-Step2_groups_members_users.png)](assets/Phase2-Step2_groups_members_users.png)

### Step 3 – Role assignment

In the final step of Phase 2 you assign the **User administrator** role to one of the accounts. The following image confirms that the role assignment was successful:

[![Phase 2 Step 3 – Role user admin assigned](assets/Phase2-Step3_role_useradmin_assigned.png)](assets/Phase2-Step3_role_useradmin_assigned.png)

## Phase 3 – Device join & Conditional Access

### Step 1 – Device join wizard

After deploying the network and identity setup you join a Windows device to Azure AD. The device join wizard steps through the join process:

[![Phase 3 Step 1 – Device join wizard](assets/Phase3-Step1_device_join_wizard.png)](assets/Phase3-Step1_device_join_wizard.png)

### Step 1 – Device list

Once the device is joined, it appears in the Azure AD devices list. The screenshot below shows the list with the newly joined device highlighted:

[![Phase 3 Step 1 – Device list](assets/Phase3-Step1_device_list.png)](assets/Phase3-Step1_device_list.png)

### Step 1 – `dsregcmd` status

On the joined device you run the `dsregcmd /status` command to verify the hybrid join status. The image captures the command output confirming that the device is Azure AD joined:

[![Phase 3 Step 1 – `dsregcmd` status](assets/Phase3-Step1_dsregcmd_status.png)](assets/Phase3-Step1_dsregcmd_status.png)

### Step 2 – Conditional Access policies

During the final phase you configure conditional‑access policies to protect the environment. Each policy is documented with a separate screenshot.

#### ID Protection dashboard

The ID Protection dashboard provides an overview of risky sign‑ins and users. Here’s what it looked like after enabling risk policies:

[![Phase 3 Step 2 – ID Protection dashboard](assets/Phase3-Step2_ID-Protection_Dashboard.png)](assets/Phase3-Step2_ID-Protection_Dashboard.png)

#### Sign‑in risk policy (On)

This policy enforces additional controls when a high sign‑in risk is detected. The image shows the policy turned on:

[![Phase 3 Step 2 – Sign‑in risk policy (On)](assets/Phase3-Step2_Sign-in-Risk-Policy_On.png)](assets/Phase3-Step2_Sign-in-Risk-Policy_On.png)

#### User risk policy (On)

The user risk policy requires multi‑factor authentication for users with an elevated risk. The screenshot confirms that the policy is enabled:

[![Phase 3 Step 2 – User risk policy (On)](assets/Phase3-Step2_User-Risk-Policy_On.png)](assets/Phase3-Step2_User-Risk-Policy_On.png)

#### Conditional Access policy list

You review the list of Conditional Access policies to verify that all required policies are configured:

[![Phase 3 Step 2 – Conditional Access policy list](assets/phase03_ca-list.png)](assets/phase03_ca-list.png)

#### Named locations

The **Named locations** blade defines trusted IP ranges and country locations used by Conditional Access. The following screenshot shows the configured named locations:

[![Phase 3 Step 2 – Named locations](assets/phase03_named-locations.png)](assets/phase03_named-locations.png)

#### CA02: Block legacy authentication

This Conditional Access policy blocks legacy (basic) authentication protocols to improve security:

[![Phase 3 Step 2 – CA02: Block legacy authentication](assets/Phase3-Step2_CA02_BlockLegacyAuth.png)](assets/Phase3-Step2_CA02_BlockLegacyAuth.png)

#### CA03: Require MFA for admin roles

Finally, you create a policy that requires multi‑factor authentication for all administrative roles. The screenshot shows the policy configuration:

[![Phase 3 Step 2 – CA03: Require MFA for admin roles](assets/Phase3-Step2_CA03_MFAforAdmins.png)](assets/Phase3-Step2_CA03_MFAforAdmins.png)

#### CA – Require MFA for admin roles (everywhere)

In some environments you may need a tenant‑wide policy that enforces multi‑factor authentication for admin roles across all scopes. The screenshot below shows the **Require MFA for admin roles (everywhere)** policy configuration:

[![CA – Require MFA for admin roles (everywhere)](assets/CA - Require MFA for admin roles (everywhere).png)](assets/CA - Require MFA for admin roles (everywhere).png)
