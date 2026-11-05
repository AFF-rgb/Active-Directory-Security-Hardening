# 09 — Project Evidence and Diagrams

This folder contains the visual overview for each chapter of the Active Directory Security Hardening lab. The diagrams summarize the documented design and controls; they are illustrative portfolio visuals, not screenshots and do not independently prove that a configuration or validation was performed.

## Chapter diagrams

| Chapter | Topic | Diagram |
|---|---|---|
| 01 | DC01 Initial Configuration | [View diagram](diagrams/chapter-01-dc01-initial-configuration.svg) |
| 02 | Active Directory Domain Services | [View diagram](diagrams/chapter-02-active-directory-domain-services.svg) |
| 03 | Organizational Units, Users, and Groups | [View diagram](diagrams/chapter-03-organizational-units-users-groups.svg) |
| 04 | Password and Account Security | [View diagram](diagrams/chapter-04-password-and-account-security.svg) |
| 05 | Windows LAPS | [View diagram](diagrams/chapter-05-windows-laps.svg) |
| 06 | File Server and Access Control | [View diagram](diagrams/chapter-06-file-server-access-control.svg) |
| 07 | Group Policy Endpoint Security | [View diagram](diagrams/chapter-07-group-policy-endpoint-security.svg) |
| 08 | Advanced Audit Policy | [View diagram](diagrams/chapter-08-advanced-audit-policy.svg) |
| 09 | Overall Active Directory Lab Architecture | [View diagram](diagrams/ad-lab-architecture.svg) |

## Representation and evidence notes

- Chapter diagrams are visual summaries of the corresponding README documentation.
- Chapter 09 provides the overall architecture view of the lab.
- These diagrams are not captured Windows screenshots, command output, or fabricated Event Viewer records.
- Configuration and validation details remain documented in the relevant chapter README files.
- Do not publish passwords, DSRM secrets, LAPS values, password hashes, private keys, access tokens, or other sensitive information.

## Lab components represented

- Domain: `stech.net`
- Domain controller: `DC01-LAB` — Windows Server 2022
- Client: Windows 10 Pro (Client A)
- Department shares: `IT_Share` and `HR_Share`
- Mapped drive: `Z:\\` containing the hidden `CompanyData$` share
