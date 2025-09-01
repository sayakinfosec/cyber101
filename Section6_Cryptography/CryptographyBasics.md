## Cryptography Basics

### 💎 Importance of Cryptography

Cryptography ensures **secure communication** in the presence of adversaries. Its core goals are:

* **Confidentiality** → Keep data private
* **Integrity** → Prevent tampering
* **Authenticity** → Verify identities

We rely on cryptography daily, often without noticing:

* Logging in (credentials encrypted in transit)
* Using SSH (secure tunnel against eavesdropping)
* Online banking (server certificates verify authenticity)
* File downloads (hashes confirm integrity)

📌 In practice:

* **PCI DSS ```(Payment Card Industry Data Security Standard)```** → Credit card data must be encrypted **at rest** and **in transit**
* **HIPAA ```(Health Insurance Portability and Accountability Act)```** and **HITECH ```(Health Information Technology for Economic and Clinical Health Act)```** in the US → Medical record protection
* **GDPR ```(General Data Protection Regulation)```** in the EU → Data protection and privacy laws
* **DPA ```(Data Protection Act)```** in the UK → Legal requirements for personal data security

👉 Cryptography works in the background, but it underpins trust in nearly every digital interaction.

**Answer:** *What is the standard required for handling credit card information?* → **PCI DSS**

---

### 💎 Plaintext to Ciphertext

Let’s start with an illustration before introducing the key terms. We begin with the **plaintext** that we want to encrypt. The plaintext is the readable data; it can be anything from a simple “hello”, a cat photo, credit card information, or medical health records. From a cryptography perspective, these are all “plaintext” messages waiting to be encrypted. The plaintext is passed through the **encryption function** along with a proper **key**; the encryption function returns a **ciphertext**. The encryption function is part of the **cipher**; a cipher is an algorithm to convert a plaintext into a ciphertext and vice versa.

* The **encryption** function takes the **plaintext** and the **key** as input and returns the **ciphertext** as output.
* To recover the plaintext, we must pass the **ciphertext** along with the proper **key** via the **decryption** function, which would give us the original plaintext.
* The **decryption** function takes the **ciphertext** and the **key** as input and returns the **plaintext** as output.

**Key terms:**

* **Plaintext** is the original, readable message or data before it’s encrypted. It can be a document, an image, a multimedia file, or any other binary data.
* **Ciphertext** is the scrambled, unreadable version of the message after encryption. Ideally, we cannot get any information about the original plaintext except its approximate size.
* **Cipher** is an algorithm or method to convert plaintext into ciphertext and back again. A cipher is usually developed by a mathematician.
* **Key** is a string of bits the cipher uses to encrypt or decrypt data. In general, the used cipher is public knowledge; however, the key must remain secret unless it is the public key in asymmetric encryption.
* **Encryption** is the process of converting plaintext into ciphertext using a cipher and a key. Unlike the key, the choice of the cipher is disclosed.
* **Decryption** is the reverse process of encryption, converting ciphertext back into plaintext using a cipher and a key. Although the cipher would be public knowledge, recovering the plaintext without knowledge of the key should be impossible (infeasible).

**Answers:**

* *What do you call the encrypted plaintext?* → **ciphertext**
* *What do you call the process that returns the plaintext?* → **decryption**

---

### 💎 Historical Ciphers

Cryptography’s history is long and dates back to ancient Egypt in 1900 BCE. However, one of the simplest historical ciphers is the **Caesar Cipher** from the first century BCE. The idea is simple: shift each letter by a certain number to encrypt the message.

**Example:**

* **Plaintext:** `TRYHACKME`
* **Key:** 3 (right shift)
* **Cipher:** Caesar Cipher → `WUBKDFNPH`

To decrypt, we need:

* **Ciphertext:** `WUBKDFNPH`
* **Key:** 3
* **Cipher:** Caesar Cipher

For encryption, we shift to the right by three; for decryption, we shift to the left by three and recover the original plaintext. If someone gives us a ciphertext and tells that it was encrypted using Caesar Cipher, recovering the original text is trivial because there are only **25 possible keys** (alphabet size 26; shifting by 26 yields the same letter). Hence, by today’s standards (where the cipher is public), Caesar Cipher is considered **insecure** and **susceptible to brute force**.

We would come across many more historical ciphers in movies and cryptography books. Examples include:

* The **Vigenère cipher** (16th century)
* The **Enigma machine** (World War II)
* The **one-time pad** (Cold War)

