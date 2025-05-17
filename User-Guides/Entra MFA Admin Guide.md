# Microsoft Entra ID: Multi-Factor Authentication

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

## Introduction

Multi-Factor Authentication (MFA) is a critical security feature that requires users to provide two or more verification methods to access resources. This guide will walk you through setting up and managing Microsoft Entra ID MFA for your organization.

### Benefits of MFA Implementation

- Reduce the risk of unauthorized access by up to 99.9%
- Meet compliance requirements for various regulations
- Protect against credential theft and phishing attacks
- Provide a layered security approach for your organization

This guide is intended for IT administrators responsible for configuring identity and access management settings in Microsoft Entra ID.

---

## Prerequisites

Before configuring MFA, ensure you have:

| Requirement | Details |
|-------------|---------|
| **Licensing** | Microsoft Entra ID tenant (free, Premium P1, or Premium P2) |
| **Administrative Access** | Global Administrator or Security Administrator role |
| **Environment Setup** | Microsoft Entra ID users created for your organization |
| **Recommended Tools** | Microsoft Edge or Google Chrome browser |

> **Note:** Some advanced features require Microsoft Entra ID Premium P1 or P2 licenses.

---

## Accessing MFA Controls in Microsoft Entra ID

### Step 1: Sign in to the Microsoft Entra admin center

