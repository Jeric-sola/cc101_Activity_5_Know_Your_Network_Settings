# CC 101 – Introduction to Computing

**Sorsogon State University – College of Information and Communications Technology, Bulan Campus**

## "Know Your Network Settings": Exploring Windows 11 Network Configuration Tools

| | |
| --- | --- |
| **Name** | JOHN JERIC L SOLA |
| **Block** | 1-1 |
| **Computer Unit Used** | HP Laptop |
| **Date** | October 14, 2026 |

---

## I. Objectives

- Locate network adapter information using Windows 11 Settings, the Control Panel, Command Prompt (CMD), and PowerShell.
- Use `ipconfig`, `getmac`, `hostname`, `ping`, `netstat`, `route print`, and `systeminfo` in CMD, and `Get-NetAdapter`, `Get-NetIPAddress`, and `Test-Connection` in PowerShell.
- Interpret a sample `ipconfig /all` output: identify its IP address, subnet mask, default gateway, and DNS servers, and classify the address as public or private.
- Relate each tool and finding to the layer of the OSI model where it works.
- Design a simple network and IP address plan for the computer laboratory.

## II. Materials Needed

- A computer running Windows 11 (no network connection required)
- This worksheet
- Week 6 lecture notes

## III. Background

Every PC has one or more network adapters (NICs), each with a permanent MAC address, and Windows always runs the TCP/IP software, including the loopback address `127.0.0.1` (IPv4) and `::1` (IPv6). Network settings can be inspected in Settings, Control Panel, CMD, and PowerShell.

> **Reminder:** This is a viewing activity. Do not change, disable, or reset any network setting.

---

## V. Data and Findings

### Table 1. Network Adapters in Settings (Part A)

Connection status shown on the Network & internet page: _(not recorded)_

| Adapter Name | Description | Physical Address (MAC) | Status |
| --- | --- | --- | --- |
| Ethernet 2 | ExpressVPN TAP Adapter | 00:ff:b1:9c:2c:17 | Not operational |
| Local Area Connection* 1 | Microsoft Wi-Fi Direct Virtual | b6:8c:9d:7b:05:23 | Not operational |
| Bluetooth Network Connection | Bluetooth Device (Personal Area Network) | b4:8c:9d:7b:05:22 | Not operational |
| Wi-Fi | Realtek RTL8821CE 802.11ac | b4:8c:9d:7b:05:23 | Operational |

### Table 2. Adapter Properties (Part B)

| Items Checked Under "This connection uses the following items" | Item Checked (continued) |
| --- | --- |
| Client for Microsoft Networks | Microsoft LLDP Protocol Driver |
| File and Printer Sharing Networks | Internet Protocol Version 6 (TCP/IPv6) |
| QoS Packet Scheduler | Link-Layer Topology Discovery Responder |
| Bridge Driver | Link-Layer Topology Mapper I/O Driver |

| TCP/IPv4 Setting | Automatic or Static? |
| --- | --- |
| IP address | Static |
| DNS server address | Static |

### Table 3. Command Prompt Results (Part C)

| Command / Item | Your Findings | OSI Layer |
| --- | --- | --- |
| `hostname` | LAPTOP-S4VDU685 | 7 |
| `getmac /v`: MAC address and Transport Name | Ethernet 2 (00-FF-B1-9C-2C-17)<br>Local Area Conn<br>Wi-Fi (B4-8C-9D-7B-05-23)<br>Bluetooth (B4-8C-9D-7B-05-22) | 2 |
| `ipconfig /all`: Media State of Ethernet | Media Disconnected | — |
| `ipconfig /all`: DHCP Enabled | Yes | — |
| `ping 127.0.0.1`: replies and time | Reply from 127.0.0.1: bytes=32 time<1ms TTL=128 | 3 |
| `ping localhost`: address that replied | Reply from ::1: time<1ms | — |
| `ping 8.8.8.8`: exact message | Reply from 8.8.8.8: bytes=32 time=138ms TTL=1 | 3 |
| `nslookup google.com`: exact message | Server: UnKnown<br>Address: 10.0.48.1<br>Non-authoritative answer:<br>Name: google.com<br>Addresses: 2404:6800:4017:806::200e, 142.250.207.46 | 7 |
| `netstat -an`: three LISTENING addresses:ports | TCP 0.0.0.0:135 0.0.0.0:0 LISTENING<br>TCP 0.0.0.0:445 0.0.0.0:0 LISTENING<br>TCP 0.0.0.0:5040 0.0.0.0:0 LISTENING | 4 |
| `route print -4`: Netmask for 127.0.0.0 | 255.0.0.0 | — |
| `systeminfo`: number of NICs and status | 4 NIC(s) Installed | — |

### Table 4. PowerShell Results (Part D)

