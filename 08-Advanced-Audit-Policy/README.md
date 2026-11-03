# 08 — Advanced Audit Policy and Security Event Monitoring

## Objective

Configure Windows Advanced Audit Policy through Group Policy and validate selected Security events on the workstation and domain controller.

## GPO configuration

- GPO: `GPO-Common-PC-Audit-Policy`
- Linked to: `STECH-HQ\Computers`
- Security filtering: Authenticated Users
- Applied to client `A`, confirmed with `gpresult /scope computer /r`.
- Advanced Audit Policy categories configured include Account Logon, Account Management, Detailed Tracking, Logon/Logoff, Object Access, Policy Change, Privilege Use, and System.
- Enabled **Audit: Force audit policy subcategory settings ... to override audit policy category settings**.
- Enabled **Include command line in process creation events**.

## Effective policy verification

Used `auditpol /get /category:*` on the workstation to inspect effective subcategory settings. The output confirmed the configured settings, including Logon Success and Failure and Process Creation Success.

## Validated event IDs

| Event ID | Meaning | Test host |
|---|---|---|
| 4624 | Successful logon | Windows 10 client A |
| 4625 | Failed logon | Windows 10 client A |
| 4688 | Process creation; command-line capture configured | Windows 10 client A |
| 4719 | System audit policy changed | DC01-Lab |
| 4720 | User account created | DC01-Lab |
| 4722 | User account enabled | DC01-Lab |
| 4725 | User account disabled | DC01-Lab |
| 4726 | User account deleted | DC01-Lab |
| 4728 | Member added to a global security group | DC01-Lab |

The user confirmed the events were found in Event Viewer. Screenshots and exact event details should be added after sanitization.

## Security considerations

Process command-line auditing can expose sensitive values if applications pass secrets as command-line arguments. Restrict Security log access and protect collected logs. Do not publish raw event data containing usernames, host identifiers, or sensitive command lines.
