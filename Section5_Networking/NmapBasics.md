
## Nmap – The Basics  

Imagine you are connected to a network and using services like email or web browsing. Two key questions arise:  

1. How do we discover **other live devices** on this or other networks?  
2. How do we find **network services** running on these live devices (e.g., SSH, HTTP)?  

Doing this manually with ping, `arp-scan`, or telnet is inefficient. Firewalls may block ICMP, ARP only works locally, and manually scanning 65k ports is impractical.  

👉 Enter **Nmap (Network Mapper)**: an open-source, powerful, and flexible scanner first released in 1997.  

---

### 💎 Host Discovery – Who is Online?  

#### Target Specification in Nmap  
- **IP range:** `192.168.0.1-10`  
- **Subnet:** `192.168.0.1/24` (equivalent to `192.168.0.0-255`)  
- **Hostname:** `example.thm`  

#### Local Network Discovery  
```bash
sudo nmap -sn 192.168.66.0/24
````

* Sends **ARP requests** → replies indicate "Host is up"
* Can reveal **MAC addresses** → useful to guess vendor/device types

📌 Example output:

```
Nmap scan report for XiaoQiang (192.168.66.1)
Host is up (0.0069s latency).
MAC Address: 44:DF:65:D8:FE:6C (Unknown)
...
Nmap done: 256 IP addresses (7 hosts up) scanned in 2.64 seconds
```

#### Remote Network Discovery

For remote networks (separated by routers), ARP won’t work.
Nmap sends **ICMP echo, TCP SYN, TCP ACK**, etc.

```bash
sudo nmap -sn 192.168.11.0/24
```

📌 Example findings:

* `192.168.11.1` → replied to ICMP echo
* `192.168.11.2` → did not respond to ICMP, TCP SYN/ACK, or UDP


#### List Scan

```bash
nmap -sL 192.168.0.1/24
```

➡️ Lists targets without scanning (useful for confirmation).


#### ❓ Practice Question

**Q:** What is the last IP address scanned with target `192.168.0.1/27`?
**A:** `192.168.0.31`

📌 Explanation:

* `/27 = 32 addresses (block size 32)`
* Range: `192.168.0.0 – 192.168.0.31`
* Broadcast = `.31`

```
For better Understanding lets analyse whats happening
/27 -> 32-27 = 5 bits  
Binary representation of the 4th octate (11100000) 
Block size we can see can be 2^5 i.e 32. 
So
1st block will be 0 - 31
2nd block will be 32 - 63
3rd block and so on...

if we check the last octate dec value its 1.  So we can say it falls under 1st block or 0 - 31 range.
So network address will be 192.168.0.0
and broadcast address will be 192.168.0.31 (last address N-Map will scan)
```

#### Example Subnet Calculation

`192.168.11.5/21`

* Block size = `2^11 = 2048` (range `192.168.8.0 – 192.168.15.255`)
* Network = `192.168.8.0`
* Broadcast = `192.168.15.255`

```
192.168.11.5/21

32-21 = 11 bits
11111111.11111111.11111000.00000000  Subnet Mask is 255.255.248.0

block size = 8 
0 - 7
8 - 15
16 - 23

11 falls in 2nd block range.

network address = 192.168.8.0
broadcast address = 192.168.15.255

