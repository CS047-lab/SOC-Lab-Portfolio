# Sudo Privilege Escalation Detection using Wazuh

## Overview

This project demonstrates how Wazuh can be used to monitor and investigate privilege escalation on a Linux system.

The scenario continues from the previous projects:

- **Project #1:** The attacker gained access to the Ubuntu server through SSH after obtaining valid credentials in the lab environment.
- **Project #2:** After gaining access, the attacker created a new Linux user account to establish persistence.
- **Project #3:** The attacker attempts to elevate privileges using `sudo` to gain full administrative control of the compromised system.

This project focuses on detecting privileged activity and understanding how a SOC analyst would investigate administrative command execution.

---

# Lab Environment

| Component | Technology |
|-----------|------------|
| SIEM | Wazuh |
| Attacker | Kali Linux |
| Victim | Ubuntu 24.04 |
| Monitoring | Wazuh Agent |
| Virtualization | VMware Workstation |

---

# Objective

- Simulate privilege escalation after a successful compromise.
- Investigate privileged command execution.
- Understand how Linux records administrative activity.
- Analyze security events using Wazuh.
- Continue documenting the attack lifecycle from initial access to privilege escalation.

---

# Attack Story

The attack follows a realistic post-compromise sequence.

## Phase 1 – Initial Access

The attacker previously obtained access to the Ubuntu server through SSH using valid credentials in the lab environment.

```bash
ssh attacker@<TARGET_IP>
```

After authentication, the attacker obtained an interactive shell.

---

## Phase 2 – Verify Access

Before performing additional actions, the attacker confirms the current account.

```bash
whoami
```

Example Output

```text
attacker
```

---

## Phase 3 – Privilege Escalation

The attacker attempts to obtain administrative privileges.

```bash
sudo -i
```

or

```bash
sudo su
```

After entering the password:

```bash
whoami
```

Example Output

```text
root
```

The attacker now has unrestricted administrative access.

---

## Phase 4 – Administrative Activity

Once root access is obtained, several privileged commands are executed.

View password hashes:

```bash
cat /etc/shadow
```

Update packages:

```bash
apt update
```

List root directory:

```bash
ls /root
```

Restart SSH service:

```bash
systemctl restart ssh
```

These commands represent actions an attacker might perform after successfully escalating privileges.

---

# Detection

Depending on the logging configuration (such as `sudo` logging and `auditd`), Linux records privileged command execution.

The Wazuh Agent collects these logs and forwards them to the Wazuh Manager, where a SOC analyst can investigate the activity.

Important investigation questions include:

- Who executed the command?
- When was it executed?
- Which command was used?
- Was the activity expected?
- Was the user authorized?

---

# MITRE ATT&CK Mapping

| Technique | ID |
|-----------|----|
| Abuse Elevation Control Mechanism | T1548 |
| Valid Accounts | T1078 |


---

# Attack Workflow

```
Kali Linux
      │
      ▼
SSH Login
      │
      ▼
Interactive Shell
      │
      ▼
Verify Current User
      │
      ▼
sudo -i
      │
      ▼
Root Shell
      │
      ▼
Administrative Commands
      │
      ▼
Linux Authentication Logs
      │
      ▼
Wazuh Agent
      │
      ▼
Wazuh Manager
      │
      ▼
Security Events
      │
      ▼
SOC Investigation
```

---

# Evidence

## Screenshot 1 – SSH Login

<img width="691" height="515" alt="Screenshot 2026-07-14 010414" src="https://github.com/user-attachments/assets/7d3bd256-fdca-445c-9f75-a2343857be4b" />


---

## Screenshot 2 – Current User Verification

<img width="602" height="135" alt="Screenshot 2026-07-14 010438" src="https://github.com/user-attachments/assets/262c8606-8fda-4c6c-90ee-4b5ab5313d5b" />


---

## Screenshot 3 – Executing `sudo -i`

<img width="490" height="63" alt="Screenshot 2026-07-14 011153" src="https://github.com/user-attachments/assets/3750d2db-2d28-4603-8e60-30bc4406f775" />


---

## Screenshot 4 – Root Shell (`whoami`)

<img width="546" height="66" alt="Screenshot 2026-07-14 011216" src="https://github.com/user-attachments/assets/fa6008ed-f187-4914-8a5a-4451ff990725" />

---

## Screenshot 5 – Accessing `/etc/shadow`

<img width="1030" height="701" alt="Screenshot 2026-07-14 011322" src="https://github.com/user-attachments/assets/17dc435b-8787-42b7-b8b1-c577c5c09f7d" />


---

## Screenshot 6 – Wazuh Security Event

<img width="1717" height="768" alt="Screenshot 2026-07-14 011621" src="https://github.com/user-attachments/assets/ab909f10-f1f6-4b50-92af-fce7caf127d9" />
<img width="1715" height="712" alt="Screenshot 2026-07-14 011657" src="https://github.com/user-attachments/assets/e8ff38fd-e6a7-43e4-ab35-434fa59e8278" />


---

## Screenshot 7 – Wazuh Dashboard

<img width="1714" height="760" alt="Screenshot 2026-07-14 011713" src="https://github.com/user-attachments/assets/b40f7f04-b7f2-4ff2-b754-fc80d97f06d8" />


---

# Indicators of Compromise (IOCs)

- Successful SSH authentication
- Execution of `sudo`
- Root shell obtained
- Access to `/etc/shadow`
- Administrative command execution
- Elevated privileges granted

---

# Analysis

After obtaining access to the Ubuntu server, the attacker attempted to elevate privileges using `sudo`.

Following successful authentication, the attacker gained a root shell and executed administrative commands that accessed sensitive resources and modified the system state.

These activities are valuable indicators during a SOC investigation because attackers commonly attempt privilege escalation after gaining initial access. By reviewing authentication logs, `sudo` activity, and system events collected by Wazuh, analysts can identify unauthorized administrative actions and determine the potential impact of the compromise.

---

# Remediation

- Apply the Principle of Least Privilege.
- Restrict `sudo` access to authorized users.
- Enable Linux auditing (`auditd`) for detailed command logging.
- Monitor privileged command execution using Wazuh.
- Review `/var/log/auth.log` and audit logs regularly.
- Enable Multi-Factor Authentication (MFA) for privileged accounts.
- Periodically review membership of the `sudo` group.
- Investigate unexpected root shell activity.

---

# Skills Demonstrated

- Linux Administration
- SSH
- Privilege Escalation Investigation
- Wazuh SIEM
- Log Analysis
- Threat Detection
- MITRE ATT&CK Mapping
- Incident Investigation
- SOC Analysis

---

# Conclusion

This project demonstrates a realistic post-compromise privilege escalation scenario in which an attacker, after gaining access to a Linux server, uses `sudo` to obtain administrative privileges.

By collecting and investigating authentication logs and privileged command execution events, Wazuh provides valuable visibility that enables SOC analysts to detect suspicious administrative activity and respond appropriately.

This project continues the documented attack chain:

- Project #1 – Initial Access (SSH)
- Project #2 – Persistence (New User Creation)
- Project #3 – Privilege Escalation (sudo)

Together, these projects represent the early stages of a realistic attack lifecycle and demonstrate practical SOC investigation skills.

---

# References

- MITRE ATT&CK – T1548 (Abuse Elevation Control Mechanism)
- MITRE ATT&CK – T1078 (Valid Accounts)
- Wazuh Documentation
- Linux `sudo` Documentation