1. Open your web browser and navigate to [https://entra.microsoft.com](https://entra.microsoft.com)
2. Sign in with your administrator account credentials
3. If prompted for MFA, complete the authentication process

![Microsoft Entra Admin Center login page](https://raw.githubusercontent.com/CRollins6020/User-Guides/main/Images/Entra-Login-Prompt-1.png)

### Step 2: Navigate to MFA settings

There are several ways to access MFA settings in the Entra admin center:

**Method 1: Through Security settings**

Protection → Authentication methods

**Method 2: Through identity settings**

Identity → Protection → Authentication methods

![Microsoft Entra admin center navigation](https://example.com/placeholder-for-navigation.png)

---

## Enabling MFA for Your Organization

Microsoft Entra ID offers two primary approaches to enable MFA:

### Method 1: Enable Security Defaults (simplest)

Security defaults is the most straightforward way to enable MFA across your organization.

1. In the Microsoft Entra admin center, select **Properties** from the left navigation
2. Scroll to the bottom and select **Manage security defaults**
3. Set the **Enable security defaults** toggle to **Yes**
4. Select **Save**

![Security defaults toggle](https://example.com/placeholder-for-security-defaults.png)

> **Important:** Security defaults will require MFA for all users, including administrators, with no exceptions. For more granular control, use Conditional Access policies instead.

### Method 2: Use Conditional Access Policies (recommended)

Conditional Access provides more granular control over when and how MFA is required.

> **Note:** Conditional Access requires Microsoft Entra ID Premium P1 or P2 licenses.

We'll cover setting up Conditional Access policies in detail in [Section 6](#creating-conditional-access-policies).

---

## Setting Up Authentication Methods

Authentication methods determine how users can verify their identity when prompted for MFA. Microsoft Entra ID supports several methods:

| Method | Security Level | Use Case |
|--------|---------------|----------|
| **Microsoft Authenticator app** | High | Recommended primary method |
| **FIDO2 security keys** | High | Best for high-security scenarios |
| **Hardware tokens (OATH)** | Medium-High | Good for disconnected environments |
| **SMS text messages** | Medium | Acceptable for most users |
| **Voice calls** | Medium | Alternative when SMS isn't available |
| **Email one-time passcodes** | Low | Backup method only |

### Configure Authentication Methods

1. In the Microsoft Entra admin center, navigate to **Protection > Authentication methods**
2. Select **Policies** from the top menu
3. For each authentication method, select the method name to configure its settings

![Authentication methods policy page](https://example.com/placeholder-for-auth-methods.png)

### Configure Microsoft Authenticator (Recommended Primary Method)

1. Select **Microsoft Authenticator** from the list
2. Set **Enable** to **Yes**
3. Under **Target**, choose either:
   - **All users** to enable for everyone
   - **Select users** to enable for specific groups
4. Configure additional settings:
   - Enable **Show application name during push notifications** for improved security
   - Enable **Require number matching for push notifications** (recommended)
5. Click **Save**

![Microsoft Authenticator configuration](https://example.com/placeholder-for-authenticator-config.png)

### Configure SMS Authentication

1. Select **SMS** from the authentication methods list
2. Set **Enable** to **Yes**
3. Under **Target**, choose your desired scope
4. Click **Save**

Repeat similar steps for other authentication methods you wish to enable.

---

## Creating Conditional Access Policies

Conditional Access policies allow you to create rules that control when MFA is required based on various signals such as user, location, device, and application.

### Create a Basic MFA Policy for All Users

1. In the Microsoft Entra admin center, navigate to **Protection > Conditional Access**
2. Click **+ New policy**
3. Enter a policy name (e.g., "Require MFA for All Users")

#### Assignments

4. Under **Assignments**:
   - Click **Users and groups**
   - Select **All users** (or select specific groups for a phased rollout)
   - Optionally, exclude privileged administrator accounts for emergency access

5. Under **Target resources**:
   - Select **Cloud apps**
   - Choose **All cloud apps**

![Conditional Access policy assignments](https://example.com/placeholder-for-ca-assignments.png)

#### Conditions (Optional)

6. Configure conditions as needed:
   - **Locations**: You can exclude trusted locations from MFA requirements
   - **Client apps**: Ensure "Browser" and "Mobile apps and desktop clients" are selected
   - **Device state**: You can exclude compliant devices if desired

#### Access Controls

7. Under **Access controls**:
   - Click **Grant**
   - Select **Require multi-factor authentication**
   - Ensure **Require all the selected controls** is selected
   - Click **Select**

![Conditional Access access controls](https://example.com/placeholder-for-ca-controls.png)

8. Set **Enable policy** to **On**
9. Click **Create** to activate the policy

### Create a Policy for High-Risk Sign-ins

If you have Microsoft Entra ID Premium P2, you can create risk-based policies:

1. Create a new Conditional Access policy
2. Name it "Require MFA for High-Risk Sign-ins"
3. Under **Assignments**:
   - Select **All users**

4. Under **Conditions**:
   - Select **Sign-in risk**
   - Set risk level to **High**

5. Under **Access controls**:
   - Select **Require multi-factor authentication**

6. Enable the policy and save

This policy will only trigger MFA when Microsoft's risk detection system identifies suspicious sign-in attempts.

---

## Managing User Enrollment

Once MFA is enabled, users will need to register their authentication methods. You can manage this process in several ways:

### View MFA Status for Users

1. In the Microsoft Entra admin center, navigate to **Users**
2. Select a user from the list
3. Select **Authentication methods**
4. View the registered methods for that user

![User authentication methods page](https://example.com/placeholder-for-user-methods.png)

### Reset a User's MFA Settings

If a user loses access to their authentication methods:

1. Navigate to the user's profile
2. Select **Authentication methods**
3. Find the method you want to reset and click the delete icon (trash can)
4. Confirm the deletion

The user will be prompted to re-register during their next sign-in.

### Bulk Operations for MFA

For bulk management of MFA:

1. In the Microsoft Entra admin center, navigate to **Users**
2. Select multiple users by checking the boxes next to their names
3. Click **Multi-factor authentication** from the top menu
4. Use the available options to enable, disable, or enforce MFA for the selected users

> **Note:** This legacy MFA portal is being phased out in favor of Conditional Access policies.

---

## Reporting and Monitoring

Monitor MFA usage and identify potential issues using the built-in reporting features.

### MFA Registration Report

1. In the Microsoft Entra admin center, navigate to **Monitoring & health > Usage & insights**
2. Select **Authentication methods activity**
3. View the report showing MFA registration status across your organization

![MFA registration report](https://example.com/placeholder-for-mfa-report.png)

### Sign-in Logs

Review sign-in logs to troubleshoot MFA issues and monitor authentication attempts:

1. Navigate to **Monitoring & health > Sign-in logs**
2. Use filters to focus on MFA-related events:
   - Filter by **Status** to find failed authentications
   - Look for entries with **Authentication requirements** set to "MFA required"
   - Check the **Authentication details** column for specific MFA information

3. Click on any sign-in event to see detailed information, including:
   - Authentication methods used
   - Conditional Access policies applied
   - MFA result (success, failure, or not required)

![Sign-in logs showing MFA-related events](https://example.com/placeholder-for-signin-logs.png)

---

## Troubleshooting Common Issues

### Users Cannot Register for MFA

**Symptoms:** Users receive errors when trying to register for MFA.

**Possible Solutions:**

- Ensure the user has a valid license assigned
- Verify that the authentication method is enabled in your policies
- Check that the user is included in the target group for the authentication method
- Clear the user's browser cache or try a different browser

### Users Locked Out After MFA Changes

**Symptoms:** Administrators or users cannot access resources after MFA changes.

**Solutions:**

- Use an emergency access account (break glass account) that is excluded from MFA
- Reset the user's authentication methods through the admin portal
- If using Conditional Access, temporarily disable the policy affecting the user

### Mobile App Not Receiving Notifications

**Solutions:**

- Verify the user's device has internet connectivity
- Ensure battery optimization is disabled for the authenticator app
- Have the user restart the authenticator app
- If necessary, delete and re-register the account in the authenticator app

### SMS or Voice Call Not Received

**Solutions:**

- Verify the phone number is entered correctly (with correct country code)
- Check if the user is in an area with poor cellular reception
- Try the alternative method (if SMS fails, try voice call or vice versa)
- Ensure the phone carrier isn't blocking automated messages

---

## Best Practices

### Implement MFA Securely

- **Use phased rollout:** Start with IT staff, then expand to other departments
- **Avoid SMS when possible:** Prefer the Authenticator app or FIDO2 keys over SMS
- **Enable number matching:** Require users to enter numbers displayed on the sign-in screen
- **Require re-authentication:** Configure sensitive applications to require frequent re-authentication

### Emergency Access Planning

- **Create break glass accounts:** Maintain at least two emergency access accounts
- **Exclude emergency accounts from MFA:** Ensure these accounts can access the system if MFA is unavailable
- **Document emergency procedures:** Create clear instructions for using emergency access
- **Review emergency access regularly:** Audit and test emergency accounts quarterly

### User Education

- **Provide training:** Ensure users understand why MFA is important
- **Create guidance documents:** Develop step-by-step guides for registering MFA
- **Set up a support process:** Establish clear procedures for handling MFA-related issues
- **Communicate changes:** Notify users before implementing MFA requirements

---

*This guide was last updated: May 2025*

*For more information, visit the [Microsoft Entra ID documentation](https://learn.microsoft.com/entra/identity/authentication/howto-mfa-getstarted)*
