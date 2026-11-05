# Chapter 08 — Advanced Audit Policy & Security Event Monitoring

## Objective

Configure centralized Windows security auditing through Group Policy and validate that security-relevant events are generated in the domain environment. The goal is to improve endpoint visibility for authentication, process execution, account changes, privilege use, object access, and audit-policy changes.

## Lab environment

- Active Directory domain: `stech.net`
- Domain Controller: `DC01-LAB` (Windows Server 2022)
- Client: `A` (Windows 10 Pro)
- Group Policy Object: `GPO-Common-PC-Audit-Policy`
- GPO link: `STECH-HQ\\Computers`
- Security filtering: `Authenticated Users`
- Validation tools: Group Policy Management Editor, `gpresult`, `auditpol`, and Windows Event Viewer

## 1. Audit policy enforcement

Verified the following security option is **Enabled**:

**Audit: Force audit policy subcategory settings (Windows Vista or later) to override audit policy category settings**

**What it does:** Ensures that Advanced Audit Policy subcategory settings take precedence over legacy category-level audit policy settings.

**Why it is used:** Prevents legacy audit-policy settings from overriding the more granular subcategory configuration managed through this GPO.

## 2. Process creation command-line auditing

Verified the following Administrative Template policy is **Enabled**:

**Include command line in process creation events**

Policy path:

`Computer Configuration > Policies > Administrative Templates > System > Audit Process Creation`

**What it does:** Adds the command-line string to process-creation event records (Event ID **4688**) when command-line information is available.

**Why it is used:** Provides additional context about how a process was launched, which can help analysts investigate suspicious commands, scripts, and execution activity. Command lines may contain sensitive information, so access to Security logs should be restricted.

## 3. Reviewed Advanced Audit Policy settings

Policy path:

`Computer Configuration > Policies > Windows Settings > Security Settings > Advanced Audit Policy Configuration > System Audit Policies`

The following subcategory settings were reviewed in the GPO editor. The purpose column briefly explains the security-monitoring value of each configured setting.

### Logon/Logoff

| Subcategory | GPO setting | What it helps monitor |
|---|---|---|
| Audit Logon | Success and Failure | Records successful and failed sign-in attempts, helping identify suspicious authentication activity and potential brute-force attempts. |
| Audit Logoff | Success and Failure | Records user logoff activity, providing session-lifecycle context during investigations. |

### Account Management

| Subcategory | GPO setting | What it helps monitor |
|---|---|---|
| Audit User Account Management | Success and Failure | Tracks user-account actions such as creation, modification, enabling, disabling, and deletion. |
| Audit Security Group Management | Success and Failure | Tracks security-group changes, including membership changes that may affect user privileges and access. |
| Audit Computer Account Management | Success and Failure | Tracks computer-account creation, modification, and deletion in the domain. |

### Detailed Tracking

| Subcategory | GPO setting | What it helps monitor |
|---|---|---|
| Audit Process Creation | Success | Records process-start activity (Event ID 4688), supporting investigation of program and command execution. |
| Audit Process Termination | Success | Records process termination activity (commonly Event ID 4689), helping reconstruct process activity and timelines. |

### Policy Change

| Subcategory | GPO setting | What it helps monitor |
|---|---|---|
| Audit Audit Policy Change | Success and Failure | Records changes to audit policy, helping detect attempts to weaken, disable, or alter security logging. |
| Audit Authentication Policy Change | Success | Records changes to authentication policies, which may affect how identities authenticate. |
| Audit Authorization Policy Change | Success | Records changes to authorization policies, which may affect permissions and access decisions. |

### Privilege Use

| Subcategory | GPO setting | What it helps monitor |
|---|---|---|
| Audit Sensitive Privilege Use | Success and Failure | Records use of sensitive user rights and privileges, helping investigate activity performed with elevated capabilities. |

### System

| Subcategory | GPO setting | What it helps monitor |
|---|---|---|
| Audit Security State Change | Success | Records changes to the system security state, such as startup or shutdown-related security state events. |
| Audit Security System Extension | Success and Failure | Records events related to security-system extensions, including installation or loading of security packages and services. |
| Audit System Integrity | Success and Failure | Records events involving system-integrity checks, including certain integrity or security-subsystem errors. |
| Audit Other System Events | Success and Failure | Captures additional security-relevant system events that do not fit into the other System subcategories. |

### Object Access

| Subcategory | GPO setting | What it helps monitor |
|---|---|---|
| Audit Detailed File Share | Success and Failure | Provides detailed records of file-share access, including requested access to shared resources. |
| Audit File Share | Success and Failure | Records attempts to access shared folders over the network. |
| Audit File System | Success and Failure | Records access to audited files and folders when matching SACL auditing entries are configured on those objects. |
| Audit Other Object Access Events | Success | Captures additional object-access activity not covered by the other object-access subcategories. |
| Audit Removable Storage | Success and Failure | Records attempts to access removable-storage devices, helping investigate USB and other removable-media activity. |

The following Object Access subcategories were **Not Configured** in this GPO and are listed for transparency; they are not presented as enabled by this chapter:

- Audit Application Generated
- Audit Certification Services
- Audit Filtering Platform Connection
- Audit Filtering Platform Packet Drop
- Audit Handle Manipulation
- Audit Kernel Object
- Audit Registry
- Audit SAM
- Audit Central Access Policy Staging

**Important:** For File System auditing, enabling the audit subcategory alone does not generate events for every file or folder. Appropriate auditing entries (SACLs) must also be configured on the target objects. “Not Configured” means the subcategory is not explicitly configured in this GPO; it does not, by itself, establish the effective setting on the client.

## 4. Group Policy application validation

On client A, ran:

```cmd
gpresult /scope computer /r
```

Result: `GPO-Common-PC-Audit-Policy` appeared under **Applied Group Policy Objects**.

The GPO is linked to `STECH-HQ\\Computers` and uses `Authenticated Users` security filtering. The `gpresult` output on client A was used to confirm that the GPO was applied to that computer.

## 5. Effective audit policy validation

On client A, ran:

```cmd
auditpol /get /category:*
```

The command returned effective audit settings across System, Logon/Logoff, Object Access, Privilege Use, Detailed Tracking, Policy Change, Account Management, DS Access, and Account Logon.

The output showed the reviewed settings in effect, including process creation and termination auditing, file system and file share auditing, account-management auditing, and audit-policy change auditing. It also showed additional enabled subcategories (for example, Kerberos and other logon events). These additional effective settings are recorded as observed output and are not attributed solely to this GPO without further resultant-policy analysis.

## 6. Security event validation

The following events were reported as validated in Windows Event Viewer during the lab exercise:

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
- Computer-account and authentication/authorization policy changes.

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
