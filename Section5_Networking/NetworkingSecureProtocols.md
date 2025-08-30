## Networking Secure Protocols  

In the **Networking Core Protocols** section, we learned about protocols used to browse the web and access email. These work fine, but they **don’t protect confidentiality, integrity, or authenticity** of the data.  

- **Confidentiality not protected** → attacker can read your passwords or credit card details in plain HTTP/email.  
- **Integrity not protected** → attacker can change data in transit (e.g., alter “pay £100” → “pay £800”).  
- **Authenticity not protected** → attacker could impersonate a fake server.  

🔐 Important transactions are **unsafe** without ensuring **confidentiality, integrity, authenticity**.  

**Solution:** Add **TLS (Transport Layer Security)** to existing protocols →  
- HTTP → **HTTPS**  
- POP3 → **POP3S**  
- SMTP → **SMTPS**  
- IMAP → **IMAPS**  

Similarly, insecure **Telnet** was replaced by **SSH**. VPNs also provide a secure network over insecure infrastructure.  

---

### 💎 TLS (Transport Layer Security)  

Before TLS, attackers could capture all packets on a network (promiscuous mode) and read emails, chats, and passwords in **cleartext**.  

**History:**  
- 1995 → Netscape releases **SSL 2.0**.  
- 1999 → IETF upgrades to **TLS 1.0**.  
- 2018 → Major overhaul → **TLS 1.3** released.  

🔑 TLS is a **cryptographic protocol** (transport layer of OSI).  
- Provides **confidentiality** (nobody can read data).  
- Provides **integrity** (nobody can tamper with data).  

💡 Without TLS, online shopping, banking, and messaging wouldn’t be possible securely.  

**Protocols upgraded with TLS:**  
- HTTP → HTTPS  
- DNS → DoT (DNS over TLS)  
- MQTT → MQTTS  
- SIP → SIPS  

**Certificates:**  
- Servers get signed **TLS certificates** from a **Certificate Authority (CA)**.  
- Hosts trust servers if CA is in their trusted store.  
- **Self-signed certificates** should not be trusted (no 3rd-party verification).  

📌 **Q&A**  
- Protocol TLS upgraded: **SSL**  
- Certificates NOT to be trusted: **Self-signed**  

---

### 💎 HTTPS (HTTP Secure)  

- **HTTP** (port 80) = plaintext traffic.  
- **HTTPS** (port 443) = HTTP over TLS.  

**HTTP Steps:**  
1. TCP 3-way handshake  
2. Send HTTP requests (e.g., `GET / HTTP/1.1`)  

**HTTPS Steps:**  
1. TCP handshake  
2. **TLS session establishment**  
3. Encrypted HTTP requests  

🧪 Wireshark Example:  
- HTTP traffic = **readable plaintext**.  
- HTTPS traffic = **encrypted gibberish**.  
- With TLS private key → can decrypt → normal HTTP requests appear.  

📌 **Q&A**  
- TLS negotiation took: **8 packets**  
- Packet containing `GET /login`: **10**  

---

### 💎 SMTPS, POP3S, IMAPS  

Adding TLS works the same as HTTPS.  

**Insecure default ports:**  
- HTTP → 80  
- SMTP → 25  
- POP3 → 110  
- IMAP → 143  

**Secure default ports:**  
- HTTPS → 443  
- SMTPS → 465, 587  
- POP3S → 995  
- IMAPS → 993  

---

### 💎 SSH (Secure Shell)  

- **Telnet** (port 23) = plaintext logins → insecure.  
- **SSH** (port 22) = secure replacement.  

**History:**  
- 1995 → SSH-1 (Tatu Ylönen).  
- 1996 → SSH-2 released.  
- 1999 → OpenBSD devs release **OpenSSH** (open-source).  

**Benefits of SSH:**  
- Secure authentication (password, public key, 2FA).  
- End-to-end encryption (confidentiality).  
- Protects **integrity** of traffic.  
- **Tunneling**: encapsulate protocols inside SSH.  
- **X11 Forwarding**: run remote graphical apps.  

**Usage Example:**  
```bash
ssh username@hostname
ssh 192.168.124.148 -X   # for GUI forwarding
````

📌 **Q\&A**

* Open-source SSH implementation: **OpenSSH**

---

### 💎 SFTP vs FTPS

* **SFTP** = SSH File Transfer Protocol

  * Uses **port 22** (same as SSH).
  * Commands:

    ```bash
    sftp username@hostname
    sftp> get filename
    sftp> put filename
    ```

* **FTPS** = FTP Secure

  * Uses **TLS** (like HTTPS).
  * Usually on **port 990**.
  * Requires certificates & firewall config (control + data channels).

---

### 💎 VPN (Virtual Private Network)

A VPN = **secure tunnel** over the Internet.

* Connects remote offices/users as if in the same local network.
* Provides **confidentiality & integrity** over public Internet.
* Routes all traffic through VPN tunnel.

**Example Use Cases:**

* Companies connect branch offices.
* Remote employees connect to HQ.
* Users bypass censorship/georestrictions.
* Protect against ISP snooping.

**Important Notes:**

* VPN traffic = encrypted → ISP sees only gibberish.
* Public IP becomes VPN server’s IP.
* Some VPNs may leak actual IP (DNS leaks).
* ⚠️ VPNs may be **illegal** in some countries.

---

### 💎 MQTT and SIP Protocols

#### MQTT (Message Queuing Telemetry Transport)

- **Purpose** → Lightweight messaging protocol for IoT (Internet of Things).  
- **Model** → Publish/Subscribe model (not client/server).  
- **Ports**:  
  - 1883 → Plain text (TCP).  
  - 8883 → Secure (TLS/SSL).  
- **Use Case**:  
  - Smart devices (sensors, thermostats, smart home).  
  - Remote monitoring.  
  - Low-bandwidth / unreliable networks (because it’s efficient and small).  
- **Example**: Your smart bulb → publishes its state → MQTT broker → your phone subscribes to that topic.  


#### SIP (Session Initiation Protocol)

- **Purpose** → Used to initiate, manage, and terminate multimedia sessions (mainly voice & video calls).  
- **Model** → Works with RTP (Real-time Transport Protocol) for actual voice/video transfer.  
- **Ports**:  
  - 5060 → Plain text SIP (UDP/TCP).  
  - 5061 → Secure SIP over TLS.  
- **Use Case**:  
  - VoIP (Voice over IP).  
  - Video conferencing systems (Zoom, MS Teams, some Google Meet internals).  
  - Internet telephony (SIP phones, PBX systems).  
- **Example**: When you dial a number on a VoIP app → SIP sets up the session → RTP carries your voice.  

⚡ Quick Memory Trick:
```
MQTT → IoT (tiny devices chatting).
SIP → Calls (voice/video sessions).
```

---

### 💎 Protocol Ports Summary  

| Protocol | Transport | Default Port | Secure (TLS/SSH) Port |
|----------|-----------|--------------|-----------------------|
| TELNET   | TCP       | 23           | ❌ (replaced by SSH 22) |
| DNS      | UDP/TCP   | 53           | DoT: 853 |
| HTTP     | TCP       | 80           | HTTPS: 443 |
| FTP      | TCP       | 21           | FTPS: 990 (implicit), 21 (explicit) |
| SMTP     | TCP       | 25           | SMTPS: 465, 587 |
| POP3     | TCP       | 110          | POP3S: 995 |
| IMAP     | TCP       | 143          | IMAPS: 993 |
| SSH      | TCP       | ❌           | 22 (secure by design) |
| SFTP     | TCP       | ❌           | 22 (runs over SSH) |

