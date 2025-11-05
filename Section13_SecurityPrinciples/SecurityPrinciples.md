
# Security Principles

## 🧩 Introduction

Security has become a buzzword; every company wants to claim its product or service is secure. But is it?

Before we start discussing the different security principles, it is vital to know the adversary against whom we are protecting our assets. Are you trying to stop a toddler from accessing your laptop? Or are you trying to protect a laptop that contains technical designs worth millions of dollars?

> Using the exact protection mechanisms against toddlers and industrial espionage actors would be ludicrous.  
> Consequently, knowing our adversary is a must so we can learn about their attacks and start implementing appropriate security controls.

It is impossible to achieve perfect security; no solution is 100% secure. Therefore, we try to improve our **security posture** to make it more difficult for adversaries to gain access.

### 🎯 Objective
This room aims to:

- Explain the security functions: **Confidentiality, Integrity and Availability (CIA)**.
- Present the opposite of CIA: **Disclosure, Alteration, and Destruction/Denial (DAD)**.
- Introduce the **fundamental concepts of security models**, such as the Bell-LaPadula model.
- Explain **security principles** such as Defence-in-Depth, Zero Trust, and Trust but Verify.
- Introduce **ISO/IEC 19249**.
- Explain the difference between **Vulnerability, Threat, and Risk**.

---

## 🔐 CIA: Confidentiality, Integrity, Availability

Before describing something as secure, we must understand what makes up security - the **CIA Triad**.

| Security Function | Description |
|-------------------|-------------|
| **Confidentiality** | Ensures that only the intended persons or recipients can access the data. |
| **Integrity** | Ensures that the data cannot be altered without detection. |
| **Availability** | Ensures that systems and data are available when needed. |

### 💡 Examples

#### 🛒 Online Shopping
- **Confidentiality**: Only the payment processor should see your credit card details.  
- **Integrity**: No one should alter your shipping address after checkout.  
- **Availability**: The store’s website/app should be up when you want to place your order.

#### 🏥 Patient Records
- **Confidentiality**: Medical records must remain private by law.  
- **Integrity**: Altered patient data can lead to incorrect treatment.  
- **Availability**: The system must be accessible to medical staff during consultations.

> Not all three CIA components carry equal weight in every situation.  
> For instance, a public university announcement prioritizes **integrity** over **confidentiality**.

---

## 🔒 Beyond CIA

Two additional concepts extend the CIA triad:

| Concept | Description |
|----------|--------------|
| **Authenticity** | Ensures data or sources are genuine, not counterfeit. |
| **Nonrepudiation** | Prevents an entity from denying its actions or communications. |

Example:  
A company receiving an order for 1000 cars must confirm the order’s authenticity and ensure the sender cannot deny placing it.

---

## 🧮 Parkerian Hexad (1998)

Proposed by Donn Parker, it includes six security elements:

1. Availability  
2. Utility  
3. Integrity  
4. Authenticity  
5. Confidentiality  
6. Possession  

**Additional Elements:**

- **Utility:** Information must be *useful* and usable. (e.g., encrypted files without the decryption key = useless)  
- **Possession:** Protect data from unauthorized taking or control (e.g., stolen backup drives or ransomware encryption).

---

## 💀 DAD: Disclosure, Alteration, Destruction/Denial

The **DAD Triad** represents the **opposite** of CIA:

| DAD Function | Opposite of | Description |
|---------------|--------------|--------------|
| **Disclosure** | Confidentiality | Unauthorized exposure of data |
| **Alteration** | Integrity | Unauthorized modification of data |
| **Destruction/Denial** | Availability | Data loss or service disruption |

### Example: Patient Records
- **Disclosure**: Leaked patient records online → loss of confidentiality.  
- **Alteration**: Modified patient data → incorrect diagnosis or treatment.  
- **Destruction/Denial**: Database unavailable → hospital operations stall.

> Protecting CIA must balance all three; too much focus on one can weaken the others.

---

## 🧱 Fundamental Security Models

