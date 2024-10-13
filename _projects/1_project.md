---
layout: page
title: Hack The Box Legacy Machine Walkthrough
# description: with background image
img: assets/img/12.jpg
importance: 1
category: work
related_publications: true
---

### Overview   
This walkthrough covers a Network Vulnerability Assessment conducted on the retired Hack The Box (HTB) machine Legacy. Using Nmap and Nessus, I identified critical vulnerabilities, including the notorious EternalBlue (MS17-010) exploit in the unpatched SMB service. This assessment demonstrates the process of identifying and verifying vulnerabilities in a vulnerable Windows XP machine, simulating a real-world enterprise environment.

**Tools Used**  
* **Nmap:** For initial network scanning and service enumeration.
* **Nessus:** For a detailed vulnerability scan, identifying critical security weaknesses.  

**Step 1: Nmap Scan**  
**Initial Scanning**  
To start the assessment, I ran an ```Nmap``` scan to identify open ports and services running on the Legacy machine.

    nmap -sS -sV -T4 -A <Legacy_IP>

---

### Scan Output Summary
The Nmap scan revealed several open ports and identified the services running on them:

 <img src="{{ '/assets/img/13.png' | relative_url }}" alt="alt text" class="img-fluid rounded" style="max-width: 100%; height: auto;">

This initial scan showed that **SMB (Server Message Block)** is running on port 445, which raised concerns about potential vulnerabilities, especially the infamous **EternalBlue (MS17-010)** exploit. The Windows XP operating system itself is known to have multiple unpatched vulnerabilities.

---

### Step 2: Nessus Scan  
**Setting Up Nessus**  
Next, I set up Nessus to run a vulnerability scan on the Legacy machine. I used the Basic Network Scan template, which covers a wide range of vulnerabilities, including:

* SMB vulnerabilities,
* Unpatched software,
* Weak configurations.
* Nessus Scan Results:
Nessus identified several critical vulnerabilities. Here are the most important findings:

<img src="{{ '/assets/img/nessus-findings.png' | relative_url }}" alt="alt text" class="img-fluid rounded" style="max-width: 100%; height: auto;">

The **EternalBlue** vulnerability is particularly noteworthy because it allows attackers to gain complete control of the system remotely. This vulnerability was a major part of the WannaCry ransomware attacks, making it a critical weakness in the network.

---

### Step 3: Manual Verification with Nmap
To manually verify the **EternalBlue** vulnerability detected by Nessus, I used **Nmap**'s scripting engine (```NSE```). This allowed me to run a script that specifically checks for MS17-010 on the target machine.

    nmap --script smb-vuln-ms17-010 -p445 <Legacy_IP>

### Verification Output:
The Nmap script confirmed that the Legacy machine is vulnerable to **MS17-010**:

```yaml
Host script results:
smb-vuln-ms17-010:
VULNERABLE:
Remote Code Execution vulnerability in Microsoft SMBv1
State: VULNERABLE
IDs:  CVE-2017-0143
Risk factor: HIGH
```
This verification step ensured that the Nessus scan result was accurate and that the machine was indeed susceptible to the EternalBlue exploit.

---

### Step 4: Exploitation (Optional)
Although this step is not part of a standard vulnerability assessment, I decided to verify the risk of the EternalBlue vulnerability by attempting to exploit it using Metasploit. This helps demonstrate the potential impact of the vulnerability.

```bash
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set RHOST <Legacy_IP>
exploit
```
Within seconds, I gained remote shell access to the Legacy machine, confirming that this vulnerability could lead to a full system compromise in an enterprise environment.

---

### Step 5: Reporting and Mitigation
**Findings Summary**
The Nessus and Nmap scans revealed several high-risk vulnerabilities on the Legacy machine, with the most critical being EternalBlue (MS17-010). Other notable issues included weak SMB configurations and multiple unpatched Windows XP services.

### Recommendations:
1. **Upgrade Operating System:** Since Windows XP is no longer supported, the organization should upgrade to a modern operating system with ongoing security updates.
2. **Disable SMBv1:** Disabling SMBv1 will mitigate vulnerabilities like EternalBlue.
3. **Apply Security Patches:** Ensure all critical updates are applied to address known vulnerabilities like MS17-010.
4. **Harden Network Configuration:** Disable or secure unnecessary services, such as NetBIOS over TCP/IP, and enforce proper authentication mechanisms for SMB.

---

### Conclusion
This vulnerability assessment of the Hack The Box Legacy machine demonstrated the use of Nmap and Nessus to identify and verify critical vulnerabilities, such as EternalBlue (MS17-010). The findings emphasized the importance of patching and upgrading legacy systems in an enterprise environment to mitigate the risk of attacks like WannaCry and NotPetya.

By following these steps and addressing the vulnerabilities, network administrators can significantly improve their security posture and protect their systems from being compromised.