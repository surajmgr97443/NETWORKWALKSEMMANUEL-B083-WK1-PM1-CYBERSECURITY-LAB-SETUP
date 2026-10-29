# NETWORKWALKSEMMANUEL-B083-WK1-PM1-CYBERSECURITY-LAB-SETUP
Networkwalks internship
# Internship Week 1 – Task 1: Virtual Lab Setup & Network Connectivity

**Name:** Suraj Pun Magar
**Company:** Networkwalks Academy
**Date:** 29-10-2026

---

## 1. Objective

Set up a virtual lab with Kali Linux, Windows 10 and an Android VM in VMware Workstation, connect them through the NAT network adapter, and verify connectivity by pinging from Kali to the Windows and Android machines.

---

## 2. Lab Environment

| Machine    | OS         | Role             | IP Address        |
|------------|------------|------------------|-------------------|
| Kali VM    | Kali Linux | Attacker machine | `10.10.10.129`    |
| Windows VM | Windows 10 | Target machine   | `10.10.10.132`  |
| Android VM | Android    | Target machine   | `10.10.10.19`  |

**Hypervisor:** VMware Workstation
**Network mode:** NAT (VMnet8), subnet `10.10.10.0/24`

### Kali VM Hardware

| Component       | Value      |
|-----------------|------------|
| Memory          | 8 GB       |
| Processors      | 4          |
| Hard Disk (SCSI)| 80.1 GB    |
| Network Adapter | NAT        |

---

## 3. VM Setup

1. Open VMware Workstation → **File → New Virtual Machine**.
2. Create three VMs: `kali`, `win_10` and `Android_VM`.
3. Select the ISO for each OS and complete the installation.
4. Allocate memory, processors and disk space to each VM.

<img width="1836" height="1042" alt="Screenshot 2026-10-29 180552" src="https://github.com/user-attachments/assets/1b695a99-b81f-4fb8-8b69-5cfe55a25094" />


---

## 4. Network Configuration (NAT Adapter)

All three VMs use the same **NAT** network adapter so they share one subnet and can reach each other and the internet.

For each VM:
1. Right-click the VM → **Settings**.
2. Select **Network Adapter**.
3. Tick **Connected** and **Connect at power on**.
4. Under Network connection, select **NAT: Used to share the host's IP address**.
5. Click **OK**.

*(Screenshot: `screenshots/kali-network-adapter-nat.png`)*

---

## 5. Kali IP Address

Command used on Kali:

```bash
ifconfig
```

Result for `eth0`:

| Field     | Value               |
|-----------|---------------------|
| IP (inet) | `10.10.10.129`      |
| Netmask   | `255.255.255.0`     |
| Broadcast | `10.10.10.255`      |
| MAC       | `00:0c:29:08:7c:57` |

*(Screenshot: `screenshots/kali-ifconfig.png`)*

---

## 6. Connectivity Test (Ping from Kali)

### 6.1 Kali → Machine 1 (`10.10.10.132`)

```bash
ping 10.10.10.132
```

- Packets: 5 transmitted, 5 received, **0% packet loss**
- Average response time: ~1.2 ms

### 6.2 Kali → Machine 2 (`10.10.10.19`)

```bash
ping 10.10.10.19
```

- Packets: 4 transmitted, 4 received, **0% packet loss**
- Average response time: ~1.1 ms

*(Screenshot: `screenshots/kali-ping-results.png`)*

---

## 7. Results

| Source | Destination                   | Result        |
|--------|-------------------------------|---------------|
| Kali   | Windows 10 (`10.10.10.<xxx>`) | ✅ Successful |
| Kali   | Android (`10.10.10.<xxx>`)    | ✅ Successful |

---

## 8. Troubleshooting Notes

- If ping times out, check that all VMs use the same NAT adapter.
- Windows Firewall may block ICMP; allow it with:
  ```cmd
  netsh advfirewall firewall add rule name="Allow ICMPv4" protocol=icmpv4:8,any dir=in action=allow
  ```
- Run `ipconfig` (Windows) or `ip a` (Android) to confirm each VM's IP.

---

## 9. Conclusion

Kali Linux, Windows 10 and Android were installed in VMware Workstation and connected through the NAT adapter. Pings from Kali (`10.10.10.129`) to both machines succeeded with 0% packet loss, confirming network connectivity for Week 1 Task 1.
