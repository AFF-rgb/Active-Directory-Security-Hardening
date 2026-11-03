# 04 — Password and Account Security

## Objective

Apply stronger domain password controls, account lockout protections, fine-grained password policy, and separation of privileged identities.

## Default domain password policy

| Setting | Verified value |
|---|---:|
| Minimum password length | 14 characters |
| Password history | 24 passwords |
| Minimum password age | 1 day |
| Maximum password age | 0 (no routine expiration) |
| Complexity | Enabled |
| Reversible encryption | Disabled |
| Lockout threshold | 5 attempts |
| Lockout duration | 15 minutes |
| Lockout observation window | 15 minutes |

## Fine-Grained Password Policy

Policy: `FGPP-Privileged-Accounts`

- Precedence: 1
- Minimum length: 16 characters
- Password history: 24
- Minimum age: 1 day
- Maximum age: 0 (no routine expiration)
- Complexity enabled; reversible encryption disabled
- Lockout threshold: 5; duration and observation window: 15 minutes

The policy is assigned through `GG-Privileged-Accounts`. Resultant policy verification confirmed it applies to `IT.sysadmin`; `a.fadous` inherits the default domain policy.

## Privileged account review

The privileged identity `IT.sysadmin` is a member of Domain Admins and `GG-Privileged-Accounts`. The standard employee account `a.fadous` was checked and was not found in the audited privileged groups.

Account-not-delegated was enabled for `IT.sysadmin` and the built-in Administrator. Workstation logon restrictions were deferred to avoid accidental administrative lockout.

## Validation limitations

Configuration and group-membership checks were performed. A failed-logon sequence was not run to behaviorally test the lockout threshold.