| Command | What It Showed | Matches CMD? (Y/N) |
| --- | --- | :---: |
| `Get-NetAdapter`: Name, Status, MacAddress, LinkSpeed | Ethernet 2 (ExpressVPN TAP Adapter)<br>Wi-Fi (Realtek RTL8821CE 802.11ac PCIe Adapter)<br>Local Area Connection (ExpressVPN Wintun Driver)<br>Bluetooth Network Connection (Bluetooth Device Personal Area Network) | Y |
| `Get-NetIPAddress`: IPv4 addresses and PrefixLength | Wi-Fi: 10.0.48.19 (PrefixLength: 24)<br>Loopback: 127.0.0.1 (PrefixLength: 8)<br>Other adapters: 169.254.x.x (PrefixLength: 16) | Y |
| `Test-Connection 127.0.0.1`: result | Source: LAPTOP-S4V..., Destination: 127.0.0.1, IPv4Address: 10.0.48.19, Bytes: 32, Time(ms): 0 | Y |
| `Get-NetTCPConnection -State Listen`: two LocalAddress:LocalPort pairs | 127.0.0.1:51489, 127.0.0.1:2015 (and other address/port pairs) | Y |

### Sample Output 1. `ipconfig /all` (excerpt) from a networked PC

```
Host Name . . . . . . . . . . . : CICT-LAB-PC07
Ethernet adapter Ethernet:
   Description . . . . . . . . . : Realtek PCIe GbE Family Controller
   Physical Address. . . . . . . : 3C-52-82-A1-7F-09
   DHCP Enabled. . . . . . . . . : Yes
   Link-local IPv6 Address . . . : fe80::8c2d:4a1f:93b2:61e5%12(Preferred)
   IPv4 Address. . . . . . . . . : 192.168.10.37(Preferred)
   Subnet Mask . . . . . . . . . : 255.255.255.0
   Lease Obtained. . . . . . . . : Wednesday, October 14, 2026 8:02:15 AM
   Lease Expires . . . . . . . . : Thursday, October 15, 2026 8:02:15 AM
   Default Gateway . . . . . . . : 192.168.10.1
   DHCP Server . . . . . . . . . : 192.168.10.1
   DNS Servers . . . . . . . . . : 192.168.10.1, 8.8.8.8
```

### Table 5. Sample Output Analysis (Part E)

| Question | Your Answer |
| --- | --- |
| IPv4 address of PC07 | 192.168.10.37 |
| Subnet mask (and its /prefix) | 255.255.255.0 (/24) |
| Default gateway, and what device it usually is | 192.168.10.1 (Router) |
| DNS servers | 192.168.10.1 and 8.8.8.8 |
| Was the address assigned by DHCP or set manually? How do you know? | Assigned by DHCP |
| Public or private? Which RFC 1918 range? | 192.198.168.0.0 to 192.168.25.255 (Class C) |
| IPv4 address of PC07 in binary (four octets) | 11000000.10101000.00001 |
| MAC address, and the OSI layer where it is used | 3C-52-82-A1-7F-09, Layer 2 |
| How long is the DHCP lease? | 24 Hours |
| If the DHCP server failed, what kind of address would PC07 assign itself? | APIPA |

### Figure 1. Proposed Laboratory Network Diagram (Part F)

Star topology: four PCs, a printer, and a router all connected to a central switch.

![Figure 1 – Laboratory network diagram](images/figure-1-network-diagram.jpg)

### Table 6. IP Address Plan (Part F)

| Device | IPv4 Address | Subnet Mask | Default Gateway |
| --- | --- | --- | --- |
| Router (gateway) | 192.168.10.1 | 255.255.255.0 | — |
| PC 1 | 192.168.10.2 | 255.255.255.0 | 192.168.10.1 |
| PC 2 | 192.168.10.3 | 255.255.255.0 | 192.168.10.1 |
| PC 3 | 192.168.10.4 | 255.255.255.0 | 192.168.10.1 |
| PC 4 | 192.168.10.5 | 255.255.255.0 | 192.168.10.1 |
| Printer | 192.168.10.5 | 255.255.255.0 | 192.168.10.1 |

### Part G Notes (Optional / Bonus)

_(Left blank in the original worksheet.)_

---

## VI. Realization (Reflection)

**1. Even without a network connection, what network information did your computer still have? Why do you think it still has it?**

> Yes, I think that my computer has it's information like the IP address, MAC address and network adapter details. This keep information because it is needed to identify the device and connect to a network.

**2. Compare Settings, Control Panel, CMD, and PowerShell. Which tool did you find most useful for viewing network information, and why?**

> For me the useful tool that I find is the CMD because it quickly show the information using the command also it's simple to use.

**3. What did the messages from `ping 8.8.8.8` and `nslookup google.com` tell you? How would you use messages like these when troubleshooting a real network?**

> The ping 8.8.8.8 command check if my computer can reach an internet server, while nslookup google.com check the domain name can be translated into an IP address.

**4. Why should you be careful about sharing your MAC address, IP address, or `ipconfig /all` screenshots online? Relate your answer to data privacy.**

> Because sharing my device can access my privacy and it might use to harm someone or scam by using my information.

---

## VII. Scoring Guide

| Criteria | Description | Points |
| --- | --- | --- |
| Completeness | All tables (1–6) and required fields are filled out. | 25 |
| Accuracy | Recorded values match what the tools show and are consistent across Settings, CMD, and PowerShell; sample analysis, binary conversion, and OSI layers are correct. | 30 |
| Network Design | Diagram shows a star topology with PCs, switch, router, printer, and ISP; IP plan uses valid, non-duplicate addresses in 192.168.10.0/24. | 15 |
| Realization / Reflection | Answers are thoughtful, based on the student's own results, and well written. | 20 |
| Neatness & Timeliness | Worksheet is legible/well-formatted and submitted on time. | 10 |
| **Total** | | **100** |
