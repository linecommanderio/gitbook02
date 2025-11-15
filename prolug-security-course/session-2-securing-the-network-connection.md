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

# Session 2: Securing the Network Connection

## Overview

Understanding and implementing network standards and compliance measures makes security controls of critical importance very effective. Networks represent one of the most common attack vectors in enterprise systems—and securing them is foundational to defensive security engineering.

This session focuses on **analyzing, configuring, and hardening** networking components using industry-standard tools and frameworks. You'll work with STIGs, OpenSCAP, DNS configurations, and network monitoring tools to reduce ingress risk and respond effectively to threats.

{% hint style="info" %}
**Defensive Security Focus** This course emphasizes the **defensive side** of security engineering—locking down servers and protecting systems against intrusion. We're building resilient infrastructure that can withstand real-world attacks.

> **Key Insight**: _Attackers either go **around** controls or **through** controls. It's much easier to go around them than through them. Your job is to eliminate paths around your technical controls._
{% endhint %}

We'll move from conceptual understanding to hands-on implementation, examining how security controls actually work in production Linux systems.

***

## 📚 Learning Objectives

By the end of Session 2, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="Core Competencies" %}
1. **STIG Analysis**: Identifying and analyzing STIGs related to Linux networking
2. **Name Resolution Security**: Understanding and configuring secure name resolution using `nsswitch.conf` and DNS
3. **Network Monitoring**: Utilizing `tcpdump`, `ngrep`, `ss`, and `netstat` to monitor network behavior
{% endtab %}

{% tab title="Professional Skills" %}
4. **Compliance Assessment**: Applying OpenSCAP and SCC tools for network compliance assessments
5. **Threat Intelligence**: Exploring known network-based exploits using the Diamond Model of Intrusion Analysis
6. **Security Trade-offs**: Understanding the balance between security hardening and system functionality
{% endtab %}
{% endtabs %}

***

## 🛠️ Labs to Build Here

{% stepper %}
{% step %}
**🔥 Pre-Lab Warm-Up: System Security Reconnaissance**

**Quick system familiarization and security posture assessment**

Run through these commands to understand your system's current security configuration:

