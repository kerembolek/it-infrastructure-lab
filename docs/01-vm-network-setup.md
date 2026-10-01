# 01 - VM and Network Setup

## Server
- OS: Windows Server 2022 (VirtualBox VM, 4 GB RAM)
- Hostname: DC01 (renamed from the auto-generated name before installing AD)

## Network adapters
- Adapter 1: NAT (internet access, address assigned by DHCP)
- Adapter 2: Internal Network `labnet` (lab traffic, future client machines)

## Adapter 2 configuration
- IPv4: 192.168.10.10 / 255.255.255.0
- Default gateway: none (internet goes through the NAT adapter)
- DNS: 127.0.0.1 (the server will be its own DNS after AD DS is installed)

## Why two adapters
A NAT-only VM cannot be reached by other VMs, so a domain-joined client
would not see the server. The internal network lets client VMs join the
domain later.
