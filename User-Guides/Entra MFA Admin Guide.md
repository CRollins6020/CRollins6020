# 🔐 Microsoft Entra ID: Multi-Factor Authentication  
*Administrator's Guide*

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

---

## 📘 Introduction

Multi-Factor Authentication (MFA) enhances security by requiring users to verify their identity using more than one method. This guide walks administrators through configuring and managing MFA within Microsoft Entra ID.

### Key Benefits

- Reduces risk of unauthorized access by over 99.9%  
- Meets compliance and regulatory requirements  
- Protects user accounts from phishing and credential theft  
- Supports secure remote access

---

## ⚙️ Prerequisites

| Requirement              | Details                                                |
|--------------------------|--------------------------------------------------------|
| **Licensing**            | Microsoft Entra ID (Free, Premium P1, or Premium P2)   |
| **Role Access**          | Global or Security Administrator                       |
| **User Setup**           | Existing users in Entra ID                             |
| **Browser Recommendation** | Microsoft Edge or Google Chrome                    |

> **Note:** Some features require a Premium P1 or P2 license.

---

## 🛠️ Accessing MFA Controls in Microsoft Entra ID

### Step 1: Sign In

1. Visit [https://entra.microsoft.com](https://entra.microsoft.com)  
2. Sign in with an administrator account  
3. Complete the MFA prompt if required

**🖼️ Screenshot: Microsoft Entra Admin Center login page**

---

### Step 2: Locate MFA Settings

Choose one of the following paths:

- **Security path:** `Protection → Authentication methods`  
- **Identity path:** `Identity → Protection → Authentication methods`

**🖼️ Screenshot: Microsoft Entra admin center navigation menu**

---

## 🔧 Enabling MFA for Your Organization

### Method 1: Enable Security Defaults

1. In the Entra admin center, go to **Properties**  
2. Scroll down and select **Manage security defaults**  
3. Toggle **Enable security defaults** to **Yes**  
4. Click **Save**

> ⚠️ Security defaults apply MFA organization-wide without exceptions.

**🖼️ Screenshot: Security defaults toggle in admin settings**

---

### Method 2: Use Conditional Access (Recommended)

> Requires Entra ID Premium P1 or P2.

Conditional Access provides more control and targeting. Setup steps are in [Section 6](#creating-conditional-access-policies).

---

## 🔑 Setting Up Authentication Methods

Microsoft Entra ID supports multiple verification methods:

| Method                    | Security Level | Use Case                                 |
|---------------------------|----------------|-------------------------------------------|
| Microsoft Authenticator   | High            | Recommended for all users                 |
| FIDO2 Security Keys       | High            | Ideal for high-security environments      |
| OATH Hardware Tokens      | Medium-High     | Offline or legacy environments            |
| SMS                       | Medium          | Backup method                             |
| Voice Calls               | Medium          | Secondary fallback                        |
| Email OTP                 | Low             | Last-resort or non-critical workflows     |

---

### Configure Authentication Methods

1. Navigate to **Protection > Authentication methods > Policies**  
2. Select a method and click to configure  
3. Define user targeting and settings  
4. Click **Save**

**🖼️ Screenshot: Authentication methods policy configuration screen**

---

### Microsoft Authenticator Setup

1. Select **Microsoft Authenticator**  
2. Enable the method  
3. Target **All users** or specific groups  
4. Optional:  
   - Show app name in push notifications  
   - Require number matching (recommended)  
5. Click **Save**

**🖼️ Screenshot: Microsoft Authenticator configuration panel**

---

### SMS Authentication Setup

1. Select **SMS**  
2. Enable the method  
3. Choose users or groups  
4. Click **Save**

Repeat as needed for other methods.

---

## 📋 Creating Conditional Access Policies

### Basic Policy Setup

1. Go to **Protection > Conditional Access**  
2. Click **+ New policy**  
3. Name the policy (e.g., “MFA – All Users”)

#### Assignments

- Assign to **All users** (or pilot group)  
- Choose **All cloud apps**

**🖼️ Screenshot: Conditional Access assignments screen**

#### Conditions (Optional)

- **Locations**: Exclude trusted networks  
- **Client apps**: Target browser and desktop  
- **Device state**: Exclude compliant devices (if configured)

#### Grant Controls

- Require **multi-factor authentication**  
- Click **Select**

**🖼️ Screenshot: Conditional Access grant settings**

4. Set policy to **On**  
5. Click **Create**

---

### High-Risk Sign-In Policy (P2 Only)

1. Create new policy  
2. Assign to **All users**  
3. Set **Sign-in risk = High**  
4. Grant **Require MFA**  
5. Enable and save

---

## 👤 Managing User Enrollment

### View or Reset MFA for a User

1. Navigate to **Users > Select user > Authentication methods**  
2. View registered methods  
3. Delete to reset MFA enrollment

**🖼️ Screenshot: User authentication methods page**

---

### Bulk MFA Management (Legacy Portal)

1. Go to **Users**  
2. Select multiple users  
3. Click **Multi-factor authentication**  
4. Enable, enforce, or disable as needed

> 🔔 This portal will be deprecated. Use Conditional Access for future-proof management.

---

## 📊 Reporting and Monitoring

### View MFA Registration Reports

1. Go to **Monitoring & health > Usage & insights**  
2. Click **Authentication methods activity**

**🖼️ Screenshot: MFA registration dashboard**

---

### Review Sign-In Logs

1. Navigate to **Sign-in logs**  
2. Filter by **Status**, **Authentication Requirements**, and **Result**

**🖼️ Screenshot: Sign-in logs filtered for MFA activity**

---

## 🧩 Troubleshooting Common Issues

### MFA Not Triggering?

- Check if policy is **in test mode**  
- Ensure **app/resource** is covered  
- Verify user group and location settings  
- Confirm **detection thresholds** are met

---

### Users Can’t Register?

- Confirm licensing is assigned  
- Enable method in **Authentication policies**  
- Clear browser cache or try another device

---

### Push Notifications Not Received?

- Confirm device has internet  
- Disable battery optimization  
- Reinstall Microsoft Authenticator

---

### Voice/SMS Not Working?

- Double-check phone format  
- Try alternate contact method  
- Confirm carrier isn’t blocking automated messages

---

## ✅ Best Practices

### MFA Rollout Strategy

- Start with admins and IT  
- Gradually expand to business units  
- Use **test mode** before enforcing  
- Prefer app or FIDO2 over SMS/voice

### Emergency Access Accounts

- Create 2+ **break glass accounts**  
- Exclude from MFA  
- Review access quarterly  
- Document recovery procedures

### User Support

- Provide MFA setup guides  
- Offer override guidance  
- Communicate rollout timelines  
- Encourage feedback channels

---

_Last updated: **May 2025**_

📖 For more info, see the [Microsoft Learn – Entra MFA Documentation](https://learn.microsoft.com/entra/identity/authentication/howto-mfa-getstarted)
