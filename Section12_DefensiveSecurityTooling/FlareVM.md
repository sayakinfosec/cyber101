## FlareVM - Arsenal of Tools  

### Introduction
**FlareVM (Forensics, Logic Analysis, and Reverse Engineering)** is a powerful, curated collection of tools designed by the **FLARE Team at FireEye**.  
It caters to:
- Reverse Engineers  
- Malware Analysts  
- Incident Responders  
- Forensic Investigators  
- Penetration Testers  

FlareVM transforms a Windows system into a full-fledged malware analysis and reverse engineering environment - allowing analysts to safely **dissect malicious binaries**, **observe malware behavior**, and **perform forensic investigations**.

---

### 🎯 Learning Objectives
- Explore tools available within FlareVM  
- Learn how to analyze potentially malicious processes effectively  
- Get familiar with tools used for **static & dynamic analysis**

---

### 🧰 Arsenal of Tools (Categorized Overview)

### 🔍 Reverse Engineering & Debugging
Understand binaries and fix issues in compiled code.  
- **Ghidra** - NSA-developed open-source reverse engineering suite  
- **x64dbg** - Open-source debugger for x64/x32 binaries  
- **OllyDbg** - Assembly-level debugger  
- **Radare2** - Advanced open-source reversing platform  
- **Binary Ninja** - Disassembler & decompiler  
- **PEiD** - Detects packers, cryptors, and compilers  


### 🧩 Disassemblers & Decompilers
Convert machine code into readable form.  
- **CFF Explorer** - PE file editor & analyzer  
- **Hopper Disassembler** - Disassembler and decompiler  
- **RetDec** - Open-source decompiler for machine code  


### ⚙️ Static & Dynamic Analysis
Examine malware safely - with or without execution.  
- **Process Hacker** - Process monitor & memory editor  
- **PEview** - Portable Executable (PE) viewer  
- **Dependency Walker** - DLL dependency mapper  
- **DIE (Detect It Easy)** - Packer/compiler detector  


### 🧾 Forensics & Incident Response
Acquire, analyze, and preserve digital evidence.  
- **Volatility** - Memory forensics framework  
- **Rekall** - Memory analysis framework  
- **FTK Imager** - Disk imaging & forensic tool  


### 🌐 Network Analysis
Understand network communications & detect anomalies.  
- **Wireshark** - Network traffic analyzer  
- **Nmap** - Network mapping & vulnerability scanner  
- **Netcat** - Data transmission utility  


### 📁 File Analysis
Examine and manipulate file data.  
- **FileInsight** - Binary file viewer/editor  
- **Hex Fiend** - Lightweight hex editor  
- **HxD** - Binary hex editor  


### ⚡ Scripting & Automation
Automate analysis and testing tasks.  
- **Python** - Automation scripts and modules  
- **PowerShell Empire** - Post-exploitation framework  


### 🧠 Sysinternals Suite
Essential for Windows system diagnostics & troubleshooting.  
- **Autoruns** - Startup process viewer  
- **Process Explorer** - Advanced task manager  
- **Process Monitor (Procmon)** - Real-time file/registry/process tracker  

---

### 🕵️‍♂️ Commonly Used Tools for Investigation

| Tool | Investigative Value |
|------|----------------------|
| **Procmon** | Tracks file, registry, and thread/process activity |
| **Process Explorer** | Shows parent-child process relationships |
| **HxD** | Hex editing for malicious file inspection |
| **Wireshark** | Analyzes network traffic for anomalies |
| **CFF Explorer** | Generates hashes & validates file integrity |
| **PEStudio** | Performs static analysis of executables |
| **FLOSS** | Extracts & de-obfuscates strings from binaries |

---

### 🔬 Key Tools Deep Dive

### 🧭 Process Monitor (Procmon)
Monitors real-time file system, registry, and process/thread activity.  
- Excellent for malware research and forensic investigations.  
- Example: Filtering for **lsass.exe** to detect suspicious access (common in credential dumping).


### ⚙️ Process Explorer (Procexp)
Displays active processes, their parents, and loaded DLLs.  
Useful for:
- Validating process origins  
- Tracking spawned processes (e.g., Word → cmd → PowerShell chain)  


