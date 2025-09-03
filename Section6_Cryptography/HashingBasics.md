## Hashing Basics

Consider the scenario where you just downloaded a 6 GB file and want to know whether the copy you downloaded is identical to the original file, bit for bit. How would you do that? Or if a good Samaritan handed you this 6 GB file on a USB memory drive, how can you be sure it is identical to the file you want to download?

The answer to both of the above questions lies in comparing the hash values of the two files; if two hash values are equal, you can say with very high certainty that the two files are identical. But what is a hash value?

A hash value is a fixed-size string or characters that is computed by a hash function. A hash function takes an input of an arbitrary size and returns an output of fixed length, i.e., a hash value.

---

### 🔹 Hash Functions

**What is a Hash Function?**  
Hash functions are different from encryption. There is no key, and it’s meant to be impossible (or computationally impractical) to go from the output back to the input.

A hash function takes some input data of any size and creates a summary or digest of that data. The output has a fixed size. It’s hard to predict the output for any input and vice versa. Good hashing algorithms will be relatively fast to compute and prohibitively slow to reverse, i.e., go from the output and determine the input. Any slight change in the input data, even a single bit, should cause a significant change in the output.

Example:  
The letter T is 54 in hexadecimal → `01010100` in binary.  
The letter U is 55 in hexadecimal → `01010101` in binary.  

Although they differ by a single bit, their hash values are entirely different.

