# 🔐 Microsoft Entra ID: Multi-Factor Authentication  
*Administrator's Guide*

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Prerequisites](#prerequisites)  
3. [Accessing MFA Controls](#accessing-mfa-controls-in-microsoft-entra-id)  
4. [Enabling MFA for Your Organization](#enabling-mfa-for-your-organization)  
5. [Setting Up Authentication Methods](#setting-up-authentication-methods)  
6. [Creating Conditional Access Policies](#creating-conditional-access-policies)  
7. [Managing User Enrollment](#managing-user-enrollment)  
8. [Reporting and Monitoring](#reporting-and-monitoring)  
9. [Troubleshooting Common Issues](#troubleshooting-common-issues)  
10. [Best Practices](#best-practices)  
11. [Security Considerations](#security-considerations)  
12. [Automation and PowerShell Examples](#automation-and-powershell-examples)  
13. [Change Management & Rollback Options](#change-management--rollback-options)

---

## 📘 Introduction

Multi-Factor Authentication (MFA) strengthens organizational security by requiring users to verify their identity through additional verification methods. This guide is designed for IT administrators managing identity access within Microsoft Entra ID.

> *Use case: This guide is ideal for organizations beginning their MFA rollout, transitioning from legacy authentication, or tightening compliance with security standards.*

---

## ⚙️ Prerequisites

| Requirement              | Details                                                |
|--------------------------|--------------------------------------------------------|
| **Licensing**            | Microsoft Entra ID (Free, P1, or P2)                   |
| **Role Access**          | Global or Security Administrator                       |
| **User Setup**           | Users must exist in the tenant                         |
| **Browser**              | Microsoft Edge or Google Chrome recommended            |

> *Note: Conditional Access and advanced MFA controls require Premium P1 or P2.*

---

## 🛠️ Accessing MFA Controls in Microsoft Entra ID

### Step 1: Sign In

1. Visit *https://entra.microsoft.com*  
2. Sign in with a valid administrator account  
3. Complete MFA if prompted

**🖼️ Screenshot: Microsoft Entra Admin Center login page**

---

### Step 2: Navigate to Authentication Settings

- *Protection → Authentication methods*  
- *Identity → Protection → Authentication methods*

**🖼️ Screenshot: Microsoft Entra admin center navigation menu**

---

## 🔧 Enabling MFA for Your Organization

### Method 1: Enable Security Defaults

1. Navigate to *Properties > Manage security defaults*  
2. Set the toggle to *Yes*  
3. Click *Save*

> ⚠️ *Security Defaults enforce MFA for all users with no exceptions.*

**🖼️ Screenshot: Security defaults toggle**

---

### Method 2: Use Conditional Access (Recommended)

Provides flexibility for targeting users, apps, and conditions. Setup details covered in [Creating Conditional Access Policies](#creating-conditional-access-policies).

---

## 🔑 Setting Up Authentication Methods

| Method                    | Security Level | Use Case                                 |
|---------------------------|----------------|-------------------------------------------|
| Microsoft Authenticator   | High            | Default recommended method                |
| FIDO2 Security Keys       | High            | Phishing-resistant MFA for secure sites   |
| OATH Hardware Tokens      | Medium-High     | Offline/air-gapped scenarios              |
| SMS                       | Medium          | Legacy fallback option                    |
| Voice Calls               | Medium          | Secondary option                          |
| Email OTP                 | Low             | Only as a backup                          |

### Configuration Steps

1. Go to *Protection > Authentication methods > Policies*  
2. Select a method  
3. Define the target users or groups  
4. Save settings

**🖼️ Screenshot: Authentication methods policy screen**

---

### Microsoft Authenticator Setup

1. Select *Microsoft Authenticator*  
2. Enable the method  
3. Target *All users* or *Select users*  
4. Recommended settings:
   - *Show app name in push notifications*
   - *Require number matching*  
5. Click *Save*

**🖼️ Screenshot: Microsoft Authenticator configuration panel**

---

### SMS Authentication Setup

1. Select *SMS*  
2. Enable the method  
3. Choose users or groups  
4. Click *Save*

Repeat for other methods as needed.

---

## 📋 Creating Conditional Access Policies

### Create a Basic MFA Policy

1. Go to *Protection > Conditional Access > + New policy*  
2. Name the policy (e.g., *MFA – All Users*)

#### Assignments

- Target: *All users* or pilot group  
- Apps: *All cloud apps*

**🖼️ Screenshot: Conditional Access assignment screen**

#### Conditions (Optional)

- Include/exclude locations  
- Select client types (browser, mobile, etc.)  
- Filter by device compliance state

#### Access Controls

- Grant *Require multi-factor authentication*  
- Enable policy

**🖼️ Screenshot: Access control grant screen**

---

### High-Risk Sign-In Policy (Premium P2)

1. Assign to *All users*  
2. Set *Sign-in risk = High*  
3. Require MFA  
4. Save and enable

---

## 👤 Managing User Enrollment

### View or Reset Methods

- Go to *Users > [Username] > Authentication methods*  
- View or remove existing methods

**🖼️ Screenshot: User authentication method panel**

---

### Bulk Management (Legacy MFA Portal)

- Navigate to *Users*, select multiple  
- Click *Multi-factor authentication*  
- Enable, disable, or enforce in bulk

> ⚠️ *Microsoft is retiring this portal. Use Conditional Access for future-proofing.*

---

## 📊 Reporting and Monitoring

### Registration Reports

1. Go to *Monitoring & health > Usage & insights*  
2. View *Authentication methods activity*

**🖼️ Screenshot: Registration reporting dashboard**

---

### Sign-in Logs

- Navigate to *Sign-in logs*  
- Filter by *Status*, *Authentication Requirements*, or *Result*

**🖼️ Screenshot: Filtered sign-in logs showing MFA activity**

---

## 🧩 Troubleshooting Common Issues

### MFA Not Triggering

- Verify the policy is *enabled*  
- Ensure the *user, app, and condition* match  
- Confirm the policy is not in *report-only* mode

---

### Users Cannot Register

- Ensure a valid *license* is assigned  
- Confirm *authentication method* is enabled  
- Clear browser cache or try another device

---

### Authenticator Push Fails

- Verify *device connectivity*  
- Disable *battery optimization*  
- Restart or reinstall the app

---

### SMS/Voice Issues

- Check *phone number format*  
- Test an *alternate method*  
- Confirm the *carrier isn’t blocking automation*

---

## ✅ Best Practices

### MFA Rollout Strategy

- Start with *admins and IT teams*  
- Expand to *business units* in phases  
- Monitor *sign-in logs* for early detection

---

### Emergency Access

- Create at least two *break glass accounts*  
- Exclude from *MFA and Conditional Access*  
- Review account access *quarterly*  
- Store credentials in an *offline secure vault*

---

### User Education

- Create *setup guides* and quick-reference material  
- Send *rollout announcements* ahead of enforcement  
- Provide *justification/override instructions*  
- Create a *support escalation path*

---

## 🔐 Security Considerations

- Use *number matching* and *app context*  
- Avoid *SMS* as a primary method  
- Use *risk-based policies* (Premium P2)  
- Pair MFA with *device compliance* or *location filters*

---

## ⚙️ Automation and PowerShell Examples

### List Users Without MFA

***Get-MsolUser -All | Where-Object { $_.StrongAuthenticationMethods.Count -eq 0 }***

### Enforce MFA for a User

***Set-MsolUser -UserPrincipalName "user@example.com" -StrongAuthenticationRequirements @(@{RelyingParty="*"; State="Enabled"})***

> *Requires the AzureAD or MSOnline PowerShell module.*  
> *Run* ***Connect-MsolService*** *before executing commands.*

---

## 🔄 Change Management & Rollback Options

### Rolling Back a Conditional Access Policy

1. Go to *Conditional Access > Policies*  
2. Select the policy  
3. Set *Enable policy* to *Off*  
4. Click *Save*  
5. Notify affected users or teams

---

### Disabling Security Defaults

1. Go to *Properties > Manage security defaults*  
2. Toggle switch to *Off*  
3. Click *Save*

> ⚠️ *Use caution. Disabling MFA increases security risk. Always document and review rollback actions.*

---

_Last updated: **May 2025**_  
📖 *For official guidance, visit* [**Microsoft Learn – Entra MFA**](https://learn.microsoft.com/entra/identity/authentication/howto-mfa-getstarted)
