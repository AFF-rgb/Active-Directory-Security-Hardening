# Chapter 08 — Advanced Audit Policy & Security Event Monitoring

## Objective

Configure centralized Windows security auditing through Group Policy and validate that security-relevant events are generated on a domain-joined Windows client. The goal is to improve endpoint visibility for authentication, process execution, account changes, privilege use, object access, and audit-policy changes.

## Lab environment

- Active Directory domain: `stech.net`
- Domain Controller: `DC01-LAB` (Windows Server 2022)
- Client: `A` (Windows 10 Pro)
- Group Policy Object: `GPO-Common-PC-Audit-Policy`
- Validation tools: Group Policy Management Editor, `gpresult`, `auditpol`, and Windows Event Viewer

## 1. Audit policy enforcement

Verified the following security option is **Enabled**:

**Audit: Force audit policy subcategory settings (Windows Vista or later) to override audit policy category settings**

This makes advanced audit policy subcategory settings take precedence over legacy category-level audit policy settings.

## 2. Process creation command-line auditing

Verified the following Administrative Template policy is **Enabled**:

**Include command line in process creation events**

Policy path:

`Computer Configuration > Policies > Administrative Templates > System > Audit Process Creation`

This setting adds command-line details to process-creation events when available. Event ID **4688** was validated for process creation and command-line capture.

## 3. Reviewed Advanced Audit Policy settings

Policy path:

`Computer Configuration > Policies > Windows Settings > Security Settings > Advanced Audit Policy Configuration > System Audit Policies`

The following subcategory settings were reviewed in the GPO editor.

### Logon/Logoff

| Subcategory | GPO setting |
|---|---|
| Audit Logon | Success and Failure |
| Audit Logoff | Success and Failure |

### Account Management

| Subcategory | GPO setting |
|---|---|
| Audit User Account Management | Success and Failure |
| Audit Security Group Management | Success and Failure |
| Audit Computer Account Management | Success and Failure |

### Detailed Tracking

| Subcategory | GPO setting |
|---|---|
| Audit Process Creation | Success |
| Audit Process Termination | Success |

### Policy Change

| Subcategory | GPO setting |
|---|---|
| Audit Audit Policy Change | Success and Failure |
| Audit Authentication Policy Change | Success |
| Audit Authorization Policy Change | Success |

### Privilege Use

| Subcategory | GPO setting |
|---|---|
| Audit Sensitive Privilege Use | Success and Failure |

### System

| Subcategory | GPO setting |
|---|---|
| Audit Security State Change | Success |
| Audit Security System Extension | Success and Failure |
| Audit System Integrity | Success and Failure |
| Audit Other System Events | Success and Failure |

### Object Access

| Subcategory | GPO setting |
|---|---|
| Audit Application Generated | Not Configured |
| Audit Certification Services | Not Configured |
| Audit Detailed File Share | Success and Failure |
| Audit File Share | Success and Failure |
| Audit File System | Success and Failure |
| Audit Filtering Platform Connection | Not Configured |
| Audit Filtering Platform Packet Drop | Not Configured |
| Audit Handle Manipulation | Not Configured |
| Audit Kernel Object | Not Configured |
| Audit Other Object Access Events | Success |
| Audit Registry | Not Configured |
| Audit Removable Storage | Success and Failure |
| Audit SAM | Not Configured |
| Audit Central Access Policy Staging | Not Configured |

**Note:** “Not Configured” means the subcategory is not explicitly configured in this GPO. It does not, by itself, establish the effective setting on the client.

## 4. Group Policy application validation

On client A, ran:

```cmd
gpresult /scope computer /r
```

Result: `GPO-Common-PC-Audit-Policy` appeared under **Applied Group Policy Objects**.

## 5. Effective audit policy validation

On client A, ran:

```cmd
auditpol /get /category:*
```

The command returned effective audit settings across System, Logon/Logoff, Object Access, Privilege Use, Detailed Tracking, Policy Change, Account Management, DS Access, and Account Logon.

The output showed the reviewed settings in effect, including process creation and termination auditing, file system and file share auditing, account-management auditing, and audit-policy change auditing. It also showed additional enabled subcategories (for example, Kerberos and other logon events). These additional effective settings are recorded as observed output and are not attributed solely to this GPO without further resultant-policy analysis.

## 6. Security event validation

The following events were reported as validated in Windows Event Viewer on the lab client:

| Event ID | Security activity | Validation |
|---|---|---|
| 4624 | Successful logon | Verified |
| 4625 | Failed logon | Verified |
| 4688 | Process creation | Verified |
| 4688 | Process command-line capture | Verified |
| 4719 | Audit policy modification | Verified |
| 4720 | User account created | Verified |
| 4722 | User account enabled | Verified |
| 4725 | User account disabled | Verified |
| 4726 | User account deleted | Verified |
| 4728 | Member added to a global security group | Verified |

## 7. Security monitoring use cases

The configured and validated auditing supports investigation of:

- Successful and failed authentication activity.
- Process execution and associated command-line details.
- Changes to Windows audit policy.
- User-account lifecycle activity, including creation, enabling, disabling, and deletion.
- Changes to global security group membership.
- File-system, file-share, and removable-storage access activity.
- Sensitive privilege use and system-integrity events.

## 8. Evidence

Add genuine, sanitized screenshots to this chapter as they are captured. Suggested evidence:

- GPO audit-policy enforcement option.
- Include command line in process creation events policy.
- Advanced Audit Policy subcategory settings.
- `gpresult /scope computer /r` showing the applied GPO.
- `auditpol /get /category:*` output from client A.
- Event Viewer examples for the validated event IDs, including Event 4688 command-line details.

Do not include passwords, tokens, private data, or other sensitive information in screenshots.

## Outcome

The Advanced Audit Policy GPO was reviewed, its application on client A was confirmed, and the effective audit policy was inspected with `auditpol`. Ten security-event checks, including Event 4688 command-line capture, were reported as verified.

This lab demonstrates practical Group Policy auditing, Windows Security log validation, and foundational endpoint-monitoring skills relevant to security operations and incident investigation.

## Validation scope and limitations

The event IDs listed above were reported as verified during the lab exercise. This README records those results; it does not claim that every possible attack scenario or every audit subcategory was tested. Additional effective audit settings shown by `auditpol` may originate from other policy sources or local configuration and should not be attributed to this GPO without further investigation.
