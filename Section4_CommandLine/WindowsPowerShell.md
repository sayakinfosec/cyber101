## Windows PowerShell

### 🔹 What is PowerShell?

* Cross-platform task automation solution made up of:

  * Command-line shell
  * Scripting language
  * Configuration management framework
* Built on the **.NET framework**
* Unlike `cmd.exe` (text-based), PowerShell is **object-oriented**
* Runs on **Windows, Linux, and macOS**

---

### 🔹 A Brief History

* Early 2000s: `cmd.exe` and batch files were too limited for enterprise systems
* **Jeffrey Snover** (Microsoft engineer) designed PowerShell as object-oriented + .NET integrated
* Released in **2006** (Windows-only)
* **2016**: PowerShell Core (open-source, cross-platform)

---

### 🔹 Objects in PowerShell

* Everything in PowerShell is an **object** (with **properties** and **methods**)
* Example:

  * **Car object** → Properties: Color, Model | Methods: Drive(), Refuel()
  * **File object** → Properties: Name, Size | Methods: Copy(), Delete()
* Traditional shells return **plain text**, PowerShell returns **objects** → no extra parsing needed

---

### 🔹 Launching PowerShell

Ways to open PowerShell in **Windows**:

* Start Menu → search `powershell`
* Run → `Win + R → powershell`
* File Explorer → type `powershell` in address bar
* Task Manager → File → Run new task → powershell
* From CMD → type `powershell`

Ways to open via **Linux SSH** into a Windows target:

* `ssh username@target_ip` → run `powershell` → prompt changes to `PS>`

---

### 🔹 Cmdlets Basics

* Commands are called **cmdlets** (pronounced *command-lets*)
* Syntax: **Verb-Noun**

  * `Get-Content` → read file contents
  * `Set-Location` → change directory

#### Essential Cmdlets

* `Get-Command` → list all commands available
* `Get-Help <cmdlet>` → get documentation & examples
* `Get-Alias` → list aliases (e.g. `dir` = `Get-ChildItem`)
* `Find-Module` → search for online PowerShell modules
* `Install-Module` → install new cmdlets

---

### 🔹 File System Navigation

| Task        | PowerShell Cmdlet | Equivalent                 |
| ----------- | ----------------- | -------------------------- |
| List files  | `Get-ChildItem`   | `dir` / `ls`               |
| Change dir  | `Set-Location`    | `cd`                       |
| Create item | `New-Item`        | `mkdir`, `echo > file.txt` |
| Copy item   | `Copy-Item`       | `copy` / `cp`              |
| Move item   | `Move-Item`       | `move` / `mv`              |
| Read file   | `Get-Content`     | `type` / `cat`             |

---

### 🔹 Piping, Filtering, Sorting

* `|` passes **objects** from one cmdlet to another.

**Example:**

```powershell
Get-ChildItem | Sort-Object Length
```

→ Lists files, sorted by size.

**Filtering – `Where-Object`:**

```powershell
Get-ChildItem | Where-Object Length -gt 100
```

→ Files larger than 100 bytes.

**Operators:**

* `-eq` → equal
* `-ne` → not equal
* `-gt` → greater than
* `-lt` → less than
* `-like` → pattern match (wildcards)

**Select-Object – pick properties or limit results:**

```powershell
Get-ChildItem | Select-Object Name,Length
```

**Select-String – search inside files (like grep):**

```powershell
Select-String -Path .\captain-hat.txt -Pattern hat
```

**Pipeline Example – largest file in directory:**

```powershell
Get-ChildItem | Sort-Object Length -Descending | Select-Object -First 1
```

---

### 🔹 System & Network Information

**Computer Info:**

* `Get-ComputerInfo` → system & OS details.
* `Get-LocalUser` → list local users.

**Network Info:**

* `Get-NetIPConfiguration` → like `ipconfig`.
* `Get-NetIPAddress` → all IP addresses.

---

### 🔹 Real-Time System Analysis

* `Get-Process` → running processes.
* `Get-Service` → running/stopped services.
* `Get-NetTCPConnection` → active TCP connections.
* `Get-FileHash` → generate file hashes (SHA256 default). `PS> Get-FileHash -Path ".\myfile.txt" -Algorithm MD5` can be SHA1 etc.

---

### 🔹 Scripting & Automation

* Scripting is the process of writing and executing a series of commands contained in a text file, known as a script, to automate tasks that one would generally perform manually in a shell, like PowerShell.
* Scripts = PowerShell commands in `.ps1` files.
* Automates **admin tasks, security checks, malware analysis, penetration testing**.

**Example automation:**

```powershell
Invoke-Command -ComputerName RoyalFortune -ScriptBlock { Get-Service }
```

→ Runs `Get-Service` on a remote machine named **RoyalFortune**.

**Invoke-Command can:**

* Run local scripts on remote machines.
* Run one-liners with `-ScriptBlock`.
* Combine with credentials (`-Credential`).


---

### 🔹 Some Q\&As

* **What cmdlet can you use instead of `type`?** → `Get-Content`
* **How to list all commands starting with `Remove`?** → `Get-Command -Name Remove*`
* **Which cmdlet is alias of `echo`?** → `Write-Output`
* **Command to get examples for `New-LocalUser`?** → `Get-Help New-LocalUser -examples`
* **Command to display contents of `C:\Users`?** → `Get-ChildItem -Path C:\Users`
