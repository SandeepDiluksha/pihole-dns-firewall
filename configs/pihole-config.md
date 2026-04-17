# Pi-hole Server Configuration Notes

## Server Details
- Provider: Oracle Cloud Free Tier
- OS: Ubuntu 22.04 LTS
- Shape: VM.Standard.E2.1.Micro
- Public IP: 140.245.224.139
- Region: India South

## Day 1 — Completed
- VM instance created and provisioned
- SSH key configured
- System updated
- Hostname set to pihole-server

## Day 3 — Completed
- Added 4 custom blocklists
- Gravity updated — 90506 (88747 unique domains)
- Verified blocking working: doubleclick.net returns 0.0.0.0
- Verified legitimate domains still resolve correctly
