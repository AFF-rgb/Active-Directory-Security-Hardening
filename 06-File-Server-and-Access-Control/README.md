# 06 — File Server and Departmental Access Control

## Objective

Provide separate IT and HR file areas using SMB share permissions, NTFS permissions, and Access-Based Enumeration (ABE).

## Implemented structure

- Data directory: `C:\CompanyData`
- Hidden SMB share: `CompanyData$`
- Department folders: `IT_Share` and `HR_Share`
- Share path: `\\DC01-Lab\CompanyData$`
- Share-level permission: Everyone — Full Control; NTFS permissions provide the departmental restriction.
- ABE: Enabled.
- Drive mapping GPO: `GPO_MapDrive_Z`, configured for IT users and linked to the IT OU.

## NTFS access model

- `STECH\GG_IT_Staff`: Modify access to `IT_Share`.
- `STECH\GG_HR_Staff`: Modify access to `HR_Share`.
- SYSTEM and Administrators retain administrative permissions.

## Validation

- `a.fadous` (IT) saw only `IT_Share`, created a test folder there, and received Access Denied when directly accessing `HR_Share`.
- `hr.test` (HR) saw only `HR_Share`, created a test folder there, and received Access Denied when directly accessing `IT_Share`.
- The IT user's Z: drive mapping appeared after Group Policy refresh and sign-in.

Test folders were retained as lab evidence. HR drive mapping was not part of the IT-only drive-mapping GPO.
