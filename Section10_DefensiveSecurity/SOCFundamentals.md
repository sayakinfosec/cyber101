# SOC Fundamentals

### 🔰 Introduction to SOC

Technology has increased efficiency but also introduced greater risks. Organizations now store vast amounts of **confidential data** digitally, which threat actors constantly target through new vulnerabilities.  
Traditional security practices often fail to prevent these evolving threats - hence, the need for a **dedicated security team**.

### 🛡️ What is a SOC?

A **Security Operations Center (SOC)** is a dedicated facility operated by a specialized security team that:

- Monitors an organization’s network and systems 24/7  
- Detects suspicious activity  
- Responds to potential threats to prevent damage  

---

### 🎯 Purpose and Core Focus

The SOC team primarily focuses on **Detection** and **Response**, supported by centralized **security solutions** that integrate and monitor the organization’s entire network.

### Detection Capabilities:
- **Detect vulnerabilities:** Identify unpatched or exploitable weaknesses in systems or software.  
- **Detect unauthorized activity:** Spot abnormal or suspicious user behavior (e.g., login from unusual locations).  
- **Detect policy violations:** Identify violations like data exfiltration, misuse of resources, or insecure file sharing.  
- **Detect intrusions:** Monitor for unauthorized access to systems or malicious compromise.

### Response Capabilities:
- **Support incident response:** Contain, investigate, and mitigate security incidents.  
- **Perform root cause analysis:** Understand how and why the incident occurred to prevent recurrence.

---

### 🧱 The Three Pillars of SOC

A mature SOC environment stands on three foundational pillars:

1. **People** – Skilled professionals managing detection and response.  
2. **Process** – Defined workflows ensuring consistency and efficiency.  
3. **Technology** – Advanced tools and platforms for visibility, detection, and automation.

---

### 👥 People

Even with advanced automation, **humans remain critical** to SOC effectiveness. Automated systems may generate excessive “noise” - false positives that need human validation.

### SOC Team Roles and Responsibilities:

| Role | Description |
|------|--------------|
| **SOC Analyst (Level 1)** | First responders performing alert triage and initial reporting. |
| **SOC Analyst (Level 2)** | Conducts deeper investigations, correlates multi-source data. |
| **SOC Analyst (Level 3)** | Experienced professionals handling high-severity incidents, containment, and recovery. |
| **Security Engineer** | Deploys and maintains security tools, ensures optimal configuration. |
| **Detection Engineer** | Designs and maintains detection rules for security solutions. |
| **SOC Manager** | Oversees processes, team operations, and communicates with the CISO. |

> **Answer Check:**  
> • Alert triage & reporting → SOC Analyst (Level 1)  
> • Establishing detection rules → Detection Engineer  

---

### ⚙️ Process

Each SOC role follows structured processes for consistency and accuracy.  
Key SOC processes include **Alert Triage**, **Reporting**, and **Incident Response**.

### 🔍 Alert Triage

Triage involves analyzing alerts to determine severity and priority using the **5 Ws** framework:

| W | Meaning | Example |
|---|----------|----------|
| **What** | What happened | Malware detected on host |
| **When** | Time of occurrence | June 5, 2024, 13:20 |
| **Where** | Affected location | Host: GEORGE PC |
| **Who** | Involved entity | User: George |
| **Why** | Root cause | Downloaded pirated software |

### 🧾 Reporting

- Escalate harmful alerts to higher-level analysts as **tickets**.  
- Include **5 Ws**, screenshots, and evidence in reports.

### 🧩 Incident Response & Forensics

- Triggered for high-severity incidents.  
- Focuses on **containment**, **eradication**, and **root cause analysis** through forensic investigation.

> **Answer Check:**  
> • John tried to steal data → “Who”  
> • Data exfiltration detected → “What”  

---

### 💻 Technology

Technology enables efficient detection and response through automation and centralization.  
The **Technology pillar** includes key security tools and platforms that reduce manual workload.

### Key SOC Technologies:

| Tool | Function |
|------|-----------|
| **SIEM** (Security Information and Event Management) | Collects & correlates logs from multiple sources; detects suspicious activity. Provides **Detection** capability. |
| **EDR** (Endpoint Detection and Response) | Offers real-time endpoint visibility, investigation, and automated response. |
| **Firewall** | Monitors network traffic, blocks unauthorized access, and filters malicious traffic. |
| **Other Tools** | IDS/IPS, Antivirus, XDR, SOAR, EPP, etc. |

> **Answer Check:**  
> • Monitors network traffic → Firewall  
> • SIEM detects & alerts → Yea ✅  

---

## 🧪 Practical Exercise: SOC in Action

**Scenario:**  
You are a **Level 1 Analyst** investigating an alert for **port scanning** detected in your SIEM.

| W | Answer |
|---|---------|
| **What** | Port Scan |
| **When** | June 12, 2024 – 17:24 |
| **Where** | Destination Host: 10.0.0.3 |
| **Who** | Source Host: Nessus |
| **Why** | Intended (authorized scan) |
| **Response sent back?** | Yea |
| **Flag:** | `THM{000_INTRO_TO_SOC}` |

---

### 🧭 Conclusion

This module provided an overview of:
- The **structure and purpose** of a SOC  
- The **three foundational pillars** - People, Process, and Technology  
- Practical insight into how a Level 1 Analyst handles alerts using real-world tools and triage techniques  

**In summary:**  
A mature SOC environment thrives on skilled professionals, standardized processes, and powerful technology - all working together to ensure continuous **detection and response** against modern cyber threats.

