# 06 — File Server and Departmental Access Control

## Objective

Configure a shared company data location for IT and HR while keeping departmental access separate. This lab combines SMB share permissions, NTFS permissions, Access-Based Enumeration (ABE), and Group Policy drive mapping.

## Lab configuration

| Item | Configuration |
|---|---|
| Domain | `stech.net` |
| File server | `DC01-Lab` |
| Local path | `C:\CompanyData` |
| SMB share | `CompanyData$` |
| UNC path | `\\DC01-Lab\CompanyData$` |
| Drive mapping | `Z:` |
| GPO | `GPO_MapDrive_Z` |

## 1. Create the folder structure

On `DC01-Lab`, use File Explorer:

1. Open the `C:\` drive.
2. Create `CompanyData`.
3. Open `C:\CompanyData`.
4. Create `IT_Share` and `HR_Share`.

## 2. Create the hidden SMB share

1. Right-click `C:\CompanyData` → **Properties** → **Sharing** → **Advanced Sharing**.
2. Select **Share this folder**.
3. Set the share name to `CompanyData$`.
4. Select **Permissions**.
5. Select `Everyone` and allow **Full Control**.
6. Apply the changes.

The trailing `$` hides the share from ordinary network browse listings. It does not prevent access by users who know the UNC path and is not an authorization control.

**Security note:** Everyone — Full Control is the lab's configured share permission. For production, use a least-privilege share ACL appropriate to the organization's access model.

## 3. Enable Access-Based Enumeration

1. Open **Server Manager** → **File and Storage Services** → **Shares**.
2. Right-click `CompanyData$` → **Properties**.
3. Open **Settings**.
4. Select **Enable access-based enumeration**.
5. Select **Apply** → **OK**.

ABE hides folders from users who do not have permission to access them. It does not grant or deny access; NTFS permissions enforce authorization.

## 4. Configure NTFS permissions

Use File Explorer on `DC01-Lab`. For each folder, right-click → **Properties** → **Security**.

### IT_Share

| Principal | Permission |
|---|---|
| `STECH\GG_IT_Staff` | Modify |
| `SYSTEM` | Full control |
| `Administrators` | Full control |

### HR_Share

| Principal | Permission |
|---|---|
| `STECH\GG_HR_Staff` | Modify |
| `SYSTEM` | Full control |
| `Administrators` | Full control |

Effective access over SMB is constrained by both share and NTFS permissions; the more restrictive effective permission applies.

## 5. Configure the Z: drive mapping through Group Policy

1. On `DC01-Lab`, open **Server Manager** → **Tools** → **Group Policy Management**.
2. Expand **Forest** → **Domains** → **stech.net**.
3. Locate and edit `GPO_MapDrive_Z`.
4. Navigate to **User Configuration** → **Preferences** → **Windows Settings** → **Drive Maps**.
5. Open the mapping item and verify:
   - **Location:** `\\DC01-Lab\CompanyData$`
   - **Drive letter:** `Z:`
   - **Action:** Update/Create, as configured in the GPO.
6. Close the editor and select `GPO_MapDrive_Z`.
7. On the **Scope** tab, review its links and Security Filtering.

### Verified scope

- **Linked OUs:** IT and HR.
- **Security Filtering:** `GG_IT_Staff` and `GG_HR_Staff`.

Both departments receive the Z: mapping. This is the configuration rechecked during the current GUI walkthrough; it supersedes the earlier IT-only description in this document.

## 6. Validate access as the IT user

On domain-joined client `A`:

1. Sign in as `a.fadous`.
2. Open **File Explorer** → **This PC**.
3. Confirm drive `Z:` appears.
4. Open it. Confirm `IT_Share` is visible and `HR_Share` is hidden.
5. Create `IT_Access_Test` inside `IT_Share`.
6. Attempt to open `\\DC01-Lab\CompanyData$\HR_Share`.

**Observed:** Z: appeared; only IT_Share was visible; folder creation succeeded; direct access to HR_Share was denied.

## 7. Validate access as the HR user

On client `A`:

1. Sign in as `hr.test`.
2. Open **File Explorer** → **This PC**.
3. Confirm drive `Z:` appears.
4. Open it. Confirm `HR_Share` is visible and `IT_Share` is hidden.
5. Create `HR_Access_Test` inside `HR_Share`.
6. Attempt to open `\\DC01-Lab\CompanyData$\IT_Share`.

**Observed:** Z: appeared; only HR_Share was visible; folder creation succeeded; direct access to IT_Share was denied.

## 8. Validation summary

| Test | Result |
|---|---|
| IT user sees IT_Share only | Passed |
| IT user creates IT_Access_Test | Passed |
| IT user denied direct access to HR_Share | Passed |
| HR user sees HR_Share only | Passed |
| HR user creates HR_Access_Test | Passed |
| HR user denied direct access to IT_Share | Passed |
| ABE enabled on CompanyData$ | Confirmed |
| Z: mapping configured for IT and HR OUs and filtered to their groups | Confirmed |

The test folders were retained as lab evidence.

## 9. Security observations

- A hidden share name and ABE improve discoverability/privacy; neither replaces authorization.
- NTFS permissions enforce the departmental access boundary.
- Everyone — Full Control is documented as the lab configuration, not a production recommendation. Apply least privilege when designing a production share ACL.
- Do not publish passwords, LAPS values, or other credentials in screenshots or repository files.

## 10. Evidence screenshots

Screenshots will be added after the configuration documentation is complete:

1. `C:\CompanyData` showing both folders.
2. IT_Share and HR_Share **Security** tabs.
3. `CompanyData$` share permissions and ABE setting.
4. `GPO_MapDrive_Z` Drive Maps configuration and Scope tab.
5. IT user's Z: view, successful write test, and direct-access denial.
6. HR user's Z: view, successful write test, and direct-access denial.

Review screenshots for passwords, personal information, and other sensitive values before committing them.
