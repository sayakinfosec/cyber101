## Networking Core Protocols  

### 💎 DNS – Remembering Addresses  
DNS (Domain Name System) maps **domain names** to **IP addresses**.  
It operates at the **Application Layer (Layer 7)** and uses **UDP 53** (default) and **TCP 53** (fallback).  

**Important DNS Records:**  
- **A Record** → Maps hostname to IPv4 (e.g., `example.com → 172.17.2.172`).  
- **AAAA Record** → Maps hostname to IPv6 (quad-A).  
- **CNAME Record** → Maps one domain to another (e.g., `www.example.com → example.com`).  
- **MX Record** → Specifies mail server for a domain.  

🔎 Example:  
Typing `example.com` in browser → browser queries DNS for **A record**.  
Sending email to `test@example.com` → mail server queries DNS for **MX record**.  

#### 🔹 Command Example  
```bash
user@TryHackMe$ nslookup www.example.com
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   www.example.com
Address: 93.184.215.14
Name:   www.example.com
Address: 2606:2800:21f:cb07:6820:80da:af6b:8b2c
````

#### 🔹 Packet Capture (tshark)

```bash
user@TryHackMe$ tshark -r dns-query.pcapng -Nn
1 0.000000000 192.168.66.89 → 192.168.66.1 DNS 86 Standard query A www.example.com
2 0.059049584 192.168.66.1 → 192.168.66.89 DNS 102 Response A 93.184.215.14
3 0.059721705 192.168.66.89 → 192.168.66.1 DNS 86 Standard query AAAA www.example.com
4 0.101568276 192.168.66.1 → 192.168.66.89 DNS 114 Response AAAA 2606:2800:21f:...
```
The query above led to four packets. In the terminal, we can see that the first and third packets send DNS queries for the A and AAAA records, respectively. The second and fourth packets show the DNS query responses.

**Q\&A:**

* Which record type refers to IPv6? → **AAAA**
* Which record type refers to email server? → **MX**

---

### 💎 WHOIS – Domain Ownership

WHOIS provides **ownership and registration details** of domains.
When you register a domain, you control its DNS records. WHOIS info includes registrar, creation date, expiry date, and contact info.

🔎 Example WHOIS lookup:

```bash
user@TryHackMe$ whois x.com
Creation Date: 1993-04-02
...other infos
```

```bash
user@TryHackMe$ whois twitter.com
Creation Date: 2000-01-21
...other infos
```

---

### 💎 HTTP(S) – Accessing the Web

HTTP = Hypertext Transfer Protocol, HTTPS = Secure HTTP.
Uses **TCP 80 (HTTP)** and **TCP 443 (HTTPS)**.

**Common Methods:**

* **GET** → Retrieve data (HTML, image, etc.).
* **POST** → Submit data (forms, uploads).
* **PUT** → Create/update a resource.
* **DELETE** → Remove a resource.

🔎 Example using **telnet**:

```bash
~# telnet 10.201.83.34 80
Trying 10.201.83.34...
Connected to 10.201.83.34.
Escape character is '^]'.
GET /flag.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hidden Message</title>
    <style>
        body {
            background-color: white;
            color: white;
            font-family: Arial, sans-serif;
        }
        .hidden-text {
            font-size: 1px;
        }
    </style>
</head>
<body>
    <div class="hidden-text">THM{TELNET-HTTP}</div>
</body>
</html>

