# Shadow Bytes - CTF Deployment Guide

## ⚡ Quick Deployment (For CTF Admin)

### Step 1: Download Challenge Files
```bash
git clone https://github.com/yyamihere/bluRange.git
cd bluRange/challenges/digital-forensics
```

### Step 2: Prepare Artifacts
```bash
# Option A: Use provided artifact pack
tar -xzf shadow-bytes-artifacts.tar.gz

# Option B: Generate new artifacts (see ARTIFACT_TEMPLATE.md)
bash generate_artifacts.sh
```

### Step 3: Verify Artifacts
```bash
# Check all required files
ls -lh forensics-lab-01.* memory.dump filesystem.tar.gz syslog.txt cron_jobs.txt browser_history.txt

# Verify PCAP integrity
file forensics-lab-01.pcap

# Verify tarball
tar -tzf filesystem.tar.gz | head -20
```

### Step 4: Create Distribution Package
```bash
mkdir shadow-bytes-ctf
cp CHALLENGE.md shadow-bytes-ctf/README.md
cp forensics-lab-01.pcap shadow-bytes-ctf/
cp memory.dump shadow-bytes-ctf/
cp filesystem.tar.gz shadow-bytes-ctf/
cp syslog.txt shadow-bytes-ctf/
cp cron_jobs.txt shadow-bytes-ctf/
cp browser_history.txt shadow-bytes-ctf/

tar -czf shadow-bytes-ctf.tar.gz shadow-bytes-ctf/
```

### Step 5: Distribute to Players
```bash
# Via CTF platform file upload
# Via HTTP download link
# Via USB (for offline CTF)

# Verify checksum
sha256sum shadow-bytes-ctf.tar.gz > shadow-bytes-ctf.tar.gz.sha256
```

---

## 📋 Pre-Game Checklist

### 48 Hours Before CTF
- [ ] Generate/verify all artifacts
- [ ] Test PCAP in Wireshark
- [ ] Test memory dump with Volatility 3
- [ ] Verify all flag components are extractable
- [ ] Create answer key document (secure storage)
- [ ] Prepare hints (if dynamic release planned)

### 24 Hours Before CTF
- [ ] Create distribution package
- [ ] Upload to CTF platform
- [ ] Test download links
- [ ] Verify file integrity (checksums)
- [ ] Brief judges/admins on answer key

### During CTF
- [ ] Monitor for technical support questions
- [ ] Have backup artifacts ready
- [ ] Track submissions in real-time
- [ ] Release hints on schedule (if applicable)

---

## 🔧 Troubleshooting

### Issue: PCAP file too large
**Solution:** Filter capture with tcpdump during generation
```bash
tcpdump -r forensics-lab-01.pcap -w small.pcap "tcp port 8080 or dns"
```

### Issue: Memory dump not recognized by Volatility
**Solution:** Verify dump format
```bash
file memory.dump
# Should output: "data" or "Linux x86 executable"
```

### Issue: Filesystem archive corrupted
**Solution:** Recreate with verbose output
```bash
tar -czvf filesystem.tar.gz filesystem/
tar -tzvf filesystem.tar.gz | tail -20  # Verify
```

---

## 🎯 Customization Options

### Difficulty Scaling

**Easy Mode:** Reduce components to 5, increase hint detail  
**Medium Mode:** Current configuration  
**Hard Mode:** Remove hints, add encrypted logs, use obfuscated malware

### Time Adjustment

**30 minutes:** Remove Tasks 6-10, focus on first 5  
**60 minutes:** Full challenge (recommended)  
**90 minutes:** Add additional artifact (registry hive, swap file)

### Domain Customization

Replace `shadowbytes.lab` with your CTF domain:
```bash
sed -i 's/shadowbytes\.lab/yourdomain\.ctf/g' CHALLENGE.md
sed -i 's/attacker@shadowbytes/attacker@yourdomain/g' syslog.txt
```

---

## 📊 Expected Results

### Typical Completion Times
- **Top Teams:** 30-40 minutes
- **Advanced Teams:** 40-50 minutes
- **Intermediate Teams:** 50-60 minutes
- **Beginner Teams:** Partial completion

### Expected Points Distribution
- **Full Flag:** 100 points
- **8/10 Components:** 75-80 points
- **6/10 Components:** 50-60 points
- **Partial Forensics:** 20-40 points

---

## 🔒 Security Notes

✅ **Data Sanitization:** All artifacts use dummy/lab data  
✅ **No Real Malware:** Only test files or harmless simulators  
✅ **Network Isolation:** C2 IPs are private lab ranges  
✅ **Legal Compliance:** Safe for educational CTF use  
✅ **PII Removal:** No personal data in artifacts  

---

## 📞 Support Resources

- **Volatility Documentation:** https://volatility3.readthedocs.io/
- **Wireshark Guide:** https://www.wireshark.org/docs/
- **Sleuth Kit Manual:** https://wiki.sleuthkit.org/index.php/TSK_Tool_Overview
- **OpenBSD Forensics:** https://www.openbsd.org/

---

**Last Updated:** 2026-05-07  
**Challenge Status:** Ready for Deployment ✅
