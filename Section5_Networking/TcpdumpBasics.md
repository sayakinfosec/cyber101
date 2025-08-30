## Tcpdump – The Basics  

The main challenge when studying networking protocols is that we don’t normally see protocol “conversations.” Tools like **Tcpdump** help us capture and analyze raw network packets.  

Tcpdump was written in C/C++ in the late 80s/90s and runs on Unix-like systems. It uses the **libpcap** library (basis for many other tools, including Wireshark).  

---

### 💎 Basic Usage  

- `tcpdump -i INTERFACE` → listen on a specific interface  
- `tcpdump -i any` → capture on all interfaces  
- `tcpdump -w FILE.pcap` → save capture to file  
- `tcpdump -r FILE.pcap` → read from a saved capture file  
- `tcpdump -c COUNT` → capture only N packets  
- `tcpdump -n` → disable DNS resolution (faster, numeric only)  
- `tcpdump -nn` → disable DNS + port resolution (raw IP + port numbers)  
- `tcpdump -v / -vv / -vvv` → increase verbosity  

📌 Example:  
```bash
sudo tcpdump -i ens5 -c 5 -n
````

---

### 💎 Filtering Expressions

Like listening in a crowded room, filtering helps focus on specific traffic.

#### 🔹 By Host

* `tcpdump host example.com -w http.pcap`
* `tcpdump src host 192.168.1.10`
* `tcpdump dst host 192.168.1.10`

#### 🔹 By Port

* `tcpdump port 53` → DNS traffic
* `tcpdump src port 22` → source SSH packets
* `tcpdump dst port 80` → destination HTTP packets

#### 🔹 By Protocol

* `tcpdump icmp` → ping traffic
* `tcpdump tcp` → TCP only
* `tcpdump udp` → UDP only

#### 🔹 Logical Operators

* `and` → both conditions true
* `or` → either condition true
* `not` → exclude condition

📌 Example:

```bash
tcpdump host 1.1.1.1 and tcp
tcpdump udp or icmp
tcpdump not tcp
```

---

### 💎 Advanced Filtering

#### 🔹 Length Filters

* `greater N` → packets ≥ N bytes
* `less N` → packets ≤ N bytes

#### 🔹 TCP Flags (using `tcp[tcpflags]`)

* `tcpdump "tcp[tcpflags] == tcp-syn"` → SYN only
* `tcpdump "tcp[tcpflags] & tcp-ack != 0"` → at least ACK set
* `tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"` → SYN or ACK set

---

### 💎 Display Options

* `-q` → quick summary (brief info)
* `-e` → show link-layer (MAC addresses)
* `-A` → show payload as ASCII
* `-xx` → show in hex format
* `-X` → show in hex **and** ASCII

📌 Example:

```bash
tcpdump -r file.pcap -X
```

---

### 💎 Common Examples

* `tcpdump -i eth0 -c 50 -v` → capture 50 packets, verbose output
* `tcpdump -i wlo1 -w data.pcap` → capture WiFi traffic to file
* `tcpdump -i any -nn` → capture all, no name/port resolution
* `tcpdump -i any tcp port 22` → SSH packets on all interfaces
* `tcpdump -i wlo1 udp port 123` → capture NTP traffic
* `tcpdump -i eth0 host example.com and tcp port 443 -w https.pcap` → HTTPS traffic with example.com

---

### 💎 Practice Q\&A

**Q1:** What option displays numeric-only IPs (no DNS)?
👉 `-n`

**Q2:** How many ICMP packets in `traffic.pcap`?
👉 `26`

```bash
tcpdump -r traffic.pcap icmp -n | wc
```

**Q3:** Which host asked for MAC of `192.168.124.137`?
👉 `192.168.124.148`

```bash
tcpdump -r traffic.pcap arp and host 192.168.124.137
```

**Q4:** What hostname appears in first DNS query?
👉 `mirrors.rockylinux.org`

```bash
tcpdump -r traffic.pcap port 53 -A
```

**Q5:** How many packets had only the TCP RST flag set?
👉 `57`

```bash
tcpdump -r traffic.pcap 'tcp[tcpflags] == tcp-rst' | wc -l
```

**Q6:** Which host sent packets larger than 15000 bytes?
👉 `185.117.80.53`

```bash
tcpdump -r traffic.pcap 'greater 15000' -n
```

---

### 💎 Summary Tables

#### General Options

| Command         | Explanation               |
| --------------- | ------------------------- |
| `-i INTERFACE`  | Capture on interface      |
| `-w FILE`       | Write packets to file     |
| `-r FILE`       | Read packets from file    |
| `-c COUNT`      | Capture N packets         |
| `-n`            | Disable DNS lookup        |
| `-nn`           | Disable DNS + port lookup |
| `-v, -vv, -vvv` | Verbosity levels          |

#### Filters

| Command                | Explanation           |
| ---------------------- | --------------------- |
| `host IP`              | All traffic with host |
| `src host IP`          | Source host only      |
| `dst host IP`          | Destination host only |
| `port N`               | Specific port         |
| `src port N`           | Source port           |
| `dst port N`           | Destination port      |
| `icmp` / `tcp` / `udp` | Protocol filter       |

#### Display

| Command | Explanation        |
| ------- | ------------------ |
| `-q`    | Quick/brief output |
| `-e`    | Show MAC headers   |
| `-A`    | ASCII payload      |
| `-xx`   | Hex dump           |
| `-X`    | Hex + ASCII        |


✨ **In short:** Tcpdump is a **command-line Wireshark**. Use it to capture, filter, and analyze packets directly from the terminal. Pair it with `-w` to save and Wireshark to visualize.


