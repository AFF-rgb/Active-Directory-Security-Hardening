# 01 — DC01 Initial Configuration

## Objective

Prepare the Windows Server 2022 host for its role as the Active Directory domain controller.

## Verified environment

| Property | Value |
|---|---|
| Operating system | Windows Server 2022 Standard Evaluation |
| OS build | 20348 |
| Architecture | 64-bit |
| Hostname | `DC01-Lab` |
| Domain DNS suffix | `stech.net` |
| IPv4 address | `192.168.1.100` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `192.168.1.1` |
| DNS client entries | `::1`, `127.0.0.1` |
| DHCP | Disabled |

The hardware MAC address and DHCPv6 DUID are intentionally excluded from this public portfolio.

## GUI procedure — Initial server preparation

The server was configured through the Windows Server graphical interface. The steps below describe the GUI workflow used in the lab.

### Step 1 — Configure the computer name

1. Open **Server Manager**.
2. Select **Local Server** in the left pane.
3. Click the current **Computer name** value.
4. In the System Properties window, select **Change**.
5. Enter the computer name: `DC01` (the resulting verified hostname is `DC01-Lab`; see note below).
6. Confirm the change and restart when prompted.

> **Naming clarification:** The current verified hostname from `ipconfig /all` is `DC01-Lab`. The original notes mention entering `DC01`; confirm whether this was later renamed to `DC01-Lab` before treating that intermediate value as final history.

### Step 2 — Configure the time zone

1. In **Server Manager → Local Server**, click the displayed **Time zone** value.
2. Select the correct time zone for the lab host.
3. Confirm the selection.

The exact time-zone selection has not yet been recorded in the available evidence.

### Step 3 — Configure a static IPv4 address

1. Open **Server Manager → Local Server**.
2. Click the **Ethernet** link to open Network Connections.
3. Right-click the active Ethernet adapter and select **Properties**.
4. Select **Internet Protocol Version 4 (TCP/IPv4)**.
5. Click **Properties**.
6. Select **Use the following IP address**.
7. Enter the lab's verified values:
   - IP address: `192.168.1.100`
   - Subnet mask: `255.255.255.0`
   - Default gateway: `192.168.1.1`
8. Configure the DNS entries as recorded in the verified adapter output: `::1` and `127.0.0.1`.
9. Click **OK**, then **Close** to apply the adapter settings.

### Step 4 — Verify network settings

1. Open Command Prompt.
2. Run `ipconfig /all`.
3. Confirm the hostname, static IPv4 address, subnet mask, gateway, and DNS entries.

**Observed result:** DHCP was disabled. The adapter reported IPv4 `192.168.1.100/24`, gateway `192.168.1.1`, and DNS entries `::1` and `127.0.0.1`.

### Step 5 — Prepare for Active Directory Domain Services

The next stage of the original GUI workflow used **Server Manager → Manage → Add Roles and Features** to install the AD DS role, followed by the post-installation promotion wizard. Those actions are documented in the separate [Active Directory Domain Services chapter](../02-Active-Directory-Domain-Services/README.md).

## Verification evidence

The following commands were used for verification only; they were not used to configure the server.

### Gateway connectivity

- Ping to `192.168.1.1`: successful replies confirmed by the user.

### Domain DNS resolution

Command: `nslookup stech.net`

- DNS server reported: localhost (`::1`)
- Resolved address: `192.168.1.100`

### Active Directory LDAP service record

Command: `nslookup -type=SRV _ldap._tcp.dc._msdcs.stech.net`

- SRV target: `dc01-lab.stech.net`
- Port: `389`
- Priority: `0`
- Weight: `100`
- Target address: `192.168.1.100`

### DNS diagnostics

Command: `dcdiag /test:dns`

- Connectivity test: Passed
- DNS test on `DC01-LAB`: Passed
- Enterprise DNS test for `stech.net`: Passed
- DNS partition checks ran for ForestDnsZones, DomainDnsZones, Schema, Configuration, and the domain partition.

### Core Active Directory services

The user checked the following services; all reported **Running**:

| Service | Display name |
|---|---|
| NTDS | Active Directory Domain Services |
| DNS | DNS Server |
| Netlogon | Netlogon |
| KDC | Kerberos Key Distribution Center |
| DFSR | DFS Replication |

### Domain and forest configuration

Verified using Active Directory PowerShell queries:

| Property | Value |
|---|---|
| DNS domain | `stech.net` |
| NetBIOS name | `STECH` |
| Domain functional level | `Windows2016Domain` |
| Forest root | `stech.net` |
| Forest functional level | `Windows2016Forest` |

These values describe the AD domain and forest functional levels, not the Windows Server operating-system version.

## Evidence checklist

- [x] Server hostname and network configuration reviewed.
- [x] Static IPv4 configuration confirmed.
- [x] Default gateway reachability confirmed.
- [x] Domain DNS A-record lookup confirmed.
- [x] LDAP SRV record lookup confirmed.
- [x] DNS diagnostics passed.
- [x] Core AD services reported Running.
- [x] Domain and forest functional levels recorded.
- [ ] Sanitized GUI screenshots of Server Manager and IPv4 properties.
- [x] Windows Update GUI checked; no updates were available at the time of the check.
- [ ] Confirm exact time-zone setting.
- [ ] Confirm hostname change history (`DC01` versus `DC01-Lab`).

## Documentation standard

This portfolio documents configuration through the GUI because that is how the lab was performed. Each implementation procedure will identify the management console, navigation path, setting, selected value, and confirmation action. Command Prompt and PowerShell output are included only as validation evidence when applicable.

## Security and privacy

Never publish passwords, DSRM secrets, password hashes, LAPS values, private keys, access tokens, MAC addresses, DHCPv6 DUIDs, or unredacted screenshots. The source notes contain credential examples; those values must not be copied into this public repository.
