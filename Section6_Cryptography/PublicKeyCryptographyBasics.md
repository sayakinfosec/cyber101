## Public Key Cryptography Basics

### 💎 Introduction

* **Symmetric (Private Key) Crypto** → fast, mainly for **confidentiality**.
* **Asymmetric (Public Key) Crypto** → slower, but supports **authentication, authenticity, integrity**.
* Use asymmetric to exchange keys, then switch to symmetric for speed.

👉 Covered systems: **RSA, Diffie-Hellman, SSH, SSL/TLS Certificates, PGP/GPG**.

---

### 💎 Key Exchange Analogy

* **Secret code** = symmetric cipher + key
* **Lock** = public key
* **Key to lock** = private key

We use public keys once (for key exchange), then switch to faster symmetric encryption.

---

### 💎 RSA (Rivest–Shamir–Adleman)

* Based on the difficulty of **factoring large numbers**.
* **Easy** → multiply two primes.
* **Hard** → reverse factorization.

**RSA Workflow:**

* **p, q** → two **large prime numbers** (secret, chosen at random).
* **n** = p × q → the **modulus**, part of the **public key**.
* **ϕ(n)** = (p−1)(q−1) → Euler’s totient, used to calculate the private key.
* **e** → the **public exponent**, usually a small number like 65537. It must be coprime with ϕ(n).
* **d** → the **private exponent**, calculated as the modular inverse of e mod ϕ(n).
* **(n, e)** → the **public key** (everyone can see this).
* **(n, d)** → the **private key** (kept secret).
* **m** → the plaintext message (as a number).
* **c** → the ciphertext (encrypted message).

👉 **Encryption**:
c = m^e mod n

👉 **Decryption**:
m = c^d mod n

So in short:

* p, q = secret primes
* n = modulus
* e = public exponent
* d = private exponent
* m = message
* c = ciphertext

✅ Example (simplified):

* p = 157, q = 199 → n = 31243, ϕ(n) = 30888
* e = 163, d = 379
* Encrypt x=13 → y = 13^163 mod 31243 = 16341
* Decrypt y=16341 → x = 16341^379 mod 31243 = 13

**In CTFs**: know terms → p, q, n, e, d, m, c.  Tools: **RsaCtfTool, rsatool**.

---

### 💎 Diffie–Hellman (DH) Key Exchange

* Purpose: establish **shared secret** over insecure channel.
* Publicly agree on `p` (prime) and `g` (generator).
* Alice: pick private `a`, compute A = g^a mod p.
* Bob: pick private `b`, compute B = g^b mod p.
* Exchange A, B.
* Alice computes Key = B^a mod p, Bob computes Key = A^b mod p → both equal g^(ab) mod p.

✅ Example: p=29, g=5, a=12, b=17

* A = 5^12 mod 29 = 7
* B = 5^17 mod 29 = 9
* Shared key: 9^12 mod 29 = 7^17 mod 29 = 24

---

### 💎 SSH (Secure Shell)

* **Server authentication**: confirm server’s public key fingerprint.
* **Client authentication**: username+password OR key-based auth (recommended).
* **Key generation**: `ssh-keygen -t rsa | ed25519 | ecdsa ...`
* Private keys = like passwords → never share.
* Passphrase protects private key (locally, never sent).
* Public key goes into `~/.ssh/authorized_keys` on server.

👉 In CTFs: can use SSH keys as backdoors for persistent access.

---

### 💎 Digital Signatures & Certificates

* **Digital Signature** = prove authenticity + integrity.

  * Private key signs → Public key verifies.
  * Signature = encrypted hash of document.
* **Certificates** = prove identity via chain of trust.

  * Root CA → Intermediate CA → Website cert.
  * Example: HTTPS relies on TLS certificates.
* Free TLS certificates: **Let’s Encrypt**.

---

### 💎 PGP & GPG

* **PGP (Pretty Good Privacy)** → software for encryption/signing.
* **GPG (GNU Privacy Guard)** → open-source implementation of OpenPGP standard.
* Used in **emails, file encryption, digital signatures**.

**Workflow:**

1. Generate key pair: `gpg --full-gen-key`
2. Share **public key**.
3. Others encrypt with your public key → you decrypt with private key.
4. Private keys can be protected with passphrases.

Use GPG to decrypt the message in ~/Public-Crypto-Basics/Task-7. What secret word does the message hold?

```
user@ip-10-201-65-12:~/Public-Crypto-Basics/Task-7$ ls
message.gpg tryhackme.key
user@ip-10-201-65-12:~/Public-Crypto-Basics/Task-7$ gpg --import tryhackme.key
gpg: /home/user/.gnupg/trustdb.gpg: trustdb created
gpg: key FFA4B5252BAEB2E6: public key "TryHackMe (Example Key)" imported
gpg: key FFA4B5252BAEB2E6: secret key imported
gpg: Total number processed: 1
gpg:
imported: 1
gpg:
secret keys read: 1
gpg: secret keys imported: 1
user@ip-10-201-65-12:~/Public-Crypto-Basics/Task-7$ gpg --decrypt message.gpg
gpg: encrypted with rsa1024 key, ID 2A0A5FDC5081B1C5, created 2020-06-30 "TryHackMe (Example Key)"
You decrypted the file!
The secret word is Pineapple.
```

---

✅ **Quick Recap for Exams/CTFs:**

* **RSA** → secure data, based on factoring. Keys: p, q, n, e, d, m, c.
* **DH** → shared secret without prior key exchange.
* **SSH** → secure login, key-based auth.
* **Signatures** → integrity + authenticity (private sign, public verify).
* **Certificates** → identity proof (CA chain, HTTPS).
* **PGP/GPG** → encrypt/sign files, often used in email & CTFs.
