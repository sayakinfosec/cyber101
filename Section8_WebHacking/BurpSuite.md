## Burp Suite

### 🔹 Introduction to Burp Suite
Burp Suite is an integrated platform used for web application security testing.  
It allows you to intercept, analyze, and modify HTTP(S) requests and responses between the browser and the target application.  

Key components include:
- **Proxy** – intercepts and inspects traffic.
- **Target & Site Map** – lists discovered endpoints.
- **Repeater** – re-sends and manipulates requests.
- **Intruder** – automates payload injection.
- **Decoder** – encodes/decodes data.
- **Comparer** – highlights differences between requests/responses.

---

### 🔹 Proxy Interception
- Ensure **Intercept is ON** to capture traffic.  
- Configure browser to use Burp as proxy:
```

127.0.0.1:8080

````
- Use Burp’s **embedded browser** (recommended for HTTPS and certificate trust issues).  

Intercepted requests can be:
- **Forwarded** – sent to the server unchanged.
- **Dropped** – blocked.
- **Modified** – altered before sending.

---

### 🔹 Target & Site Map
- The **Site Map** displays all discovered endpoints.  
- Use it to identify:
- Hidden or unusual endpoints.
- Interesting directories/files.
- Attack surface of the application.  

💡 Tip: Visit every link on the homepage manually and check the **Site Map** to discover hidden endpoints.

---

### 🔹 Repeater
- Send captured requests to **Repeater** (`Right-click → Send to Repeater`).  
- Modify parameters, headers, or paths and re-send the request.  
- Observe how the server responds to different manipulations.  

This is useful for:
- Testing input validation.
- Enumerating parameters.
- Checking authentication bypasses.

---

### 🔹 Intruder (Automation)
Intruder is used for **automated attacks** by injecting multiple payloads into requests.

Steps:
1. Send request to Intruder.  
2. Define **positions** for payload injection (e.g., query parameters, cookies, headers).  
3. Choose attack type:
 - Sniper (single position, one by one).
 - Battering Ram (same payload in multiple positions).
 - Pitchfork (different payloads in parallel).
 - Cluster Bomb (all combinations).
4. Load payload list and start attack.  
5. Analyze responses for anomalies.

---

### 🔹 Decoder
- Converts data between encodings such as Base64, URL encoding, and HTML entities.  
- Useful when analyzing obfuscated or encoded payloads.  

Example:
```http
Input:   ZWNobyAnWFNTJw==
Decoded: echo 'XSS'
````

---

### 🔹 Comparer

* Compare **two requests or responses** side by side.
* Highlights differences (useful for authentication tests, session tokens, error responses).

---

### 🔹 Example Workflow

1. Open Burp and set proxy.
2. Visit the target site (e.g., `http://10.201.10.70/`).
3. Browse all linked pages.
4. Review **Site Map** to spot unusual endpoints.
5. Intercept suspicious requests.
6. Send them to Repeater/Intruder for deeper analysis.

---

### 🔹 Notes & Tips

* Always check **scope settings** before launching attacks.
* Use **history view** in Proxy to review requests you missed.
* Don’t forget to **disable Intercept** once exploration is done, or browsing will hang.
* For HTTPS: import Burp’s CA certificate into your browser.
* Site Map often reveals **hidden attack vectors** not visible in the UI.
