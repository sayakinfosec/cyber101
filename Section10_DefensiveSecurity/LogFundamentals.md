
## Logs Fundamentals

### Introduction to Logs
Attackers often try to avoid leaving traces, but security teams can determine attack methods and sometimes the attacker.  
Logs act as digital footprints of activities, both normal and malicious, within a system.

**Analogy:**  
Imagine policemen investigating a stolen locket in a snowy cabin. They use door damage, footprints, and CCTV footage to find the culprit. Similarly, logs provide traces for digital investigations.

**Use Cases of Logs:**

| Use Case | Description |
|----------|-------------|
| Security Events Monitoring | Detect anomalous behavior via real-time log monitoring. |
| Incident Investigation & Forensics | Trace activities during incidents and perform root cause analysis. |
| Troubleshooting | Diagnose system or application errors. |
| Performance Monitoring | Gain insights into application performance. |
| Auditing & Compliance | Establish a trail of activities for compliance. |

**Learning Objectives:**  
- Understand different types of logs  
- Learn log analysis techniques  
- Analyze Windows Event Logs  
- Analyze Web Access Logs  

**Q&A:**  
**Q:** Where can we find the majority of attack traces in a digital system?  
**A:** Logs  

---

### Types of Logs
Logs are categorized based on the type of information they provide, making investigations more efficient.

| Log Type | Usage | Example |
|----------|-------|---------|
| System Logs | Troubleshooting OS issues | Startup/shutdown, driver load, hardware events |
| Security Logs | Detect & investigate security incidents | Authentication, authorization, policy changes, abnormal activity |
| Application Logs | Track application-specific events | User interactions, updates, errors |
| Audit Logs | Compliance & detailed monitoring | Data access, system changes, user activity, policy enforcement |
| Network Logs | Incoming/outgoing network traffic | Traffic logs, connection logs, firewall logs |
| Access Logs | Resource access tracking | Webserver, database, application, API access |

**Q&A:**  
**Q:** Which logs contain network traffic info? → **Network Logs**  
**Q:** Which logs contain authentication & authorization events? → **Security Logs**

---

### Windows Event Logs Analysis
Windows OS logs activities in different categories:

- **Application:** Application errors, warnings, compatibility issues  
- **System:** Driver issues, hardware problems, startup/shutdown info  
- **Security:** Authentication, user account changes, policy updates  

**Event Viewer:** GUI utility to view and analyze logs.  

**Key Fields in Event Logs:**  
- **Description:** Activity details  
- **Log Name:** Name of the log file  
- **Logged:** Time of the event  
- **Event ID:** Unique identifier (e.g., 4624 = successful login)

**Important Event IDs:**

| Event ID | Description |
|----------|-------------|
| 4624 | Successful login |
| 4625 | Failed login |
| 4634 | Successful logoff |
| 4720 | User account created |
| 4724 | Password reset attempted |
| 4722 | User account enabled |
| 4725 | User account disabled |
| 4726 | User account deleted |

**Exercise Q&A Example:**  
- Last user created: **hacked**  
- Created by: **Administrator**  
- Enabled on: **6/7/2024**  
- Password reset: **Yes**

---

### Web Server Access Logs Analysis
Web servers log all requests including timestamps, IP addresses, request type, and URLs.

**Key Fields:**  
- **IP Address**: User IP  
- **Timestamp**: Request time  
- **HTTP Method**: GET, POST, etc.  
- **URL**: Requested resource  
- **Status Code**: Server response (200, 404, etc.)  
- **User-Agent**: OS/browser info  

**Linux CLI Commands for Manual Log Analysis:**  
- **cat:** Display contents of a log file  
  ```bash
  cat access.log
  cat access1.log access2.log > combined_access.log
  ```

* **grep:** Search for specific patterns or IPs

  ```bash
  grep "192.168.1.1" access.log
  ```
* **less:** View logs page by page

  ```bash
  less access.log
  ```

  * Use `/pattern` to search, `n` for next, `N` for previous

**Exercise Q&A Example:**

* Last GET request to `/contact`: **10.0.0.1**
* Last POST request by `172.16.0.1`: **06/Jun/2024:13:55:44**
* POST request URL: **/contact**

---

### Conclusion

Logs are critical in digital investigations.

* Different types of logs serve specific purposes.
* Manual and automated log analysis techniques help identify abnormal activity.
* Practical exercises in Windows Event Logs and Web Access Logs provide hands-on experience in analyzing incidents.
