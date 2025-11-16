# session\_lab\_1\_balanced

{% hint style="warning" %}
**Important**: If you are unable to finish the lab in the ProLUG lab environment, please **reboot** the machine from the command line so that other students will have the intended environment.
{% endhint %}

***

## Lab Overview

This lab builds foundational skills in system security reconnaissance and compliance-based hardening. You'll learn to systematically assess Linux systems against Defense Information Systems Agency (DISA) Security Technical Implementation Guides (STIGs) — the same configuration standards used to secure DoD systems, financial infrastructure, and critical enterprise databases.

**What You'll Accomplish**:

* System reconnaissance to discover security-relevant configurations
* STIG-based hardening implementation for database services
* Professional security assessment and documentation workflows

***

## Required Materials

{% tabs %}
{% tab title="🛠️ Tools" %}
**SSH Client**:

* PuTTY (Windows) or other connection tool
* OpenSSH (Linux/macOS)

**Lab Environment**:

* ProLUG Lab Server (Hammer server)
* Root or sudo command access

**Security Assessment**:

* STIG Viewer 2.18
* Download: https://public.cyber.mil/stigs/downloads/

**STIG Checklist**:

* MariaDB Enterprise 10.x STIG (import into STIG Viewer)
{% endtab %}

{% tab title="📚 Prerequisites" %}
**Required Knowledge**:

* Basic command line navigation (`cd`, `ls`, `pwd`)
* Text filtering with `grep` and pipes
* Package management with `dnf`/`yum`
* SystemD service control (`systemctl`)
* Basic SQL database concepts

**Required Skills**:

* SSH connection establishment
* Privilege escalation with `sudo`
* Configuration file editing (`vim`/`nano`)
* Command output analysis and interpretation
{% endtab %}
{% endtabs %}

***

## Module 1: Exploring System Information

{% hint style="info" %}
**Why System Reconnaissance Matters**

Before hardening a system, you must understand its current security posture. This module teaches reconnaissance techniques used by security auditors and systems engineers to discover security-relevant configurations.

**Critical Insight**: Security isn't just about adding controls—it's about understanding what controls already exist and how they interact.
{% endhint %}

### Exercise 1.1: Familiarizing Ourselves with the System

**Objective**: Discover how mounted filesystems restrict dangerous operations.

Run the following commands:

{% code overflow="wrap" %}
```bash
# Find filesystems mounted with noexec (prevents code execution)
mount | grep -i noexec

# Find filesystems mounted with nodev (prevents device file interpretation)
mount | grep -i nodev

# Find filesystems mounted with nosuid (prevents SUID bit escalation)
mount | grep -i nosuid
```
{% endcode %}

**Analysis Question**: Approximately how many of your mounted filesystems have each of these values?

<details>

<summary><strong>Understanding Mount Security Options</strong></summary>

These mount options provide defense-in-depth layers that persist regardless of application-level permissions:

**Security Impact**:

* **`noexec`**: Prevents executing binaries or scripts on the filesystem
  * Critical for `/tmp`, `/var/tmp`, `/home`
  * Blocks attackers from running malicious code uploaded to these locations
* **`nodev`**: Prevents creating or interpreting device nodes
  * Important for all non-root filesystems
  * Blocks privilege escalation via device file manipulation
* **`nosuid`**: Prevents SUID bit privilege escalation
  * Essential for user-writable filesystems
  * Stops attackers from placing SUID binaries to gain elevated privileges

**Real-World Application**: If `/tmp` or `/var/tmp` lack these options, they represent privilege escalation vectors. Attackers upload malicious code to world-writable temp directories and execute it.

</details>

***

### Exercise 1.2: Checking Mounted Systems

**Objective**: Understand kernel-level network security controls and their configuration.

**Part A: IPv4 and IPv6 Parameters**

{% code overflow="wrap" %}
```bash
# List all IPv4 kernel parameters
sysctl -a | grep -i ipv4

# List all IPv6 kernel parameters
sysctl -a | grep -i ipv6
```
{% endcode %}

