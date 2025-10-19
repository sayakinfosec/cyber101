## Gobuster

### 🔹 1. Fast intro & core concepts

* **Gobuster:** Golang tool for brute-forcing web directories, DNS subdomains, vhosts, GCS & S3 buckets.
* **Placement in testing:** Between reconnaissance and scanning phases (enumeration-heavy).
* **Enumeration:** Listing resources (accessible or not) — e.g., directories, hosts.
* **Brute force:** Trying wordlist entries until a match (each word forms a URL/host query).

---

### 🔹 2. Gobuster at a glance (help / common flags)

* Run help: `gobuster --help` or `gobuster <mode> --help`.
* Common commands: `dir`, `dns`, `vhost`, `fuzz`, `gcs`, `s3`.
* Highly used flags:

  * `-u/--url` target URL (required for dir/vhost).
  * `-d/--domain` target domain (required for dns).
  * `-w/--wordlist` path to wordlist (required).
  * `-t/--threads` concurrent threads (default 10).
  * `--delay` wait time between requests.
  * `-o/--output` write results to file.
  * `--no-progress`, `--no-color`, `--debug` for output control.

### 🔹 2.a Quick example syntax

```bash
gobuster dir -u "http://example.thm" -w /path/to/wordlist -t 64
gobuster dns -d example.thm -w /path/to/subdomains.txt
gobuster vhost -u "http://MACHINE_IP" -w /path/to/list --domain example.thm --append-domain
```

### 🔹 2.b Tips

* Use hostname (not IP) when you expect virtual hosts.
* Increase threads for speed but watch target stability and IDS triggers.
* Use `--delay` to avoid detection/rate-limiting.

---

### 🔹 3. Dir mode (directory & file enumeration)

* Purpose: discover directories and files on a web server.
* Required flags: `dir -u <URL> -w <wordlist>`.
* Useful flags:

  * `-x/--extensions` scan specific extensions (e.g., `.php,.js`).
  * `-r/--follow-redirect` follow 3xx redirects.
  * `-s/--status-codes` / `-b/--status-codes-blacklist` filter results by status codes.
  * `-H/--headers` add custom headers (e.g., Auth or Host).
  * `-c/--cookies` include cookies for authenticated scans.
  * `-k/--no-tls-validation` skip TLS checks (self-signed certs).
* Behavior notes:

  * Gobuster does **not** recurse automatically — re-run for discovered folders.
  * Status codes and response sizes help prioritize findings (200, 301/302, 401).

### 🔹 3.a Example

```bash
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js -t 40 -r
```

* Use `-x` to find files with chosen extensions and `-r` to follow redirects.

---

### 🔹 4. DNS mode (subdomains) & VHOST mode (virtual hosts)

#### DNS mode (subdomain brute-force)

* Purpose: brute-force subdomains for a domain (detect forgotten or vulnerable subdomains).
* Required flags: `dns -d <domain> -w <wordlist>`.
* Useful flags:

  * `-i/--show-ips` display resolved IPs.
  * `-c/--show-cname` show CNAME records.
  * `-r/--resolver` use custom DNS server.
* Example:

```bash
gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

#### VHOST mode (virtual host enumeration)

* Purpose: brute-force Host header values to discover virtual hosts served on an IP.
* Key difference vs dns: **vhost changes the HTTP Host header**, dns performs real DNS lookups.
* Required flags: `vhost -u <URL/IP> -w <wordlist>`.
* Important flags:

  * `--domain` set base domain to append (e.g., `example.thm`).
  * `--append-domain` append domain to each wordlist entry (useful when DNS not present).
  * `--exclude-length` filter common false-positive response sizes.
  * `--follow-redirect` follow redirects.
  * `--method` change HTTP method when required.
* Example:

```bash
gobuster vhost -u "http://10.10.XX.XX" --domain example.thm -w /path/to/subdomains.txt --append-domain --exclude-length 250-320
```

* Use `--exclude-length` to reduce false positives where the server returns similar-sized 404 pages.

### 🔹 4.a Practical workflow suggestions

* Start with **dns** to find subdomains; use `--show-ips` to map hosts.
* Use **dir** on interesting hosts/paths discovered to find files and panels.
* Use **vhost** against IPs when DNS is incomplete (or to find hidden sites on the same server).
* Save outputs (`-o`) and re-run with focused wordlists for discovered directories.
* Combine with other tools (nmap, curl, browser) to validate findings and inspect responses.
* 
