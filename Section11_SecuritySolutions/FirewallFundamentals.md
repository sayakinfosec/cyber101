## Firewall Fundamentals

### What Is the Purpose of a Firewall?

Just as security guards protect buildings by checking who enters or leaves, **firewalls** act as digital guards for our devices and networks.  
They inspect **incoming and outgoing traffic** to ensure only authorized connections pass through.  

A firewall’s main job is to enforce **rules** — allowing or blocking traffic based on specific conditions.  
Anything entering or leaving your network must pass through it first.  

Modern firewalls also provide **advanced features** like deep packet inspection, threat intelligence, and intrusion prevention.

**Analogy:**  
Firewall = Security Guard checking who enters/exits a building.

---

### Learning Objectives

After completing this module, you’ll understand:

- The types of firewalls  
- Firewall rules and their components  
- Hands-on: Windows built-in firewall  
- Hands-on: Linux built-in firewall  

**Room Prerequisite:** Basic Networking Concepts

**Checkpoint:**  
✅ Which security solution inspects the incoming and outgoing traffic of a device or network?  
**Answer:** Firewall

---

### Types of Firewalls

Firewall deployment became essential as organizations realized their ability to filter harmful traffic.  
Each firewall type works on different **OSI model layers** and serves specific roles.

#### **1. Stateless Firewall**
- Works on **Layer 3 & Layer 4**  
- Filters packets using **predefined rules** only  
- Does **not remember** previous connections  
- Very fast, but not context-aware  

#### **2. Stateful Firewall**
- Works on **Layer 3 & Layer 4**  
- Maintains a **state table** to remember active connections  
- Makes decisions based on **connection history**  
- Offers improved security by tracking session states  

#### **3. Proxy Firewall (Application-Level Gateway)**
- Operates at **Layer 7 (Application Layer)**  
- Acts as an **intermediary** between private network and Internet  
- **Inspects packet content**  
- Masks internal IPs and supports **content filtering**  
- Enables **application control**

#### **4. Next-Generation Firewall (NGFW)**
- Operates from **Layer 3 to Layer 7**  
- Offers **deep packet inspection** and **intrusion prevention system (IPS)**  
- Supports **heuristic analysis** and **SSL/TLS decryption**  
- Integrates with **threat intelligence feeds** for real-time protection  

---

### Comparison Table

| Firewall Type | Characteristics |
|----------------|----------------|
| **Stateless** | Basic filtering, no connection tracking, efficient for high-speed networks |
| **Stateful** | Tracks sessions, applies complex rules, monitors active connections |
| **Proxy** | Inspects packet data, provides content filtering & application control |
| **Next-Gen (NGFW)** | Advanced threat protection, IPS, heuristic analysis, SSL/TLS inspection |

---

### Checkpoint

- 🧠 Which firewall maintains the state of connections? → **Stateful Firewall**  
- 🧠 Which offers heuristic analysis? → **Next-Generation Firewall**  
- 🧠 Which inspects traffic at application level? → **Proxy Firewall**

---

### Rules in Firewalls

Firewalls use **rules** to control network traffic. These can be **built-in** or **customized** for specific needs.  

#### **Components of a Firewall Rule**
- **Source address:** Originating IP  
- **Destination address:** Target IP  
- **Port:** Specific communication port  
- **Protocol:** Communication protocol (TCP, UDP, etc.)  
- **Action:** What to do (Allow, Deny, Forward)  
- **Direction:** Inbound or Outbound

---

### Types of Actions

#### **Allow**
Permits defined traffic.

Example:
| Action | Source | Destination | Protocol | Port | Direction |
|--------|---------|--------------|-----------|-------|------------|
| Allow | 192.168.1.0/24 | Any | TCP | 80 | Outbound |

#### **Deny**
Blocks defined traffic.

Example:
| Action | Source | Destination | Protocol | Port | Direction |
|--------|---------|--------------|-----------|-------|------------|
| Deny | Any | 192.168.1.0/24 | TCP | 22 | Inbound |

#### **Forward**
Redirects traffic to another destination.

Example:
| Action | Source | Destination | Protocol | Port | Direction |
|--------|---------|--------------|-----------|-------|------------|
| Forward | Any | 192.168.1.8 | TCP | 80 | Inbound |

---

### Directionality of Rules

| Rule Type | Description |
|------------|-------------|
| **Inbound** | Applies to incoming traffic (e.g., allow port 80 for web server) |
| **Outbound** | Applies to outgoing traffic (e.g., block SMTP port 25 for all devices) |
| **Forward** | Redirects traffic within the network (e.g., forward HTTP to internal web server) |

---

### Checkpoint

✅ What action permits any traffic? → **Allow**  
✅ Direction of rule for traffic leaving the network? → **Outbound**

---

### Windows Defender Firewall

Windows includes a **built-in firewall** to manage inbound and outbound network traffic.

#### **Network Profiles**
- **Private Network:** Used at home; trusted connections  
- **Public Network:** Used in public areas; blocks most incoming connections  

**Options in Dashboard:**
1. Allow/Disallow apps per profile  
2. Turn Firewall On/Off or block all incoming connections  
3. Restore default settings  

#### **Creating Custom Rules**
Example: Block all outgoing HTTP (80) & HTTPS (443) traffic  
Steps:
1. Open “Advanced Settings”  
2. Choose **Outbound Rules → New Rule**  
3. Select **Custom → All programs → TCP → Specific ports (80,443)**  
4. Choose **Block the connection**  
5. Apply to all profiles and finish  

Result: All browser traffic is blocked.

---

### Exercise (Windows Firewall)

-  Rule blocking all incoming SSH traffic → **Core Op**  
-  Rule allowing SSH from single IP → **Infra team**  
-  Allowed IP under that rule → **192.168.13.7**

---

### Linux iptables Firewall

Linux offers built-in firewall tools based on the **Netfilter** framework.  

#### **Common Firewall Utilities**
- **iptables** – Traditional packet filter  
- **nftables** – Successor to iptables (modern, efficient)  
- **firewalld** – Zone-based, dynamic management  
- **ufw (Uncomplicated Firewall)** – Beginner-friendly CLI frontend  

---

### Basic ufw Commands

| Task | Command |
|------|----------|
| Check firewall status | `sudo ufw status` |
| Enable firewall | `sudo ufw enable` |
| Disable firewall | `sudo ufw disable` |
| Allow all outgoing traffic | `sudo ufw default allow outgoing` |
| Deny incoming SSH traffic | `sudo ufw deny 22/tcp` |
| List active rules | `sudo ufw status numbered` |
| Delete rule by number | `sudo ufw delete <rule_number>` |

---

### Checkpoint

✅ Successor of iptables → **nftables**  
✅ Command to deny all outgoing traffic → **ufw default deny outgoing**

---

### Summary

- Firewalls act as **digital security guards** for networks.  
- Types: Stateless, Stateful, Proxy, Next-Gen.  
- They enforce **rules** to control data flow.  
- Available on both **Windows (Defender Firewall)** and **Linux (iptables/ufw)**.  
- Essential for protecting against **unauthorized access and attacks**.

---
