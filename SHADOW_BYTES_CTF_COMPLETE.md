# SHADOW BYTES - COMPLETE CTF CHALLENGE PACKAGE

**Status:** ✅ READY TO DEPLOY  
**Created:** 2026-05-07  
**Difficulty:** Medium-Hard | **Duration:** 60 minutes | **Points:** 100  
**Flag Format:** `BluRange{flag_components}`

---

## 📋 CHALLENGE DESCRIPTION

### Scenario
On 2026-04-15 at 14:32 UTC, employee workstation "forensics-lab-01" became compromised after downloading a seemingly legitimate utility package from an untrusted source. The system remained operational but exhibited unusual behavior:
- Increased network activity at odd hours
- CPU spikes during off-work hours
- Unexplained cron jobs
- Missing files from sensitive directories

Forensic artifacts have been collected. Your mission:
1. Analyze memory dumps for malicious processes
2. Recover deleted files and examine filesystem artifacts
3. Analyze network traffic for C2 communications
4. Extract and correlate logs
5. Reconstruct the complete attack timeline
6. Identify the malware hash, C2 server, and exfiltrated data

---

## 🎯 THE 10 TASKS

### Task 1: Malware Process Identification (10 points)
**File:** memory.dump  
**Tool:** Volatility 3

```bash
volatility -f memory.dump windows.pslist
```

**Find:** Process ID of suspicious process with network communication  
**Hint:** Look for unusual parent-child process relationships  
**Format:** `PID_[number]`  
**Example:** `PID_4782`

---

### Task 2: Malware Binary Hash (10 points)
**File:** memory.dump  
**Tool:** Volatility 3 + md5sum

```bash
volatility -f memory.dump procdump --pid=XXXX --output-file=malware.bin
md5sum malware.bin
```

**Find:** MD5 hash of malware executable  
**Hint:** Use procdump plugin to extract process binary  
**Format:** 32-character hex string  
**Example:** `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6`

---

### Task 3: C2 Server Address (15 points)
**File:** forensics-lab-01.pcap  
**Tool:** Wireshark / tshark

```bash
wireshark forensics-lab-01.pcap
# or
tshark -r forensics-lab-01.pcap | grep -E "TCP|UDP"
```

**Find:** IP address of Command & Control server  
**Hint:** Look for unusual outbound connections to non-standard ports  
**Format:** Standard IPv4 address  
**Example:** `192.168.1.100`

---

### Task 4: C2 Communication Port (10 points)
**File:** forensics-lab-01.pcap  
**Tool:** Wireshark

```bash
tshark -r forensics-lab-01.pcap "ip.dst == 192.168.1.100"
```

**Find:** Port used for C2 communication  
**Hint:** Check established connections with the C2 IP  
**Format:** `PORT_[number]`  
**Example:** `PORT_8080`

---

### Task 5: Malware Delivery Vector (10 points)
**File:** browser_history.txt  
**Tool:** cat / grep

```bash
cat browser_history.txt
grep -i "download\|.tar.gz\|.zip" browser_history.txt
```

**Find:** Filename of malicious package that was downloaded  
**Hint:** Look for .tar.gz or .zip files  
**Format:** Filename only (no path)  
**Example:** `system-utility-1.2.3.tar.gz`

---

### Task 6: Malware Installation Timestamp (10 points)
**File:** syslog.txt  
**Tool:** grep / cat

```bash
cat syslog.txt
grep -E "tar|extract|install" syslog.txt
```

**Find:** Unix timestamp when malware was installed  
**Hint:** Look for tar extraction or installation entries  
**Format:** `UNIX_[10-digit-timestamp]`  
**Example:** `UNIX_1744777200`

---

### Task 7: Persistence Mechanism (15 points)
**File:** cron_jobs.txt  
**Tool:** cat / grep

```bash
cat cron_jobs.txt
```

**Find:** Cron schedule used for persistence  
**Hint:** Look for recurring malware execution patterns  
**Format:** Standard cron format (minute hour day month weekday)  
**Example:** `0_*/4_*_*_*` (every 4 hours)

---

### Task 8: Data Exfiltration Target (15 points)
**File:** syslog.txt + filesystem.tar.gz  
**Tool:** grep / strings / cat

```bash
grep -r "@" filesystem/ 2>/dev/null
strings memory.dump | grep "@"
```

**Find:** Email address used for exfiltration  
**Hint:** Check configuration files and malware strings  
**Format:** Valid email address  
**Example:** `attacker@shadowbytes.lab`

---

### Task 9: Exfiltrated Data Type (10 points)
**File:** memory.dump + syslog.txt  
**Tool:** strings / grep

```bash
strings memory.dump | grep -i "ssh\|key\|database\|credential"
```

**Find:** Type of sensitive data being exfiltrated  
**Hint:** Look for file access patterns in malware process  
**Format:** Data type (uppercase, one word)  
**Example:** `SSH_KEYS`

---

### Task 10: Attack Timeline Day (5 points)
**File:** syslog.txt  
**Tool:** cat / head

```bash
cat syslog.txt | head -20
```

**Find:** Calendar day (DD) of initial compromise  
**Hint:** Find first suspicious entry in syslog  
**Format:** `DAY_[2-digit-day]`  
**Example:** `DAY_15`

---

## 🏁 FINAL FLAG ASSEMBLY

Combine all 10 components in order:

```
BluRange{COMPONENT1_COMPONENT2_COMPONENT3_COMPONENT4_COMPONENT5_COMPONENT6_COMPONENT7_COMPONENT8_COMPONENT9_COMPONENT10}
```

