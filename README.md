# Home Cyber Lab Day 1 8/30/2026

## Objective
This project establishes a home lab environment to build hands-on skills in Digital Forensics and Incident Response (DFIR). The lab uses two machines on my home network: an older computer configured as a target/server system, and my main computer running a working VM for analysis and monitoring. The goal is to practice identifying systems on a network, gaining access, and pulling information through tools like Nmap and Wireshark, then build toward deeper detection and forensic analysis as the lab evolves.

### Skills Learned
*[Update as you go]*
- Setting up and configuring virtual machines for a home lab environment
- Configuring an isolated wireless router as a private lab network
- Diagnosing and resolving Windows Firewall/ICMP connectivity issues between lab machines
- Network discovery and reconnaissance using Nmap
- Network traffic analysis using Wireshark
- (Add more as skills are developed)

### Tools Used
- **Target/Server Machine:** Older laptop, wiped and reimaged, repurposed as a target system hosting a Metasploitable2 VM
- **Working Machine:** Daily driver running VMware Workstation, hosting a Kali Linux VM for monitoring and analysis
- **Lab Network:** ASUS Wireless-AC1300 (RT-ACRH13) router, WAN disconnected, used as an isolated network separate from the home network
- **Monitoring OS (TBD):** SANS SIFT Workstation or Kali Linux — still deciding
- **Network Analysis:** Nmap, Wireshark

## Steps

### Step 1: Lab Planning & Architecture
*[Add a network diagram screenshot here once the lab layout is finalized]*
*Ref 1: Network Diagram*

I'm building a home network to practice cybersecurity concepts. The first requirement is a safe way to run VMs and vulnerable images for testing.

I'm using two systems: my daily driver, which will host a Kali Linux VM to provide my working toolset, and an older second computer, which will host a Metasploitable2 VM as the target.

The core problem: I don't want a machine with known, exploitable vulnerabilities anywhere near my home network. To solve this, I built a second, fully isolated lab network with no connection to my home network. The exploit computer (old laptop) lives permanently on this separate network. My daily driver only joins it when I'm actively running a lab session. The old laptop was wiped and freshly imaged before starting, so I can re-image it again later if needed.

To build the isolated network, I repurposed an old ASUS wireless router. My plan was: install VMware on both systems, download the VM images I'd need, get the isolated network running, connect both computers to it, and confirm connectivity with a ping test.

#### Network Setup — ASUS Wireless-AC1300 (RT-ACRH13)
- Reset router via the reset button
- Connected to the wireless network via default password
- Accessed router admin panel via 192.168.50.1
- Established new networks for testing on both the 2.4GHz and 5GHz bands
- Confirmed WAN is disconnected, so the network has no path to the internet or home network

#### Connectivity Check: Ping Troubleshooting
Once the router was configured and both systems were connected, I ran a ping test to confirm they could actually communicate.

**Symptom**
Two Windows machines connected to the same isolated lab router (via Wi-Fi), each with a valid, distinct IP address on the same subnet (e.g., 192.168.50.201 and 192.168.50.210). Pinging from one machine to the other returned "Request timed out" for every attempt.

**Root Cause**
Windows Defender Firewall blocks incoming ICMP Echo Requests (ping) by default on both Windows 10 and 11. The network connection itself was fine — both machines had correct IPs and were on the same subnet — but Windows was silently dropping the ping packets before responding, making it look like a connectivity failure when it was actually a firewall rule.

**Fix: Enable ICMPv4 Echo Request in Windows Firewall**
Performed on both machines, since each one needs to allow incoming pings in order for the other machine to successfully ping it.

1. Open Windows Defender Firewall (search "firewall" in the Start menu).
2. Click Advanced settings in the left sidebar (admin approval may be required).
3. Click Inbound Rules in the left panel.
4. Locate the rule(s) named "File and Printer Sharing (Echo Request - ICMPv4-In)" — there are usually two: one for Private/Domain profiles, one for Public.
5. Right-click each rule and select Enable Rule.
6. Repeat steps 1–5 on the second machine.
7. Re-test: `ping 192.168.50.XXX` (the other machine's IP) — should now return replies instead of timing out.

**Result**
Confirmed working — both machines successfully pinged each other after enabling the rule.

**Next step:** confirm the VMs themselves — not just the host machines — can reach each other across the lab network. With host-level ping working, the network foundation looks solid.

### Step 2: Setting Up the Target Machine
**Installing Metasploitable2 on the Exploit Computer**

The old laptop was freshly reimaged and used as the host for the target VM.

*[TBD — continue documenting: VM software used, Metasploitable2 import steps, network adapter configuration, and any issues encountered]*

### Step 3: Setting Up the Working/Monitoring Machine
**Installing the Working VM — SANS SIFT vs. Kali Linux**

Installation went smoothly. I already had VMware Workstation set up on my daily driver from working toward my GCIH cert, so I used that as the base rather than installing from scratch.

Before starting lab work, I made sure Kali was fully up to date.

*[TBD — continue: document the update commands used (e.g. `apt update && apt upgrade`) and note any issues encountered]*

*[Add final decision: SANS SIFT vs. Kali Linux, and the reasoning behind it]*

### Step 4: Network Discovery
*[Document using Nmap to identify the target machine on the network — include screenshots and command output]*

### Step 5: Traffic Analysis
*[Document capturing and analyzing traffic with Wireshark]*

### Step 6: Findings & Lessons Learned
*[Summarize what worked, what didn't, and what you'd do differently]*
### Step 6: Findings & Lessons Learned
*[Summarize what worked, what didn't, and what you'd do differently]*
