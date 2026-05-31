# 🌐 Campus Network — DHCP, DNS & Static Routing

> A fully simulated multi-site network built in **Cisco Packet Tracer** — covering dynamic IP allocation, domain name resolution, web hosting, and inter-site WAN connectivity through static routing.

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![Field](https://img.shields.io/badge/Field-Computer%20Networks%20%26%20Data%20Communication-0D9373?style=for-the-badge)
![Networking](https://img.shields.io/badge/Topics-DHCP%20%7C%20DNS%20%7C%20Static%20Routing-E05A2B?style=for-the-badge)

---

## 📸 Network Topology

![Network Topology](topology%20(2).png)

*Three interconnected sites linked via serial WAN links, each with its own subnet and switch — served by centralized DHCP, DNS, and Web servers.*

---

## 🧠 What This Project Does

This simulation models how a real-world multi-branch network operates:

- **Site A (Left)** hosts the **Web Server** and **DNS Server**
- **Site B (Center)** hosts the **DHCP Server** and a client PC
- **Site C (Right)** has two client PCs that receive IPs dynamically
- All three sites communicate over **WAN serial links** using **static routes**
- PCs across all sites automatically receive IP addresses from the central DHCP server via **DHCP relay (`ip helper-address`)**
- Users can browse `www.campus.com` from any PC — DNS resolves it, and the web server responds

---

## 🗺️ Topology Overview

| Site | Router | LAN Subnet | Devices |
|------|--------|------------|---------|
| Site A — Left | Router0 | `10.10.10.0/8` | Web Server, DNS Server |
| Site B — Center | Router1 | `20.20.20.0/8` | DHCP Server, PC0 |
| Site C — Right | Router2 | `50.50.50.0/8` | PC1, PC2 |
| WAN Link | R1 ↔ R2 | `40.40.40.0/8` | Router1, Router2 |

---

## 📋 IP Addressing Scheme

### Site A — Left LAN (`10.10.10.0/8`)

| Device | IP Address | Subnet Mask | Gateway |
|--------|------------|-------------|---------|
| Web Server | 10.10.10.2 | 255.0.0.0 | 10.10.10.1 |
| DNS Server | 10.10.10.3 | 255.0.0.0 | 10.10.10.1 |
| Router0 (Fa0/0) | 10.10.10.1 | 255.0.0.0 | — |

### Site B — Center LAN (`20.20.20.0/8`)

| Device | IP Address | Subnet Mask | Gateway |
|--------|------------|-------------|---------|
| DHCP Server | 20.20.20.2 | 255.0.0.0 | 20.20.20.1 |
| PC0 | *via DHCP* | 255.0.0.0 | 20.20.20.1 |
| Router1 (Fa0/0) | 20.20.20.1 | 255.0.0.0 | — |

### WAN — Router1 ↔ Router2 (`40.40.40.0/8`)

| Device | IP Address | Subnet Mask |
|--------|------------|-------------|
| Router1 (Se3/0) | 40.40.40.1 | 255.0.0.0 |
| Router2 (Se2/0) | 40.40.40.2 | 255.0.0.0 |

### Site C — Right LAN (`50.50.50.0/8`)

| Device | IP Address | Subnet Mask | Gateway |
|--------|------------|-------------|---------|
| PC1 | *via DHCP* | 255.0.0.0 | 50.50.50.1 |
| PC2 | *via DHCP* | 255.0.0.0 | 50.50.50.1 |
| Router2 (Fa0/0) | 50.50.50.1 | 255.0.0.0 | — |

---

## ⚙️ Key Configurations

### DHCP Relay — `ip helper-address`

The DHCP server lives in Site B. For PCs in Sites A and C to receive IPs automatically, the local router interface must relay DHCP broadcasts:

```cisco
Router(config-if)# ip helper-address 20.20.20.2
```

### Static Routing — Router0 Example

```cisco
Router0(config)# ip route 20.20.20.0 255.0.0.0 <next-hop>
Router0(config)# ip route 40.40.40.0 255.0.0.0 <next-hop>
Router0(config)# ip route 50.50.50.0 255.0.0.0 <next-hop>
```

### DNS Configuration

- DNS Server IP: `10.10.10.3`
- A Record: `www.campus.com` → `10.10.10.2` (Web Server)

### Web Server

- Hosts a simple HTTP page accessible from all three sites
- Reachable via IP address or domain name once DNS is set on client PCs

---

## 🚀 Getting Started

1. Download and install **[Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer)** (v8.x recommended — free with a Networking Academy account)

2. Clone this repo:
   ```bash
   git clone https://github.com/wahidali-glitch/Campus-Network-DHCP-DNS.git
   ```

3. Open `DNS_DHCP_by_Static_routing.pkt` in Packet Tracer

4. Switch to **Simulation Mode** to watch packets travel between sites in real time

---

## ✅ Testing Checklist

| Test | How to Test | Expected Result |
|------|-------------|-----------------|
| Cross-site ping | `ping 50.50.50.x` from PC0 | Successful replies |
| DHCP lease | PC → Desktop → IP Configuration | Auto-assigned IP from pool |
| DNS resolution | PC browser → `www.campus.com` | Web page loads |
| Direct web access | PC browser → `10.10.10.2` | Web page loads via IP |

---

## 📁 Repository Structure

```
Campus-Network-DHCP-DNS/
│
├── DNS_DHCP_by_Static_routing.pkt   # Cisco Packet Tracer simulation file
├── topology (2).png                  # Network topology screenshot
├── README.md                         # This file
└── LICENSE                           # MIT License
```

---

## 🔖 Field

**Computer Networks & Data Communication (CNDC)**

Core concepts covered in this project:

| Concept | Description |
|---------|-------------|
| **IP Addressing & Subnetting** | Assigning logical addresses across multiple subnets |
| **DHCP** | Automatic IP assignment using a centralized server with relay |
| **DNS** | Domain name to IP resolution across subnets |
| **Static Routing** | Manual route configuration for inter-site communication |
| **WAN Links** | Serial connections simulating wide-area network links |
| **HTTP / Web Services** | Hosting and accessing a web server across the network |
| **Network Hierarchy** | Access → Distribution → Core layered design |

---

## 👤 Author

**Wahid Ali**
GitHub: [@wahidali-glitch](https://github.com/wahidali-glitch)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
