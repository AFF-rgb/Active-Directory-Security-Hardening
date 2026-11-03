# 04 — Password and Account Security

## Objective

Apply domain-wide password requirements and account lockout controls, then use a Fine-Grained Password Policy (FGPP) for privileged accounts.

## Default domain password policy

The following values were reviewed in the **Default Domain Policy**:

| Setting | Verified value |
|---|---:|
| Minimum password length | 14 characters |
| Enforce password history | 24 passwords |
| Minimum password age | 1 day |
| Maximum password age | 0 days (no routine expiration) |
| Password complexity | Enabled |
| Reversible encryption | Disabled |
| Account lockout threshold | 5 invalid attempts |
| Account lockout duration | 15 minutes |
| Reset account lockout counter after | 15 minutes |

## GUI configuration and review — Default Domain Policy

### 1. Open Group Policy Management

1. Sign in to the domain controller.
2. Open **Server Manager**.
3. Select **Tools → Group Policy Management**.
4. Expand **Forest: stech.net → Domains → stech.net**.
5. Right-click **Default Domain Policy** and select **Edit**.

### 2. Review Password Policy

In Group Policy Management Editor, navigate to:

**Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy**

Review these settings:

- Enforce password history: 24 passwords remembered.
- Maximum password age: 0 days.
- Minimum password age: 1 day.
- Minimum password length: 14 characters.
- Password must meet complexity requirements: Enabled.
- Store passwords using reversible encryption: Disabled.

For each setting, double-click it to view its configured value. The values above are the verified lab settings.

### 3. Review Account Lockout Policy

In the same editor, navigate to:

**Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy**

Review:

- Account lockout threshold: 5 invalid logon attempts.
- Account lockout duration: 15 minutes.
- Reset account lockout counter after: 15 minutes.

These settings were reviewed in the domain policy. A failed-logon sequence was not performed to behaviorally test the lockout threshold.

## Fine-Grained Password Policy (FGPP)

### Policy details

Policy name: `FGPP-Privileged-Accounts`

The policy settings are:

| Setting | Value |
|---|---:|
| Precedence | 1 |
| Minimum password length | 16 characters |
| Enforce password history | 24 passwords |
| Minimum password age | 1 day |
| Maximum password age | 0 days (no routine expiration) |
| Password complexity | Enabled |
| Reversible encryption | Disabled |
| Account lockout threshold | 5 attempts |
| Account lockout duration | 15 minutes |
| Reset lockout counter after | 15 minutes |

**Directly Applies To:** `GG-Privileged-Accounts`

### GUI configuration — Active Directory Administrative Center

1. Open **Server Manager → Tools → Active Directory Administrative Center**.
2. Select the `stech.net` domain.
3. Navigate to **System → Password Settings Container**.
4. In the Tasks pane, select **New → Password Settings**.
5. Enter the name `FGPP-Privileged-Accounts` and set precedence to `1`.
6. Enter the password settings listed in the table above.
7. Under **Directly Applies To**, select **Add**.
8. Enter `GG-Privileged-Accounts`, select **Check Names** if available, and confirm the selection.
9. Review the settings and save the password setting object.

### Assigning the privileged account to the policy group

The policy is assigned to the security group rather than directly to an individual user.

To review the group membership in ADUC:

1. Open **Server Manager → Tools → Active Directory Users and Computers**.
2. Navigate to `STECH-HQ → Groups`.
3. Right-click `GG-Privileged-Accounts` and select **Properties**.
4. Open **Members** and confirm `IT.sysadmin` is listed.

### GUI verification — Resultant Password Settings

The resultant policy was checked in Active Directory Administrative Center:

1. Open **Server Manager → Tools → Active Directory Administrative Center**.
2. Navigate to `stech.net → STECH-HQ → Departments → IT`.
3. Locate `IT.sysadmin`.
4. Right-click the account and select **View Resultant Password Settings**.
5. Confirm the resultant policy is `FGPP-Privileged-Accounts`.
6. Review the effective values.

The resultant policy view confirmed that `FGPP-Privileged-Accounts` applies to `IT.sysadmin`, including the 16-character minimum password length and the other configured FGPP values.

## Privileged account review

- `IT.sysadmin` is a member of `Domain Admins` and `GG-Privileged-Accounts`.
- The standard employee account `a.fadous` is not a member of the audited privileged groups and receives the default domain password policy.
- Account-not-delegated was enabled for `IT.sysadmin` and the built-in Administrator.
- Workstation logon restrictions were deferred to avoid accidental administrative lockout.

## Validation limitations

- Domain policy settings and FGPP settings were reviewed through the Windows GUI.
- Resultant Password Settings confirmed the privileged policy for `IT.sysadmin`.
- The lockout threshold was configured and reviewed but was not behaviorally tested with repeated failed logons.
- No account passwords or other authentication secrets are included in this documentation.
