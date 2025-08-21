# Azure IAM Lab Evidence

This README serves as a template to organize your Azure IAM lab evidence. Replace each `<...>` placeholder with your own information and update the screenshot filenames to match the files in your repository.

## 1. Tenant and User Setup
- **Screenshot file:** `<tenant-users.png>`
- **Description:** Show the Microsoft Entra admin center with your lab tenant domain (`iamsecuresolutionsproton.onmicrosoft.com`) and the list of test users (user1–user5).
- **Notes:** Highlight that the users are created with UPNs user1@… etc and assigned passwords.

## 2. Group Creation
- **Screenshot file:** `<groups.png>`
- **Description:** Show creation of the IAM‑Users and IAM‑Admins groups. Include membership lists for each group.
- **Notes:** Mention that role‑assignable groups were unavailable.

## 3. Role Assignment
- **Screenshot file:** `<role-assignment.png>`
- **Description:** Show the User Administrator role assigned to your admin account.
- **Notes:** Identify where the role assignment was applied.

## 4. Graph/PowerShell Setup
- **Screenshot file:** `<powershell-login.png>`
- **Description:** Capture your PowerShell/Cloud Shell session connecting to the correct tenant via `Connect‑MgGraph` and verifying tenant context.
- **Notes:** Mention how you resolved module loading issues and ensured idempotent scripts.

## 5. Windows Client Enrollment
- **Screenshot files:** `<windows-join-wizard.png>`, `<entra-devices.png>`, `<dsregcmd.png>`
- **Description:** Show your Windows 11 Pro client being converted from “Azure AD registered” to “Microsoft Entra joined.” Include the join wizard, the Devices list in the portal, and the `dsregcmd /status` output confirming AzureADJoin = YES.

## 6. Identity Protection Awareness
- **Screenshot file:** `<identity-protection-dashboard.png>`
- **Description:** Show the Identity Protection blade and note any licensing/tenant issues you resolved (e.g., second trial tenant).
- **Notes:** Describe how you confirmed you were working in the correct tenant.

## 7. Conditional Access – Require MFA
- **Screenshot file:** `<ca-policy.png>`
- **Description:** Show the CA policy targeting the IAM‑Users group, excluding IAM‑Admins, applying to all cloud apps, with the Grant control “Require multi‑factor authentication.”
- **Notes:** Capture the final summary page before creating/enabling the policy.

## 8. Sign‑in Log (MFA Proof)
- **Screenshot file:** `<signin-log-mfa.png>`
- **Description:** Capture a sign‑in event for one of your test users showing that MFA was required/enforced.
- **Notes:** Highlight the conditional access policy in effect.

## 9. Identity Protection Policies
- **Screenshot files:** `<sign-in-risk-policy.png>`, `<user-risk-policy.png>`
- **Description:** Show configuration of the Sign‑in risk policy and User risk policy, including the conditions and controls you set.
- **Notes:** Summarize why you chose the settings and how they enforce security.

## Additional Notes
Use this section to record any other observations, challenges, or decisions made during the lab.