### 1️⃣ Bell-LaPadula Model
Focus: **Confidentiality**

- **Simple Security Property (“no read up”)**: Lower-level subjects cannot read higher-level data.  
- **Star Property (“no write down”)**: Higher-level subjects cannot write to lower-level objects.  
- **Discretionary Security Property**: Access controlled via an access matrix.

> Summary: **Write up, Read down.**

### 2️⃣ Biba Model
Focus: **Integrity**

- **Simple Integrity Property (“no read down”)**  
- **Star Integrity Property (“no write up”)**

> Summary: **Read up, Write down.**

### 3️⃣ Clark-Wilson Model
Focus: **Integrity via Controlled Transactions**

- **CDI**: Constrained Data Item (data whose integrity must be preserved).  
- **UDI**: Unconstrained Data Item (e.g., user input).  
- **TPs**: Transformation Procedures (operations that maintain integrity).  
- **IVPs**: Integrity Verification Procedures (validate integrity).

Other models: Brewer-Nash, Goguen-Meseguer, Sutherland, Graham-Denning, Harrison-Ruzzo-Ullman.

---

## 🛡️ Defence-in-Depth

Also called **Multi-Level Security**, it emphasizes layered protection.

Example:  
Locked drawer → locked room → locked apartment → locked building → surveillance cameras.

Each layer slows attackers and adds resilience even if one control fails.

---

## 🌍 ISO/IEC 19249

**Standard:** *ISO/IEC 19249:2017 – Catalogue of architectural and design principles for secure products, systems and applications.*

### 🧩 Architectural Principles
1. **Domain Separation**
2. **Layering**
3. **Encapsulation**
4. **Redundancy**
5. **Virtualization**

### ⚙️ Design Principles
| # | Principle | Description |
|---|------------|-------------|
| 1 | **Least Privilege** | Give only required access ("need-to-know" basis). |
| 2 | **Attack Surface Minimisation** | Disable unnecessary services and features. |
| 3 | **Centralized Parameter Validation** | Validate user/system inputs in one secure place. |
| 4 | **Centralized Security Services** | Use centralized authentication/security control. |
| 5 | **Error & Exception Handling** | Design systems to fail safe without data leakage. |

---

## 🔁 Zero Trust vs Trust but Verify

### 🧭 Trust but Verify
- Always verify trusted entities through **logging and monitoring**.  
- Practical, but limited by human and resource capacity.

### 🚫 Zero Trust
- Treat **trust as a vulnerability**: “Never trust, always verify.”  
- Requires authentication and authorization for every access.  
- Uses **microsegmentation** (network split into smallest units).  
- Ideal for containing breaches and minimizing insider threats.

---

## ⚠️ Threat vs Risk

| Term | Definition |
|------|-------------|
| **Vulnerability** | A weakness or flaw in a system. |
| **Threat** | Potential danger that exploits a vulnerability. |
| **Risk** | The likelihood and impact of a threat exploiting a vulnerability. |

Example:
- A car showroom with glass walls → vulnerability (glass).  
- Potential for breakage → threat.  
- Probability and business loss from breakage → risk.

In IT:
- Database system has known vulnerability → **threat becomes real** when exploit code is released → organization must **assess risk** and act.

---

## 🧾 Conclusion

You should now understand:
- **CIA & DAD Triads**
- **Authenticity and Nonrepudiation**
- **Security Models (Bell-LaPadula, Biba, Clark-Wilson)**
- **ISO/IEC 19249 Principles**
- **Defence-in-Depth, Zero Trust, Trust but Verify**
- **Vulnerability, Threat, and Risk**

Finally, consider the **Shared Responsibility Model** in cloud security:

| Cloud Service Model | Customer Responsibility | Provider Responsibility |
|---------------------|--------------------------|--------------------------|
| **IaaS** | OS, applications, and data | Infrastructure |
| **SaaS** | Data and usage | OS and infrastructure |

Security in the cloud is a **shared duty**-both provider and user must play their part.

