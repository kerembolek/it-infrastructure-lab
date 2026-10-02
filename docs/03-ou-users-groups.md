# 03 - Organizational Units, Users and Groups

## Objective
Create a basic organizational structure in the `lab.local` domain with
test users and security groups, to be used for client domain join and
access testing.

## Structure
| Object | Type | Location |
|---|---|---|
| Company | Organizational unit | `DC=lab,DC=local` |
| Users | Organizational unit | `OU=Company` |
| Computers | Organizational unit | `OU=Company` |
| Groups | Organizational unit | `OU=Company` |
| IT-Staff | Global security group | `OU=Groups,OU=Company` |
| Accounting-Staff | Global security group | `OU=Groups,OU=Company` |
| it.user | User account | `OU=Users,OU=Company` |
| acc.user | User account | `OU=Users,OU=Company` |

## Group membership
- `IT-Staff`: it.user
- `Accounting-Staff`: acc.user

## Procedure
Created with PowerShell (ActiveDirectory module) on DC01:

```powershell
New-ADOrganizationalUnit -Name "Company" -Path "DC=lab,DC=local"
New-ADOrganizationalUnit -Name "Users" -Path "OU=Company,DC=lab,DC=local"
New-ADOrganizationalUnit -Name "Computers" -Path "OU=Company,DC=lab,DC=local"
New-ADOrganizationalUnit -Name "Groups" -Path "OU=Company,DC=lab,DC=local"
New-ADGroup -Name "IT-Staff" -GroupScope Global -Path "OU=Groups,OU=Company,DC=lab,DC=local"
New-ADGroup -Name "Accounting-Staff" -GroupScope Global -Path "OU=Groups,OU=Company,DC=lab,DC=local"
$pw = Read-Host "Password" -AsSecureString
New-ADUser -Name "IT User" -SamAccountName it.user -Path "OU=Users,OU=Company,DC=lab,DC=local" -AccountPassword $pw -Enabled $true
New-ADUser -Name "Accounting User" -SamAccountName acc.user -Path "OU=Users,OU=Company,DC=lab,DC=local" -AccountPassword $pw -Enabled $true
Add-ADGroupMember -Identity "IT-Staff" -Members it.user
Add-ADGroupMember -Identity "Accounting-Staff" -Members acc.user
```

## Verification
`Get-ADUser`, `Get-ADGroup` and `Get-ADGroupMember` confirmed that both
users and both groups exist and that each user is a member of the
expected group.

## Notes
- Account passwords were entered interactively (`Read-Host
  -AsSecureString`) and are not stored in this repository.
- Accounts were created without a User Principal Name; sign-in uses the
  `LAB\sAMAccountName` format (for example `LAB\it.user`).
