## Defensive Security Intro

### 🎯 Objective
Introduce defensive security concepts, including **Threat Intelligence**, **SOC**, **DFIR**, **Malware Analysis**, and **SIEM**.

### 🛠 Tools Used
- **SIEM Tool** → Monitor alerts and identify unauthorized login attempts.  
- **IP Scanner / Threat Intelligence Databases** → AbuseIPDB, Cisco Talos Intelligence, etc., to check reputation and location of suspicious IPs.  
- **Firewall / Block Lists** → Add malicious IPs to prevent further attacks.

### 💻 Command Executed
GUI Tool used SIEM, IP SCANNER, FIREWALL

### 🔍 Command / Tool Breakdown
- **SIEM** → Collects and analyzes security events.  
- **IP Scanner / Threat DB** → Helps analysts verify if an IP is malicious and gather contextual info.  
- **Firewall Block List** → Enforces network-level mitigation by blocking malicious IPs.

### 📂 Output Findings
- Unauthorized login attempt detected from a suspicious IP.  
- IP reputation flagged it as malicious in multiple databases.  
- Firewall block successfully added to prevent future attempts.

### 📝 Observations
- Defensive security relies heavily on **monitoring, intelligence, and proactive response**.  
- SIEM + Threat Intelligence databases are essential for a SOC analyst.  
- Real-world defensive work often involves coordination and rapid action to mitigate threats.