**Analysis Questions**:

1. How many of each are there?
2. What categories of network controls do you see? (forwarding, ICMP, TCP behavior, etc.)

**Part B: IP Forwarding Analysis**

{% code overflow="wrap" %}
```bash
# Check if IPv4 forwarding is enabled on interfaces
sysctl -a | grep -i ipv4 | grep -i forward
```
{% endcode %}

**Critical Question**: Does IPv4 forward on interfaces?

<details>

<summary><strong>Understanding IP Forwarding Security</strong></summary>

| Forwarding State          | Meaning                   | Security Impact                                                                        |
| ------------------------- | ------------------------- | -------------------------------------------------------------------------------------- |
| `net.ipv4.ip_forward = 0` | System is an **endpoint** | Cannot route traffic between networks (secure default for workstations)                |
| `net.ipv4.ip_forward = 1` | System is a **router**    | Can forward packets between interfaces (required for gateways, dangerous on endpoints) |

**Real-World Scenario**: If an attacker compromises a system with IP forwarding enabled, they can pivot through it to reach other networks. Disabling forwarding on non-router systems is defense-in-depth.

</details>

**Part C: Netfilter Kernel Module Verification**

{% code overflow="wrap" %}
```bash
# Check what firewall-related kernel modules are loaded
lsmod | grep -i tables
```
{% endcode %}

**Analysis Questions**:

1. What type of tables exist?
2. What types of "tables" modules do you see? (`iptables`, `ip6tables`, `ebtables`, `arptables`)

<details>

<summary><strong>Why Netfilter Operates at Kernel Level</strong></summary>

**Architectural Understanding**: Netfilter operates at the kernel level because it must inspect every packet entering or leaving the system. User-space tools like `firewall-cmd` are just interfaces that configure these kernel modules.

**Module Types**:

* `iptables`: IPv4 firewall
* `ip6tables`: IPv6 firewall
* `ebtables`: Ethernet bridge firewall
* `arptables`: ARP-level firewall

</details>

***

## Module 2: PreLAB Preparation

{% hint style="info" %}
**What are STIGs?**

Security Technical Implementation Guides (STIGs) are configuration standards published by the Defense Information Systems Agency (DISA). They provide security requirements for specific technologies and are used across DoD, government agencies, and regulated industries.

STIGs represent industry best practices for security hardening based on threat intelligence and compliance requirements.
{% endhint %}

{% stepper %}
{% step %}
### Download STIG Viewer

Download STIG Viewer 2.18 from: https://public.cyber.mil/stigs/downloads/
{% endstep %}

{% step %}
### Import MariaDB STIG

Download the MariaDB STIG and import it into your STIG viewer.
{% endstep %}

{% step %}
### Familiarize with STIG Viewer

Familiarize yourself with the STIG Viewer interface and how to navigate findings.
{% endstep %}
{% endstepper %}

***

## Module 3: Lab - STIG-Based Database Hardening

{% hint style="success" %}
**Lab Objective**

Practice securing a Linux database service against configuration standards using STIG Viewer 2.18. You'll evaluate security findings, classify control types, and implement remediation measures.

This is the methodology used by security engineers in production environments to maintain compliance posture.
{% endhint %}

### MariaDB Service Configuration

**Step 1: Install and Verify MariaDB**

Connect to a hammer server and install MariaDB:

{% code overflow="wrap" %}
```bash
dnf install mariadb-server

# Ensure that it is running
systemctl start mariadb
systemctl status mariadb
ss -ntulp | grep 3306
```
{% endcode %}

{% hint style="info" %}
**Verification Check**: You should see MariaDB listening on port 3306. If the service fails to start, check logs with `journalctl -xeu mariadb`.
{% endhint %}

***

### STIG Finding Evaluation Framework

For each STIG finding below, systematically evaluate using this four-question framework:

1. **What is the problem?** (Identify the security gap)
2. **What is the fix?** (Determine the remediation action)
3. **What type of control is being implemented?** (Classify: Technical/Operational/Management; Preventive/Detective/Corrective)
4. **Is it set properly on your system?** (Verify current state)

