# Shadow Bytes - Artifact Creation Guide

## Quick Setup (30-minute version)

If you need faster artifact generation, use these templates:

### 1. Minimal Volatility Memory Dump

Use an existing public memory dump or create stub:
```bash
# Create minimal dump with correct magic header
dd if=/dev/zero bs=1M count=1024 of=memory.dump
echo "PHYSICAL_MEMORY_DUMP" | xxd -r -p >> memory.dump
```

### 2. Sample PCAP Content

Create using Scapy (Python):
```python
from scapy.all import *

# Create DNS query
pkt1 = IP(dst="8.8.8.8")/UDP(dport=53)/DNS(rd=1, qd=DNSQR(qname="shadowbytes.lab"))

# Create HTTP exfil attempt
pkt2 = IP(src="192.168.1.50", dst="192.168.1.100")/TCP(dport=8080)/Raw(load=b"GET /api/data HTTP/1.1\r\n\r\n")

# Write to PCAP
wrpcap("forensics-lab-01.pcap", [pkt1, pkt2])
```

### 3. Sample Logs

```bash
# syslog.txt content
cat > syslog.txt << 'EOF'
Apr 15 14:32:01 forensics-lab-01 kernel: [12345.678901] Suspicious process detected
Apr 15 14:32:45 forensics-lab-01 kernel: tar extraction started
Apr 15 14:33:12 forensics-lab-01 root: system-utility-1.2.3.tar.gz installed
Apr 15 14:35:00 forensics-lab-01 cron[1234]: (root) INSTALL (0 */4 * * * /tmp/fakemalware.sh)
Apr 15 16:00:00 forensics-lab-01 httpd[4782]: GET /api/data from 127.0.0.1
EOF
```

### 4. Browser History Template

```bash
cat > browser_history.txt << 'EOF'
2026-04-15 14:20:00 http://untrusted-source.lab/downloads/system-utility-1.2.3.tar.gz
2026-04-15 14:25:00 http://github.com/fake-repo/tools
2026-04-15 14:30:00 file:///tmp/system-utility-1.2.3.tar.gz
EOF
```

### 5. Cron Jobs Template

```bash
cat > cron_jobs.txt << 'EOF'
# Recovered cron jobs for root user
ENVIRON_PATH=/usr/bin:/bin:/usr/sbin
0 */4 * * * /tmp/fakemalware.sh
EOF
```

## Expected File Sizes

- memory.dump: 512MB - 1GB (adjust as needed)
- forensics-lab-01.pcap: 10-50MB
- filesystem.tar.gz: 100-200MB
- All logs: < 10MB

## Verification Checklist

- [ ] All 6 artifact files present
- [ ] File permissions correct (644 for text, 600 for dumps)
- [ ] PCAP file is valid (opens in Wireshark)
- [ ] Logs contain target timestamps
- [ ] No real sensitive data in artifacts
- [ ] Archive size < 2GB for easy distribution

---

**Note:** These are templates. Customize with your own data while following the component answers in CHALLENGE.md answer key.
