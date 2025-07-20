# Office-Network-Design
This project involves the design and simulation of a secure, cost-effective, and scalable network using Cisco Packet Tracer. The objective is to create a network layout that caters to the connectivity and isolation needs of various functional areas within a company environment, such as the reception, office, machine room, and meeting rooms.

## 🧠 Final Topology & Justification

### 🌐 Final Topology

In this implemented network, a **star topology** was used to create connections between endpoints and core infrastructure.

---

#### 🏗️ Physical Layer:
- Two core switches are used as the central hub of the network.
- The core switches are interconnected using a trunk link.

#### 📶 Access Layer:
- Multiple access switches are deployed in different areas.
- Each access switch is individually connected to the core switches via trunk ports.
- This centralizes control and simplifies management.

#### 💻 End Devices:
- End devices (e.g., PCs, printers, IoT devices) connect directly to their local access switches.
- Wireless Access Points (APs) are also connected to access switches and to the appropriate VLANs.

---

### 🔁 Logical Flow:
- All inter-VLAN routing is centrally handled through a **router-on-a-stick** setup or **Layer 3 interfaces** on the core switch.
- **DHCP** and **ACL rules** are centrally managed.
- VLAN segregation ensures isolation per department or zone.

---

### 🧱 VLAN Design & Subnetting

To create a secure and logically separated network, VLANs were used to isolate traffic:

| VLAN | Department / Area | Subnet Example       |
|------|--------------------|----------------------|
| 10   | Offices            | 192.168.10.0/24      |
| 20   | Kitchen            | 192.168.20.0/24      |
| 30   | Meeting Room       | 192.168.30.0/24      |
| 60   | Guest Wi-Fi        | 192.168.60.0/24      |
| 70   | Open Space         | 192.168.70.0/24      |
| 80   | Reception          | 192.168.80.0/24      |
| 88   | Technician         | 192.168.88.0/24      |
| 99   | Server             | 192.168.99.0/24      |

Each VLAN was assigned a dedicated subnet for better traffic management, routing, and ACL control.

---

### 🔐 ACL Implementation
- Access Control Lists (ACLs) were created to block communication between different VLANs.
- For example, the office VLAN cannot talk to the kitchen or reception VLANs.
- ACLs were applied to router sub-interfaces, allowing only essential traffic like internet access and machine room communication.
- Wireless and wired device isolation is enforced via ACLs.

---

### 📡 DHCP Deployment
- A **central DHCP server** assigns IP addresses to devices across all VLANs.
- Each VLAN has a configured DHCP pool.
- Default gateways match interface IPs (e.g., 192.168.70.1 for VLAN 70).
- `ip helper-address` commands were used to relay DHCP requests where needed.

---

### 🔁 Core Switching & Trunking
- Two core switches provide scalability and cable management.
- Trunk ports were configured between:
  - Access switches and core switches
  - The two core switches themselves
- Trunks carry multiple VLANs using **Dot1Q encapsulation**.

---

### 📶 Guest Wi-Fi (VLAN 60)
- VLAN 60 was created for guests with internet-only access.
- Devices in VLAN 60 receive IPs automatically via DHCP.
- ACLs block access to internal resources and allow NAT-based internet access only.

---

### 🧑‍💼 Staff Wi-Fi Design

| Area           | Access Points | VLAN |
|----------------|----------------|------|
| Offices        | 3              | 10   |
| Kitchen        | 1              | 20   |
| Meeting Room   | 1              | 30   |
| Reception      | 1 (Guest Wi-Fi)| 60   |
| Open Space     | 3              | 70   |
| Technician     | 1              | 88   |

- Each AP shares the same VLAN as nearby wired devices to maintain **isolation** and ensure local communication.

---

### 👨‍🔧 Technician & Server VLANs

- Technician PCs connect via console to the router.
- Dual-password authentication enforced.
- ISOLATION is maintained as the technician and server are on different VLANs.
- Gigabit Ethernet used for technician switch to enable:
  - Fast server access
  - Backup and config transfers
  - Minimal latency for critical tasks

---

### 🏢 Open Space Area (VLAN 70)
- Due to 8-port switch limitations:
  - 5 x 2960-24 switches were installed in the machine room.
  - Cables run via raised floors to the Open Space.
- VLAN 70’s DHCP pool assigns IPs automatically.
- Supports **up to 120 users** efficiently and affordably.

---

### 🖨️ Printer Access (VLAN 70)
- Networked printers are connected to OpenSpace VLAN 70.
- IPs assigned via DHCP.
- Only VLAN 70 users can access these printers.
- ACLs prevent access from other VLANs, maintaining security boundaries.

