# 01 — DC01 Initial Configuration

## Objective

Prepare the Windows Server 2022 host that will become the lab's Active Directory domain controller.

## Verified environment details

- Operating system: Windows Server 2022 Standard Evaluation
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

## Network connectivity validation

- [x] Default gateway ping to `192.168.1.1` — successful replies confirmed.
- [x] DNS lookup for `stech.net` — resolved to `192.168.1.100` using the local DNS service (`localhost / ::1`).
- [ ] External network connectivity test, if applicable.

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
- [ ] Sanitized screenshots of network configuration and connectivity tests.
- [ ] Windows update status and installed roles/features.
- [ ] Pre-promotion readiness checks.

## Notes

The host is configured with a static IPv4 address and uses loopback DNS entries. The successful lookup confirms that the local DNS service returned the domain's A record as `192.168.1.100`. It does not, by itself, validate all required AD DNS service records (such as SRV records) or domain-controller health.

> Do not publish credentials, recovery secrets, MAC addresses, DHCPv6 DUIDs, or screenshots containing sensitive information.
