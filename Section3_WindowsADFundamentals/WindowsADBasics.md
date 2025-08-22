
## Windows AD Basics

### 1. **Windows Domains**

* Small networks = manual management works.
* Large orgs = need **centralized administration** → that’s where **Windows Domains + AD** come in.
* **Domain Controller (DC):** server that runs Active Directory services, managing authentication, identity, and policy enforcement.

✅ Advantages:

* **Centralized Identity Management** → one place to manage users, groups, devices.
* **Security Policies** → enforce org-wide via AD + GPOs.

---

### 2. **Active Directory (AD DS) Objects**

* **Users (security principals)**

  * Represent *people* (employees) or *services* (IIS, SQL).
  * Can be authenticated + assigned permissions.

* **Machines (computer accounts)**

  * Each joined machine = object in AD, with its own account (`DC01$`).
  * Passwords = auto-rotated, 120-char random strings.

* **Security Groups**

  * Collection of users/machines for *resource access control*.
  * Groups can nest other groups.
  * Default groups exist (Admins, Users, etc).

---

### 3. **Organizational Units (OUs) vs Security Groups**

* **OUs** → for applying policies (via GPOs). A user = *only in 1 OU*.
* **Groups** → for assigning permissions to resources. A user = *can be in multiple groups*.

🔑 Rule:

* **OUs = policy management**
* **Groups = resource permissions**

---

### 4. **Managing Users & Delegation**

* Default containers: `Builtin`, `Computers`, `Domain Controllers`, `Users`, etc.
* Delegation → grant limited admin control (e.g., IT support can reset passwords).

---

### 5. **Managing Computers**

* By default → machines land in `Computers` container.
* Best practice → separate into:

  1. **Workstations** (used by employees, no privileged accounts)
  2. **Servers** (provide services)
  3. **Domain Controllers** (most sensitive, contain all password hashes)

---

### 6. **Group Policy Objects (GPOs)**

* Used to **apply security baselines & settings**.
* Linked to OUs → affects users/computers inside.
* Distributed via **SYSVOL share** on DC.
* Force update:

  ```powershell
  gpupdate /force
  ```

---

### 7. **Authentication Protocols**

* **Kerberos** (default in modern Windows) → secure, uses **Ticket Granting Ticket (TGT)** → requests more TGS tickets.
* **NTLM / NetNTLM** → legacy, weaker, still around for compatibility.

---

### 8. **Trees, Forests & Trusts**

* **Tree** → multiple domains with same namespace (`uk.thm.local`, `us.thm.local`).
* **Forest** → multiple trees with different namespaces (`thm.local`, `mht.local`).
* **Trusts** → allow cross-domain resource access.

  * *One-way trust* → A trusts B, so B’s users can access A’s resources.
  * *Two-way trust* → mutual trust, default within forests.
