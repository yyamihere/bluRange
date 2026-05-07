# Blue Team CTF Challenge: Shadow Bytes

## 📚 Challenge Documentation

This directory contains a **Medium-Hard difficulty Digital Forensics CTF challenge** designed for blue team operators.

### 📁 Contents

1. **CHALLENGE.md** - Full challenge description, tasks, and scoring
2. **DEPLOYMENT.md** - CTF admin deployment guide  
3. **ARTIFACT_TEMPLATE.md** - Guide for creating forensic artifacts
4. **FLAGS.txt** - Answer key (admin only, keep secure)
5. **README.md** - This file

### 🎯 Quick Start

**For CTF Admins:**
1. Read DEPLOYMENT.md
2. Generate artifacts using ARTIFACT_TEMPLATE.md
3. Distribute challenge package to players
4. Use FLAGS.txt as answer key (keep private)

**For Players:**
1. Extract `shadow-bytes-ctf.tar.gz`
2. Read CHALLENGE.md for full instructions
3. Install forensic tools (Volatility 3, Wireshark, Sleuth Kit)
4. Perform analysis tasks 1-10
5. Extract all 10 flag components
6. Submit composite flag in format: `BluRange{COMPONENT1_COMPONENT2_...}`

### ⏱️ Time Estimate

- **Setup & Tool Installation:** 5 minutes
- **Analysis Tasks:** 50 minutes
- **Flag Assembly & Submission:** 5 minutes
- **Total:** 60 minutes (hard limit)

### 🔧 Required Tools

- Volatility 3 (memory forensics)
- Wireshark (network analysis)
- Sleuth Kit (filesystem forensics)
- Standard Linux tools (strings, grep, xxd, etc.)

### 📊 Points Breakdown

| Task | Points |
|------|--------|
| Memory Process ID | 10 |
| Malware Hash | 10 |
| C2 Server IP | 15 |
| C2 Port | 10 |
| Delivery Vector | 10 |
| Install Timestamp | 10 |
| Persistence Method | 15 |
| Exfil Target | 15 |
| Exfil Data Type | 10 |
| Timeline Day | 5 |
| **TOTAL** | **100** |

### 🎓 Learning Outcomes

Participants will learn:
- ✅ Physical memory analysis (volatility plugins)
- ✅ Network traffic forensics (PCAP analysis)
- ✅ Filesystem artifact recovery (deleted file recovery)
- ✅ Timeline correlation (cross-source validation)
- ✅ Malware behavior identification
- ✅ Incident response methodology

### ⚖️ Legal & Ethical Notes

✅ **VM-Based Artifacts** - All data from isolated virtual environments  
✅ **No Real Malware** - Uses harmless test files or simulators  
✅ **Educational Purpose** - Safe for academic and CTF use  
✅ **No PII** - All sensitive data removed/anonymized  
✅ **Lab Network** - C2 simulation uses private IP ranges

### 🎯 Difficulty Levels

- **Current:** Medium-Hard (60 minutes)
- **Easy:** First 5 tasks only (30 minutes)
- **Hard:** No hints, obfuscated artifacts (90+ minutes)

### 📝 Version

- **Created:** 2026-05-07
- **Status:** Ready for Deployment ✅
- **CTF:** BluRange
- **Category:** Digital Forensics

---

**Questions?** Refer to CHALLENGE.md or consult CTF admin using FLAGS.txt answer key.
