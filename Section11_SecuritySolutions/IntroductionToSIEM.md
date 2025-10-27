### 11.1 Introduction to SIEM

### Introduction
SIEM stands for **Security Information and Event Management system**. It is a tool that collects data from various endpoints and network devices across the network, stores them in a centralized place, and performs correlation on them. This section covers the fundamental concepts required to understand SIEM and how it works.

### Learning Objectives
- Understand what SIEM is and how it works.  
- Learn why SIEM is needed.  
- Understand the concept of Network Visibility.  
- Learn about Log Sources and Log Ingestion.  
- Identify the key capabilities of a SIEM.  

**Question:** What does SIEM stand for?  
**Answer:** Security Information and Event Management system  

---

### Network Visibility through SIEM
Before understanding the importance of SIEM, we need to know why network visibility is critical. A simple network may consist of multiple Linux/Windows-based endpoints, one data server, and one website, all communicating through a router.  

Each network component has one or more log sources generating logs. For example, Windows Event Logs and Sysmon help gain visibility on Windows endpoints.

#### 1) Host-Centric Log Sources
Log sources that capture events within or related to the host. Examples include:
- Windows Event Logs  
- Sysmon  
- Osquery  

**Example activities:**
- User accessing a file  
- Authentication attempts  
- Process execution activity  
- Registry modifications  
- PowerShell executions  

#### 2) Network-Centric Log Sources
Logs generated when hosts communicate or access external resources.

**Examples include:**
- SSH connection  
- File transfer via FTP  
- Web traffic  
- VPN access  
- Network file sharing activity  

**Questions:**
- Is Registry-related activity host-centric or network-centric?  
  **Answer:** Host-centric  
- Is VPN-related activity host-centric or network-centric?  
  **Answer:** Network-centric  

---

### Importance of SIEM
Each device generates hundreds of events per second, making manual analysis nearly impossible. A SIEM system centralizes this data for real-time analysis, correlation, and alerting.

**Key Features of SIEM:**
- Real-time log ingestion  
- Alerting for abnormal activities  
- 24/7 monitoring and visibility  
- Early threat detection  
- Data insights and visualization  
- Ability to investigate past incidents  

---

### Log Sources and Log Ingestion
Every device generates logs whenever activities occur. Common log sources include:

#### Windows Machine
Logs are recorded in **Event Viewer**, where each log type is assigned a unique Event ID. Logs from all Windows endpoints are forwarded to SIEM for centralized monitoring.

#### Linux Workstation
Common Linux log locations:
- `/var/log/httpd` → HTTP request/response and error logs  
- `/var/log/cron` → Cron job events  
- `/var/log/auth.log` & `/var/log/secure` → Authentication logs  
- `/var/log/kern` → Kernel-related logs  

**Sample cron log:**
```

May 28 13:04:20 ebr crond[2843]: /usr/sbin/crond 4.4 started with loglevel notice
Jun 13 07:46:22 ebr crond[3592]: unable to exec /usr/sbin/sendmail

```

#### Web Server
Logs all requests/responses to detect possible attacks.  
Common locations: `/var/log/apache` or `/var/log/httpd`

**Sample Apache log:**
```

192.168.21.200 - - [21/March/2022:10:17:10 -0300] "GET /cgi-bin/try/ HTTP/1.0" 200 3395
127.0.0.1 - - [21/March/2022:10:22:04 -0300] "GET / HTTP/1.0" 200 2216

```

**Question:** In which location within a Linux environment are HTTP logs stored?  
**Answer:** /var/log/httpd  

#### Log Ingestion Methods
1. **Agent/Forwarder:** Lightweight tools (e.g., Splunk Forwarder) that send endpoint logs to SIEM.  
2. **Syslog:** Protocol used for transmitting logs to a centralized server.  
3. **Manual Upload:** Allows ingestion of offline data.  
4. **Port-Forwarding:** Endpoints send logs to a specific SIEM port.  

---

### Why SIEM
SIEM provides data correlation and alerts when threats or anomalies are detected. It enables quick response and comprehensive visibility of network activities.

---

### SIEM Capabilities
SIEM is a key component of the SOC ecosystem, providing:
- Event correlation across different sources  
- Host-centric and network-centric visibility  
- Threat investigation and response capabilities  
- Threat hunting for undetected issues  

---

### SOC Analyst Responsibilities
SOC Analysts use SIEM to:
- Monitor and investigate alerts  
- Identify and reduce false positives  
- Tune noisy rules  
- Generate compliance reports  
- Detect network blind spots  

---

### Analysing Logs and Alerts
Once logs are ingested, SIEM searches for suspicious patterns. If a rule condition is met, an alert is triggered.

#### Dashboards
Dashboards visualize SIEM data into actionable insights.

**Common dashboard information:**
- Alerts and notifications  
- System health  
- Failed login attempts  
- Ingested event counts  
- Triggered rules  
- Top domains visited  

---

### Correlation Rules
Correlation rules detect threats in real-time by defining conditions based on log field values.

**Examples:**
- 5 failed logins in 10 seconds → Alert for multiple failed login attempts  
- Successful login after multiple failures → Alert for successful login after brute-force  
- USB connection alert → Possible policy violation  
- Outbound traffic > 25 MB → Possible data exfiltration  

#### Example Rules:
**Use Case 1:**  
```

Condition: If Log Source = WinEventLog AND EventID = 104
Alert: Event Log Cleared

```

**Use Case 2:**  
```

Condition: If Log Source = WinEventLog AND EventCode = 4688 AND NewProcessName contains "whoami"
Alert: WHOAMI command execution detected

```

**Question:** Which Event ID is generated when event logs are removed?  
**Answer:** 104  

**Question:** What type of alert may require tuning?  
**Answer:** False Alarm  

---

### Alert Investigation
Analysts monitor dashboards to analyze triggered alerts and verify conditions.

**Investigation outcomes:**
- **False Alarm:** Tune rule to avoid repeats.  
- **True Positive:** Investigate further.  
- **Confirmed Threat:**  
  - Contact asset owner  
  - Isolate infected host  
  - Block malicious IP  

---

### Lab Work
**Lab Questions and Answers:**
- Process that caused the alert: **cudominer.exe**  
- User responsible: **chris**  
- Hostname of suspect user: **HR_02**  
- Rule-matching term: **miner**  
- Event type: **True-Positive**  
- **FLAG:** THM{000_SIEM_INTRO}  
