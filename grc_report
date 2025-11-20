# GRC Report: SSH Brute-Force Detection and Mitigation Lab

**Author:** Gene Crittenden  
**Date:** 17 November 2025  
**Lab Environment:** VirtualBox host-only network  
**Target System:** Web Server VM – 192.168.56.101  
**Attacking System:** Kali Linux Attack VM – 192.168.56.105  

---

## **1. Purpose**

This report maps technical actions performed during the SSH Brute-Force Detection and Mitigation Lab to common Governance, Risk, and Compliance (GRC) frameworks. The objective is to demonstrate how hands-on SOC exercises align with organizational controls and risk management requirements.

---

## **2. Lab Controls Mapping**

| Control Framework | Relevant Control               | Lab Action / Evidence                                                                                   | Result / Compliance Demonstration |
|------------------|--------------------------------|--------------------------------------------------------------------------------------------------------|----------------------------------|
| **NIST CSF**      | PR.AC-1 (Access Control)       | Hardened SSH configuration: `PermitRootLogin no`, restricted users, password quality enforcement      | Reduced attack surface; unauthorized logins blocked |
|                  | DE.CM-1 (Monitoring)           | Deployed Suricata IDS, monitored fast.log alerts                                                       | Detected brute-force attempts in real-time |
|                  | DE.AE-2 (Threat Detection)     | Created custom local.rules for SSH brute-force alerts                                                 | IDS triggered alerts for each attack attempt |
|                  | PR.PT-3 (Access Restrictions)  | Implemented Fail2Ban jails to automatically ban IPs after repeated login failures                     | Automated blocking of malicious IPs |
| **ISO 27001**     | A.8.20 (Logging)               | Collected Suricata logs, auth logs, and packet captures                                               | Centralized logging ensures traceability |
|                  | A.8.16 (Monitoring Activities) | Continuous monitoring of SSH traffic using Suricata and tcpdump                                        | SOC-style detection validated |
|                  | A.8.18 (Preventing Misuse)     | Fail2Ban automated banning combined with SSH hardening                                                | Prevented unauthorized access to SSH |
| **MITRE ATT&CK**  | T1110 (Brute Force)            | Simulated brute-force attack using Hydra                                                              | Attack vector replicated for testing defenses |
|                  | D3-FA (Network: IDS)           | Suricata network IDS detected malicious SSH attempts                                                  | Confirmed network-level detection and alerting |

---

## **3. Evidence Collected for Compliance**

- **Nmap Scan Results:** Confirmed SSH port is open and version detected (`ssh_scan.txt`)  
- **Packet Captures:** Wireshark `.pcapng` files showing repeated login attempts  
- **Suricata Logs:** `fast.log` saved as `suricata_ssh_alerts.log`  
- **Fail2Ban Status:** Banned IPs confirmed via `fail2ban-client status sshd`  
- **Firewall Verification:** iptables rules created by Fail2Ban blocking attacker IP  
- **SSH Configuration:** Screenshots / config files demonstrating root login disabled and password policy enforced  

---

## **4. Observations & Compliance Outcomes**

- All simulated attacks were detected by Suricata, generating actionable alerts.  
- Automated mitigation via Fail2Ban successfully blocked the attacking IP after 3 failed login attempts.  
- SSH hardening effectively reduced the attack surface and enforced organizational security best practices.  
- Layered controls (IDS + logging + automated banning + SSH hardening) provide redundancy and verification for compliance alignment.  
- Documentation and captured evidence meet audit requirements for both technical and procedural controls.

---

## **5. Recommendations for Ongoing Compliance**

1. **Maintain and update IDS rulesets** (Suricata ET/Open) to cover emerging attack techniques.  
2. **Monitor Fail2Ban logs regularly** to ensure effectiveness of bans and detect repeated offenders.  
3. **Enforce key-based authentication** and restrict access to authorized users only.  
4. **Integrate IDS and Fail2Ban alerts into a SIEM** for centralized monitoring and compliance reporting.  
5. **Regularly test configurations and mitigations** with controlled attack simulations to validate control effectiveness.

---

**End of GRC Report**