Connection closed by foreign host.
```

---

### 💎 FTP – Transferring Files

FTP (File Transfer Protocol) is used for uploading/downloading files.

* Uses **TCP port 21** (control), separate connections for data.

**Common Commands:**

* `USER` → Enter username
* `PASS` → Enter password
* `RETR` → Download file
* `STOR` → Upload file

🔎 Example session:

```bash
user@TryHackMe$ ftp 10.201.83.34
Name: anonymous
Password:
ftp> ls
-rw-r--r--    1 0   0   1480 Jun 27 08:03 coffee.txt
-rw-r--r--    1 0   0     14 Jun 27 08:04 flag.txt
ftp> type ascii
ftp> get flag.txt
ftp> quit
```
flag.txt file will be downloaded to our system just by `ls` we can see it inside the current directory.
To view the contents of the file we can use cat command
cat flag.txt or may be nano also.

---

### 💎 SMTP – Sending Email

SMTP (Simple Mail Transfer Protocol) is for **sending emails**.
Uses **TCP port 25**.

**Common Commands:**

* `HELO / EHLO` → Initiate session
* `MAIL FROM` → Sender address
* `RCPT TO` → Recipient address
* `DATA` → Start email content
* `.` → End of email message

🔎 Example via **telnet**:

```bash
user@TryHackMe$ telnet 10.201.122.160 25
HELO client.thm
MAIL FROM: <user@client.thm>
RCPT TO: <strategos@server.thm>
DATA
From: user@client.thm
To: strategos@server.thm
Subject: Telnet email

Hello. I am using telnet to send you an email!
.
QUIT
```

**Q\&A:**

* Which command starts email content? → **DATA**
* What ends the message? → **.**

---

### 💎 POP3 – Receiving Email

POP3 (Post Office Protocol v3) retrieves emails from a server.

* Uses **TCP port 110**.

**Common Commands:**

* `USER` / `PASS` → Login
* `STAT` → Get message count/size
* `LIST` → List messages
* `RETR <n>` → Retrieve message
* `DELE <n>` → Delete message
* `QUIT` → Logout

🔎 Example session:

```bash
user@TryHackMe$ telnet 10.201.122.160 110
Trying 10.201.122.160...
Connected to 10.201.122.160.
Escape character is '^]'.
+OK [XCLIENT] Dovecot (Ubuntu) ready.
AUTH
+OK
PLAIN
.
USER strategos
+OK
PASS 
+OK Logged in.
STAT
+OK 3 1264
LIST
+OK 3 messages:
1 407
2 412
3 445
.
RETR 3
+OK 445 octets
Return-path: <user@client.thm>
Envelope-to: strategos@server.thm
Delivery-date: Thu, 27 Jun 2024 16:19:35 +0000
Received: from [10.11.81.126] (helo=client.thm)
        by example.thm with smtp (Exim 4.95)
        (envelope-from <user@client.thm>)
        id 1sMrpq-0001Ah-UT
        for strategos@server.thm;
        Thu, 27 Jun 2024 16:19:35 +0000
From: user@client.thm
To: strategos@server.thm
Subject: Telnet email

Hello. I am using telnet to send you an email!
.
QUIT
+OK Logging out.
Connection closed by foreign host.
```

**Q\&A:**

* POP3 server name? → **Dovecot**
* Flag from 4th email? → `THM{TELNET_RETR_EMAIL}`

---

### 💎 IMAP – Synchronizing Email

IMAP (Internet Message Access Protocol) synchronizes emails across devices (desktop, laptop, phone).
Unlike POP3, IMAP keeps messages on server.

* Uses **TCP port 143**.

**Common Commands:**

* `LOGIN` → Authenticate
* `SELECT inbox` → Open inbox
* `FETCH <n>` → Retrieve message
* `MOVE <n>` → Move message
* `COPY <n>` → Copy message
* `LOGOUT` → End session

🔎 Example session:

```bash
user@TryHackMe$ telnet 10.10.41.192 143
A LOGIN strategos
B SELECT inbox
C FETCH 3 body[]
D LOGOUT
```

**Q\&A:**

* Command to retrieve 4th message? → **FETCH 4 body\[]**

---

### 💎 Protocol Ports Summary

| Protocol | Transport | Default Port |
| -------- | --------- | ------------ |
| TELNET   | TCP       | 23           |
| DNS      | UDP/TCP   | 53           |
| HTTP     | TCP       | 80           |
| HTTPS    | TCP       | 443          |
| FTP      | TCP       | 21           |
| SMTP     | TCP       | 25           |
| POP3     | TCP       | 110          |
| IMAP     | TCP       | 143          |

