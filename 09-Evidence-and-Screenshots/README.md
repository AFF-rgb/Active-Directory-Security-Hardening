# 09 — Evidence, Screenshots and Diagrams

This chapter contains supporting visuals for the Active Directory Security Hardening lab.

## Evidence types

- **Authentic lab screenshots:** captured directly from DC01-LAB or the Windows 10 client. Use these to prove configuration and validation.
- **Illustrative diagrams:** designed visuals that explain architecture, control relationships, and project scope. They are not screenshots and must not be treated as proof that a setting or test was performed.

## Lab resources represented in this project

- Domain: `stech.net`
- Domain controller: `DC01-LAB` — Windows Server 2022
- Client: Windows 10 Pro (Client A)
- Department shares: `IT_Share` and `HR_Share`
- Mapped drive: `Z:\\` containing the hidden `CompanyData$` share

## Suggested evidence folders

- `01-DC01-Initial-Configuration/`
- `02-AD-DS/`
- `03-OU-Users-Groups/`
- `04-Password-Policies/`
- `05-Windows-LAPS/`
- `06-File-Server/`
- `07-Endpoint-GPO/`
- `08-Audit-Policy/`

## Before publishing authentic screenshots

- Redact passwords, password hashes, LAPS values, tokens, recovery keys, and private keys.
- Remove personal email addresses and unrelated account details.
- Review command lines and event data for embedded secrets.
- Avoid screenshots of credential dialogs or password fields.
- Caption each screenshot with the exact configuration or validation it demonstrates.
- Do not claim a test passed unless the corresponding lab result was observed.

**Evidence integrity:** illustrative diagrams are labelled as such. Only screenshots captured from the actual lab are presented as authentic implementation or validation evidence.
