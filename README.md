# An Empirical Analysis of Brute Force Authentication Attacks and Preventive Controls

> A cybersecurity project demonstrating SSH brute force attacks in a controlled Microsoft Azure environment and evaluating defensive mechanisms such as strong password policies and Fail2Ban.

---

## Overview

This project explores how brute force attacks exploit weak authentication systems and how basic security controls can drastically reduce attack success rates. The experiment was conducted using two Ubuntu virtual machines hosted on Microsoft Azure: one acting as the attacker system and the other as the target victim machine. 

The primary focus areas include:

* Simulating SSH brute force attacks
* Monitoring authentication logs
* Observing attack behavior
* Implementing preventive controls
* Evaluating defense effectiveness

---

## Objectives

* Demonstrate how weak passwords are compromised using automated attack tools
* Analyze SSH authentication logs during brute force attacks
* Implement and test preventive security mechanisms
* Compare attack success before and after defenses

---

## Technologies Used

| Technology          | Purpose                              |
| ------------------- | ------------------------------------ |
| Microsoft Azure     | Cloud infrastructure                 |
| Ubuntu Server 24.04 | Attacker and target virtual machines |
| Hydra               | Brute force attack automation        |
| Fail2Ban            | Intrusion prevention                 |
| SSH                 | Authentication service               |
| Linux Auth Logs     | Attack monitoring and analysis       |

---

## Architecture

### Virtual Network Configuration

| Component       | Configuration |
| --------------- | ------------- |
| Virtual Network | `10.0.0.0/16` |
| Attacker Subnet | `10.0.1.0/24` |
| Target Subnet   | `10.0.2.0/24` |

### Virtual Machines

#### Attacker VM

* Ubuntu Server 24.04
* Public IP enabled
* Used to launch Hydra attacks

#### Target VM

* Ubuntu Server 24.04
* Public IP enabled
* SSH service hosted

Network Security Groups (NSGs) were configured to only allow SSH traffic from authorized IPs including the attacker machine and administrator system. 

---

## Vulnerable Target Configuration

The target machine was intentionally configured with insecure authentication settings:

```bash
Username: vulnvictim
Password: 12345
```

Additional insecure configurations:

* SSH password authentication enabled
* Weak password policy
* Public SSH exposure

This setup replicated common real-world misconfigurations. 

---

## Attack Execution

Hydra was used to perform SSH brute force attacks against the target system.

### Hydra Command

```bash
hydra -l victim -P wordlist.txt 20.244.24.0 ssh -t 4
```

### Attack Behavior Observed

During the attack:

* Continuous failed authentication attempts were logged
* High-frequency SSH requests were detected
* The attacker source IP remained consistent

Example log entry:

```bash
Failed password for victim from 98.70.28.75 port 22 ssh2
```



---

## Preventive Controls

### 1. Strong Password Enforcement

The weak password was replaced with a stronger credential.

#### Result

* Hydra failed to discover valid credentials
* Attack duration increased significantly
* Successful compromise was prevented

---

### 2. Fail2Ban Configuration

Fail2Ban was implemented to automatically block malicious login attempts.

### Configuration

```ini
maxretry = 3
bantime = 600
findtime = 60
```

### Observed Behavior

* Attacker IP was automatically banned
* Subsequent SSH requests were denied
* Authentication attack rate dropped immediately



---

## Experimental Results

| Scenario | Password Strength | Protection | Attempts | Time  | Outcome |
| -------- | ----------------- | ---------- | -------- | ----- | ------- |
| 1        | Weak (`12345`)    | None       | Low      | Fast  | Success |
| 2        | Strong            | None       | High     | Long  | Failure |
| 3        | Weak              | Fail2Ban   | Limited  | Short | Blocked |



---

## Key Findings

The experiment demonstrated several critical cybersecurity insights:

* Weak passwords remain one of the largest authentication vulnerabilities
* Automated tools like Hydra can rapidly compromise poorly secured systems
* Authentication logs provide strong indicators of attack activity
* Basic defensive mechanisms significantly improve security posture
* Fail2Ban effectively mitigates repeated brute force attempts through automated blocking

Even simple defensive controls can drastically improve system resilience. 

---

## Screenshots Included

The project documentation contains screenshots demonstrating:

* Azure virtual network setup
* VM deployment
* SSH connectivity
* Hydra attack execution
* Authentication logs
* Fail2Ban status monitoring
* Banned IP entries
* Failed post-defense attack attempts



---

## Limitations

* The project primarily focused on SSH brute force attacks
* Web-based brute force testing was minimally explored
* Advanced SIEM solutions such as Wazuh or ELK Stack were not implemented



---

## Future Improvements

Potential enhancements include:

* Integration with Wazuh SIEM
* ELK Stack log visualization
* Multi-factor authentication testing
* Distributed brute force simulation
* Web application authentication analysis
* Automated incident response workflows

---

## Conclusion

This project demonstrates that brute force attacks are simple yet highly effective against poorly secured systems. However, it also proves that strong passwords, monitoring, and automated blocking mechanisms can drastically reduce attack success rates.

The study reinforces the importance of:

* Secure authentication practices
* Continuous monitoring
* Log analysis
* Proactive defensive configurations



---

## Author

**Group – 9**

---

## Disclaimer

This project was conducted strictly in a controlled and authorized lab environment for educational and research purposes only. Unauthorized brute force attacks against systems without permission are illegal and unethical.

---

## Project Report: Attached through the pdf

Source document: *An Empirical Analysis of Brute Force Authentication Attacks and Preventive Controls*
