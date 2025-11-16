---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# session\_lab\_1\_simplified

{% hint style="warning" %}
**Important**: If you are unable to finish the lab in the ProLUG lab environment, please **reboot** the machine from the command line so that other students will have the intended environment.
{% endhint %}

***

## Required Materials

{% tabs %}
{% tab title="Tools" %}
**SSH Client**:

* PuTTY (Windows) or other connection tool
* OpenSSH (Linux/macOS)

**Lab Environment**:

* ProLUG Lab Server (Hammer server)
* Root or sudo command access

**Security Assessment**:

* STIG Viewer 2.18
* Download: https://public.cyber.mil/stigs/downloads/
{% endtab %}

{% tab title="Prerequisites" %}
**Required Knowledge**:

* Basic command line navigation (`cd`, `ls`, `pwd`)
* Text filtering with `grep` and pipes
* Package management with `dnf`/`yum`
* SystemD service control (`systemctl`)

**Required Skills**:

* SSH connection establishment
* Privilege escalation with `sudo`
* Configuration file editing
* Command output analysis
{% endtab %}
{% endtabs %}

***

## Downloads

The lab has been provided below. The document(s) can be transposed to the desired format so long as the content is preserved. For example, the .txt could be transposed to a .md file.

***

## Module 1: Exploring System Information

### Exercise 1.1: Familiarizing Ourselves with the System

Run the following commands to discover filesystem security options:

{% code overflow="wrap" %}
```bash
mount | grep -i noexec
mount | grep -i nodev
mount | grep -i nosuid
```
{% endcode %}

Question: Approximately how many of your mounted filesystems have each of these values?

***

### Exercise 1.2: Checking Mounted Systems

Part A: IPv4 and IPv6 Parameters

{% code overflow="wrap" %}
```bash
sysctl -a | grep -i ipv4
sysctl -a | grep -i ipv6
```
{% endcode %}

Question: How many of each are there?

Part B: IPv4 Forwarding

{% code overflow="wrap" %}
```bash
sysctl -a | grep -i ipv4 | grep -i forward
```
{% endcode %}

Question: Does IPv4 forward on interfaces?

Part C: Netfilter Tables

{% code overflow="wrap" %}
```bash
lsmod | grep -i tables
```
{% endcode %}

Question: What type of tables exist?

***

## Module 2: PreLAB

Complete the following preparation steps before beginning Module 3:

{% stepper %}
{% step %}
### Download STIG Viewer

1. Download STIG Viewer 2.18 from: https://public.cyber.mil/stigs/downloads/
{% endstep %}

{% step %}
### Import STIG for MariaDB

1. Download the STIG for MariaDB and import it into your STIG viewer
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**What are STIGs?**

Security Technical Implementation Guides (STIGs) are configuration standards published by the Defense Information Systems Agency (DISA). They provide security requirements for specific technologies and are used across DoD, government agencies, and regulated industries.
{% endhint %}

***

## Module 3: Lab

{% hint style="success" %}
**Lab Objective**

Practice securing a Linux server or service against a set of configuration standards. These standards are sometimes called benchmarks, checklists, or guidelines.

You will use STIG Viewer 2.18 to evaluate and remediate security findings.
{% endhint %}

### MariaDB Service Configuration

{% stepper %}
{% step %}
### Step: Connect and Install

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
{% endstep %}
{% endstepper %}

***

### Check and Remediate v-253666 STIG

Analysis Questions:

1. What is the problem?
2. What is the fix?
3. What type of control is being implemented?
4. Is it set properly on your system?

Verification Steps:

Connect to MariaDB locally:

{% code overflow="wrap" %}
```bash
mysql
```
{% endcode %}

Run the SQL command in the STIG's Fix Text section:

{% code overflow="wrap" %}
```sql
SELECT user, max_user_connections FROM mysql.user;
```
{% endcode %}

Question: Can you remediate this finding?

***

### Check and Remediate v-253677 STIG

Analysis Questions:

1. What is the problem?
2. What is the fix?
3. What type of control is being implemented?
4. Is it set properly on your system?

***

### Check and Remediate v-253678 STIG

Analysis Questions:

1. What is the problem?
2. What is the fix?
3. What type of control is being implemented?
4. Is it set properly on your system?

***

### Check and Remediate v-253734 STIG

Analysis Questions:

1. What is the problem?
2. What is the fix?
3. What type of control is being implemented?
4. Is it set properly on your system?

***

## Lab Completion

{% hint style="warning" %}
**Before Leaving**: Be sure to **reboot** the lab machine from the command line when you are done.

```bash
sudo reboot
```
{% endhint %}

***

## Visual Recommendations

{% hint style="info" %}
Page Cover Recommendation: Hero-width cover showing a security compliance dashboard or STIG checklist interface to immediately convey the lab's focus on security standards and compliance validation

Page Icon Recommendation: 🛡️ (shield) for immediate security/hardening topic recognition

Section Icons:

* Module 1: 🔍 (magnifying glass for system exploration)
* Module 2: 📥 (download box for preparation)
* Module 3: 🎯 (target for focused remediation work)
{% endhint %}
