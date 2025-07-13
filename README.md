# tp-link-er605-vlan-failover-demo
# 🛠️ SME Branch Network – TP-Link ER605 VLAN + Failover Setup

## 📍 Overview
This is a demonstration of setting up **network segmentation and dual WAN load balancing** using a **TP-Link ER605 Gigabit VPN Router** for a small-to-medium enterprise (SME) branch office.

---

## 🎯 Project Goals
- Enable **load balancing and failover** between two ISP connections
- Segment internal departments using **VLANs**
- Enhance internal **network security** through isolation

---

## 🌐 Dual WAN Setup

| Interface | ISP Label | Role               | IP Type     |
|-----------|-----------|--------------------|-------------|
| WAN1      | ISP 1     | Primary / Load Balancing | Static / DHCP |
| WAN2      | ISP 2     | Secondary / Load Balancing | Static / DHCP |

✅ **Load Balancing Mode**: Both WANs are active; if one fails, the other automatically takes over.

---

## 🔀 VLAN Configuration

| VLAN ID | Subnet           | Description       | DHCP     |
|---------|------------------|-------------------|----------|
| 1       | 192.168.10.1/24  | General Office LAN| Enabled  |
| 10      | 192.168.1.1/24 | Surveillance Zone | Optional (Static Preferred) |

✅ VLANs are isolated to prevent cross-department access unless explicitly allowed.

---

## 🔐 Firewall & Security

- Blocked inter-VLAN traffic (VLAN10 → VLAN1)
- DHCP enabled per VLAN or managed by external DHCP
- Static IP used for NVR/CCTV on VLAN 10

---

## 🧱 Network Devices

| Router Port | Connected Device     | VLAN |
|-------------|----------------------|------|
| WAN1        | ISP 1 Modem          | -    |
| WAN2        | ISP 2 Modem          | -    |
| LAN1        | Office Switch (LAN)  | 10   |
| LAN2        | NVR / IP Cameras     | 1    |

---

## 💾 Management

- Router used in **Standalone Mode** (not via Omada controller)
- Configuration managed via **Web UI**


## 🌐 Dual WAN Setup

### WAN1 (ISP 1)
- Role: Primary
- Connection: Static or DHCP
- Function: Handles general office traffic

### WAN2 (ISP 2)
- Role: Secondary (load balanced)
- Connection: Static or DHCP
- Function: Redundancy & shared load

---

## 🔁 Load Balancing

- Both ISPs contribute to bandwidth
- Router auto-fails over to backup WAN if primary fails

---

## 🔀 VLAN Segmentation

### VLAN 1 – General Users
- Subnet: `192.168.10.1/24`
- DHCP: Enabled
- Devices: Office PCs, Admin Staff

### VLAN 10 – Surveillance
- Subnet: `192.168.1.1/24`
- DHCP: Disabled (Manual IPs for NVR & Cameras)
- Devices: NVR, IP Cameras

---

## 🔐 Firewall/ACL Rules

| Rule                | Source VLAN | Destination VLAN | Action |
|---------------------|-------------|------------------|--------|
| Block Surveillance → Office | 10 | 1 | Deny   |
| Allow Office → Surveillance | 1  | 10 | Optional Allow |

---

## 🖧 Physical Port Mapping

| Router Port | Device Connected   | VLAN |
|-------------|--------------------|------|
| WAN1        | ISP 1 Modem        | -    |
| WAN2        | ISP 2 Modem        | -    |
| LAN1        | Office Switch      | 1    |
| LAN2        | NVR Switch / CCTV  | 10   |

