SHADOW BYTES - FORENSIC ARTIFACTS
==================================

This directory contains all forensic artifacts for the Shadow Bytes CTF challenge.

ARTIFACT FILES:
1. memory.dump - Physical memory from compromised OpenBSD system (512MB placeholder)
2. forensics-lab-01.pcap - Network traffic capture (24-hour period)
3. filesystem.tar.gz - Filesystem snapshot with deleted file recovery
4. syslog.txt - System logs with suspicious entries
5. cron_jobs.txt - Recovered cron job history
6. browser_history.txt - Web browser artifacts

To use these artifacts:
1. Extract all files to a working directory
2. Players analyze them using Volatility, Wireshark, and Sleuth Kit
3. Extract 10 flag components from artifacts
4. Submit composite flag

See CHALLENGE.md for complete instructions.
