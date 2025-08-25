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

### 🔹 Some Q\&As

* **What cmdlet can you use instead of `type`?** → `Get-Content`
* **How to list all commands starting with `Remove`?** → `Get-Command -Name Remove*`
* **Which cmdlet is alias of `echo`?** → `Write-Output`
* **Command to get examples for `New-LocalUser`?** → `Get-Help New-LocalUser -examples`
* **Command to display contents of `C:\Users`?** → `Get-ChildItem -Path C:\Users`