```
---

### 💎 Port Scanning

#### TCP Connect Scan (-sT)

```bash
nmap -sT 192.168.124.148
```

* Completes **3-way handshake**
* If open → connection established then reset
* If closed → target replies with RST

#### SYN (Stealth) Scan (-sS)

```bash
nmap -sS 192.168.124.148
```

* Sends **SYN** only
* If SYN-ACK → port open, Nmap replies with RST (never completes handshake)
* Stealthier (fewer logs)


#### UDP Scan (-sU)

```bash
nmap -sU 192.168.124.148
```

* For services like DNS, NTP, DHCP
* Closed ports reply with **ICMP port unreachable**


#### Limiting Ports

* `-F` → Fast mode (100 common ports)
* `-p10-1024` → scan specific range
* `-p-25` → ports 1–25
* `-p-` → all 65535 ports


#### ❓ Practice Questions

**Q:** How many TCP ports are open on 10.201.104.113?
👉 `6`

```
7/tcp    open  echo
9/tcp    open  discard
13/tcp   open  daytime
17/tcp   open  qotd
22/tcp   open  ssh
8008/tcp open  http
```

**Q:** What is the flag from the web server at 10.201.104.113?
👉 `THM{SECRET_PAGE_38B9P6}`

```
http://10.201.104.113:8008
```

---

### 💎 Version Detection – Extract More Information

#### OS Detection (-O)

```bash
nmap -sS -O 192.168.124.211
```

* Guesses OS based on packet signatures
* Example: Linux 4.x | 5.x


#### Service & Version Detection (-sV)

```bash
nmap -sV 192.168.124.211
```

* Adds **VERSION** column
* Example:

```
22/tcp open ssh OpenSSH 8.9p1 Ubuntu
```


#### Aggressive Scan (-A)

* Combines `-O`, `-sV`, **traceroute**, and more

#### Force Scan (-Pn)

* Treat all hosts as up, even if they don’t reply


#### ❓ Practice Question

**Q:** What is the name & version of the web server on 10.201.104.113?
👉 `lighttpd 1.4.74`

```bash
nmap -sV -p8008 10.201.104.113
```

---

### 💎 Timing – How Fast is Fast?

Nmap timing templates:

* `-T0` → paranoid (very slow, stealthy)
* `-T1` → sneaky
* `-T2` → polite
* `-T3` → normal (default)
* `-T4` → aggressive
* `-T5` → insane

📌 Lab results for 100 ports:

| Timing | Duration     |
| ------ | ------------ |
| T0     | 9.8 hours    |
| T1     | 27.5 minutes |
| T2     | 40.5 seconds |
| T3     | 0.15 seconds |
| T4     | 0.13 seconds |


#### Other Timing Controls

* `--min-parallelism / --max-parallelism` → control concurrent probes
* `--min-rate / --max-rate` → packets per second
* `--host-timeout <time>` → max wait per host


#### ❓ Practice Question

**Q:** Non-numeric equivalent of `-T4`?
👉 `-T aggressive`

---

### 💎 Output – Controlling What You See

#### Verbosity & Debugging

* `-v` → verbose (progress info)
* `-vv / -vvvv` → increase verbosity
* `-d` → debugging output
* `-d9` → max debugging

#### Saving Scan Reports

* `-oN file` → normal text
* `-oX file` → XML
* `-oG file` → grepable
* `-oA base` → all formats

📌 Example:

```bash
nmap -sS 192.168.139.1 -oA gateway
```

➡️ Creates `gateway.nmap`, `gateway.xml`, `gateway.gnmap`


#### ❓ Practice Question

**Q:** Which option enables debugging?
👉 `-d`

---

### 💎 Conclusion

* Use **`-sn`** for host discovery
* Use **`-sT`, `-sS`, `-sU`** for port scans
* Add **`-sV`, `-O`, or `-A`** for deeper info
* Control speed with **`-T0–5`**
* Save results with **`-oA`**
* Run with **sudo** to unlock full capabilities (e.g., SYN scans)

---

### 💎 Summary Table

| Category              | Option                     | Description                                   |
| --------------------- | -------------------------- | --------------------------------------------- |
| **Host Discovery**    | `-sL`                      | List scan (list only, no probing)             |
|                       | `-sn`                      | Ping scan – host discovery only               |
| **Port Scanning**     | `-sT`                      | TCP Connect scan                              |
|                       | `-sS`                      | TCP SYN (stealth) scan                        |
|                       | `-sU`                      | UDP Scan                                      |
|                       | `-F`                       | Fast scan (100 common ports)                  |
|                       | `-p`                       | Custom ports (`-p-` for all)                  |
|                       | `-Pn`                      | Treat all hosts as online                     |
| **Service Detection** | `-O`                       | OS detection                                  |
|                       | `-sV`                      | Service/version detection                     |
|                       | `-A`                       | Aggressive (OS, version, scripts, traceroute) |
| **Timing**            | `-T0..5`                   | Timing templates (paranoid → insane)          |
|                       | `--min-rate`, `--max-rate` | Packets/sec control                           |
|                       | `--host-timeout`           | Stop after timeout                            |
| **Output**            | `-v`                       | Verbosity level                               |
|                       | `-d`                       | Debug level                                   |
|                       | `-oN`                      | Normal output                                 |
|                       | `-oX`                      | XML output                                    |
|                       | `-oG`                      | Grepable output                               |
|                       | `-oA`                      | All formats                                   |


#### ❓ Final Question

**Q:** What kind of scan does Nmap use if run as local user (no sudo)?
👉 **TCP Connect Scan (`-sT`)**
