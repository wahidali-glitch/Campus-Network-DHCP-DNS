# 🌐 Campus Network with DHCP & DNS Services

> A simulated multi-site campus network built in **Cisco Packet Tracer**, featuring dynamic IP allocation via DHCP, domain name resolution via DNS, and inter-site connectivity through static routing.

---

## 📌 Project Overview

This project simulates a real-world campus network infrastructure for an educational institution. Three geographically separated sites (departments) are interconnected via WAN serial links, with centralized server services providing DHCP and DNS to all connected clients across subnets.

Designed and implemented as part of the **Computer Networks & Data Communications** course at **Shaheed Zulfikar Ali Bhutto Institute of Science & Technology (SZABIST)**.

---

## 🗺️ Network Topology

```
[Web Server]  [DNS Server]          [DHCP Server]  [PC0]          [PC1]  [PC2]
      |              |                     |           |              |      |
   [ Switch0  ]                       [ Switch1  ]              [ Switch2 ]
         |                                 |                          |
     [Router0] =====(Se2/0↔Se2/0)===== [Router1] ===(Se3/0↔Se2/0)=== [Router2]
      Fa0/0                               Fa0/0                       Fa0/0
   10.10.10.0/8                        20.20.20.0/8                50.50.50.0/8
                          WAN: 40.40.40.0/8 (R1↔R2)
```

| Site | Router | LAN Network | Devices |
|------|--------|-------------|---------|
| Left (Site A) | Router0 | `10.10.10.0/8` | Web Server, DNS Server |
| Center (Site B) | Router1 | `20.20.20.0/8` | DHCP Server, PC0 |
| Right (Site C) | Router2 | `50.50.50.0/8` | PC1, PC2 |
| WAN (R1↔R2) | — | `40.40.40.0/8` | Router1, Router2 |

---

## 🔧 Technologies & Services

| Component | Technology |
|-----------|------------|
| Simulation Tool | Cisco Packet Tracer |
| Dynamic IP Assignment | DHCP (Dynamic Host Configuration Protocol) |
| Domain Name Resolution | DNS (Domain Name System) |
| Web Hosting | HTTP Server |
| Inter-site Routing | Static Routing |
| WAN Links | Serial (Se2/0, Se3/0) |

---

## 📋 IP Addressing Scheme

### Site A — Left LAN (`10.10.10.0/8`)

| Device | IP Address | Subnet Mask | Default Gateway |
|--------|------------|-------------|-----------------|
| Web Server | 10.10.10.2 | 255.0.0.0 | 10.10.10.1 |
| DNS Server | 10.10.10.3 | 255.0.0.0 | 10.10.10.1 |
| Router0 (Fa0/0) | 10.10.10.1 | 255.0.0.0 | — |

### Site B — Center LAN (`20.20.20.0/8`)

| Device | IP Address | Subnet Mask | Default Gateway |
|--------|------------|-------------|-----------------|
| DHCP Server | 20.20.20.2 | 255.0.0.0 | 20.20.20.1 |
| PC0 | DHCP | 255.0.0.0 | 20.20.20.1 |
| Router1 (Fa0/0) | 20.20.20.1 | 255.0.0.0 | — |

### WAN Link — Router1 ↔ Router2 (`40.40.40.0/8`)

| Device | IP Address | Subnet Mask |
|--------|------------|-------------|
| Router1 (Se3/0) | 40.40.40.1 | 255.0.0.0 |
| Router2 (Se2/0) | 40.40.40.2 | 255.0.0.0 |

### Site C — Right LAN (`50.50.50.0/8`)

| Device | IP Address | Subnet Mask | Default Gateway |
|--------|------------|-------------|-----------------|
| PC1 | DHCP | 255.0.0.0 | 50.50.50.1 |
| PC2 | DHCP | 255.0.0.0 | 50.50.50.1 |
| Router2 (Fa0/0) | 50.50.50.1 | 255.0.0.0 | — |

---

## ⚙️ Key Configurations

### DHCP Relay (ip helper-address)
Since the DHCP server resides in Site B, routers at Site A and Site C must forward DHCP broadcast requests using:
```
Router(config-if)# ip helper-address 20.20.20.2
```

### Static Routing Example (Router0)
```
Router0(config)# ip route 20.20.20.0 255.0.0.0 <WAN-next-hop>
Router0(config)# ip route 50.50.50.0 255.0.0.0 <WAN-next-hop>
```

### DNS Server
- Configured at `10.10.10.3`
- Resolves hostnames (e.g., `www.campus.com`) to the Web Server IP (`10.10.10.2`)

---

## 🚀 How to Open

1. Install **[Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer)** (version 8.x recommended)
2. Clone this repository:
   ```bash
   git clone https://github.com/wahidali-glitch/Campus-Network-DHCP-DNS.git
   ```
3. Open `DNS_DHCP_by_Static_routing.pkt` in Cisco Packet Tracer
4. Use **Simulation Mode** to observe packet flow between devices

---

## ✅ Testing & Verification

| Test | Command | Expected Result |
|------|---------|-----------------|
| Ping between sites | `ping 50.50.50.2` from PC0 | Reply received |
| DHCP assignment | Check PC IP config | Auto-assigned from DHCP pool |
| DNS resolution | `nslookup www.campus.com` | Returns Web Server IP |
| Web access | Open browser → `www.campus.com` | Web page loads |

---

## 📁 Repository Structure

```
Campus-Network-DHCP-DNS/
│
├── DNS_DHCP_by_Static_routing.pkt   # Cisco Packet Tracer simulation file
├── README.md                         # Project documentation
└── LICENSE                           # License information
```

---

## 🎯 Learning Outcomes

- Designed a hierarchical 3-site campus network topology
- Configured DHCP for dynamic IP assignment across multiple subnets
- Implemented DHCP relay (`ip helper-address`) for cross-subnet DHCP
- Set up DNS for domain name resolution across the network
- Configured static routes for inter-site WAN connectivity
- Hosted and accessed a web server across different network segments

---

## 👤 Author

**Wahid Ali**
BS Computer Science — SZABIST
GitHub: [@wahidali-glitch](https://github.com/wahidali-glitch)

---

## 📄 License

This project is licensed under the terms of the [LICENSE](LICENSE) file included in this repository.
