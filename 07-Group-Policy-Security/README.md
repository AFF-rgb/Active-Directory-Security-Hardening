# 07 — Group Policy Endpoint Security

## Objective

Apply a common endpoint-security baseline to domain-joined Windows computers through Group Policy, covering Microsoft Defender Antivirus and Windows Defender Firewall.

## Lab scope

- Domain: `stech.net`
- Domain Controller: `DC01-Lab`
- Computer OU: `STECH-HQ\Computers\Standard-PCs`
- GPO: `GPO-Common-PC-Security`
- GPO link: `STECH-HQ\Computers`
- Security filtering: `Authenticated Users`
- Client validated: `A` (Windows 10 Pro)

## Configuration

All configuration below was reviewed in the Group Policy Management Editor and Windows Defender Firewall Properties.

### Microsoft Defender Antivirus

GUI path:

`Computer Configuration > Policies > Administrative Templates > Windows Components > Microsoft Defender Antivirus`

- **Turn off real-time protection:** Disabled. This policy therefore does not disable real-time protection.
- **Join Microsoft MAPS:** Enabled; membership option set to **Advanced MAPS**.
- **Configure extended cloud check:** Enabled; extended cloud check time set to **0 seconds**.

### Windows Defender Firewall

GUI path:

`Computer Configuration > Policies > Windows Settings > Security Settings > Windows Defender Firewall with Advanced Security`

The Domain, Private, and Public profiles were each verified with:

| Setting | Configured value |
|---|---|
| Firewall state | On |
| Inbound connections | Block (default) |
| Outbound connections | Allow (default) |
| Log dropped packets | Yes |
| Log successful connections | No |
| Log file path | `%systemroot%\system32\logfiles\firewall\pfirewall.log` |
| Maximum log size | 16,384 KB |

## Validation

Validation was performed on client `A`.

### Group Policy application

Command:

```cmd
gpresult /scope computer /r
```

Result: `GPO-Common-PC-Security` appeared under **Applied Group Policy Objects**.

### Microsoft Defender status

Command:

```powershell
Get-MpComputerStatus | Select-Object AMServiceEnabled, AntivirusEnabled, RealTimeProtectionEnabled, BehaviorMonitorEnabled, IoavProtectionEnabled, NISEnabled
```

All six returned values were `True`:

- `AMServiceEnabled`
- `AntivirusEnabled`
- `RealTimeProtectionEnabled`
- `BehaviorMonitorEnabled`
- `IoavProtectionEnabled`
- `NISEnabled`

### Firewall profile validation

The three firewall profiles were checked with `Get-NetFirewallProfile` during the lab validation. The user also reviewed each profile's properties and logging customization in the GUI. All three profiles showed the configured firewall state, default actions, and logging values listed above.

## Validation limitations

- MAPS membership and extended cloud-check settings were confirmed in the policy editor; no separate functional test of cloud-delivered protection behavior was performed.
- This chapter documents the configured baseline and the checks performed on the lab client. It does not claim that every possible Defender or firewall detection scenario was tested.