{% code title="security-recon.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Check IPv4 forwarding configuration
sysctl -a | grep -i ipv4 | grep -i forward

# Question: Does this system appear to be set to forward? Why or why not?

# Check martian packet handling
sysctl -a | grep -i ipv4 | grep -i martian

# Question: What are martians and is this system allowing them?

# Check kernel panic behavior
sysctl -a | grep -i panic

# Question: How does this system handle panics?

# Check cryptographic settings
sysctl -a | grep -i crypto

# Question: What settings do you see? Is FIPS enabled?

# Verify FIPS and SELinux status
cat /proc/cmdline
fips-mode-setup --check
sestatus
cat /etc/selinux/config
```
{% endcode %}

<details>

<summary>🎯 What You're Looking For</summary>

**Security Posture Indicators**:

* **SELinux status**: Is it enforcing, permissive, or disabled?
* **FIPS mode**: Is Federal Information Processing Standard cryptography enabled?
* **Forwarding settings**: Is this system configured as a router or gateway?
* **Martian packets**: Are impossible source addresses being logged and dropped?
* **Panic handling**: How does the kernel respond to critical failures?

These baseline checks reveal your system's current security configuration before hardening.

</details>

{% hint style="success" %}
**✅ Outcome**: Comprehensive baseline understanding of system security configuration
{% endhint %}
{% endstep %}

{% step %}
**🔧 STIG Viewer Setup and RHEL 9 STIG Import**

**Configure your security assessment toolkit**

{% stepper %}
{% step %}
#### Download STIG Viewer

Download **STIG Viewer 2.18** from [https://public.cyber.mil/stigs/downloads/](https://public.cyber.mil/stigs/downloads/)
{% endstep %}

{% step %}
#### Import RHEL 9 STIG

Download the **RHEL 9 STIG** and import it into STIG Viewer
{% endstep %}

{% step %}
#### Create Checklist

Create a checklist from the opened RHEL 9 STIG for systematic assessment
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Tool Context** STIG Viewer is the **industry-standard tool** for DoD and high-security enterprise environments. This is the same tool security engineers use in federal government and defense contractor positions.
{% endhint %}

{% hint style="success" %}
**✅ Outcome**: Operational STIG Viewer environment ready for network security assessment
{% endhint %}
{% endstep %}

{% step %}
**🌐 Network Service STIG Remediation**

**Hands-on network configuration hardening**

Connect to your lab server and begin systematic STIG assessment:

{% code overflow="wrap" %}
```bash
# Filter STIGs by IPv4 to see network-related controls

# You should see multiple STIGs related to network hardening
```
{% endcode %}

**Examine and Remediate V-257957: TCP SYN Cookies**

{% tabs %}
{% tab title="Analysis" %}
**What is the problem?**

* System may be vulnerable to TCP SYN flood attacks

**What is the fix?**

* Enable TCP SYN cookies to protect against SYN floods

**What type of control?**

* **Technical Preventative Control** - System setting that prevents resource exhaustion attacks

**Current Status Check**:

{% code overflow="wrap" %}
```bash
sysctl -a | grep -i ipv4 | grep -i syncookies
```
{% endcode %}
{% endtab %}

{% tab title="Remediation" %}
If the value is not correctly set:

{% code title="/etc/sysctl.d/00-remediate.conf" %}
```bash
# Enable TCP SYN cookie protection
net.ipv4.tcp_syncookies = 1
```
{% endcode %}

Apply the configuration:

{% code overflow="wrap" %}
```bash
# Reload sysctl configuration
sysctl --system

# Verify the change
sysctl net.ipv4.tcp_syncookies
```
{% endcode %}
{% endtab %}
{% endtabs %}

**Examine and Remediate V-257958**

{% hint style="warning" %}
**Assignment** Follow the same analysis pattern:

{% stepper %}
{% step %}
### What is the problem?
{% endstep %}

{% step %}
### What is the fix?
{% endstep %}

{% step %}
### What type of control is being implemented?
{% endstep %}

{% step %}
### Is it set properly on your system?
{% endstep %}

{% step %}
### How would you remediate this?
{% endstep %}
{% endstepper %}
{% endhint %}

**Examine V-257960 and V-257961 (Related STIGs)**

**Key Question**: How are these two STIGs related? What do they accomplish together?

{% hint style="success" %}
**✅ Outcome**: Systematic STIG analysis methodology with hands-on remediation experience
{% endhint %}
{% endstep %}

{% step %}
**🔥 Firewall STIG Analysis**

**Understanding firewall security controls**

{% code overflow="wrap" %}
```bash
# Filter STIG Viewer by "firewall" keyword

# Observe the number of firewall-related STIGs
```
{% endcode %}

**Analysis Questions**:

* How many firewall STIGs do you see?
* What are these STIGs trying to accomplish?
* What types of controls are they? (Preventative, detective, corrective?)

<details>

<summary>🎯 Firewall Control Categories</summary>

Firewall STIGs typically implement:

* **Technical Preventative Controls**: Block unauthorized network access
* **Technical Detective Controls**: Log connection attempts for analysis
* **Configuration Standards**: Ensure consistent security posture across systems

These controls work together to create defense-in-depth at the network boundary.

</details>

{% hint style="success" %}
**✅ Outcome**: Understanding of firewall STIG requirements and control categorization
{% endhint %}
{% endstep %}

{% step %}
**🚪 Firewall Port Exposure Lab**

**Real-world scenario: Exposing services securely**

{% hint style="info" %}
**Scenario** Your team needs to use `node_exporter` with Prometheus to scrape system metrics for network monitoring. You're running a firewall, so you must expose the port that `node_exporter` uses to the network outside your system.
{% endhint %}

{% code title="firewall-port-exposure.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Verify firewall is running
systemctl status firewalld

# Check if node_exporter service is defined
firewall-cmd --get-services | grep -i node
ls /usr/lib/firewalld/services | grep -i node

# Verify service is NOT currently enabled
firewall-cmd --list-services

# Examine the firewall service definition structure
cat /usr/lib/firewalld/services/prometheus-node-exporter.xml

# Enable the service through your firewall permanently
firewall-cmd --permanent --add-service=prometheus-node-exporter

# Reload firewall to apply changes
firewall-cmd --reload

# Verify the service is now enabled
firewall-cmd --list-services
```
{% endcode %}

<details>

<summary>📋 Understanding Firewall Service Definitions</summary>

When you examine `/usr/lib/firewalld/services/prometheus-node-exporter.xml`, you'll see:

* **Port number**: Which port the service uses
* **Protocol**: TCP or UDP
* **Description**: What the service does

This XML structure allows firewalld to manage services by name rather than memorizing port numbers.

</details>

{% hint style="success" %}
**✅ Outcome**: Practical experience exposing services through enterprise firewalls while maintaining security
{% endhint %}
{% endstep %}

{% step %}
**🤖 Automated STIG Remediation Tools**

**Generate automated remediation scripts**

{% hint style="warning" %}
**Critical Understanding** Security engineers **don't blindly apply** all STIG remediations. We must understand **trade-offs** between security and productivity. Some controls may break applications or cause operational issues.

> **Your value**: _Knowing which controls to apply and which to adapt based on business requirements._
{% endhint %}

**Download Official DISA STIG Remediation Ansible**

{% code title="download-stig-ansible.sh" overflow="wrap" lineNumbers="true" %}
```bash
cd /root
mkdir stigs
cd stigs

# Download official RHEL 9 STIG Ansible remediation
wget -O U_RHEL_9_V2R4_STIG_Ansible.zip \
  https://dl.dod.cyber.mil/wp-content/uploads/stigs/zip/U_RHEL_9_V2R4_STIG_Ansible.zip

# Extract the archive
unzip U_RHEL_9_V2R4_STIG_Ansible.zip

# Create working directory and extract role
mkdir ansible
cp rhel9STIG-ansible.zip ansible/
cd ansible
unzip rhel9STIG-ansible.zip
```
{% endcode %}

{% hint style="info" %}
**Note** If lab downloads don't work, check `/labs` folder on your server for `[course]_[unit#].zip` backup files.
{% endhint %}

**Examine Default STIG Values**

{% code title="examine-defaults.sh" overflow="wrap" %}
```bash
# Navigate to defaults directory
cd /root/stigs/ansible/roles/rhel9STIG/defaults/

# Open default values file
vim main.yml

# Search for specific STIGs (use /257957 in vim to search)

# Example: /257957 to find that STIG's default configuration
```
{% endcode %}

**What You'll See**:

* Default values for each STIG rule
* Variables you can customize for your environment
* Boolean flags to enable/disable specific remediations

**Examine Ansible Playbook Implementation**

{% code title="examine-playbook.sh" overflow="wrap" %}
```bash
# View how STIGs are implemented in Ansible
vim /root/stigs/ansible/roles/rhel9STIG/tasks/main.yml

# Search for the same STIG (use /257957 in vim)

# Observe how the default values are applied to the system
```
{% endcode %}

<details>

<summary>🔍 Understanding Ansible Remediation Structure</summary>

The playbook shows:

* **985 configuration lines** of defaults
* **2,991 Ansible playbook tasks** for remediation
* Each task implements a specific STIG control

**Key Insight**: You can modify `defaults/main.yml` to customize which STIGs apply and with what values, then run the playbook against your systems for automated compliance.

</details>

**Generate OpenSCAP Remediation Scripts**

{% code title="openscap-remediation.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Install OpenSCAP tools
dnf -y install openscap-scanner openscap-utils scap-security-guide

# Create working directory
cd /root
mkdir openscap
cd openscap

# Generate Ansible remediation playbook
oscap xccdf generate fix \
  --profile ospp \
  --fix-type ansible \
  /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml \
  > draft-disa-remediate.yml

# Examine the generated playbook
vim draft-disa-remediate.yml

# Generate Bash script version
oscap xccdf generate fix \
  --profile ospp \
  --fix-type bash \
  /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml \
  > draft-disa-remediate.sh

# Examine the bash script
vim draft-disa-remediate.sh
```
{% endcode %}

{% tabs %}
{% tab title="Ansible Version" %}
**What You'll See**:

* YAML playbook format with tasks for each remediation
* Variables at the top for customization
* Tags for selective remediation
* Ansible modules like `lineinfile`, `package`, `service`

**Use Case**: Automated deployment across multiple systems using Ansible infrastructure
{% endtab %}

{% tab title="Bash Version" %}
**What You'll See**:

* Pure bash script with if/then/else logic
* Nested conditionals for complex remediations
* Direct system commands and file modifications

**Learning Opportunity**: If you want to improve your bash scripting, this is **excellent reference code** showing production-quality conditional logic and error handling.

**Use Case**: Single-system remediation without Ansible dependencies
{% endtab %}
{% endtabs %}

<details>

<summary>⚠️ Critical Warning: Don't Blindly Apply</summary>

**Both methods generate comprehensive remediation scripts, but**:

* **Not all fixes apply to all environments**
* Some remediations may break applications
* GRUB configurations won't work on certain boot systems
* Network settings may cause connectivity loss
* Service restrictions may impact functionality

**Professional Approach**:

1. Review generated scripts thoroughly
2. Test in non-production environment first
3. Apply selectively based on your threat model
4. Document any STIGs you cannot remediate and why
5. Create compensating controls for unimplemented STIGs

> **This is your value as a security engineer**: _Understanding which controls to implement and which to adapt._

</details>

{% hint style="success" %}
**✅ Outcome**:

* Ability to generate automated STIG remediation scripts
* Understanding of Ansible and Bash remediation approaches
* Professional judgment for selective implementation
* Reference code for advanced bash scripting patterns
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
**🧹 Lab Cleanup** When you're finished with the lab environment, please `reboot` your machine from the command line so other students have a clean environment.
{% endhint %}

***

## 🎯 Relevance & Context

{% columns %}
{% column width="60%" %}
**Your Professional Responsibility**

Networks represent **one of the most common attack vectors** in enterprise systems. Misconfigured name resolution, open ports, and insecure protocols are all doorways to intrusion. As system engineers building resilient systems, you need **deep understanding** of how data flows through network pathways and what tools can monitor and secure them.

**The Fundamental Security Principle**:

Attackers either go **around** controls or **through** controls:

* **Going through controls**: Defeating technical capabilities (difficult)
* **Going around controls**: Bypassing them entirely (much easier)

{% hint style="danger" %}
**Real-World Example** Instead of brute-forcing SSH authentication (going through), attackers steal credentials or exploit MFA fatigue attacks where users approve requests just to stop notification spam (going around).

> **Your job**: _Eliminate paths around your technical controls._
{% endhint %}

**Control Types Matter**:

Understanding control categories is **critical** to defensive security:

| Control Type    | Description                            | Example                  |
| --------------- | -------------------------------------- | ------------------------ |
| **Technical**   | Applied to systems, cannot be bypassed | SSH disallows root login |
| **Physical**    | Real-world barriers                    | Locked server room       |
| **Managerial**  | Requires authority to enforce          | Security policies        |
| **Operational** | Procedures and practices               | SOPs, change management  |

**Key Insight**: Technical and physical controls **prevent** actions. Managerial and operational controls require **authority** and \*\*will—if someone doesn't respect that authority, they can ignore the control entirely.

**Enterprise Defense Strategy**

**These capabilities enable you to**:

* **Reduce attack surface** systematically through STIG compliance and network hardening
* **Detect intrusions early** with comprehensive network monitoring and analysis
* **Respond effectively** to security incidents using structured analytical frameworks
* **Maintain compliance** with automated scanning and remediation workflows
* **Balance security with functionality** through professional judgment and trade-off analysis
* **Document security posture** professionally for audit, GRC teams, and incident response

**Real-World Application**

By learning to assess and remediate network-related STIGs and implementing structured standards, you'll gain skills to:

* **Reduce ingress risk** through proper network configuration and hardening
* **Monitor network behavior** for anomalous activity indicating compromise
* **Respond to threats** using analytical frameworks like the Diamond Model
* **Meet compliance requirements** with automated tooling and professional documentation
* **Collaborate with security operations** through clear technical communication

**Not Just Compliance**:

While adhering to standards is important, **understanding what controls do** is more important. Sometimes you must break the rules because blanket application doesn't make sense for your environment. Security engineers need **professional judgment** to implement appropriate controls while maintaining system functionality.
{% endcolumn %}

{% column %}
{% hint style="success" %}
**Network Security Skills**

I'm learning that network security isn't just about firewalls and access controls—it's about understanding the **entire data flow** from application layer to physical layer, monitoring for anomalies, and implementing defense-in-depth.

**Key Realization**: My network configurations **directly determine** organizational risk exposure. Misconfigured services create attack vectors that skilled adversaries will discover and exploit.
{% endhint %}

{% hint style="info" %}
**Key Resources**

* **DISA STIGs**: Official DoD network security guidance ([https://public.cyber.mil/stigs](https://public.cyber.mil/stigs))
* **OpenSCAP**: Automated compliance assessment ([https://www.open-scap.org/](https://www.open-scap.org/))
* **Diamond Model**: Structured threat analysis framework for understanding adversary behavior
* **SCAP Security Guide**: Comprehensive security content automation

**Professional Development**: These skills are **required** for network security engineering, SOC analyst, incident response, and systems engineering roles in high-security environments including DoD, defense contractors, and Fortune 500 enterprises.
{% endhint %}

{% hint style="warning" %}
**Consistency Over Intensity**

Like physical fitness, **you can't go to the gym once a week and get in shape**. Linux security engineering requires **consistent daily practice**:

* Spend a little time every day rather than cramming
* Build hands-on experience incrementally
* Practice remediation workflows regularly
* Review and document what you learn

> **Career Impact**: _Consistent daily practice builds muscle memory and professional judgment that interviews and certifications can't replace._
{% endhint %}

{% hint style="info" %}
**Key Terms and Definitions**

* **sysctl**: Kernel parameter configuration utility
* **nsswitch.conf**: Name service switch configuration file
* **DNS**: Domain Name System for name resolution
* **OpenSCAP**: Open Security Content Automation Protocol
* **CIS Benchmarks**: Center for Internet Security configuration standards
* **ss/netstat**: Socket statistics and network connection tools
* **tcpdump**: Packet capture and analysis utility
* **ngrep**: Network grep for packet content searching
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 2, you must possess Linux Administration competencies including:

* **Command Line Proficiency**: BASH shell operations and command chaining
* **Package Management**: Installing and updating system packages via DNF/YUM
* **Network Fundamentals**: TCP/IP, DNS, routing concepts, and network protocols
* **System Tools**: `sysctl`, `firewalld`, `grep`, `ss`, `netstat`, and related utilities
* **Text Editing**: File manipulation using `vim` or other CLI editors
* **Security Tools**: `oscap`, `tcpdump`, and network analysis utilities
* **Required Software**: STIG Viewer v2.18 from [https://public.cyber.mil/stigs/downloads/](https://public.cyber.mil/stigs/downloads/)

**Foundation Requirement**: This session builds on Session 1's security framework foundation. You must understand:

* Control types (technical, managerial, operational, physical)
* Control categories (preventative, detective, corrective, compensating)
* STIG structure and remediation workflows
* Professional security documentation standards
{% endhint %}

***
