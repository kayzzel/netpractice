# NetPractice

*This project has been created as part of the 42 curriculum by gabach.*

## Description

NetPractice is a practical networking exercise designed to introduce students to the fundamentals of computer networking. The project focuses on configuring small-scale networks through a web-based training interface where students work with simulated network topologies.

**Goal:** To develop a solid understanding of how TCP/IP addressing works and how to properly configure network devices to enable communication between hosts.

**Overview:** This project involves solving 10 networking problems of increasing complexity. Each level presents a non-functioning network diagram with various devices (hosts, routers, switches) that students must configure correctly. Through hands-on practice, learners understand key networking concepts including IP address assignment, subnet masks, default gateways, and routing.

## Instructions

### Prerequisites
- Python 3 (for running the local web server)
- A modern web browser

### Running the Training Interface

1. **Extract the project files** download, extract, and move into the project resources:
    ```sh
    wget https://cdn.intra.42.fr/document/document/45519/net_practice.1.9.tgz
    tar xvf net_practice.1.9.tgz
    cd net_practice
    ```

2. **Start the web server** using the provided script:
   ```bash
   ./run.sh
   ```
   This script will automatically launch a local web server and open the training interface in your default web browser.

3. **Alternative manual startup** (if `run.sh` does not work):
   ```bash
   python3 -m http.server 49242
   ```
   Then navigate to `http://localhost:49242` in your web browser (you can use a different port number if needed).

4. **Access the interface:**
   - Click the "Training" tab to practice with your personal login configuration
   - Click the "Evaluation" tab to generate random configurations for self-testing

### Working Through Levels

- Each level displays a network diagram with unshaded (editable) fields
- Use the **[Check again]** button to verify your configuration
- Use the **[Get my config]** button to export your configuration file
- Progress through all 10 levels, exporting your configuration after each successful completion

### Exporting Configurations

Before moving to the next level, always export your configuration:
1. Click **[Get my config]** button
2. The files are stored with the format `level_X` (where X is the level number 1-10)
3. These files are stored at the root of your Git repository

### Submission Requirements

- Submit exactly **10 configuration files** (one per level) at the repository root
- Files should be named clearly (e.g., `level_1`, `level_2`, etc.)
- Ensure you entered your login in the training interface before exporting
- Include this README.md at the repository root
- All files must be committed to your Git repository

## Resources

### Networking Concepts Covered - Detailed Explanations

#### 1. TCP/IP Addressing (IPv4)

**What it is:**
IPv4 (Internet Protocol version 4) addresses are the fundamental identifiers for devices on a network. Every device that communicates over a network must have a unique IP address within its network.

**How it works:**
- An IPv4 address consists of **four octets** (groups of 8 bits), separated by dots
- Example: `192.168.1.100`
- Each octet is a number from 0 to 255 (2^8 possible values)
- This gives a total of 2^32 (approximately 4.3 billion) possible IPv4 addresses

**Address Structure:**
An IPv4 address is divided into two parts:
- **Network portion** - Identifies the specific network
- **Host portion** - Identifies the specific device on that network

Example: In `192.168.1.100` with a subnet mask of `255.255.255.0`:
- Network portion: `192.168.1.0` (the network)
- Host portion: `.100` (the specific device)

**IP Address Classes (Legacy):**
- **Class A** (1.0.0.0 - 126.255.255.255) - Large networks, first octet determines network
- **Class B** (128.0.0.0 - 191.255.255.255) - Medium networks, first two octets determine network
- **Class C** (192.0.0.0 - 223.255.255.255) - Small networks, first three octets determine network

**Private IP Ranges (RFC 1918):**
- Class A private: `10.0.0.0 - 10.255.255.255`
- Class B private: `172.16.0.0 - 172.31.255.255`
- Class C private: `192.168.0.0 - 192.168.255.255`

These private addresses cannot be routed on the public internet and are used for internal networks.

---

#### 2. Subnet Masks

**What it is:**
A subnet mask is a 32-bit value (in the same dotted-decimal format as IP addresses) that determines which portion of an IP address is the network and which is the host.

**How it works:**
- A subnet mask uses binary notation where `1` bits indicate the network portion and `0` bits indicate the host portion
- Common subnet masks:
  - `255.255.255.0` (binary: 24 ones, 8 zeros) - divides the address at the third octet
  - `255.255.255.128` (binary: 25 ones, 7 zeros) - divides the address in the middle of the fourth octet
  - `255.255.0.0` (binary: 16 ones, 16 zeros) - divides the address at the second octet

