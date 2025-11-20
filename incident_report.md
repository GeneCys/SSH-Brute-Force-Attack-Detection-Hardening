# Incident Report: SSH Brute-Force Attack Detection and Mitigation

- **Author:** Gene Crittenden
- **Date:** 17 November 2025
- **Lab Environment:** VirtualBox host-only network
- **Target System:** Web Server VM – 192.168.56.101
- **Attacking System:** Kali Linux Attack VM – 192.168.56.105

## 1. Executive Summary

On 17 November 2025, a controlled SSH brute-force attack was simulated against the Web Server VM using Hydra from the Kali Linux Attack VM. The objective was to:
- Evaluate IDS detection using Suricata
- Capture network traffic evidence via Wireshark and tcpdump
- Apply automated mitigation using Fail2Ban
- Harden SSH configuration to prevent unauthorized access  
The exercise successfully demonstrated detection, alerting, and mitigation of SSH brute-force attempts, confirming that layered defenses can prevent unauthorized access and generate actionable SOC alerts.

## 2. Incident details

| Item                   | Details                                                          |
| ---------------------- | ---------------------------------------------------------------- |
| **Incident Type**      | SSH Brute-Force Attempt                                          |
| **Date/Time Detected** | 17 November 2025, ~14:00 local                                   |
| **Detection Method**   | Suricata IDS (fast.log alerts), Wireshark/tcpdump packet capture |
| **Source IP**          | 192.168.56.105 (Kali VM)                                         |
| **Target IP**          | 192.168.56.101 (Web Server VM)                                   |
| **Attack Tool**        | Hydra                                                            |
| **Attack Vector**      | Repeated SSH login attempts using a password list                |
| **Severity Level**     | Medium (controlled environment; repeated attempts detected)      |

## 3. Evidence Collected
### 3.1 Network Traffic Capture

Wireshark Filters Used:
```wireshark
ssh && tcp contains "ssh"
```

3.2 Packet Analysis
```bash
sudo tcpdump -i enp0s3 port 22 -nn
sudo tail -f /var/log/suricata/fast.log
```
Suricata fast.log snippet:
```yaml
14:05:12.112025 [**] LOCAL SSH Brute-Force Attempt [**] [Classification: Attempted Administrator Privilege Gain] [Priority: 1] {TCP} 192.168.56.105:34567 -> 192.168.56.101:22
```

##4. Mitigation Actions

**Fail2Ban Deployment:**
- Configured [sshd] jail with maxretry=3 and bantime=10m.
- Verified automatic banning of malicious IPs.

**SSH Hardening:**
- Disabled root login: PermitRootLogin no
- Enforced strong password policy using PAM
- Limited authentication attempts via SSH config

**Rule Verification**
- Custom Suricata rule local.rules added to alert on brute-force attempts
- Ensured proper YAML configuration and restart of Suricata

## 5. Verification & Validation
- Post-mitigation, repeated brute-force attempts from 192.168.56.105 were blocked immediately by Fail2Ban.
- Suricata continued to log attempts, confirming detection, but blocked packets prevented successful authentication.
- SSH login attempts from the attack VM now failed instantly.
- Reduced packet flow in Wireshark confirmed firewall-level mitigation.

##6. Lessons Learned
- IDS logs attacks even after firewall blocks, emphasizing monitoring vs. mitigation distinction.
- Small configuration mistakes (duplicate Fail2Ban jails, commented headers) can prevent services from functioning.
- Layered security (IDS + logging + automated banning + SSH hardening) is more effective than any single control.
- Packet capture tools are critical for validating detection and mitigation in SOC exercises.

## 7. Recommended Next Steps
- Maintain a regularly updated and tested ruleset in Suricata
- Rotate and monitor Fail2Ban logs for repeated offenders
- Consider implementing SSH key-based authentication only
- Expand logging and alert correlation to integrate with a SIEM platform for full SOC simulation

## 8. References / Supporting Evidence
- ssh_scan.txt – Nmap version scan results
- Wireshark .txt packet captures (original .pcapng available upon request)
- Suricata fast.log saved as suricata_ssh_alerts.log
- Fail2Ban jail status outputs and firewall rule snapshots
- Screenshots of SSH hardening edits and Wireshark filters

---

**End of Incident Report**
