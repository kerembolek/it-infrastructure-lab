# 04 - Client Domain Join

## Objective
Join a Windows 11 client (CLIENT01) to the `lab.local` domain and verify
that a domain user can sign in.

## Client network configuration
| Setting | Value |
|---|---|
| Network adapter | Internal Network `labnet` |
| IPv4 address | 192.168.10.20 / 255.255.255.0 |
| Preferred DNS server | 192.168.10.10 (DC01) |

The static address and DNS server were configured with PowerShell:

```powershell
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.10.20 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 192.168.10.10
```

## Connectivity checks
- `Resolve-DnsName lab.local` returned 192.168.10.10 (DC01).
- `Test-NetConnection 192.168.10.10 -Port 53` returned
  `TcpTestSucceeded: True`.

Note: ICMP echo (ping) is blocked by the default Windows Firewall
configuration on DC01, so DNS resolution and a TCP port test were used
instead.

## Domain join
Performed through the GUI:
1. Open `sysdm.cpl` > **Computer Name** > **Change**.
2. Select **Domain**, enter `lab.local`.
3. Authenticate with a domain administrator account.
4. Restart the computer.

## Verification
- Signed in to CLIENT01 as `LAB\it.user`.
- `whoami` returned `lab\it.user`.
- The computer account CLIENT01 was created in the domain (visible in
  Active Directory Users and Computers).

## Observations
- `Resolve-DnsName lab.local` also returned DC01's NAT adapter address
  (10.0.2.15), because DC01 registers all of its adapter addresses in
  DNS. The domain join completed without issue.
- Keyboard layout is a per-user setting: the first sign-in with a new
  domain user starts with the default layout and must be adjusted per
  profile.