**CIDR Notation:**
Modern networks use CIDR (Classless Inter-Domain Routing) notation:
- `/24` means the first 24 bits are the network, last 8 bits are the host
- `192.168.1.0/24` is equivalent to `192.168.1.0` with subnet mask `255.255.255.0`

**Calculating Usable Addresses:**
- If you have 8 host bits available: 2^8 = 256 addresses
- However, two addresses are reserved:
  - **Network address** (all host bits = 0) - e.g., `192.168.1.0`
  - **Broadcast address** (all host bits = 1) - e.g., `192.168.1.255`
- **Usable addresses** = 256 - 2 = 254 addresses for hosts

**Example:**
```
IP Address:      192.168.1.100
Subnet Mask:     255.255.255.0
Network Address: 192.168.1.0 (identify the network)
Broadcast:       192.168.1.255 (send to all devices on this network)
Usable Range:    192.168.1.1 - 192.168.1.254
```

---

#### 3. Default Gateway

**What it is:**
The default gateway is the IP address of the router interface that devices use to send traffic to other networks.

**How it works:**
When a device wants to send data to another device:

1. **Same Network Communication:**
   - Device checks if the destination IP is on the same network (using subnet mask)
   - If yes: Send the packet directly to that device using ARP (Address Resolution Protocol)

2. **Different Network Communication:**
   - Device checks if the destination IP is on a different network
   - If yes: Send the packet to the default gateway instead
   - The gateway then forwards the packet toward its destination

**Example:**
```
Device A: 192.168.1.10 (subnet mask 255.255.255.0)
Wants to reach: 192.168.2.20

Device A calculates:
- Am I on the same network as 192.168.2.20?
- My network: 192.168.1.0 (first 3 octets match my subnet)
- Their network: 192.168.2.0 (first 3 octets are different)
- NO, they're on a different network
- So Device A sends the packet to the default gateway: 192.168.1.1
- The gateway knows how to reach 192.168.2.0
```

**Important:** Every device needs a default gateway configured to communicate outside its local network.

---

#### 4. Routers

**What it is:**
A router is a network device that operates at **Layer 3 (Network Layer)** of the OSI model. It forwards data packets between different networks based on IP addresses.

**How routers work:**

1. **Receiving Packets:** Router receives a packet on one of its interfaces
2. **Reading the Destination:** Router examines the destination IP address
3. **Consulting Routing Table:** Router looks up the destination IP in its routing table
4. **Forwarding Decision:** Router decides which interface to send the packet out of
5. **Sending Packet:** Router sends the packet toward the destination

**Router Interfaces:**
- Routers have multiple network interfaces (e.g., `eth0`, `eth1`)
- Each interface has its own IP address
- Each interface belongs to a different network

**Routing Table:**
A routing table is a set of rules (called routes) that tell the router:
- "If you see a packet destined for network X, send it out interface Y"

Example routing table:
```
Destination Network    Subnet Mask        Gateway           Interface
192.168.1.0           255.255.255.0      Direct            eth0
192.168.2.0           255.255.255.0      192.168.1.254     eth0
10.0.0.0              255.255.0.0        192.168.2.254     eth1
0.0.0.0               0.0.0.0            192.168.1.1       eth0 (default)
```

---

#### 5. Switches

**What it is:**
A switch is a network device that operates at **Layer 2 (Data Link Layer)** of the OSI model. It connects devices on the same network using MAC addresses (physical addresses).

**How switches work:**

1. **Learning:** Switch learns MAC addresses by examining the source MAC of incoming frames
2. **Forwarding:** When a frame arrives, switch looks up the destination MAC in its MAC address table
3. **Decision:**
   - If destination MAC is known: forward to that specific port
   - If destination MAC is unknown: flood to all ports (except incoming port)
4. **Building Tables:** Over time, the switch learns all MAC addresses and creates an optimal forwarding table

**Key Differences from Routers:**
- **Switches** forward based on MAC addresses (Layer 2) within the same network
- **Routers** forward based on IP addresses (Layer 3) between different networks
- **Switches** are "dumb" - they don't understand IP addresses
- **Routers** are "smart" - they understand IP routing and can make intelligent forwarding decisions

**Example:**
```
A switch receives a frame from Device A (MAC: AA:AA:AA:AA:AA:AA)
- Switch notes: "AA:AA:AA:AA:AA:AA is on Port 1"
- Frame is destined for Device B (MAC: BB:BB:BB:BB:BB:BB)
- Switch doesn't know where B is
- Switch floods the frame to all ports (except Port 1)
- Device B receives the frame and responds
- Switch notes: "BB:BB:BB:BB:BB:BB is on Port 2"
- Next time: Device A sends to B, switch forwards directly Port 1 → Port 2
```

