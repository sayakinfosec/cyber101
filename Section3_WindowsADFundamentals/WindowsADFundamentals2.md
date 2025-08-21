## Windows Active Directory Fundamentals 2 

### 🖥️ System Configuration (MSConfig)

* Used for **advanced troubleshooting** (startup issues).
* Requires **admin rights**.
* Tabs: **General | Boot | Services | Startup | Tools**
* ⚠️ Not for startup mgmt → use **Task Manager** instead.
* **Commands**:

  * Control Panel → `control.exe`
  * Troubleshooting → `control.exe /name Microsoft.Troubleshooting`
  * UAC Settings → `UserAccountControlSettings.exe`

---

### 🛠️ Computer Management (`compmgmt.msc`)

* Sections: **System Tools | Storage | Services & Applications**
* Cybersecurity relevance:

  * View/manage **local users/groups**
  * Monitor **services** (stop suspicious ones)
  * Check **disk/storage activity**

---

### 📊 System Information (`msinfo32.exe`)

* Tool for **hardware, components, software environment** overview.
* Divided into:

  * **Hardware Resources**
  * **Components**
  * **Software Environment**
* Useful for diagnostics & audits.

---

### 📈 Resource Monitor (`resmon.exe`)

* Shows **CPU, Disk, Network, Memory** (per-process + aggregate).
* Can:

  * Track **file handles & modules**
  * Detect **deadlocks & file locks**
  * Start/stop/pause/resume services.

---

### 💻 Command Prompt (`cmd.exe`)

* **Basics**:

  * Hostname → `hostname`
  * Current user → `whoami`
  * Network config → `ipconfig` | detailed → `ipconfig /all`
  * Connections → `netstat` (`-a`, `-b`, `-e` etc.)
  * Network mgmt → `net` (`net user`, `net localgroup`, `net share`, etc.)
* Show help → `<command> /?` or `net help <subcommand>`
* MSConfig internet protocol example:

  * `C:\Windows\System32\cmd.exe /k %windir%\system32\ipconfig.exe`

---

### 🗂️ Registry Editor (`regedt32.exe`)

* **Central hierarchical DB** for:

  * User profiles
  * Installed apps + associations
  * System + hardware config
  * Port usage
* ⚠️ High risk — wrong edits can break the OS.
* Used via **Registry Editor**.
