# 05 — Windows LAPS

## Objective

Use Windows Local Administrator Password Solution (Windows LAPS) to manage and protect the local Administrator password on the Windows 10 client.

## Implemented configuration

- Prepared the Active Directory schema using the Windows LAPS PowerShell module.
- Created `GPO-Windows-LAPS` and configured its settings through Group Policy Management Editor.
- Configured password backup to Active Directory.
- Configured a 16-character password using all four character types and a 30-day age.
- Enabled password encryption.
- Configured `STECH\Domain Admins` as the authorized password-decryption principal.
- Enabled the local built-in Administrator account on client `A` after LAPS reported that the configured account was disabled.
- Linked the GPO at the domain and subsequently linked it to the `STECH-HQ\Computers` OU. Security filtering included computer account `A$`. The child OU `Standard-PCs` is within the Computers OU.

## OU move and troubleshooting

The computer object was moved from the default Computers container to:

`OU=Standard-PCs,OU=Computers,OU=STECH-HQ,DC=stech,DC=net`

After the move, Windows LAPS reported `LDAP_INSUFFICIENT_RIGHTS` while attempting to update the password attribute in Active Directory. The client also reported error `0x80070032` during policy processing. Refreshing Group Policy and changing the GPO link did not resolve the issue.

The LAPS event identified missing computer self-permissions on the new OU. The permission was corrected on `DC01-Lab` using the Windows LAPS PowerShell cmdlet:

```powershell
Set-LapsADComputerSelfPermission -Identity 'OU=Standard-PCs,OU=Computers,OU=STECH-HQ,DC=stech,DC=net'
```

This is a documented PowerShell prerequisite/administrative permission step, not a GUI configuration step. The GPO settings themselves were configured and reviewed through the GUI.

## Validation

- Refreshed Group Policy on client `A` after the OU permission update.
- Windows LAPS Operational log reported that policy processing succeeded.
- In Active Directory Users and Computers, the computer object's `msLAPS-EncryptedPassword` attribute was populated with encrypted data.
- The legacy `msLAPS-Password` attribute was not set, consistent with the configured encrypted-password storage.
- Before the OU move, an authorized Domain Admin successfully retrieved and decrypted the computer's LAPS password. A standard user (`a.fadous`) was not authorized to decrypt it. Password values are intentionally not included in this repository.

## Remaining verification

- Reconfirm authorized Domain Admin retrieval/decryption for computer `A` after the OU move and self-permission correction.
- Review whether any delegated LAPS password-read permissions are required on `Standard-PCs` for additional administrative groups. The self-permission correction alone does not establish that other groups have read access.
- Do not expand deployment until the required OU permissions and authorized retrieval workflow have been validated for the intended administrative groups.

## Security precautions

Never commit LAPS passwords, password attribute contents, or credentials. Treat both encrypted and clear-text LAPS attributes as sensitive data.
