## Windows Active Directory Fundamentals 1

### Desktop vs. Server

* **Windows 10/11** → for end-users (desktop use).
* **Windows Servers** → dominate in enterprises needing **Active Directory (AD)** and other services like enterprise file sharing, enterprise apps.
* **Linux Servers** → dominate for web servers, open-source flexibility.

**Remote Access**

* Linux: `ssh username@IP`
* Windows: use **RDP (Remote Desktop Protocol)**.

---

### Windows File System

* **NTFS (New Technology File System)** → default modern file system.
* Replaced FAT16/FAT32 & HPFS.
* **FAT** still used in USBs, SD cards.

**NTFS Features**

* Supports files > 4GB.
* Permissions on files/folders.
* Compression & **EFS (Encryption File System)**.
* Journaling → auto-repairs after crashes.

**Permissions**

* Full Control, Modify, Read & Execute, List, Read, Write.
* View: *Right-click > Properties > Security tab*.

**Alternate Data Streams (ADS)**

* Files can hold hidden streams.
* Used for metadata (e.g., marking Internet downloads).
* Sometimes abused by malware.

---

### System Folders

* **C:\Windows** → core OS files (can reside on other drives).
* **System32** → critical system files.

  * ⚠️ Deleting breaks Windows.
* **System variable**: `%windir%`.
* **Why 32?** → originated from 32-bit architecture, name retained for compatibility.

---

### User Accounts

* **Administrator** → full system changes.
* **Standard User** → only personal files, no system-level changes.

**Run → `lusrmgr.msc`**

* Manage **Users** and **Groups**.

---

### User Account Control (UAC)

* Introduced in Vista, continues in newer Windows.
* Prevents malware from auto-running with admin privileges.
* **How it works**:

  * Admin sessions run as *standard* by default.
  * Prompt appears when higher privileges are required.
* Shield icon = UAC prompt.
* ⚠️ Doesn’t apply to built-in Administrator account.

---

### Settings vs. Control Panel

* **Settings** (since Windows 8) → modern UI, simpler controls.
* **Control Panel** → legacy, complex system settings.
* Sometimes Settings redirects to Control Panel.

---

### Task Manager

* Shortcut: **Ctrl+Shift+Esc**.
* Shows:

  * Running processes & apps.
  * Performance (CPU, RAM, etc.).
  * Useful for troubleshooting slowdowns.
