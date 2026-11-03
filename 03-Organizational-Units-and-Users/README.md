# 03 — Organizational Units, Users, and Groups

## Objective

Organize the lab directory to represent a small enterprise with IT and HR departments.

## Confirmed directory structure

```text
stech.net
└── STECH-HQ
    ├── Departments
    │   ├── IT
    │   │   └── a.fadous
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

## Identity model

- `a.fadous`: standard IT employee account.
- `hr.test`: HR test account used for departmental access validation.
- `IT.sysadmin`: dedicated privileged administrative account.
- `A`: Windows 10 client computer object.

## Notes

The computer object `A` is placed in the `Standard-PCs` OU. Department membership is assigned to user accounts through security groups; the workstation is not placed in an employee user group.
