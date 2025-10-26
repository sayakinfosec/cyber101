## Incident Response Fundamentals

### Introduction to Incident Response

Imagine living in an insecure neighborhood with expensive items at home. You would likely install security systems like CCTV cameras or hire guards to protect your valuables. But what if someone still manages to break in? You’d need a plan for how to respond after the intrusion.

Similarly, organizations face cyber attacks that can cause financial and reputational losses. These are called **cybersecurity incidents**.  
**Incident Response (IR)** is the structured approach used to detect, respond to, and recover from these incidents effectively.

Incident Response manages an incident from beginning to end — prevention, detection, containment, eradication, and recovery.

**Learning Objectives**
- Understand incidents and severity levels  
- Identify types of incidents  
- Learn SANS and NIST frameworks  
- Explore tools and playbooks for response  
- Understand the Incident Response Plan  

---

### What are Incidents

Computing devices generate many **events** through processes. Security tools collect these as **logs** and analyze them for malicious patterns. When a harmful pattern is detected, an **alert** is triggered.

Alerts are then analyzed by the security team:
- **False Positive:** Benign activity mistakenly flagged as malicious.  
  *Example:* Backup data transfer flagged as data exfiltration.  
- **True Positive:** Correct detection of actual malicious activity.  
  *Example:* A phishing email detected and confirmed as real.

True positives become **incidents**, which are categorized by **severity** (low, medium, high, critical).

**Quick Check**
- What is triggered after harmful activity? → Alert  
- Correctly identified harmful activity? → True Positive  
- Fire alarm triggered by cooking smoke? → False Positive  

---

### Types of Incidents

Cyber incidents appear in many forms and affect organizations differently.

**Malware Infections:**  
Malicious software (malware) that damages systems, networks, or applications. Often spread via infected attachments or downloads.

**Security Breaches:**  
Unauthorized access to confidential data. These incidents are severe and can cause significant financial or reputational damage.

**Data Leaks:**  
Exposure of sensitive information to unauthorized entities. Can be caused intentionally or accidentally.

**Insider Attacks:**  
Attacks initiated by internal personnel such as disgruntled employees. These can be more dangerous since insiders have access privileges.

**Denial of Service (DoS):**  
Flooding systems or networks with fake requests, making them unavailable to legitimate users.

**Quick Check**
- Compromise after downloading a file? → Malware Infection  
- Attack disrupting availability? → Denial of Service  

---

### Incident Response Process

Incidents require a structured and repeatable process. Frameworks such as **SANS** and **NIST** define these processes.

**SANS Framework (PICERL)**

| Phase | Description | Example |
|-------|--------------|----------|
| Preparation | Build resources like IR teams, tools, and plans. | Conduct phishing awareness training. |
| Identification | Detect abnormal behavior. | Identify data exfiltration from a host. |
| Containment | Minimize attack impact. | Isolate compromised host. |
| Eradication | Remove threat completely. | Run malware scans to clean systems. |
| Recovery | Restore systems and operations. | Rebuild host and restore data. |
| Lessons Learned | Analyze incident to improve process. | Conduct post-incident review. |

Mnemonic: **PICERL** → Preparation, Identification, Containment, Eradication, Recovery, Lessons Learned.

**NIST Framework**

1. Preparation  
2. Detection and Analysis  
3. Containment, Eradication, and Recovery  
4. Post-Incident Activity  

The **Lessons Learned** phase in SANS corresponds to **Post-Incident Activity** in NIST.

**Incident Response Plan (IRP)**  
A formal document that defines procedures before, during, and after incidents.  
Key components:
- Roles and Responsibilities  
- Response Methodology  
- Communication Plan  
- Escalation Path  

**Quick Check**
- Disabling a system’s internet post-attack? → Containment  
- SANS “Lessons Learned” = NIST “Post-Incident Activity”  

---

### Incident Response Techniques

Manual detection is inefficient. Security solutions help automate and manage incidents.

**SIEM (Security Information and Event Management):**  
Centralizes and correlates logs for identifying incidents.

**AV (Antivirus):**  
Detects and removes known malicious software.

**EDR (Endpoint Detection and Response):**  
Protects endpoints, detects advanced threats, and supports containment and eradication.

**Playbooks and Runbooks**

**Playbooks:** Step-by-step *guidelines* for specific incidents.  
**Runbooks:** Detailed *execution steps* based on available tools.

**Example Phishing Email Playbook**
1. Notify stakeholders  
2. Analyze email header and body  
3. Check attachments  
4. Identify if attachments were opened  
5. Isolate infected systems  
6. Block sender  

**Quick Check**
- Comprehensive step-by-step IR guidelines → Playbooks  

---

### Lab Work: Practical Incident Response

**Scenario:**  
A phishing email containing a malicious attachment infects multiple systems. The task is to identify infected hosts, isolate them, and analyze the attack timeline.

**Findings**
- Malicious sender: Jeff Johnson  
- Threat vector: Email Attachment  
- Devices downloaded attachment: 3  
- Devices executed file: 1  
- Flag: `THM{My_First_Incident_Response}`  

---

### Conclusion

In this module, we learned:
- How events become incidents and how to classify them  
- Common incident types such as malware and DoS  
- Frameworks (SANS & NIST) and their phases  
- Tools such as SIEM, AV, and EDR  
- Role of playbooks and structured response processes  

Incident Response ensures organizations are **prepared, proactive, and systematic** in handling cybersecurity threats.
