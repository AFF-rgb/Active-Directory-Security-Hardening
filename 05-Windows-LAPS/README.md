# 05 — Windows LAPS

## Objective

Use Windows Local Administrator Password Solution (Windows LAPS) to manage and protect the local Administrator password on the Windows 10 client.

## Implemented configuration

- Prepared the Active Directory schema using the Windows LAPS PowerShell module.
- Created and linked `GPO-Windows-LAPS`.
- Configured password backup to Active Directory.
- Configured a 16-character password using all four character types and a 30-day age.
- Enabled password encryption.
- Configured `STECH\Domain Admins` as authorized password decryptors.
- Enabled the local built-in Administrator account on client `A` after LAPS reported that the configured account was disabled.

## Validation

- Ran Group Policy refresh and LAPS policy processing on client `A`.
- Retrieved the computer's LAPS record from the domain controller.
- Confirmed successful decryption by an authorized Domain Admin.
- Tested retrieval as standard user `a.fadous`; decryption was unauthorized and the password was not disclosed.

## Follow-up required

The computer object was subsequently moved from the default Computers container to:

`OU=Standard-PCs,OU=Computers,OU=STECH-HQ,DC=stech,DC=net`

The computer self-permission and delegated read-password permissions were originally configured on the default Computers container. Recheck and apply the required LAPS permissions to the new OU before treating the OU scope as production-ready or expanding deployment.

Never commit LAPS passwords, password attributes, or screenshots exposing them.
