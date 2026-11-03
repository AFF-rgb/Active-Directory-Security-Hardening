# 03 — Organizational Units, Users, and Groups

## Objective

Organize the `stech.net` domain to represent a small enterprise with IT and HR departments, dedicated security groups, a privileged administrator account, and a workstation OU.

## Confirmed directory structure

```text
stech.net
└── STECH-HQ
    ├── Departments
    │   ├── IT
    │   │   ├── a.fadous
    │   │   └── IT.sysadmin
    │   └── HR
    │       └── hr.test
    ├── Computers
    │   └── Standard-PCs
    │       └── A
    └── Groups
        ├── GG_IT_Staff
        ├── GG_HR_Staff
        └── GG-Privileged-Accounts
```

**Important:** `IT.sysadmin` is located in the IT OU and is a member of `GG-Privileged-Accounts`. It is not listed as a member of `GG_IT_Staff`. OU placement and security-group membership are separate directory relationships.

## Configuration procedure (GUI)

All configuration in this chapter was performed through the Windows graphical interface using **Server Manager** and **Active Directory Users and Computers (ADUC)**. No PowerShell or command-line commands are required for the configuration steps below.

### 1. Open Active Directory Users and Computers

1. Sign in to the domain controller.
2. Open **Server Manager**.
3. Select **Tools → Active Directory Users and Computers**.
4. Expand the `stech.net` domain.

### 2. Create the company OU

1. Right-click `stech.net`.
2. Select **New → Organizational Unit**.
3. Enter `STECH-HQ`.
4. Leave **Protect container from accidental deletion** checked.
5. Select **OK**.

### 3. Create the top-level child OUs

For each OU, right-click `STECH-HQ`, select **New → Organizational Unit**, enter the name, keep accidental-deletion protection checked, and select **OK**.

Create these three OUs:

- `Departments`
- `Computers`
- `Groups`

### 4. Create the department OUs

1. Right-click `Departments`.
2. Select **New → Organizational Unit**.
3. Create `IT`.
4. Repeat to create `HR`.
5. Keep accidental-deletion protection enabled for both.

### 5. Create the security groups

For each group:

1. Right-click `STECH-HQ → Groups`.
2. Select **New → Group**.
3. Enter the group name.
4. Set **Group scope** to **Global**.
5. Set **Group type** to **Security**.
6. Select **OK**.

Create:

- `GG_IT_Staff`
- `GG_HR_Staff`
- `GG-Privileged-Accounts`

### 6. Create and organize the user accounts

The user accounts in this lab are:

- `a.fadous` — standard IT employee account.
- `hr.test` — HR test account.
- `IT.sysadmin` — dedicated privileged administrative account.

To create an account through ADUC:

1. Right-click the default **Users** container under `stech.net`.
2. Select **New → User**.
3. Enter the account's name and user logon name.
4. Select **Next**, set a strong password privately, and complete the wizard.
5. Do not include passwords or other authentication secrets in project documentation.

The accounts were organized into their department OUs using the ADUC **Move** action:

1. Select the default **Users** container.
2. Find the relevant account.
3. Right-click it and select **Move**.
4. Browse to the destination OU and select **OK**.

Destinations:

| Account | Destination |
|---|---|
| `a.fadous` | `STECH-HQ → Departments → IT` |
| `IT.sysadmin` | `STECH-HQ → Departments → IT` |
| `hr.test` | `STECH-HQ → Departments → HR` |

### 7. Assign users to security groups

For each group membership:

1. Open `STECH-HQ → Groups`.
2. Right-click the relevant group and select **Properties**.
3. Open the **Members** tab and select **Add**.
4. Enter the account name.
5. Select **Check Names → OK**.
6. Select **Apply → OK**.

Memberships configured:

| Security group | Member |
|---|---|
| `GG_IT_Staff` | `a.fadous` |
| `GG_HR_Staff` | `hr.test` |
| `GG-Privileged-Accounts` | `IT.sysadmin` |

The privileged account is kept separate from the standard IT staff group.

### 8. Create the workstation OU and move the client

1. Right-click `STECH-HQ → Computers`.
2. Select **New → Organizational Unit**.
3. Enter `Standard-PCs`.
4. Keep **Protect container from accidental deletion** checked and select **OK**.
5. Expand the default **Computers** container under `stech.net`.
6. Find computer object `A`, right-click it, and select **Move**.
7. Select `STECH-HQ → Computers → Standard-PCs`, then select **OK**.

## GUI verification checklist

In ADUC, expand the relevant OUs and confirm:

- [ ] `Departments → IT` contains `a.fadous` and `IT.sysadmin`.
- [ ] `Departments → HR` contains `hr.test`.
- [ ] `Groups → GG_IT_Staff → Properties → Members` lists `a.fadous`.
- [ ] `Groups → GG_HR_Staff → Properties → Members` lists `hr.test`.
- [ ] `Groups → GG-Privileged-Accounts → Properties → Members` lists `IT.sysadmin`.
- [ ] `Computers → Standard-PCs` contains computer object `A`.

## Identity model and design notes

- Department OUs organize user accounts by department.
- Global security groups represent department membership and can be used when assigning access to resources.
- The dedicated privileged account is placed in the IT OU and assigned to a separate privileged security group.
- The workstation computer object is organized under `Standard-PCs`; it is not a member of an employee user group.
- OU placement does not, by itself, grant administrative privileges. Privileged access is controlled separately through group membership and policy.

## Security and documentation notes

- Never publish account passwords, DSRM passwords, recovery secrets, or other credentials.
- Use separate standard and administrative accounts.
- Verify group membership and OU placement in ADUC before relying on them in later policy or access-control configuration.