---

#### 6. OSI Model Layers

**What it is:**
The OSI (Open Systems Interconnection) model is a conceptual framework that divides network communication into 7 layers. Each layer has specific functions and works with the layers above and below it.

**The 7 Layers (from bottom to top):**

| Layer | Name | Function | Examples | Unit |
|-------|------|----------|----------|------|
| 7 | Application | User applications and services | HTTP, HTTPS, SMTP, FTP, SSH | Data |
| 6 | Presentation | Data formatting, encryption, compression | SSL/TLS, JPEG, MPEG | Data |
| 5 | Session | Manages connections and sessions | Session management, authentication | Data |
| 4 | Transport | End-to-end communication, reliability | TCP, UDP | Segment |
| 3 | Network | Routing, IP addressing | IP, ICMP, Routers | Packet |
| 2 | Data Link | MAC addressing, switching | Ethernet, MAC addresses, Switches | Frame |
| 1 | Physical | Physical transmission of bits | Cables, signals, voltages | Bits |

**How it applies to this project:**
- **Layer 3 (Network):** This project focuses heavily on IP addresses and routing - core Layer 3 concepts
- **Layer 2 (Data Link):** Understanding how switches forward frames using MAC addresses
- **Routers operate at Layer 3:** They make decisions based on IP addresses
- **Switches operate at Layer 2:** They make decisions based on MAC addresses

**Key Concept:**
Data flows down the layers on the sending device, across the network, and back up the layers on the receiving device.

---

#### 7. Network Communication Flow

**How two devices on different networks communicate:**

```
Step 1: Device A wants to send data to Device C (different network)
Device A: 192.168.1.10/24
Device C: 192.168.2.30/24
Router Interface 1: 192.168.1.1/24
Router Interface 2: 192.168.2.1/24

Step 2: Device A checks its subnet mask
- "Is 192.168.2.30 on my network (192.168.1.0)?"
- NO - it's on 192.168.2.0
- "I need to send this to my default gateway: 192.168.1.1"

Step 3: Device A sends packet to Router (192.168.1.1)
- Layer 3: Destination IP = 192.168.2.30
- Layer 2: Destination MAC = Router's MAC address (found via ARP)

Step 4: Router receives packet on Interface 1
- Router reads destination IP: 192.168.2.30
- Router consults routing table: "192.168.2.0? Send it out Interface 2"
- Router forwards packet out Interface 2

Step 5: Router sends packet on Interface 2
- Layer 3: Destination IP = 192.168.2.30 (unchanged)
- Layer 2: Destination MAC = Device C's MAC address

Step 6: Device C receives packet
- Recognizes it's the destination IP
- Processes the data
```

---

#### 8. Network Configuration Best Practices

**When configuring a device, you need:**

1. **IP Address** - Unique identifier on the network
2. **Subnet Mask** - Defines the network boundary
3. **Default Gateway** - Router to reach other networks
4. **DNS (Optional)** - Server to resolve domain names to IP addresses

**Common Mistakes to Avoid:**
- Assigning two devices the same IP address on the same network
- Using wrong subnet mask for the network size
- Not setting a default gateway when devices need to reach other networks
- Configuring a router interface with a host address instead of a network address

**Example Correct Configuration:**

Device on 192.168.1.0/24 network:
```
IP Address:        192.168.1.100
Subnet Mask:       255.255.255.0
Default Gateway:   192.168.1.1
DNS Server:        8.8.8.8 (optional)
```

Router connecting two networks:
```
Interface 1 (Network 1):
  IP Address:      192.168.1.1
  Subnet Mask:     255.255.255.0

Interface 2 (Network 2):
  IP Address:      192.168.2.1
  Subnet Mask:     255.255.255.0
```

### Reference Materials

- [RFC 1918 - Private Address Space](https://tools.ietf.org/html/rfc1918)
- [IPv4 Addressing](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html)
- [Subnet Masks Explained](https://www.ibm.com/cloud/learn/subnet)
- [OSI Model Overview](https://www.cisco.com/c/en/us/support/docs/security/secure-access-manager/112264-secnet-faq.html)
- [Routing Basics](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/8414-routing-basics.html)

### AI Usage

AI tools were used in this project as follows:
- **Documentation** - AI assisted in writing clear explanations of networking concepts in the README