### 🧮 HxD
Fast hex editor to analyze binary structure.  
- Reveals file signatures (e.g., `4D 5A` = MZ header for executables).  
- The **Data Inspector** helps interpret byte values in multiple formats.  


### 🧩 CFF Explorer
PE file analyzer.  
- Generates hashes for integrity verification  
- Identifies compiler, linker, and import/export tables  
- Helps detect **tampered system files** or **packed binaries**  


### 🌐 Wireshark
Network traffic capture & analysis.  
- Detects C2 communications  
- Displays IPs, ports, and encrypted sessions (e.g., **TLSv1.2**)  


### 🧱 PEStudio
Performs **static malware analysis** without execution.  
- Detects **packed files**, **high entropy**, **blacklisted APIs**  
- Useful for identifying **language anomalies** (e.g., Russian strings in non-Russian orgs)  
- **Entropy 7.999** ⇒ Strongly packed/encrypted  


### 🧩 FLOSS
**FLARE Obfuscated String Solver** - extracts & deobfuscates strings from binaries.  
Example usage:
```powershell
floss .\cobaltstrike.exe
````

Extracts encoded strings, registry paths, URLs, or API calls hidden within malware.

---

### 🧪 Hands-On: Analyzing Malicious Files

### 🧱 File: `windows.exe`

**Observation (PEStudio):**

* MD5: `9FDD4767DE5AEC8E577C1916ECC3E1D6`
* SHA-1: `A1BC55A7931BFCD24651357829C460FD3DC4828F`
* **Entropy:** 7.999 → Packed/obfuscated
* **Suspicious:** Russian strings in metadata
* **APIs:**

  * `set_UseShellExecute` → Executes other processes
  * `RijndaelManaged` → Indicates AES encryption (possible ransomware behavior)


### 🔍 Analysis using FLOSS

```powershell
FLOSS.exe .\windows.exe > windows.txt
```

* Extracts same cryptographic & process-related strings found via PEStudio
* Confirms behavior overlap


### 🌐 CobaltStrike Analysis (Network Behavior)

**Goal:** Detect network connection to potential C2 server

1. Launch `cobaltstrike.exe`
2. Check parent process in **Process Explorer** → `explorer.exe`
3. Inspect TCP/IP tab → Connection to IP `47.120.46.210` on port `81`

Cross-verified using **Procmon** via filters (`Process Name contains cobalt`)

🧩 **Result:**
Binary connected to defanged IP → `47[.]120[.]46[.]210`

---

### 🧾 Key Q&A Summary

| Question                                            | Answer                               |
| --------------------------------------------------- | ------------------------------------ |
| Open-source debugger for x64/x32 binaries           | **x64dbg**                           |
| PE editor & analyzer                                | **CFF Explorer**                     |
| Memory editor & process watcher                     | **Process Hacker**                   |
| Forensic image tool                                 | **FTK Imager**                       |
| Binary file hex editor                              | **HxD**                              |
| Tool formerly FireEye Labs Obfuscated String Solver | **FLOSS**                            |
| Parent process of cobaltstrike.exe                  | **explorer.exe**                     |
| Imphash of cobaltstrike.exe                         | **92EEF189FB188C541CBD83AC8BA4ACF5** |
| Defanged C2 IP                                      | **47[.]120[.]46[.]210**              |
| Destination port                                    | **81**                               |


### 🧩 Key Findings

* **High Entropy (7.999)** → Packed or obfuscated binary
* **Suspicious Metadata** → Foreign-language strings (possible attribution clue)
* **Crypto APIs Detected** → Rijndael/AES use (encryption or ransomware behavior)
* **Network Activity** → Outbound connection to remote C2 server

---

### 🧠 Conclusion

FlareVM is a complete **malware analysis ecosystem** - a Swiss Army knife for DFIR professionals.
In this lab, we:

* Explored tool categories inside FlareVM
* Practiced static and dynamic malware analysis
* Used tools like **PEStudio**, **CFF Explorer**, **Procmon**, **Procexp**, and **FLOSS**
* Identified suspicious binaries, unpacking patterns, and C2 communications

💡 *In essence: FlareVM equips analysts to safely analyze, dissect, and understand malware behavior in a controlled environment.*
