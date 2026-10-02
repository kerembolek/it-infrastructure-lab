# 02 - Active Directory Domain Services Setup

## Objective
Promote DC01 to a domain controller and create a new Active Directory
forest named `lab.local`.

## Environment
| Item | Value |
|---|---|
| Server | DC01 (Windows Server 2022) |
| Domain / forest | `lab.local` |
| NetBIOS name | `LAB` |
| DNS | Integrated with AD DS, hosted on DC01 |

## Procedure
1. Install the AD DS role and management tools:
```powershell
   Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```
2. Promote the server and create the forest with integrated DNS:
```powershell
   Install-ADDSForest -DomainName "lab.local" -DomainNetbiosName "LAB" -InstallDns
```
3. The server restarts automatically. Sign in as `LAB\Administrator`.

## Verification
`Get-ADDomain` was run in an elevated PowerShell session on DC01:

```powershell
Get-ADDomain
```

| Property | Value |
|---|---|
| Domain name | `lab.local` |
| NetBIOS name | `LAB` |
| Domain functional level | Windows2016Domain |
| PDC Emulator / RID Master / Infrastructure Master | `DC01.lab.local` |
| Replica directory servers | `DC01.lab.local` |

DC01 is the only domain controller in the forest.

## Troubleshooting
- **Promotion hung on the first attempt.** The VM became unresponsive
  during the process, caused by resource contention on the host (4 GB RAM
  allocated to the VM). Resetting the VM and re-running the command
  completed the promotion successfully.
- **Warnings during promotion** (NT 4.0 cryptography default, no DNS
  delegation for `lab.local`, DHCP on the NAT adapter) are expected in an
  isolated lab environment and do not affect the result.

## Security note
The Directory Services Restore Mode (DSRM) password is stored outside
this repository.
