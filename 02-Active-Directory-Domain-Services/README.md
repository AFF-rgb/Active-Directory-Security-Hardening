# 02 — Active Directory Domain Services

## Objective

Install the Active Directory Domain Services (AD DS) role and promote the Windows Server 2022 host to the first domain controller in a new forest.

## Verified domain details

| Property | Verified value |
|---|---|
| DNS domain / forest root | `stech.net` |
| NetBIOS domain name | `STECH` |
| Domain controller hostname | `DC01-Lab` |
| Domain functional level | `Windows2016Domain` |
| Forest functional level | `Windows2016Forest` |

The functional-level values above are the verified final environment values. The original setup notes state that the wizard's functional-level selections were left at their defaults.

## GUI implementation — Install the AD DS role

The role installation was performed through Server Manager.

1. Sign in to the Windows Server using an administrative account.
2. Open **Server Manager**.
3. Select **Manage → Add Roles and Features**.
4. In the installation wizard, proceed with **Role-based or feature-based installation**.
5. Select the current server from the server pool.
6. On the Server Roles page, select **Active Directory Domain Services**.
7. When prompted, select **Add Features** to include the required management tools and supporting features.
8. Continue through the wizard using the selections recorded in the lab notes.
9. Select **Install** and wait for the role installation to complete.

## GUI implementation — Promote the server to a domain controller

After the AD DS role installation completed:

1. In **Server Manager**, select the notification flag in the upper-right corner.
2. Select **Promote this server to a domain controller**.
3. In Deployment Configuration, choose **Add a new forest**. This was a fresh AD installation.
4. Enter `stech.net` as the root domain name.
5. Continue to Domain Controller Options.
6. Leave the forest and domain functional-level selections at their wizard defaults, as recorded in the original notes. The resulting environment was later verified as Windows Server 2016 functional level for both domain and forest.
7. Set the Directory Services Restore Mode (DSRM) password.
   - **Credential handling:** The password is intentionally not documented or published.
   - Keep this recovery credential in an approved secure location.
8. Continue through the remaining wizard pages using the recorded default selections. The source notes do not specify every individual option on these pages, so this document does not claim additional custom selections.
9. Review the configuration and start the installation.
10. Allow the server to restart when the promotion process completes.

## GUI verification — Confirm the domain controller

1. Sign in after the server restarts.
2. Open **Server Manager**.
3. Open **Tools → Active Directory Users and Computers**.
4. Expand the `stech.net` domain.
5. Confirm that the domain and domain controller objects are visible.

The original notes identify this console check as the post-promotion confirmation step.

## Technical validation evidence

The following checks were performed after deployment. They are validation evidence, not configuration steps.

| Validation | Observed result |
|---|---|
| `Get-ADDomain` | DNS root `stech.net`; NetBIOS name `STECH`; domain mode `Windows2016Domain` |
| `Get-ADForest` | Forest root `stech.net`; forest mode `Windows2016Forest` |
| `nslookup stech.net` | Resolved to `192.168.1.100` |
| LDAP SRV lookup | `_ldap._tcp.dc._msdcs.stech.net` returned `dc01-lab.stech.net` on port 389 |
| `dcdiag /test:dns` | DNS connectivity and DNS tests passed |
| Core services | NTDS, DNS, Netlogon, KDC, and DFSR reported Running |

- [ ] Screenshot of AD DS selected in Add Roles and Features.
- [ ] Screenshot of the successful role installation notification.
- [ ] Screenshot of the new-forest selection showing `stech.net` (do not expose credentials).
- [ ] Screenshot of the post-promotion Server Manager notification / completion state.
- [ ] Screenshot of Active Directory Users and Computers showing the `stech.net` domain.
- [x] Domain and forest identity verified.
- [x] DNS and LDAP service records verified.
- [x] DNS diagnostics passed.
- [x] Core AD services reported Running.

## Security and documentation notes

- Never publish the DSRM password, domain administrator passwords, recovery codes, or other credentials.
- The original notes mention DNS values during initial NIC setup. The final adapter output and later DNS validation are documented in Stage 01; this chapter records the AD DS deployment sequence.
- This chapter documents the GUI workflow used in the lab. PowerShell and command-line tools are included only as post-configuration validation evidence.
