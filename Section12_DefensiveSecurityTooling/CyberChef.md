## CyberChef - The Basics  

### Introduction  
**CyberChef** is a simple, intuitive web-based application designed to assist with various cyber operations directly in your web browser.  
Think of it as a **Swiss Army knife for data**, offering a toolbox for performing tasks such as:  
- Simple encodings like **XOR** or **Base64**  
- Complex operations like **AES encryption** or **RSA decryption**  

CyberChef operates using **recipes** - sequences of operations executed in order.

### Learning Objectives  
- Understand what CyberChef is  
- Learn how to navigate the interface  
- Explore common operations  
- Learn how to create recipes and process data  

---

### Accessing CyberChef  

**Online Access:**  
Access CyberChef directly in your browser [here](https://gchq.github.io/CyberChef/).

**Offline or Local Copy:**  
Download the latest release to run it offline on **Windows** or **Linux**. It’s best to use the most stable version.

---

### Navigating the Interface  

CyberChef consists of four key areas:  
1. **Operations**  
2. **Recipe**  
3. **Input**  
4. **Output**

#### 1. Operations Area  
A categorized repository of all available operations.  
You can search, drag, and use operations for encoding, decoding, encryption, and data transformation.

**Examples of Common Operations**

| Operation | Description | Example |
|------------|--------------|----------|
| From Morse Code | Translates Morse Code into alphanumeric characters. | `- .... .-. . .- - ...` → `THREATS` |
| URL Encode | Encodes special characters in URLs. | `https://tryhackme.com` → `https%3A%2F%2Ftryhackme%2Ecom` |
| To Base64 | Encodes text into Base64. | `This is fun!` → `VGhpcyBpcyBmdW4h` |
| To Hex | Converts text to hexadecimal bytes. | `Hello` → `48 65 6C 6C 6F` |
| ROT13 | Caesar cipher shifting letters by 13. | `Digital Forensics` → `Qvtvgny Sberafvpf` |

---

#### 2. Recipe Area  
The **heart** of CyberChef - where you select, arrange, and fine-tune operations.

**Features:**  
- **Save Recipe** – store your workflow  
- **Load Recipe** – reuse past operations  
- **Clear Recipe** – reset current recipe  
- **BAKE!** – execute the recipe manually  
- **Auto Bake** – automatically apply changes

---

#### 3. Input Area  
Where you paste or upload text/files to be processed.

**Features:**  
- Add new input tabs  
- Open file or folder as input  
- Clear input/output  
- Reset pane layout  

---

#### 4. Output Area  
Displays the processed results.  

**Features:**  
- Save output as `.dat` file  
- Copy raw output to clipboard  
- Replace input with output  
- Maximize pane  

---

### Before Anything Else: The CyberChef Thought Process  

CyberChef follows a simple 4-step workflow:  

1. **Set an Objective** – What do you want to achieve?  
2. **Input Data** – Paste or upload data.  
3. **Select Operations** – Choose suitable operations (e.g., ROT13, Base64).  
4. **Check Output** – Confirm results. If not correct, refine and repeat.  

---

### Practice Exercises  

#### Extractors  
| Operation | Description |
|------------|-------------|
| Extract IP addresses | Finds all IPv4 and IPv6 addresses |
| Extract URLs | Finds all valid URLs |
| Extract email addresses | Finds valid email addresses |

#### Date and Time  
| Operation | Description |
|------------|-------------|
| From UNIX Timestamp | Converts UNIX time → datetime |
| To UNIX Timestamp | Converts datetime → UNIX time |

#### Data Format  
| Operation | Description | Example |
|------------|--------------|----------|
| From Base64 | Decodes Base64 to text | `V2VsY29tZSB0byB0cnloYWNrbWUh` → `Welcome to tryhackme!` |
| URL Decode | Decodes percent-encoded URLs | `%3A%2F` → `:/` |
| From Base85 | Decodes ASCII Base85 | `BOu!rD]j7BEbo7` → `hello world` |
| From Base58 | Decodes Base58 | `AXLU7qR` → `Thm58` |
| To Base62 | Encodes text to Base62 | `Thm62` → `6NiRkOY` |

---

### Manual Example: Encoding “THM” in Base64  

**Step 1:** Convert to Binary  
```

T = 01010100
H = 01001000
M = 01001101
Merged: 010101000100100001001101

```

**Step 2:** Split into 6-bit chunks → Convert to Decimal  
```

010101 → 21
000100 → 4
100001 → 33
001101 → 13

```

**Step 3:** Match with Base64 Index Table → VEhN  

✅ **Result:** “THM” → `VEhN`

---

### Practical Task Answers  

| Question | Answer |
|-----------|---------|
| Hidden email address | `hidden@hotmail.com` |
| Hidden IP ending in .232 | `102.20.11.232` |
| Domain starting with “T” | `TryHackMe.com` |
| Binary of 78 | `01001110` |
| URL encoded `https://tryhackme.com/r/careers` | `https%3A%2F%2Ftryhackme%2Ecom%2Fr%2Fcareers` |

---

### Your First Official Cook  

| Question | Answer |
|-----------|---------|
| IP starting and ending with “10” | `10.10.2.10` |
| Base64 of “Nice Room!” | `TmljZSBSb29tIQ==` |
| URL decoded value | `https://tryhackme.com/r/room/cyberchefbasics` |
| Datetime for UNIX 1725151258 | `Sun 1 September 2024 00:40:58 UTC` |
| Base85 decoded `<+oue+DGm>Ap%u7` | `This is fun!` |

---

### Conclusion  
CyberChef is a **powerful data manipulation tool** that simplifies encoding, decoding, and data extraction tasks.  
From Base64 to URL decoding, it provides a visual interface for hands-on data transformation.  
With its recipe-based design, CyberChef becomes a must-have **Swiss Army knife for cybersecurity practitioners**.


Would you like me to include a **short summary section at the top** (for example: “Room Overview”, “Skills Learned”, and “Difficulty Level”) like in a portfolio-style `.md` for your TryHackMe progress tracking?
