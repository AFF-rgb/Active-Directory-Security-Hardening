# 07 — Group Policy Endpoint Security

## Objective

Apply a common baseline to employee Windows computers through Group Policy.

## OU and GPO scope

- Computer OU: `STECH-HQ\Computers\Standard-PCs`
- GPO: `GPO-Common-PC-Security`
- GPO linked to parent OU: `STECH-HQ\Computers`
- Security filtering: Authenticated Users
- Client tested: `A`

## Configured controls

### Microsoft Defender Antivirus

- The policy **Turn off real-time protection** is set to Disabled, so this GPO does not turn off real-time protection.
- Microsoft MAPS membership configured as Basic.
- Extended cloud check configured.

### Windows Defender Firewall

Domain, Private, and Public profiles:
- Firewall enabled.
- Default inbound action: Block.
- Default outbound action: Allow.
- Dropped-packet logging enabled.
- Log file configured as `%systemroot%\system32\logfiles\firewall\pfirewall.log`.
- Maximum log size configured as 16,384 KB.

## Validation

- `gpresult /scope computer /r` showed the GPO as applied on client `A`.
- `Get-MpComputerStatus` showed antivirus and real-time protection status enabled.
- `Get-NetFirewallProfile` was checked and the user confirmed the three profiles were enabled with the intended default actions and blocked-packet logging.

MAPS and extended cloud-check behavior were configured but not independently demonstrated by a dedicated functional test.
