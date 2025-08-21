## Windows Active Directory Fundamentals 3

### Windows Update

* Microsoft’s service to deliver **security patches, bug fixes, and feature updates**.
* **Patch Tuesday** → 2nd Tuesday of each month.
* Critical patches can be released outside of Patch Tuesday if urgent.
* Quick command:

  ```
  control /name Microsoft.WindowsUpdate
  ```

---

### Windows Security

Central dashboard to manage the security of the system. Main protection areas:

1. **Virus & threat protection**

   * Handles **real-time scanning**, **on-demand scans**, and quarantine of malicious files.
   * Integrated with Microsoft Defender Antivirus.
   * Provides threat history, protection updates, and ransomware protection.

2. **Firewall & network protection**

   * Manages Windows Defender Firewall settings.
   * Can configure different profiles (Domain, Private, Public).
   * Useful for blocking unauthorized inbound/outbound traffic.

3. **App & browser control**

   * Based on **Windows Defender SmartScreen**.
   * Helps prevent malicious downloads, phishing attempts, and potentially dangerous websites.
   * Can enforce exploit protection for apps.

4. **Device security**

   * Shows built-in security features tied to the device’s hardware (e.g., TPM, Secure Boot).
   * Ensures the OS has not been tampered with.
   * Core isolation and memory integrity options are available here.

---

### BitLocker

* Full-disk encryption feature to protect data against theft or unauthorized access.
* Provides strongest protection when paired with **TPM (Trusted Platform Module 1.2 or later)**.
* Prevents data access if device is **lost, stolen, or improperly decommissioned**.
* Without proper decryption key, data remains inaccessible even if the drive is removed.

---

### Volume Shadow Copy Service (VSS)

* Creates **snapshots (restore points)** of system state and files.
* Stores data in **System Volume Information** folder.
* User can:

  * Create a restore point
  * Perform a system restore
  * Configure or delete restore points

⚠️ **Security Note**: Malware/ransomware often deletes shadow copies to block recovery. That’s why offline or cloud backups are critical.
