## John the Ripper
John the Ripper (JtR) is a well-known, versatile hash-cracking tool.  
It combines **fast cracking speed** with support for an **extraordinary range of hash types**.

---

### 🔹 What are Hashes?
- A **hash** takes input of any length and converts it into a **fixed-length output** using a hashing algorithm.  
- Examples: **MD4, MD5, SHA1, NTLM**.  
- Hashes are **one-way functions**:  
  - Easy to compute.  
  - Hard (computationally infeasible) to reverse.  

**Example:**  
```bash
MD5("polo")       = b53759f3ce692de7aff1b5779d3964da
MD5("polomints")  = 584b6e4f4586e136bc280f27f9c64f3b
````

---

### 🔹 Security of Hashes (P vs NP)

* Hashing = **P (Polynomial Time)** → easy to compute.
* Reversing hashes = **NP** → infeasible to compute.
* Thus, we rely on brute-force / dictionary attacks to "crack" them.

---

### 🔹 Where John Comes In

* If you know the hashing algorithm → hash a large wordlist → compare with target hash.
* If match → cracked.
* This is called a **Dictionary Attack**.

John the Ripper (Jumbo John version) automates this efficiently.

✅ **Answer:** The most popular extended version of John the Ripper = **Jumbo John**

---

### 🔹 Cracking Basics Hashes

#### 📌 Syntax

```bash
john [options] [file path]
```

* **john** → runs the program
* **\[options]** → flags/mode
* **\[file path]** → file containing hashes

#### 📌 Automatic Cracking

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

* JtR auto-detects the hash type.
* Sometimes unreliable.

#### 📌 Identifying Hashes

* John sometimes fails to auto-detect.
* Use external tools like **hashid** or **hash-identifier**.

**Example (hash-identifier):**

```bash
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
python3 hash-id.py
```

#### 📌 Format-Specific Cracking

```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

List all formats available in JtR:

```bash
john --list=formats
john --list=formats | grep -iF "md5"
```

---

#### 🧩 Practical Q\&A (Task 04)

Below are examples of cracking different hash types with John the Ripper.  
Each hash file contains a single hash, and we use `rockyou.txt` as the wordlist.  

#### 🧩 hash1.txt (MD5)
**Dummy hash (32 chars):**
```
5d41402abc4b2a76b9719d911017c592
````

**Command:**
```bash
john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
````

**Output:**

```
biscuit          (?)
```

✅ **Type:** MD5
✅ **Password:** biscuit


#### 🧩 hash2.txt (SHA1)

**Dummy hash (40 chars):**

```
356a192b7913b04c54574d18c28d46e6395428ab
```

**Command:**

```bash
john --format=Raw-SHA1 --wordlist=/usr/share/wordlists/rockyou.txt hash2.txt
```

**Output:**

```
kangeroo         (?)
```

✅ **Type:** SHA1
✅ **Password:** kangeroo


#### 🧩 hash3.txt (SHA256)

**Dummy hash (64 chars):**

```
6b3a55e0261b0304143f805a249bcaa2a0e4d5a4f14f7a64f1e6d8c2c3d3e7a0
```

**Command:**

```bash
john --format=Raw-SHA256 --wordlist=/usr/share/wordlists/rockyou.txt hash3.txt
```

**Output:**

```
microphone       (?)
```

✅ **Type:** SHA256
✅ **Password:** microphone


#### 🧩 hash4.txt (Whirlpool)

**Dummy hash (128 chars):**

```
76a2173be6393254e72ffa4d6df1030a7a3e7d6b51c46e4c4f3f5b0b59abf232
6f1d9f42d9c72ff16e94308c0e7c1d6b4d89e04b5e3c0ffcbf3f9aef3b9d3a7c
```

**Command:**

```bash
john --format=whirlpool --wordlist=/usr/share/wordlists/rockyou.txt hash4.txt
```

**Output:**

```
colossal         (?)
```

✅ **Type:** Whirlpool
✅ **Password:** colossal

---

### 🔹 Cracking Windows Authentication Hashes (NTLM)

* **NT Hash / NTLM** = hash format for Windows passwords.
* Stored in **SAM** database or **NTDS.dit** (AD).
* Often cracked with John, or used in **Pass-the-Hash** attacks.

**Example (Task 05):**

```bash
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt ntlm.txt
```

✅ **Answer:**

* Format = `nt`
* Cracked password = `mushroom`

---

### 🔹 Cracking `/etc/shadow` Hashes (Linux)

* Linux stores password hashes in `/etc/shadow`.
* Combine `/etc/passwd` + `/etc/shadow` using `unshadow`.

**Example:**

```bash
unshadow local_passwd local_shadow > unshadowed.txt
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
```

**Sample from Task 06:**

```bash
root:x:0:0::/root:/bin/bash
root:$6$Ha.d5nGupBm29pYr$yugXSk24ZljLTAZZ...:18576::::::
```

✅ **Answer:** Root password = `1234`

---

### 🔹 Single Crack Mode

* Uses **username info** to build possible passwords (word mangling).
* Requires prepending hash with username.

**Syntax:**

```bash
john --single --format=raw-md5 hash07.txt
```

**Example file format:**

```
Joker:7bf6d9bb82bed1302f331fc6b816aada
```

✅ **Answer (Task 07):** Joker’s password = `Jok3r`
