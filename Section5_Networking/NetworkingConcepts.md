## Networking Concepts

### 💎 OSI Model
The **OSI (Open Systems Interconnection)** model is a conceptual framework by ISO that describes how communication occurs in a network.  
It has **7 layers** (bottom → top):

1. Physical  
2. Data Link  
3. Network  
4. Transport  
5. Session  
6. Presentation  
7. Application  

Mnemonic: *“Please Do Not Throw **Spinach Pizza** Away”* 😂 

#### Layer Details
- **Layer 1: Physical** → Mediums (cables, WiFi bands, optical fiber). Transmission of bits (0s & 1s).  
- **Layer 2: Data Link** → Protocols on the same segment. Ethernet (802.3), WiFi (802.11), MAC addresses.  
- **Layer 3: Network** → Logical addressing & routing. IP, ICMP, VPNs.  
- **Layer 4: Transport** → End-to-end comms, segmentation, error correction. TCP, UDP.  
- **Layer 5: Session** → Establish, maintain, synchronize sessions. NFS, RPC.  
- **Layer 6: Presentation** → Data encoding, compression, encryption. ASCII, Unicode, MIME, JPEG/PNG.  
- **Layer 7: Application** → Services to applications. HTTP, FTP, DNS, SMTP, IMAP.  

#### Summary Table

| Layer No. | Layer Name        | Function                                   | Examples                        |
|-----------|------------------|-------------------------------------------|---------------------------------|
| 7         | Application       | User services, app interaction            | HTTP, FTP, DNS, SMTP, IMAP      |
| 6         | Presentation      | Encoding, encryption, compression         | ASCII, Unicode, JPEG, MIME      |
| 5         | Session           | Session control, sync                     | NFS, RPC                        |
| 4         | Transport         | End-to-end communication, segmentation    | TCP, UDP                        |
| 3         | Network           | Routing, logical addressing               | IP, ICMP, IPSec                 |
| 2         | Data Link         | Local node-to-node transfer, MAC          | Ethernet, WiFi                  |
| 1         | Physical          | Physical transmission (bits)              | Cables, WiFi bands, Optical     |

---

### 💎 TCP/IP Model
A **practical implementation model** (DoD, 1970s). More resilient, supports routing even during failures.  

- **Application Layer** → OSI 5,6,7  
- **Transport Layer** → OSI 4  
- **Internet Layer** → OSI 3  
- **Link Layer** → OSI 2  

Some textbooks show 5 layers (add Physical).  

#### Mapping Table

| OSI Model             | TCP/IP Model       | Protocols                       |
|------------------------|--------------------|---------------------------------|
| Application, Pres., Ses| Application        | HTTP, HTTPS, FTP, SSH, SMTP     |
| Transport              | Transport          | TCP, UDP                        |
| Network                | Internet           | IP, ICMP, IPSec                 |
| Data Link              | Link               | Ethernet, WiFi                  |
| Physical               | (sometimes shown)  | Cables, Radio signals           |

---

### 💎 IP Addresses & Subnets 

* **IPv4**: 32 bits = 4 octets (0–255 each). Example: `192.168.1.1`
  
#### Subnetting (Definition + Quick Math)

* Definition: Subnetting is the process of dividing a larger IP network into smaller, more manageable subnetworks (subnets).
  
* CIDR Notation: /N → means N bits for the network, remaining bits for hosts.
  
* Formula: Usable hosts = 2^(host bits) - 2
  (subtract 2 for network + broadcast addresses).
  
  Examples:
```
  /24 → 32 - 24 = 8 host bits → 2^8 - 2 = 254 usable IPs.
  /25 → 32 - 25 = 7 host bits → 2^7 - 2 = 126 usable IPs.
  /30 → 32 - 30 = 2 host bits → 2^2 - 2 = 2 usable IPs (used in point-to-point links).
```

#### 🎭 What is a Subnet Mask?

A **subnet mask** is like a filter (mask) that separates:

* The **network portion** of the IP address (fixed)  
* The **host portion** of the IP address (devices)  

It literally **"masks"** (hides) which part of the IP is **network** and which part is **hosts**.

**Example:**

* IP Address: `192.168.1.10`  
* Subnet Mask: `255.255.255.0` → means `/24`  

Now in binary:
```
IP Address: 11000000.10101000.00000001.00001010
Subnet Mask: 11111111.11111111.11111111.00000000
```

So:

* **Network** = `192.168.1.0`  
* **Broadcast** = `192.168.1.255`  
* **Usable Range** = `192.168.1.1 – 192.168.1.254`  

✨ **Easy way to think of it:**  

