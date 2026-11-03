# 01 — DC01 Initial Configuration

## Objective

Prepare the Windows Server 2022 host that will become the lab's Active Directory domain controller.

## Verified environment details

- Operating system: Windows Server 2022 Standard Evaluation
- Windows version reported: `2009`
- OS build: `20348`
- Architecture: 64-bit
- Hostname: `DC01-Lab`
- Primary DNS suffix: `stech.net`
- DNS suffix search list: `stech.net`
- Network adapter: Intel(R) 82574L Gigabit Network Connection
- DHCP: Disabled (manually configured IPv4 address)

## Verified IPv4 configuration

| Setting | Value |
|---|---|
| IPv4 address | `192.168.1.100` |
| Subnet mask | `255.255.255.0` (`/24`) |
| Default gateway | `192.168.1.1` |
| Preferred DNS entries | `::1`, `127.0.0.1` |
| IPv4 address assignment | Static / DHCP disabled |

The network configuration was captured from `ipconfig /all` on `DC01-Lab`. The adapter's MAC address and DHCPv6 DUID are intentionally omitted from this public portfolio.

## Network and DNS validation

- [x] Default gateway ping to `192.168.1.1` — successful replies confirmed.
- [x] DNS lookup for `stech.net` — resolved to `192.168.1.100` using the local DNS service (`localhost / ::1`).
- [x] Active Directory LDAP SRV lookup — `_ldap._tcp.dc._msdcs.stech.net` returned an SRV record pointing to `dc01-lab.stech.net` on TCP port `389` (priority 0, weight 100); the hostname resolved to `192.168.1.100`.
- [ ] External network connectivity test, if applicable.

### Core Active Directory services

The following services were queried with `Get-Service NTDS,DNS,Netlogon,KDC,DFSR` on `DC01-Lab`; all five reported `Running`:

| Service | Role | Observed status |
|---|---|---|
| NTDS | Active Directory Domain Services | Running |
| DNS | DNS Server | Running |
| Netlogon | Netlogon | Running |
| KDC | Kerberos Key Distribution Center | Running |
| DFSR | DFS Replication | Running |
- [x] `dcdiag /test:dns` — Connectivity test passed; DNS test passed for `DC01-LAB`; enterprise DNS test passed for `stech.net`. Partition checks ran for ForestDnsZones, DomainDnsZones, Schema, Configuration, and the domain partition.

## Configuration record

The chronological setup record will cover:

1. Windows Server installation and edition
2. Hostname configuration
3. Static IPv4 configuration
4. DNS client configuration
5. Network connectivity checks
6. Windows updates and installed roles/features
7. Pre-promotion readiness checks

## Evidence

- [x] `ipconfig /all` output reviewed.
- [x] Default gateway reachability confirmed.
- [x] `nslookup stech.net` result reviewed.
- [x] `nslookup -type=SRV _ldap._tcp.dc._msdcs.stech.net` result reviewed.
- [x] `dcdiag /test:dns` output reviewed.
- [ ] Sanitized screenshots of network configuration and connectivity tests.
- [ ] Windows update status and installed roles/features.
- [ ] Pre-promotion readiness checks.

## Notes

The host is configured with a static IPv4 address and uses loopback DNS entries. The successful LDAP SRV lookup confirms that DNS advertises the domain controller's LDAP service locator record and resolves the target host to the expected lab address. The DNS diagnostic also reported successful connectivity and DNS tests. These checks provide useful evidence of AD DNS health, but do not constitute a complete `dcdiag` health assessment.

> Do not publish credentials, recovery secrets, MAC addresses, DHCPv6 DUIDs, or screenshots containing sensitive information.