***

### Check and Remediate: V-253666 STIG

**Finding**: Database user connection limits

**Initial Assessment**:

* What is the problem?
* What is the fix?
* What type of control is being implemented?
* Is it set properly on your system?

**Verification Steps**:

Connect to MariaDB locally:

{% code overflow="wrap" %}
```bash
mysql
```
{% endcode %}

Run the SQL command from the STIG's Fix Text section:

{% code overflow="wrap" %}
```sql
SELECT user, max_user_connections FROM mysql.user;
```
{% endcode %}

**Remediation Question**: Can you remediate this finding based on the STIG guidance?

<details>

<summary><strong>Understanding Connection Limit Security</strong></summary>

**Attack Scenario**: Without connection limits, an attacker can open unlimited database connections, each consuming:

* Memory (connection buffers)
* CPU (connection handling)
* File descriptors (network sockets)

This leads to resource exhaustion and denial of service.

**Defense**: Implement per-user connection limits to prevent resource exhaustion:

```sql
SET max_user_connections = 50;  -- Adjust based on legitimate usage patterns
```

**Enterprise Pattern**:

* Web Applications: 10-50 connections per app user
* Batch Jobs: 5-10 connections per job scheduler
* Admin Accounts: 3-5 connections for interactive use
* Service Accounts: Defined based on workload requirements

</details>

***

### Check and Remediate: V-253677 STIG

**Finding**: (Consult STIG Viewer for details)

**Assessment Questions**:

1. What is the problem?
2. What is the fix?
3. What type of control is being implemented?
4. Is it set properly on your system?

**Implementation**: Follow the remediation steps in the STIG's Fix Text section.

***

### Check and Remediate: V-253678 STIG

**Finding**: (Consult STIG Viewer for details)

**Assessment Questions**:

1. What is the problem?
2. What is the fix?
3. What type of control is being implemented?
4. Is it set properly on your system?

**Implementation**: Follow the remediation steps in the STIG's Fix Text section.

***

### Check and Remediate: V-253734 STIG

**Finding**: (Consult STIG Viewer for details)

**Assessment Questions**:

1. What is the problem?
2. What is the fix?
3. What type of control is being implemented?
4. Is it set properly on your system?

**Implementation**: Follow the remediation steps in the STIG's Fix Text section.

***

## Lab Completion

{% hint style="success" %}
**What You've Accomplished**

You've performed professional security engineering work:

* ✅ System reconnaissance to discover security configurations
* ✅ STIG-based assessment using industry-standard tools
* ✅ Security control classification (Technical/Operational; Preventive/Detective/Corrective)
* ✅ Database hardening implementation with verification
* ✅ Professional compliance documentation practices

These are the exact workflows used in government and enterprise security operations.
{% endhint %}

{% hint style="warning" %}
**Before Leaving**: Be sure to **reboot** the lab machine from the command line when you are done.

```bash
sudo reboot
```

This ensures other students have a clean environment for their lab work.
{% endhint %}

***

## Professional Skills Developed

**Technical Competencies**:

* System security reconnaissance and configuration discovery
* STIG-based compliance assessment and remediation
* Database security hardening and access control
* Security control classification and risk analysis

**Career-Relevant Skills**:

* Security baseline implementation for production systems
* Regulatory compliance validation (DoD, PCI-DSS, HIPAA patterns)
* Risk mitigation through defense-in-depth configuration
* Professional security documentation and auditor communication

***

## Visual Recommendations

**Page Cover Recommendation**: Hero-width cover showing a security compliance dashboard or STIG Viewer interface with finding status indicators (Open/Not\_Reviewed/NotAFinding) to immediately convey the lab's focus on systematic security assessment.

**Page Icon Recommendation**: 🛡️ (shield) for immediate security hardening topic recognition

**Section Icons**:

* Module 1: 🔍 (system exploration and reconnaissance)
* Module 2: 📥 (preparation and tool setup)
* Module 3: 🎯 (focused remediation and hardening)