* **Subnet Mask** = boundary line between *network ID* and *device IDs*.  
* `/24` → mask has **24 ones** (`255.255.255.0`)  
* `/25` → mask has **25 ones** (`255.255.255.128`)  


#### Reserved Special Addresses (in every subnet)

* **Network Address** → *First address* in the subnet.

  * Identifies the **network itself**, not a device.
  * Example: `192.168.1.0/24` → `192.168.1.0` is the **network address**.
* **Broadcast Address** → *Last address* in the subnet.

  * Used to send a message to **all devices** in that network.
  * Example: `192.168.1.0/24` → `192.168.1.255` is the **broadcast address**.
  * Devices cannot use these two addresses. Only the range in between is usable.

#### Private Address Ranges (RFC 1918) 👈🏻 Must memorize

* `10.0.0.0 – 10.255.255.255 (/8)`
* `172.16.0.0 – 172.31.255.255 (/12)`
* `192.168.0.0 – 192.168.255.255 (/16)`

#### Checking IP

* **Windows**: `ipconfig`
* **Linux**: `ifconfig` or `ip a`

Example Linux output:

```
inet 192.168.66.89/24 brd 192.168.66.255
```

* **inet** → shows your IPv4 address line.
* `192.168.66.89/24` → device IP (`/24` = subnet mask 255.255.255.0).
* `brd 192.168.66.255` → broadcast address for that subnet.
* Range: `192.168.66.1 – 192.168.66.254` are valid host IPs.


🤞🏻 In short:

* **Network address** = label for the subnet.
* **Subnetting** helps efficiently use IPs, control traffic, and isolate networks.
* **Broadcast address** = shout-out to everyone in that subnet.
* **inet** = just how Linux labels the line showing your IPv4 address.
  
---

### 💎 Routing
- Router = **post office analogy** → forwards packets closer to destination.  
- Operates at **Layer 3** (Network).  
- Inspects IP headers & selects best path.  

---

### 💎 UDP vs TCP

#### UDP (User Datagram Protocol)
- **Connectionless**, no delivery guarantee.  
- Faster, less overhead.  
- Port range: `1–65535` (2^16 - 1).  
- Example: like sending **regular mail** without confirmation.  

#### TCP (Transmission Control Protocol)
- **Connection-oriented**, reliable.  
- Ensures delivery with sequence & ACK numbers.  
- Uses **3-way handshake**:  
  1. SYN ( Client → Server )
  2. SYN-ACK ( Server → Client )
  3. ACK ( Client → Server )

---

### 💎 Encapsulation
- Each layer adds its own **header/trailer** to data.  
- Order:  
  - App Data → Transport (Segment/Datagram) → Network (Packet) → Link (Frame).  

👉 At receiving side: process reversed.  

#### Q&A
- On WiFi, within what is an IP packet encapsulated? → **Frame**  
- UDP data unit encapsulating app data? → **Datagram**  
- TCP data unit encapsulating app data? → **Segment**  

---

### 💎 Life of a Packet
1. Browser sends HTTP request (App Layer).  
2. TCP (Transport) segments request (3-way handshake).  
3. IP (Network) adds source & destination IPs.  
4. Link Layer frames it & sends to router.  
5. Routers inspect & forward until destination.  
6. At destination, headers stripped layer by layer → original app data delivered.  

---

### 💎 Telnet
- Protocol for **remote terminal connection**.  
- Communicates with any server on a TCP port.  
- Rarely used today (security risks).  

#### Examples
- **Echo server** (port 7) → repeats input.  
- **Daytime server** (port 13) → returns current time.  
- **HTTP server** (port 80):  

**command**:  telnet target_ip port
```
telnet 10.201.22.39 80
GET / HTTP/1.1
Host: telnet.thm
```

→ Returns webpage response.

#### Telnet vs SSH 💪🏻
| Feature            | **Telnet**                                                                  | **SSH (Secure Shell)**                                         |
| ------------------ | --------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **Purpose**        | Remote login to another device (e.g., router, server).                      | Same (remote login + secure management).                       |
| **Encryption**     | ❌ None (sends everything in **plain text**, including username + password). | ✅ Encrypted (all communication secured).                       |
| **Port**           | TCP **23**                                                                  | TCP **22**                                                     |
| **Security**       | Very insecure — attackers can sniff credentials easily.                     | Secure — data protected with strong cryptography.              |
| **Usage today**    | Almost obsolete, rarely used (only for legacy systems).                     | Standard for remote admin & widely used in IT & cybersecurity. |
| **Extra features** | Only login + command execution.                                             | Tunneling, secure file transfer (SCP/SFTP), port forwarding.   |


