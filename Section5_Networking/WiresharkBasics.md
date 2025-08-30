## Wireshark - The Basics  

### **Definition:**  
Wireshark is a free, open-source network packet analyser. It captures, inspects, and dissects network traffic but does **not** block/modify packets (not an IDS).  

```Wireshark = X-ray goggles for the network🔍 → captures packets, breaks them down by OSI layers, and helps analysts investigate issues or suspicious activity.```

---

### **Main Uses:**  
- Troubleshooting network issues (congestion, failures)  
- Detecting anomalies (rogue hosts, weird ports, suspicious traffic)  
- Learning protocols (headers, response codes, payloads)  

---

### **GUI Key Sections:**  
- **Toolbar** → shortcuts (start/stop capture, filters, export)  
- **Filter Bar** → display/capture filters  
- **Interfaces** → choose network adapter (eth0, lo, wlan0)  
- **Packet Panes** →  
  - *List* (summary of packets)  
  - *Details* (layered protocol view)  
  - *Bytes* (hex/raw view)  

---

### **Packet Structure (OSI mapped):**  
1. Frame → Physical  
2. MAC → Data Link  
3. IP → Network  
4. TCP/UDP → Transport  
5. App Protocol (HTTP/FTP/SMB) → Application  
6. App Data → Payload  

---

### **Core Features:**  
- 📌 **Coloring** → highlights protocols/events  
- 🔍 **Filtering** →  
  - Capture filter = record only matching traffic  
  - Display filter = view specific packets  
- 🏷️ **Mark/Comment Packets** → for investigations  
- 📂 **Export Packets/Objects** → extract files or share subset  
- 🧵 **Follow Stream** → reconstruct conversations (HTTP/TCP/UDP)  
- ⚠️ **Expert Info** → auto-flags anomalies (warnings, errors)  

---

### Wireshark Filters Cheatsheet  

**🔎 Filter Types:**  
- **Capture Filter** → decides what packets to **record** (set before starting capture).  
- **Display Filter** → decides what packets to **show** (applied after capture).  


**🎯 Common Display Filters:**  

| Filter | Meaning |
|--------|---------|
| `ip.addr == 192.168.1.1` | Traffic to/from a specific IP |
| `ip.src == 192.168.1.1` | Packets from that IP |
| `ip.dst == 192.168.1.1` | Packets going to that IP |
| `tcp.port == 80` | Traffic on port 80 (HTTP) |
| `udp.port == 53` | DNS queries/responses |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | Only TCP SYN packets (new connections) |
| `http` | Show only HTTP traffic |
| `http.request` | Only HTTP requests |
| `http contains "password"` | HTTP packets with the word “password” |
| `dns` | Only DNS packets |
| `arp` | Only ARP packets |
| `ftp` | Only FTP packets |


**📌 Capture Filter Examples:**  

| Filter | Meaning |
|--------|---------|
| `host 192.168.1.1` | Capture traffic to/from 192.168.1.1 |
| `net 192.168.1.0/24` | Capture entire subnet traffic |
| `port 22` | Only SSH traffic |
| `tcp port 443` | Only HTTPS traffic |
| `udp` | Only UDP traffic |
| `icmp` | Only ping (ICMP) traffic |


✨ **Tip:**  
- Use **capture filters** to reduce clutter while recording.  
- Use **display filters** to zoom in after capture.  
- Combine filters with `and`, `or`, `not`. Example:  
  - `ip.src == 10.0.0.5 and tcp.port == 80` → HTTP traffic from 10.0.0.5
