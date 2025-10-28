## IDS Fundamentals

### What Is an IDS?

Firewalls are deployed at the network boundary to inspect and control incoming and outgoing traffic. However, once a connection is established, a firewall’s job is done — it doesn’t analyze what happens afterward. If a threat actor successfully bypasses a firewall and begins malicious activity inside the network, another layer of defense is needed to **detect** that intrusion.  
This is where an **Intrusion Detection System (IDS)** comes in.

Think of it like building security:  
- The **firewall** is the gatekeeper that checks who enters or leaves.  
- The **IDS** is the network of **surveillance cameras** monitoring ongoing activities inside.  

An IDS continuously observes the network traffic and looks for suspicious or abnormal behavior based on **signatures** (known attack patterns) or **anomalies** (deviation from normal activity).  
When detected, it generates **alerts** for the security team — but does **not** take direct preventive action.

---

### Learning Objectives

After completing this section, you should understand:

- Types of IDS and their detection capabilities  
- Working of **Snort IDS**  
- Default and custom rules in Snort IDS  
- How to make a custom rule in Snort IDS  

**Prerequisites:**  
- Firewall Fundamentals  
- Networking Concepts  

**Question:**  
Can an IDS prevent the threat after it detects it?  
✅ **Answer:** Nay  

---

### Types of IDS

IDS can be classified based on **deployment** and **detection** methods.

#### Deployment Modes

- **Host Intrusion Detection System (HIDS)**  
  Installed directly on individual hosts and monitors activities specific to that host.  
  Provides detailed visibility but can be difficult to manage across large networks.

- **Network Intrusion Detection System (NIDS)**  
  Monitors traffic across the entire network instead of individual systems.  
  Provides centralized visibility of all communications in the environment.

#### Detection Modes

- **Signature-Based IDS**  
  Detects known attacks using pre-defined patterns (signatures).  
  Efficient for known threats but **cannot detect zero-day attacks** (new, unknown threats).

- **Anomaly-Based IDS**  
  Learns normal network behavior (baseline) and flags any deviation as suspicious.  
  Can detect **zero-day attacks**, but may produce **false positives**.

- **Hybrid IDS**  
  Combines both signature and anomaly-based detection for better coverage.  
  Uses signatures for known attacks and anomaly detection for new ones.

| IDS Type | Strength | Limitation |
|-----------|-----------|-------------|
| Signature-Based | Quick detection of known attacks | Cannot detect zero-days |
| Anomaly-Based | Detects new/unknown threats | High false positives |
| Hybrid | Balanced, broader coverage | More complex configuration |

**Questions:**  
- Which type of IDS detects threats across the network? → **Network Intrusion Detection System**  
- Which IDS uses both signature-based and anomaly-based detection? → **Hybrid IDS**

---

### IDS Example: Snort

**Snort** is a widely used open-source IDS developed in 1998.  
It supports **signature-based** and **anomaly-based** detection methods.

Snort relies on **rule files** — these contain the attack patterns or traffic behaviors it monitors for. You can enable, disable, or create **custom rules** to tailor Snort to your environment.

#### Modes of Snort

| Mode | Description | Use Case |
|------|--------------|----------|
| **Packet Sniffer Mode** | Reads and displays network packets without analysis | Network monitoring and troubleshooting |
| **Packet Logging Mode** | Logs all traffic (as PCAP files) for later analysis | Forensics and investigation |
| **Network Intrusion Detection System (NIDS) Mode** | Monitors network traffic in real-time and raises alerts when detecting malicious patterns | Primary IDS functionality |

**Questions:**  
- Which mode logs network traffic in a PCAP file? → **Packet Logging Mode**  
- What is the primary mode of Snort? → **Network Intrusion Detection System Mode**

---

### Snort Usage

When installing Snort, you’ll define:
- Network interface  
- Monitored network range  

If you want to capture **all** network traffic (not just for your host), you must enable **promiscuous mode** on your network interface.

#### Snort Directory Structure

All Snort configurations and rule files are stored under:
```

/etc/snort

```
This includes:
```

classification.config
reference.config
rules/
snort.conf

```

#### Rule Format

Each Snort rule follows a structured syntax:
```

alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:10001; rev:1;)

```

**Components:**
- **Action:** What to do when triggered (e.g., `alert`)  
- **Protocol:** Network protocol (e.g., `ICMP`)  
- **Source IP / Port:** Origin of the traffic (`any`)  
- **Destination IP / Port:** Target of the traffic (`$HOME_NET`, `any`)  
- **Rule Metadata:**  
  - `msg` – Message displayed when triggered  
  - `sid` – Signature ID (unique identifier)  
  - `rev` – Revision number of the rule  

---

### Creating and Testing a Custom Rule

To create a custom rule, edit the following file:
```

sudo nano /etc/snort/rules/local.rules

```

Add this rule:
```

alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)

```

Save and exit with `CTRL + X`, then `Y`.

#### Running Snort
Start Snort to detect activity:
```

sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf

```

Ping the loopback address:
```

ping 127.0.0.1

```

You should see alerts like:
```

[**] [1:1000001:1] Loopback Ping Detected [**] {ICMP} 127.0.0.1 -> 127.0.0.1

```

---

### Running Snort on PCAP Files

Snort can also analyze **historical network traffic** stored in `.pcap` files.

```

sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf

```

This command scans the recorded packets in the PCAP and displays detections on the console.

---

### Key Points Recap

| Concept | Description |
|----------|-------------|
| **IDS Purpose** | Detect malicious activities inside the network |
| **Snort Directory** | `/etc/snort` |
| **Custom Rule File** | `local.rules` |
| **Rule Revision Field** | `rev` |
| **Sample Protocol Used** | `ICMP` |
| **Alert Message Example** | “Loopback Ping Detected” |

---

### Questions & Answers

- **Where is the main directory of Snort that stores its files?**  
  ✅ `/etc/snort`

- **Which field indicates the revision number of the rule?**  
  ✅ `rev`

- **Which protocol is defined in the sample rule created in the task?**  
  ✅ `icmp`

- **What is the file name that contains custom rules for Snort?**  
  ✅ `local.rules`

---

