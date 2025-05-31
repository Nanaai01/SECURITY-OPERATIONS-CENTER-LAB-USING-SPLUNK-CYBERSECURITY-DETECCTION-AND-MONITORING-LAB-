# 🔍 SOC Lab Setup Using Splunk for Monitoring & Detection

This guide walks you through setting up a **Security Operations Center (SOC) lab** for **log monitoring and attack detection** using **Splunk**, **Sysmon**, and simulated attacks from **Kali Linux**. Ideal for cybersecurity learners and professionals, this setup reflects real-world monitoring environments.

---

## 🧰 Lab Components

| Role              | System                          | Tools Installed                                 |
| ----------------- | ------------------------------- | ----------------------------------------------- |
| Attacker          | Kali Linux VM                   | Pre-installed offensive security tools          |
| Victim            | Windows 11 VM                   | Sysmon, Windows Event Logging, Splunk Forwarder |
| Monitoring Server | Host Machine (Windows or Linux) | Splunk Enterprise (Web Interface)               |

---

## ⚙️ Step-by-Step Setup

---

### 1️⃣ Download Required Software

#### 📥 [Kali Linux](https://www.kali.org/get-kali/)

* Download the ISO or VM image
* Install it in **VMware** or **VirtualBox**

#### 📥 [Windows 11 ISO](https://www.microsoft.com/en-us/software-download/windows11)

* Create a new VM for Windows 11 in VMware

#### 📥 [Sysmon (System Monitor)](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

* Tool from Microsoft Sysinternals for advanced logging

#### 📥 [Splunk Universal Forwarder](https://www.splunk.com/en_us/download/universal-forwarder.html)

* For collecting and sending logs from Windows 11 to Splunk

#### 📥 [Splunk Enterprise (Free version)](https://www.splunk.com/en_us/download/splunk-enterprise.html)

* Install on the **host machine** for monitoring and analysis

---

### 2️⃣ Set Up Kali Linux (Attacker)

* Launch Kali Linux VM
* Ensure it has network access to the Windows VM (NAT or Host-only mode)
* Use tools like **Nmap**, **Metasploit**, or **custom scripts** for simulations

---

### 3️⃣ Set Up Windows 11 VM (Victim)

#### ✅ Configure Logging:

* Enable **Windows Event Logging**:
  Go to **Local Security Policy > Advanced Audit Policy Configuration > Audit Policies**, enable:

  * Process Creation
  * Logon Events
  * Object Access
  * PowerShell Logging

#### ✅ Install & Configure Sysmon:

1. Download Sysmon and Sysmon config file (e.g. [SwiftOnSecurity's config](https://github.com/SwiftOnSecurity/sysmon-config))
2. Install via CMD as admin:

   ```
   sysmon -accepteula -i sysmonconfig.xml
   ```

#### ✅ Install Splunk Universal Forwarder:

1. Run installer on the Windows VM
2. During setup:

   * Enter the IP of the **host machine** running Splunk Enterprise
   * Use port `9997` for forwarding
3. Select logs to forward (Windows Event Logs, Sysmon logs)

---

### 4️⃣ Set Up Splunk Enterprise (Host)

1. Install Splunk on the **host machine**
2. Launch Splunk Web (usually at [http://localhost:8000](http://localhost:8000))
3. Create a new **index** (e.g., `winlogs`)
4. Go to **Settings > Data Inputs > Forwarded Data**
5. Enable receiving on port `9997`

---

### 5️⃣ Validate Log Forwarding

* From Splunk Web, search:

  ```
  index=winlogs sourcetype=WinEventLog*
  ```
* Confirm you're receiving logs from Windows 11
* You should see **Sysmon**, **Security**, and **System** events

---

### 6️⃣ Simulate Attacks from Kali Linux

Perform controlled attacks such as:

* **Nmap scans**
* **Brute force login attempts**
* **Reverse shells** (for behavioral detection)
* **Suspicious PowerShell commands**

---

### 7️⃣ Detect & Monitor in Splunk

Use **Splunk SPL (Search Processing Language)** to query logs:

```spl
index=winlogs EventCode=4688
```

> See process creation events (Sysmon & Windows Security Logs)

```spl
index=winlogs CommandLine=*powershell*
```

> Monitor suspicious PowerShell usage

Create **dashboards** and **alerts** based on malicious behavior patterns.

---

## ✅ Final Outcome

You now have a functioning **SOC lab** that mimics a real-world blue team setup:

* Detects simulated attacks
* Visualizes suspicious behaviors
* Trains you on log analysis and threat hunting

---

## 💡 Why This Lab Stands Out

* Uses **real tools** from professional SOC environments
* Built on **hands-on attacker-victim interaction**
* Supports learning of **SIEM**, **Sysmon**, and **threat detection**
* Fully customizable for red team and blue team scenarios

---

Let me know if you want me to generate a custom Splunk dashboard, detection rules, or a visual architecture diagram to include in your repo!