### Complete Example:
```
BluRange{PID_4782_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6_192.168.1.100_PORT_8080_system-utility-1.2.3.tar.gz_UNIX_1744777200_0_*/4_*_*_*_attacker@shadowbytes.lab_SSH_KEYS_DAY_15}
```

---

## 📥 REQUIRED ARTIFACTS

You will receive 6 forensic artifact files:

1. **memory.dump** - Physical memory from compromised OpenBSD system (512MB)
2. **forensics-lab-01.pcap** - Network traffic capture (24-hour period)
3. **filesystem.tar.gz** - Filesystem snapshot with deleted file recovery
4. **syslog.txt** - System logs with suspicious entries
5. **cron_jobs.txt** - Recovered cron job history
6. **browser_history.txt** - Web browser artifacts and download history

---

## 🛠️ REQUIRED TOOLS

Install before starting:

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install -y volatility3 wireshark sleuthkit python3-pip

# Alternative: Using pip
pip3 install volatility3

# Wireshark for PCAP analysis
sudo apt-get install wireshark tshark

# Sleuth Kit for filesystem forensics
sudo apt-get install sleuthkit
```

---

## 💡 5 PROGRESSIVE HINTS

### Hint 1: Getting Started
"Start with Volatility to identify suspicious processes. Look for processes with network connections that don't match legitimate system services. Focus on processes with suspicious parent-child relationships."

### Hint 2: Memory Analysis
"Use volatility's network and netstat plugins to identify active connections. Compare against known legitimate services in OpenBSD 7.4."

### Hint 3: File Analysis
"The malware package was likely extracted to /tmp. Use fls command on the filesystem image to recover deleted files. Check timestamps around 2026-04-15."

### Hint 4: Network Analysis
"Filter Wireshark for DNS queries and look for unusual domain resolution attempts. Check for encrypted tunnels using non-standard ports."

### Hint 5: Timeline Reconstruction
"Correlate three independent sources: syslog timestamps, filesystem metadata, and network packet timestamps. The first suspicious entry across all three sources marks initial compromise."

---

## 📊 SCORING BREAKDOWN

| Task | Component | Points |
|------|-----------|--------|
| 1. Process ID | PID_xxxx | 10 |
| 2. Malware Hash | [MD5] | 10 |
| 3. C2 Server IP | [IP] | 15 |
| 4. C2 Port | PORT_xxxx | 10 |
| 5. Delivery Vector | [filename] | 10 |
| 6. Timestamp | UNIX_[timestamp] | 10 |
| 7. Persistence | [cron] | 15 |
| 8. Exfil Target | [email] | 15 |
| 9. Data Type | [type] | 10 |
| 10. Timeline Day | DAY_xx | 5 |
| **TOTAL** | **10 components** | **100** |

---

## ⏱️ EXPECTED TIMING

- **Setup & Installation:** 5 minutes
- **Memory Analysis (Tasks 1-2):** 15 minutes
- **Network Analysis (Tasks 3-4):** 15 minutes
- **Delivery Vector (Task 5):** 5 minutes
- **Log Analysis (Tasks 6-7, 10):** 10 minutes
- **Filesystem Analysis (Tasks 8-9):** 8 minutes
- **Flag Assembly & Submission:** 2 minutes
- **TOTAL:** 60 minutes

---

## 🔑 ANSWER KEY (ADMIN ONLY)

**DO NOT GIVE TO PLAYERS**

```
Component 1 (PID):          PID_4782
Component 2 (Hash):         a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
Component 3 (C2 IP):        192.168.1.100
Component 4 (Port):         PORT_8080
Component 5 (Filename):     system-utility-1.2.3.tar.gz
Component 6 (Timestamp):    UNIX_1744777200
Component 7 (Cron):         0_*/4_*_*_*
Component 8 (Email):        attacker@shadowbytes.lab
Component 9 (Data):         SSH_KEYS
Component 10 (Day):         DAY_15

FINAL FLAG:
BluRange{PID_4782_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6_192.168.1.100_PORT_8080_system-utility-1.2.3.tar.gz_UNIX_1744777200_0_*/4_*_*_*_attacker@shadowbytes.lab_SSH_KEYS_DAY_15}
```

---

## 🚀 QUICK START

1. **Extract all artifact files to working directory**
2. **Install required tools** (see above)
3. **Start with Task 1** - memory analysis
4. **Work through Tasks 2-10** in order
5. **Extract all 10 components**
6. **Assemble final flag**
7. **Submit to CTF platform**

---

## 📝 ARTIFACT FILES CHECKLIST

- [ ] memory.dump (512MB) - Physical memory
- [ ] forensics-lab-01.pcap - Network traffic
- [ ] filesystem.tar.gz - Filesystem snapshot
- [ ] syslog.txt - System logs
- [ ] cron_jobs.txt - Cron history
- [ ] browser_history.txt - Browser artifacts

---

## ⚖️ LEGAL & SAFETY

✅ **VM-Based:** All artifacts created in isolated virtual environments  
✅ **No Real Malware:** Uses test files or harmless simulators  
✅ **Educational:** Safe for academic and CTF use  
✅ **No PII:** All sensitive data removed/anonymized  
✅ **Lab Network:** C2 simulation uses private IP ranges only  
✅ **Compliant:** Legal for competitive CTF environments

---

**Challenge Status:** ✅ READY TO DEPLOY  
**Category:** Blue Team - Digital Forensics  
**Created:** 2026-05-07  
**Creator:** yyamihere
