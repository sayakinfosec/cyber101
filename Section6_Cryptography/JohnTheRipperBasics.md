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

---

### 🔹 Custom Rules

#### What are Custom Rules?
Custom rules in **John the Ripper** allow us to define **patterns** that mimic how users typically create passwords.  
This lets us **exploit password complexity predictability** (like capital letters at the start, numbers/symbols at the end, etc.).

#### Examples of Common Custom Rule Modifiers
- `Az` → Append characters at the end  
- `A0` → Prepend characters at the beginning  
- `c` → Capitalises the first character  

Character sets inside brackets `[]`:
- `[0-9]` → Numbers 0–9  
- `[A-Z]` → Uppercase letters  
- `[a-z]` → Lowercase letters  
- `[!£$%@]` → Symbols  

#### Example: PoloPassword Rule
```

\[List.Rules\:PoloPassword]
cAz"\[0-9]\[!£\$%@]"

````

This means:
- `c` → Capitalise first letter  
- `Az"[0-9]` → Append a number  
- `Az"[!£$%@]"` → Append one of the listed symbols  

So, `polopassword` → `Polopassword1!`

#### Using Custom Rules
We can run custom rules with:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --rule=PoloPassword hash.txt
````

#### Questions & Answers

**Q1: What do custom rules allow us to exploit?**
👉 Password complexity predictability

**Q2: What rule would we use to add all capital letters to the end of the word?**
👉 `Az"[A-Z]"`

**Q3: What flag would we use to call a custom rule called THMRules?**
👉 `--rule=THMRules`

#### Cracking Password Protected Zip Files

We can also use **John the Ripper** to crack password-protected zip files.
This involves two steps:

1. **Extract the hash from the zip file** using `zip2john`
2. **Crack it with John**

#### Step 1: Extract Hash with zip2john

```bash
zip2john secure.zip > secureZipHash.txt
```

#### Step 2: Crack with John

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt secureZipHash.txt
```

After some time, John finds the password:

```
pass123
```

#### Step 3: Unzip with Found Password

```bash
unzip secure.zip
```

Terminal session:

```bash
user@ip-10-201-117-217:~/John-the-Ripper-The-Basics/Task09$ unzip secure.zip 
Archive:  secure.zip
[secure.zip] zippy/flag.txt password: 
 extracting: zippy/flag.txt          
```

#### Step 4: Retrieve the Flag

```bash
cd zippy
cat flag.txt
```

Terminal session:

```bash
user@ip-10-201-117-217:~/John-the-Ripper-The-Basics/Task09/zippy$ cat flag.txt 
THM{w3ll_d0n3_h4sh_r0y4l}
```

#### Questions & Answers

**Q1: What is the password for the secure.zip file?**
👉 `pass123`

**Q2: What is the contents of the flag inside the zip file?**
👉 `THM{w3ll_d0n3_h4sh_r0y4l}`

---

### 🔹 Cracking Password-Protected RAR Archives

Cracking a Password-Protected RAR Archive  
We can use a similar process to the one we used for Zip files to obtain the password for RAR archives. RAR archives are compressed files created by the WinRAR archive manager. Like Zip files, they compress folders and files.

#### Rar2John
Just like the `zip2john` tool, we use the `rar2john` tool to convert the RAR file into a hash format that John can understand.  

**Basic syntax:**
```bash
rar2john [rar file] > [output file]
````

* `rar2john`: Invokes the rar2john tool
* `[rar file]`: Path to the RAR file
* `>`: Redirects the output
* `[output file]`: File to store the hash

**Example usage:**

```bash
rar2john rarfile.rar > rar_hash.txt
```

#### Cracking

We can now take the hash output file and crack it with John:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
```


#### Practical

RAR file location:
`~/John-the-Ripper-The-Basics/Task10/`

**Q1. What is the password for the secure.rar file?**
`password` ✅

**Q2. What are the contents of the flag inside the rar file?**
`THM{r4r_4rch1ve5_th15_t1m3}` ✅


#### Important Commands

**Step 1: Crack the password**

```bash
$ ls
secure.rar

$ rar2john secure.rar > secureRarFile.txt
OK.

$ ls
secure.rar  secureRarFile.txt

$ john --wordlist=/usr/share/wordlists/rockyou.txt secureRarFile.txt
...
...
password
```

**Step 2: Extract the RAR archive**

```bash
$ unrar x secure.rar
OK.

$ ls
secure.rar  secureRarFile.txt  flag.txt
```

---

### 🔹Cracking SSH Keys with John

#### Cracking SSH Key Passwords
SSH private keys (`id_rsa`) are often used instead of passwords for authentication over SSH. However, these private keys can themselves be protected with a passphrase. John can crack such passphrases and allow us to use the key for authentication.

#### SSH2John
We first need to convert the `id_rsa` file into a hash format that John understands. This is done using the `ssh2john` (or `ssh2john.py`) tool.

**Syntax:**
```
ssh2john \[id_rsa private key file] > \[output file]
```

- `ssh2john`: Invokes the tool  incase its not installed we used the python version from /opt/john/ssh2john.py
- `[id_rsa private key file]`: The SSH private key file (id_rsa)  
- `>`: Redirects output into a file  
- `[output file]`: Where the converted hash will be saved  

**Example Usage:**
```
/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```

#### Cracking
Once converted, use John with a wordlist to crack the passphrase:
```
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```

#### Practical
- Location of the file: `~/John-the-Ripper-The-Basics/Task11/`  
- Cracked password: **mango**

#### Original Terminal Output
```
user\@ip-10-201-117-217:~~/John-the-Ripper-The-Basics/Task11\$ ls
id_rsa
user\@ip-10-201-117-217:~~/John-the-Ripper-The-Basics/Task11\$ /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
user\@ip-10-201-117-217:~~/John-the-Ripper-The-Basics/Task11\$ ls
id_rsa  id_rsa_hash.txt
user\@ip-10-201-117-217:~~/John-the-Ripper-The-Basics/Task11\$ john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key \[RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher \[0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: Passwords longer than 10 \[worst case UTF-8] to 32 \[ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
mango            (id_rsa)
1g 0:00:00:00 DONE (2025-09-05 11:52) 50.00g/s 214400p/s 214400c/s 214400C/s praise..mango
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
user\@ip-10-201-117-217:\~/John-the-Ripper-The-Basics/Task11\$
```