**Answer:** *Knowing that `XRPCTCRGNEI` was encrypted using Caesar Cipher, what is the original plaintext?* → **ICANENCRYPT**
```
 1: WQOBSBQFMDH
 2: VPNARAPELCG
 3: UOMZQZODKBF
 4: TNLYPYNCJAE
 5: SMKXOXMBIZD
 6: RLJWNWLAHYC
 7: QKIVMVKZGXB
 8: PJHULUJYFWA
 9: OIGTKTIXEVZ
10: NHFSJSHWDUY
11: MGERIRGVCTX
12: LFDQHQFUBSW
13: KECPGPETARV
14: JDBOFODSZQU
15: ICANENCRYPT
16: HBZMDMBQXOS
17: GAYLCLAPWNR
18: FZXKBKZOVMQ
19: EYWJAJYNULP
20: DXVIZIXMTKO
21: CWUHYHWLSJN
22: BVTGXGVKRIM
23: AUSFWFUJQHL
24: ZTREVETIPGK
25: YSQDUDSHOFJ
```
The meaningful plaintext is at shift 15:
ICANENCRYPT

---

### 💎 Types of Encryption

The two main categories of encryption are **symmetric** and **asymmetric**.

#### Symmetric Encryption

Symmetric encryption (a.k.a. private key cryptography) uses the **same key** to encrypt and decrypt the data. Keeping the key **secret** is a must. Communicating the key to intended parties can be challenging (it requires a secure channel), especially with many recipients or powerful adversaries (e.g., industrial espionage).

**Example scenario:** We create a password‑protected document to share with a colleague. We can email the encrypted file, but we **shouldn’t** email the password via the same channel.

**Examples of symmetric ciphers:**

* **DES** (Data Encryption Standard) — adopted **1977**, **56-bit key**. Broken in <24 hours in 1999 due to increased computing power → deprecated.
* **3DES** (Triple DES) — DES applied three times; key size **168 bits** (effective \~112). Deprecated in **2019**; replace with AES (still seen in legacy systems).
* **AES** (Advanced Encryption Standard) — adopted **2001**; key sizes **128/192/256 bits**.

There are many more symmetric ciphers, but not all are standards.

#### Asymmetric Encryption

Asymmetric encryption uses a **pair of keys**: one to encrypt and the other to decrypt. To protect **confidentiality**, data is encrypted with the **recipient’s public key**; only the **recipient’s private key** can decrypt it.

**Examples:** RSA, Diffie‑Hellman, Elliptic Curve Cryptography (ECC).

* Asymmetric crypto tends to be **slower**; keys are **larger**.
* **RSA** typical key sizes: **2048/3072/4096 bits** (2048-bit minimum recommended).
* **Diffie‑Hellman**: minimum **2048 bits**; 3072/4096 for enhanced security.
* **ECC** achieves equivalent security with **shorter keys** (e.g., ECC‑256 ≈ RSA‑3072).

Asymmetric security relies on one‑way mathematical problems (easy forward, **infeasible** to reverse with today’s tech).

**Summary of New Terms:**

* **Alice** and **Bob** are fictional parties for cryptographic examples.
* **Symmetric encryption:** same secret key for encryption & decryption; key must remain confidential.
* **Asymmetric encryption:** public key (share) + private key (keep secret).

**Answers:**

* *Should you trust DES?* → **Nay**
* *When was AES adopted as an encryption standard?* → **2001**

---

### 💎 Basic Math

Modern cryptography is built on mathematics. Here we cover two operations frequently used in crypto:

#### XOR Operation

**XOR** (exclusive OR), symbol **⊕** or `^`, compares two bits and returns **1** if different, **0** if the same.

**Truth table:**

| A | B | A ⊕ B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 1     |
| 1 | 0 | 1     |
| 1 | 1 | 0     |

**Example:** `1010 ⊕ 1100 = 0110`

**Properties:**

* `A ⊕ A = 0`
* `A ⊕ 0 = A`
* Commutative: `A ⊕ B = B ⊕ A`
* Associative: `(A ⊕ B) ⊕ C = A ⊕ (B ⊕ C)`

**Crypto use (toy example):**

* Ciphertext `C = P ⊕ K` (plaintext XOR key)
* Recover plaintext: `C ⊕ K = (P ⊕ K) ⊕ K = P ⊕ (K ⊕ K) = P ⊕ 0 = P`

#### Modulo Operation

**Modulo** ( `%` or `mod` ) returns the **remainder** when dividing.

**Examples:**

* `25 % 5 = 0` because `25 = 5×5 + 0`
* `23 % 6 = 5` because `23 = 3×6 + 5`
* `23 % 7 = 2` because `23 = 3×7 + 2`

Notes:

* Modulo is **not reversible**: e.g., `x % 5 = 4` has infinite solutions for `x`.
* Result is always **0..(n−1)** for `a % n`.
* For big‑number work, languages like **Python** handle arbitrary‑precision integers; tools like **WolframAlpha** can also help.

**Answer:** *What’s `1001 ⊕ 1010`?* → **0011**