```bash
user@ip-10-201-63-235:~/Hashing-Basics$ ls
Task-2 Task-6 Task-7 Task-8 id rsa 1593558668558.id rsa

user@ip-10-201-63-235:~/Hashing-Basics$ cd Task-2
user@ip-10-201-63-235:~/Hashing-Basics/Task-2$ ls
file1.txt file2.txt passport.jpg

user@ip-10-201-63-235:~/Hashing-Basics/Task-2$ sha1sum *.txt
c2c53d66948214258a26ca9ca845d7ac0c17f8e7 file1.txt
b2c7c0caa10a0cca5ea7d69e54018ae0c0389dd6 file2.txt

user@ip-10-201-63-235:~/Hashing-Basics/Task-2$ sha256sum *.txt
e632b7095b0bf32c260fa4c539e9fd7b852d0de454e9be26f24d0d6f91d069d3 file1.txt
a25513c7e0f6eaa80a3337ee18081b9e2ed09e00af8531c8f7bb2542764027e7 file2.txt

user@ip-10-201-63-235:~/Hashing-Basics/Task-2$ md5sum *.txt
b9ece18c950afbfa6b0fdbfa4ff731d3 file1.txt
4c614360da93c0a041b22e537del5leb file2.txt

user@ip-10-201-63-235:~/Hashing-Basics/Task-2$ sha256sum passport.jpg
77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02 passport.jpg
````

The output of a hash function is typically raw bytes, which are then encoded. Common encodings are base64 or hexadecimal.

---

### 🔹 Why is Hashing Important?

Hashing plays a vital role in our daily use of the Internet. Like other cryptographic functions, hashing remains hidden from the user. Hashing helps protect data’s integrity and ensure password confidentiality.

Example: When you log into TryHackMe, the server uses hashing to verify your password. Servers store only the hash of your password, not the password itself.

---

### 🔹 What’s a Hash Collision?

* A hash collision is when two different inputs give the same output.
* Collisions are unavoidable due to the pigeonhole principle.
* Good hash functions make collisions extremely unlikely.
* MD5 and SHA1 are considered insecure due to collision attacks.

**Q: What is the output size in bytes of the MD5 hash function?**
**A: 16 bytes.**

---

### 🔹 Insecure Password Storage for Authentication

**Bad Practices:**

1. Storing passwords in plaintext
2. Storing with deprecated encryption
3. Storing with insecure hashing algorithms

Example: The *rockyou.txt* wordlist came from RockYou storing passwords in plaintext.

```bash
user@ip-10-201-63-235:/usr/share/wordlists$ head -20 rockyou.txt
123456
12345
123456789
password
iloveyou
princess
1234567
rockyou
12345678
abc123
nicole
daniel
babygirl
monkey
lovely
jessica
654321
michael
ashley
qwerty
```

---

### 🔹 Using Hashing for Secure Password Storage

* Store only the **hash of the password**.
* Add a **unique salt** per user.
* Use secure hashing functions like Argon2, Scrypt, Bcrypt, PBKDF2.

Example process:

1. Select a secure hashing function.
2. Add a unique salt (e.g., `Y4UV*^(=go_!`).
3. Concatenate password + salt.
4. Hash result and store **hash + salt**.

Rainbow Tables → Precomputed hash-to-password mappings used for cracking unsalted hashes.

---

### 🔹 General-Purpose vs Password Hashing Functions

It’s important to distinguish between **general cryptographic hash functions** and **password hashing algorithms**:

- **General-purpose hashing functions**  
  Examples: **MD5, SHA-1, SHA-256, SHA-512**  
  - Designed to be **fast**  
  - Used for **integrity checking**, digital signatures, checksums  
  - Not suitable for password storage (fast = easy to brute force)

- **Password hashing algorithms (key derivation functions)**  
  Examples: **Argon2, Scrypt, Bcrypt, PBKDF2**  
  - Designed to be **slow and resource-intensive**  
  - Resist brute force and GPU/ASIC cracking  
  - Best choice for **secure password storage**

👉 In short:  
- **SHA1, SHA256, MD5 = Cryptographic hash functions**  
- **Argon2, Scrypt, Bcrypt, PBKDF2 = Password hashing algorithms**  

---

### 🔹 Recognising Password Hashes

* Use **context + tools** (hashID, Hashcat Example Hashes).
* Linux stores password hashes in `/etc/shadow`.

Format: `$prefix$options$salt$hash`

Common Prefixes:

| Prefix                         | Algorithm     |
| ------------------------------ | ------------- |
| \$y\$                          | yescrypt      |
| \$gy\$                         | gost-yescrypt |
| \$7\$                          | scrypt        |
| \$2b\$, \$2y\$, \$2a\$, \$2x\$ | bcrypt        |
| \$6\$                          | sha512crypt   |
| \$md5                          | SunMD5        |
| \$1\$                          | md5crypt      |

**Linux Example:**

```bash
root@TryHackMe# sudo cat /etc/shadow | grep strategos
strategos:$y$j9T$76UzfgEM5PnymhQ7TlJey1$/OOSg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4:19965:0:99999:7:::
```

**Windows Example:**

* Uses **NTLM (variant of MD4)**.
* Hashes stored in **SAM database**.

**Q\&A:**

* Hash size in yescrypt → **256**
* Hash-Mode for Cisco-ASA MD5 → **2410**
* Cisco-IOS \$9\$ → **scrypt**

---

### 🔹 Password Cracking

* Hashes are **not decrypted**; they’re cracked by brute force/dictionaries.
* Tools: **Hashcat, John the Ripper**.
* GPUs accelerate cracking (except for GPU-resistant algorithms like Bcrypt).

**Hashcat Syntax:**

```bash
hashcat -m <hash_type> -a <attack_mode> hashfile wordlist
```

* **`-m` (hash mode):**
  Specifies the type of hash you are attacking. Each algorithm has a unique ID number.
  Example:

  * `0` → MD5
  * `100` → SHA1
  * `3200` → bcrypt
  * `1400` → sha256
  * `1800` → sha512
    Full list available here: [Hashcat Example Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)

* **`-a` (attack mode):**
  Specifies how Hashcat generates guesses.
  Common modes:

  * `0` → Straight (dictionary attack)
  * `1` → Combination (combine words from multiple lists)
  * `3` → Brute-force / Mask attack (e.g., trying patterns like `?d?d?d?d` for 4 digits)
  * `6` → Hybrid wordlist + mask (append pattern to dictionary words)
  * `7` → Hybrid mask + wordlist (prepend pattern to dictionary words)


Examples:

```bash
hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt

user@ip-10-201-93-190:~/Hashing-Basics/Task-6$ hashcat m 3200 a 0 hash.txt /usr/share/wordlists/rockyou.txt
hashcat (v6.2.6) starting
OpenCL API (OpenCL 3.0 PoCL 5.0+debian VM 16.0.6, SLEEF, DISTRO, POCL DEBUG) Linux, None+Asserts, RELOC, SPIR, LL Platform #1 [The pocl project]
===
==
* Device #1: cpu-haswell-AMD EPYC 7571, 1418/2901 MB (512 MB allocatable), 2
MCU
Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 72
Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1
Optimizers applied:
* Zero-Byte
* Single-Hash
* Single-Salt
Watchdog: Temperature abort trigger set to 90c
Host memory required for this attack: 0 MB
Dictionary cache building /usr/share/wordlists/rockyou.txt: 33553435 bytes (
Dictionary cache building /usr/share/wordlists/rockyou.txt: 67106875 bytes (
Dictionary cache building /usr/share/wordlists/rockyou.txt: 100660309 bytes
Dictionary cache building /usr/share/wordlists/rockyou.txt: 134213745 bytes
Dictionary cache built:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344391
* Bytes... 139921497
* Keyspace..: 14344384

...
Candidates. #1. 85208520  -> 25251325
...


```

**Practice Q\&A:**

Q: Crack `$2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG`
A: `85208520`

Q: Crack `$6$GQXVvW4EuM$ehD6jWi...`
A: `spaceman`

Q: Crack `b6b0d451bbf6fed658659a9e7e5598fe`
A: `funforyou`

---

### 🔹 Hashing for Integrity Checking

* Used to confirm files are unchanged.
* Example: verifying Fedora ISO checksums.

```bash
root@AttackBox# head Fedora-Workstation-40-1.14-x86_64-CHECKSUM
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

# Fedora-Workstation-Live-osb-40-1.14.x86_64.iso: 2623733760 bytes
SHA256 (Fedora-Workstation-Live-osb-40-1.14.x86_64.iso) = 8d3cb4d99f27eb932064915bc9ad34a7529d5d073a390896152a8a899518573f
# Fedora-Workstation-Live-x86_64-40-1.14.iso: 2295853056 bytes
SHA256 (Fedora-Workstation-Live-x86_64-40-1.14.iso) = dd1faca950d1a8c3d169adf2df4c3644ebb62f8aac04c401f2393e521395d613
```
---

### 🔹 HMACs

HMAC (Keyed-Hash Message Authentication Code): ensures **authenticity + integrity**.

HMAC (Keyed-Hash Message Authentication Code) is a type of message authentication code (MAC) that uses a cryptographic hash function in combination with a secret key to verify the authenticity and integrity of data.

An HMAC can be used to ensure that the person who created the HMAC is who they say they are, i.e., authenticity is confirmed; moreover, it proves that the message hasn’t been modified or corrupted, i.e., integrity is maintained. This is achieved through the use of a secret key to prove authenticity and a hashing algorithm to produce a hash and prove integrity.


Formula:

```
HMAC(K,M) = H((K⊕opad) || H((K⊕ipad) || M))
```

**Q\&A:**

* SHA256 hash of `libgcrypt-1.11.0.tar.bz2` ?
```
sha256sum ~/Hashing-Basics/Task-7/libgcrypt-1.11.0.tar.bz2
09120c9867ce7f2081d6aaa1775386b98c2f2f246135761aae47d81f58685b9c
```
* Hashcat mode for HMAC-SHA512 (key = \$pass) → **1750**

---

### 🔹 Conclusion

Hashing, as already stated, is a process that takes input data and produces a hash value, a fixed-size string of characters, also referred to as digest. This hash value uniquely represents the data, and any change in the data, no matter how small, should lead to a change in the hash value. Hashing should not be confused with encryption or encoding; hashing is one-way, and you can’t reverse the process to get the original data.

Encoding converts data from one form to another to make it compatible with a specific system. ASCII, UTF-8, UTF-16, UTF-32, ISO-8859-1, and Windows-1252 are valid encoding methods for the English language. Note that UTF-8, UTF-16, and UTF-32 are Unicode encodings, and they can represent characters from other languages, such as Arabic and Japanese.

Another type of encoding commonly used when sending or saving data is not for any specific language. Examples include Base32 and Base64 encoding. Consider the following example of using base64 to encode and decode.

Encoding should not be confused with encryption, as using a specific encoding does not protect the confidentiality of the message. Encoding is reversible; anyone can change the data encoding with the right tools.

Only encryption, which we covered in the previous rooms, protects data confidentiality using a cryptographic cipher and a key. Encryption is reversible, provided we know the cipher and can access the key.


* **Hashing** → One-way, ensures integrity, cannot be reversed.
* **Encoding** → Converts data format, reversible, not secure.
* **Encryption** → Two-way, reversible with a key, ensures confidentiality.

**Base64 Example:**

```bash
strategos@g5000 ~> base64
TryHackMe
VHJ5SGFja01lCg==

strategos@g5000 ~> base64 -d
VHJ5SGFja01lCg==
TryHackMe
```

**Final Q\&A:**
Q: Decode `RU5jb2RlREVjb2RlCg==`
A: `ENcodeDEcode`

```bash
base64 -d ~/Hashing-Basics/Task-8/decode-this.txt
```

