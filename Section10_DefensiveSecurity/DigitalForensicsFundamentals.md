## Digital Forensics

### Introduction to Digital Forensics

Forensics involves applying scientific methods to investigate and solve crimes. **Digital Forensics** is the branch that deals with crimes involving digital devices - known as **cyber crimes**. These crimes are investigated using specialized tools and techniques to collect, analyze, and present digital evidence.

With the rise of digital devices, the number of digital crimes has also increased. Cyber criminals exploit systems, steal data, and misuse digital communication platforms.

### 💼 Example Scenario:
Law enforcement raided a bank robber’s home and found digital devices (laptop, mobile, hard drive, USB). The **Digital Forensics Team** was assigned the case and discovered:
- A digital map of the bank (on the laptop)
- Documents showing entrances, escape routes, and security bypass plans
- Media files of previous robberies
- Illegal chat groups and call records on the phone  

All this evidence was crucial in legal proceedings.

---

### 🎯 Learning Objectives
- Understand the **phases of digital forensics**
- Learn **types of digital forensics**
- Study **evidence acquisition procedures**
- Explore **Windows forensics**
- Solve a **forensics case**

---

### ✅ Question:
**Which team was handed the case by law enforcement?**  
→ *Digital Forensics*

---

### 🧭 Digital Forensics Methodology

The **National Institute of Standards and Technology (NIST)** defines four main phases for digital forensics investigations:

### 1️⃣ Collection
- Identify and collect all potential digital evidence (PCs, laptops, USBs, etc.).
- Ensure **data integrity** - evidence must not be altered.
- Properly **document** all collected items.

### 2️⃣ Examination
- Filter and extract relevant data from the collected evidence.
- Example: selecting media files from a specific date or extracting data from a specific user account.

### 3️⃣ Analysis
- **Correlate** multiple pieces of evidence to reconstruct events.
- Draw conclusions and timeline activities related to the case.

### 4️⃣ Reporting
- Prepare a **detailed report** covering methodology, findings, and recommendations.
- Include an **executive summary** for easier understanding by management and legal teams.

---

### 🧩 Common Types of Digital Forensics

| Type | Description |
|------|--------------|
| **Computer Forensics** | Investigating computers and digital storage devices. |
| **Mobile Forensics** | Examining mobile phones for SMS, call logs, GPS data, etc. |
| **Network Forensics** | Analyzing network logs and traffic to detect intrusions. |
| **Database Forensics** | Investigating tampering or exfiltration from databases. |
| **Cloud Forensics** | Investigating evidence on cloud storage or infrastructure. |
| **Email Forensics** | Analyzing emails for phishing or fraudulent activity. |

---

### ✅ Questions:
**Which phase correlates collected data to draw conclusions?**  
→ *Analysis*

**Which phase extracts data of interest from the collected evidence?**  
→ *Examination*

---

### 🧰 Evidence Acquisition

Acquiring evidence is a **sensitive and critical** process. The data must be collected securely, preserving its integrity and authenticity.

### 🪪 1️⃣ Proper Authorization
- Obtain **legal authorization** before collecting data.
- Evidence gathered without approval may be **inadmissible in court**.
- Protects the privacy rights of individuals and organizations.

### 📜 2️⃣ Chain of Custody
A **Chain of Custody** document maintains a record of:
- Description and type of evidence  
- Who collected it and when  
- Storage location  
- Access records  

This ensures accountability and proves evidence integrity in court.

### 💽 3️⃣ Use of Write Blockers
- Write blockers prevent accidental modification of the original evidence.
- They allow **read-only access** to digital media.
- Example: Using a write blocker when imaging a suspect’s hard drive ensures timestamps and files remain unaltered.

---

### ✅ Questions:
**Which tool ensures data integrity during evidence collection?**  
→ *Write Blocker*

**What document contains details of all collected evidence?**  
→ *Chain of Custody*

---

### 💻 Windows Forensics

