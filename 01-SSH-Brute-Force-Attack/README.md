# SSH Brute Force Detection using Wazuh

## Overview

This project demonstrates how Wazuh detects an SSH brute-force attack in a home SOC lab.

The attack was launched from a Kali Linux machine against an Ubuntu server monitored by the Wazuh agent.

---

## Objective

Simulate an SSH brute-force attack and analyze the resulting security alerts.

---

## Lab Environment

| Component | Technology |
|-----------|------------|
| Attacker | Kali Linux |
| Victim | Ubuntu |
| SIEM | Wazuh |
| Attack Tool | Hydra |

---

## Attack Simulation

### Command Used

```bash
hydra -l testuser -P rockyou.txt ssh://<TARGET-IP>
```

Replace `<TARGET-IP>` with your Ubuntu IP address before publishing.

---

## Detection

Wazuh detected repeated failed SSH authentication attempts and generated security alerts.

---

## MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Brute Force | T1110 |

---

## Evidence


### 1. Hydra Attack

<img width="1683" height="305" alt="Screenshot 2026-07-07 083927" src="https://github.com/user-attachments/assets/3bf9bf05-3075-4a17-ac1c-a7b748befee6" />


### 2. Wazuh Alert

<img width="1696" height="696" alt="Screenshot 2026-07-07 084805" src="https://github.com/user-attachments/assets/15cb5743-0280-4071-8e23-0dccb7583f65" />


### 3. Authentication Logs

<img width="1559" height="824" alt="Screenshot 2026-07-07 084655" src="https://github.com/user-attachments/assets/4a53bc91-39dc-44f7-ae40-a662fc13edff" />


### 4. Wazuh Dashboard

<img width="1718" height="444" alt="Screenshot 2026-07-07 084038" src="https://github.com/user-attachments/assets/fa5ed5ee-d503-4971-ab6c-ffd08c5da9b6" />


---

## Analysis

The attack generated multiple failed SSH authentication events. Wazuh collected these logs from the Ubuntu system, correlated the repeated failures, and generated alerts that can be investigated by a SOC analyst.

---

## Remediation

- Enable SSH key authentication
- Disable root login
- Configure Fail2Ban
- Enforce strong password policies
- Enable Multi-Factor Authentication (MFA)

---

## Skills Demonstrated

- Linux
- Wazuh
- Hydra
- SSH
- Log Analysis
- Threat Detection
- MITRE ATT&CK
