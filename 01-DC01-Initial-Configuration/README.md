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
- [ ] Sanitized screenshot of the network configuration.
- [ ] Connectivity tests (gateway and DNS resolution).
- [ ] Windows update status and installed roles/features.
- [ ] Pre-promotion readiness checks.

## Notes

The host is configured with a static IPv4 address and uses loopback DNS entries. The available output confirms the configured DNS addresses, but does not by itself prove successful DNS resolution or domain-controller health. Those checks will be documented separately when performed.

> Do not publish credentials, recovery secrets, MAC addresses, DHCPv6 DUIDs, or screenshots containing sensitive information.