Windows systems are commonly investigated in digital crimes. Forensic imaging of these systems involves two main types of data acquisition:

### 🧱 1️⃣ Disk Image
- Bit-by-bit copy of the **non-volatile storage** (HDD/SSD).
- Includes files, documents, browsing history, and more.
- Data persists even after shutdown or reboot.

### ⚡ 2️⃣ Memory Image
- Captures **volatile data** from the system’s RAM.
- Includes running processes, open files, and active network connections.
- Must be acquired **first**, since data is lost upon reboot or shutdown.

---

### 🧩 Common Windows Forensics Tools

| Tool | Purpose | Description |
|------|----------|--------------|
| **FTK Imager** | Disk Image Acquisition | GUI-based tool for creating and analyzing disk images in multiple formats. |
| **Autopsy** | Disk Image Analysis | Open-source platform with keyword search, file recovery, and metadata analysis. |
| **DumpIt** | Memory Image Acquisition | CLI-based utility to capture volatile memory data. |
| **Volatility** | Memory Analysis | Open-source tool with plugins for analyzing memory dumps across multiple OS types. |

---

### ✅ Question:
**Which type of forensic image is used to collect volatile data?**  
→ *Memory Image*

---

### 🧪 Practical Example of Digital Forensics

Every digital action - from creating files to taking photos - leaves **traces** that investigators can recover and analyze.

### 🐾 Case Scenario: The Kidnapped Cat “Gado”
A kidnapper sends a **ransom letter** in an MS Word document. The investigators convert it to a PDF and extract an image for analysis.

---

### 🔍 Step 1: PDF Metadata Analysis

Use the `pdfinfo` command to read metadata from the PDF:

```bash
pdfinfo ransom-letter.pdf
````

Example Output:

```
Creator:        Microsoft® Word for Office 365
Producer:       Microsoft® Word for Office 365
CreationDate:   Wed Oct 10 21:47:53 2018 EEST
ModDate:        Wed Oct 10 21:47:53 2018 EEST
```

🧠 **Insight:**
The PDF was created using *Microsoft Word for Office 365* on *October 10, 2018*.
Metadata can reveal **author names, timestamps, and software used**, helping trace the origin of a file.

---

### ✅ Question:

**Using pdfinfo, find the author of ransom-letter.pdf**
→ *Ann Gree Shepherd*

---

### 📸 Step 2: Image Metadata (EXIF) Analysis

Images contain **EXIF (Exchangeable Image File Format)** metadata, which may include:

* Camera or smartphone model
* Date and time of capture
* GPS coordinates (latitude/longitude)
* Camera settings (aperture, ISO, shutter speed)

---

### 🔧 Tool: exiftool

`exiftool` can extract metadata from images:

```bash
exiftool IMAGE.jpg
```

Example Output:

```
GPS Position : 51 deg 31' 4.00" N, 0 deg 5' 48.30" W
Camera Model : Canon EOS R6
```

By converting coordinates (e.g., `51°31'4.0"N 0°05'48.3"W`) and searching them in Google Maps or Bing Maps, investigators can identify the **exact location** where the image was captured.

---

### ✅ Questions:

**What is the name of the street where the photo was taken?**
→ *Milk Street*

**What is the model name of the camera used to take the photo?**
→ *Canon EOS R6*

---

### 🧾 Summary

| Concept                   | Description                                     |
| ------------------------- | ----------------------------------------------- |
| **Phases of Forensics**   | Collection → Examination → Analysis → Reporting |
| **Data Integrity Tool**   | Write Blocker                                   |
| **Documentation**         | Chain of Custody                                |
| **Volatile Data Capture** | Memory Image                                    |
| **Key Tools**             | FTK Imager, Autopsy, DumpIt, Volatility         |
| **PDF Metadata Tool**     | pdfinfo                                         |
| **Image Metadata Tool**   | exiftool                                        |

---

**Digital forensics** reveals how every digital trace - from file metadata to GPS data - can tell a story, helping investigators reconstruct events and uncover the truth.

