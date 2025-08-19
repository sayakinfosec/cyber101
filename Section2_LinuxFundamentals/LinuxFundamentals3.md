## 2.3 Linux Fundamentals 3

### Terminal Text Editors

* **nano**

  * Easy to use, beginner-friendly
  * Supports: search, copy-paste, jump to line, line numbers
* **vim**

  * Steeper learning curve, but very powerful
  * Customizable shortcuts
  * Syntax highlighting (great for code)
  * Works on all terminals (where nano may not be installed)

---

### File Transfer Tools

#### `wget`

* Downloads files from the internet
* Example:

  ```bash
  wget http://example.com/file.txt
  ```

#### `scp` (Secure Copy)

* Transfers files securely between local ↔ remote machines via SSH
* Format: `scp SOURCE DESTINATION`

Examples:

* Local → Remote:

  ```bash
  scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt
  ```
* Remote → Local:

  ```bash
  scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt
  ```

---

### Serving Files via Web (Python3 HTTP Server)

* Start a simple server in current directory:

  ```bash
  python3 -m http.server
  ```
* Accessible at: `http://<IP>:8000/`
* Example to download a file:

  ```bash
  wget http://10.201.52.159:8000/myfile
  ```

⚠️ Keep the server terminal open, and run `wget` in another terminal.

---

### Processes 101

* **Process** = a running program, managed by the kernel
* Each has a **PID** (Process ID)

#### Commands

* `ps aux` → show all processes
* `top` → real-time process monitoring
* `kill <PID>` → terminate a process

#### Kill Signals

* **SIGTERM** → ends process, allows cleanup
* **SIGKILL** → force kill, no cleanup
* **SIGSTOP** → suspend process

#### Init and Systemd

* First process (PID 0/1) at boot = **init/systemd**
* Other programs run as child processes of `systemd`

---

### Managing Services (systemctl)

Format: `systemctl [option] [service]`
Options:

* `start`
* `stop`
* `enable` (auto-start on boot)
* `disable`

Example:

```bash
systemctl start apache2
systemctl enable apache2
```

---

### Background & Foreground Jobs

* Run in background with `&`:

  ```bash
  echo "Hello" &
  ```
* Bring process back to foreground:

  ```bash
  fg
  ```

---

### System Automation (Cron Jobs)

* **crontab** = scheduler for automated tasks
* Example: Backup Documents every 12 hours

  ```bash
  0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/
  ```

---

### Package Management

* `apt` → install, update, upgrade packages
* `add-apt-repository` → add new repo
* `dpkg` → package manager backend

---

### System Logs

* Stored in `/var/log`
* Automatically rotated to prevent huge log files
