
### Introduction

CAPA (by FireEye/FLARE) is an open-source forensic tool used to analyze executable files (mainly Windows PE files) and identify the capabilities of the program - in other words, *what the binary can do*. Instead of running the malware, CAPA inspects the file statically and reports potential behaviors based on predefined rules. It’s a foundational tool for malware analysts and reverse engineers, especially when doing static triage.

---

### Tool Overview: How CAPA Works

CAPA uses a **rule-based system**. Each rule describes how to detect a specific capability, such as keylogging, process injection, persistence, or network communication. These rules look for **API calls, string patterns, code structures**, or **instruction sequences** inside the binary.

**How CAPA works (step-by-step):**

1. The user provides a binary to CAPA.
2. CAPA scans the file’s disassembled code (via libraries like vivisect or IDA export).
3. It matches findings against its extensive rule set.
4. The tool outputs structured results showing which capabilities were found, how, and where.

In short, CAPA reads the file’s *code DNA* and maps it to known malicious or functional patterns.

---

### Dissecting CAPA Results Part 1: General Information, MITRE and MAEC

When you run CAPA, the first section usually presents **General Information**:

* **File type:** Executable, DLL, shellcode, etc.
* **Architecture:** x86, x64
* **Rule count:** Number of rules applied and matched.
* **Analysis summary:** High-level overview of what CAPA found.

**MITRE ATT&CK Mapping:**
CAPA integrates with the MITRE ATT&CK framework, linking capabilities to adversarial tactics and techniques (like *Persistence: Registry Run Keys*, *Execution: Process Injection*, etc.). This helps analysts understand how the sample aligns with known attacker behaviors.

**MAEC (Malware Attribute Enumeration and Characterization):**
CAPA also aligns with MAEC taxonomy, which structures malware capabilities and behaviors in a standardized format. This provides a more formalized, machine-readable summary of what the malware can do.

---

### Dissecting CAPA Results Part 2: Malware Behavior Catalogue

This section summarizes all **detected behaviors** grouped into meaningful categories. Examples include:

* **Persistence Mechanisms:** Registry keys, startup folders, scheduled tasks.
* **Execution & Injection:** Running commands, process hollowing, DLL injection.
* **Data Theft:** Accessing keystrokes, screenshots, clipboard.
* **Network Activities:** HTTP requests, DNS communication, socket creation.

Each listed capability includes evidence from CAPA’s rules - for instance, detected API calls or byte patterns. Analysts use this section to quickly assess the sample’s potential threat level and operational intent.

---

### Dissecting CAPA Results Part 3: Namespaces

Namespaces in CAPA act as a **hierarchical structure** that organizes rules. They help group related behaviors logically for better readability.

For example:

```
anti-analysis/
    debugger-detection/
collection/
    keylogger/
    screenshot/
execution/
    process-injection/
```

This structure helps analysts navigate results and understand context quickly - e.g., whether a finding relates to persistence, collection, or defense evasion.

Namespaces also make it easier to filter and maintain CAPA rules over time.

---

### Dissecting CAPA Results Part 4: Capability

The **Capability section** is the heart of CAPA’s output. Each capability represents a specific action the binary is capable of performing. Examples include:

* **Create a process** - matches APIs like `CreateProcessA/W`
* **Inject into another process** - matches `WriteProcessMemory`, `OpenProcess`, `CreateRemoteThread`
* **Encrypt data using RC4** - identifies cryptographic algorithm usage
* **Establish persistence via registry key** - shows registry modification routines

Each capability lists:

* **Rule name**
* **Matched evidence (API calls, strings, etc.)**
* **Matched locations in the code**

This section is crucial for building hypotheses about malware intent and determining what parts of the code to inspect further in IDA, Ghidra, or a sandbox.

---

### More Information, more fun!

To explore more about CAPA and strengthen your understanding:

* Visit the official CAPA GitHub: [https://github.com/mandiant/capa](https://github.com/mandiant/capa)
* Study the rule repository to learn how rules are structured.
* Try running CAPA on different malware samples from public repositories (like MalwareBazaar) to see varied behaviors.
* Use CAPA’s **JSON output** for integration with automation pipelines or reporting tools.

CAPA is best combined with other static and dynamic tools (strings, PEStudio, IDA, or sandboxes like Any.Run) for holistic analysis.

💻 CAPA Web Explorer (Highly Recommended):
Instead of running CAPA from the terminal, most analysts use the CAPA Web Explorer - a visual, interactive interface hosted at https://mandiant.github.io/capa/explorer/#/.
It allows you to:

Upload your CAPA output (JSON or text) and explore it visually.

Filter by namespaces, MITRE mappings, or rule categories.

Click through capabilities and evidence for an intuitive investigation flow.

This makes learning and reviewing CAPA results much easier, especially when you’re just starting out or handling large, complex samples.

---

### Conclusion

The CAPA room may initially feel overwhelming - and that’s normal. It packs a lot of technical depth into one exercise. The key takeaway: CAPA helps analysts **understand what a binary can do** without running it. Think of it as your *forensic microscope* that highlights capabilities, tactics, and potential threats in a structured and repeatable way.
