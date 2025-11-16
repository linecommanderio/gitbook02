# session\_2\_overview\_gitbook

We will focus on **understanding and implementing network standards and compliance measures** that make security controls critically effective.

This session introduces foundational knowledge for analyzing, configuring, and hardening networking components using enterprise-grade tools and frameworks including STIGs, OpenSCAP, and secure DNS configurations.

Network security isn't about memorizing commands—it's about understanding how data flows through your systems and implementing controls that prevent unauthorized access while maintaining operational efficiency.

{% hint style="info" %}
**Course Context**: This is Week 2 of our defensive security focus. We're building on last week's control categories to **lock down server network connections** and establish secure communication boundaries.

> _Our job is to_ **reduce ingress risk** _through_ **structured standards and automated compliance tools** _that defend against network-based attacks._
{% endhint %}

***

## 📚 Learning Objectives

By the end of Session 2, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="💪 Core Competencies" %}
1. **STIG Analysis**: Identifying and analyzing Security Technical Implementation Guides related to Linux networking
2. **Secure Name Resolution**: Understanding and configuring nsswitch.conf and DNS for secure hostname lookups
3. **Network Monitoring**: Using tcpdump, ngrep, ss, and netstat to monitor and analyze network behavior
4. **Compliance Assessment**: Applying OpenSCAP and SCC tools for automated network compliance validation
5. **Threat Analysis**: Exploring known network-based exploits using the Diamond Model of Intrusion Analysis
{% endtab %}

{% tab title="🔧 Technical Skills" %}
6. **STIG Remediation**: Generating and applying Ansible playbooks and Bash scripts for compliance fixes
7. **Control Implementation**: Distinguishing between technical, managerial, operational, and physical controls
8. **Attack Vector Recognition**: Understanding how attackers go _around_ versus _through_ security controls
9. **Configuration Validation**: Verifying network hardening measures with command-line tools
10. **Automation Fundamentals**: Reading and modifying Ansible playbooks for system hardening
{% endtab %}
{% endtabs %}

***

## 🎯 Relevance & Context

**Networks represent one of the most common attack vectors in enterprise systems.** Misconfigured name resolution, open ports, and insecure protocols create doorways for intrusion that defenders must close systematically.

#### Why Network Security Matters

{% columns %}
{% column width="60%" %}
**The Challenge**:

* Network connections are the primary entry point for external attacks
* Misconfigurations in DNS, open ports, and protocols create vulnerabilities
* Manual compliance checking doesn't scale in enterprise environments
* Organizations need provable, auditable security postures for compliance

**The Solution**:

* **STIGs provide standardized security configurations** developed by DISA for government and enterprise use
* **OpenSCAP automates compliance assessment** across hundreds of security rules simultaneously
* **Network monitoring tools** provide visibility into what's actually happening on your systems
* **Automated remediation** through Ansible and Bash reduces human error and ensures consistency

**Real-World Application**: Think of network security like airport immigration control. You don't just check one person's passport—you have _standardized procedures_, _automated scanning systems_, and _multiple layers of verification_ that process thousands of travelers efficiently while maintaining security. That's exactly what STIGs and OpenSCAP do for your Linux systems.
{% endcolumn %}

{% column width="40%" %}
{% hint style="success" %}
**What You're Building**:

An automated compliance system that assesses your network security posture against 401+ STIG rules, generates remediation playbooks, and provides verification of implemented controls.
{% endhint %}

{% hint style="warning" %}
**Critical Understanding**:

You don't blindly apply all STIGs—some will break your system. Understanding _what_ each control does and _why_ it matters is essential for making informed decisions about which controls to implement.
{% endhint %}

{% hint style="info" %}
**Career Reality**:

As system engineers, building resilient systems requires deep understanding of how data flows through network pathways and what tools can monitor and secure them. These skills are crucial for both compliance requirements and real-world defense.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## 🔍 Understanding Security Controls

Before diving into network-specific configurations, you need to understand the framework we're working within.

{% tabs %}
{% tab title="Control Categories" %}
{% stepper %}
{% step %}
### Technical Controls

Applied directly to systems through configuration.

Example: SSH configured to disallow password authentication.\
Effect: System prevents the action—no way around it.
{% endstep %}

{% step %}
### Physical Controls

Applied to the physical environment.

Example: Server room access badge system.\
Effect: Physical barrier to access.
{% endstep %}

{% step %}
### Managerial Controls

Require organizational authority.

Example: Security policy requiring MFA for all users.\
Effect: Only works if people respect the authority.
{% endstep %}

{% step %}
### Operational Controls

Procedural recommendations and guidelines.

Example: Standard Operating Procedure for password management.\
Effect: Only effective if followed voluntarily.
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
**Key Insight**: Technical and physical controls are **enforced by the system**. Managerial and operational controls require **human compliance**.

> _"If I put a nice banner of the day message saying 'don't do anything bad,' but you have no intention of listening—it literally doesn't matter. My authority means nothing."_
{% endhint %}
{% endtab %}

{% tab title="Control Types" %}
{% stepper %}
{% step %}
### Preventative Controls

Stop actions before they happen.

Examples:

* Firewall rule blocking port 23 (Telnet)
* SSH configuration: `PermitRootLogin no`
{% endstep %}

{% step %}
### Detective Controls

Identify when something bad has happened.

Examples:

* Log monitoring that alerts on failed login attempts
* Intrusion Detection System (IDS) alerts
{% endstep %}

{% step %}
### Corrective Controls

Fix problems after they occur.

Examples:

* Automated scripts that restore configurations
* Backup and recovery procedures
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Defense Strategy**: You need **all three types** working together. Preventative controls reduce attacks, detective controls identify breaches, corrective controls minimize damage.
{% endhint %}
{% endtab %}

{% tab title="Attack Patterns" %}
**Two Ways Attackers Defeat Security**:

<details>

<summary><strong>Going AROUND Controls</strong> (Easier)</summary>

Attackers bypass security measures without defeating them technically:

* Stealing credentials instead of brute-forcing passwords
* Phishing users to click malicious links
* Social engineering to gain physical access
* Exploiting misconfigured permissions

**Example**: MFA fatigue attacks—sending dozens of authentication requests until the user gets tired and just clicks "Accept" to stop the notifications.

</details>

<details>

<summary><strong>Going THROUGH Controls</strong> (Harder)</summary>

Attackers defeat the technical capabilities of security measures:

* Exploiting software vulnerabilities
* Brute-forcing encryption
* Breaking authentication mechanisms
* Bypassing access control lists

**Example**: Zero-day exploits that leverage unknown vulnerabilities in software.

</details>

{% hint style="danger" %}
**Defensive Priority**: Your job is to implement **technical preventative controls** that force attackers to either go through (difficult) or around (detectable). Never rely solely on managerial/operational controls for critical security.
{% endhint %}
{% endtab %}
{% endtabs %}

***

## 🛠️ Session Tools & Frameworks

This session introduces several enterprise-standard tools for network security assessment and remediation:

{% tabs %}
{% tab title="STIGs" %}
#### Security Technical Implementation Guides (STIGs)

**What**: Standardized security configurations developed by DISA (Defense Information Systems Agency) for U.S. Department of Defense systems—widely adopted in enterprise and government environments.

**Purpose**: Provide specific, actionable security requirements that systems must meet for compliance and hardening.

**For This Session**: We focus on **401 STIG rules** specific to RHEL 9 networking configuration.

**Key Concepts**:

* Each STIG has a unique identifier (e.g., `V-257790`)
* STIGs specify exact file paths, permissions, and configuration values
* Not all STIGs apply to every system—context matters
* Blind application can break functionality

{% code title="Example STIG Rule" lineNumbers="true" %}
```yaml
# STIG V-257790: GRUB bootloader ownership
- name: "Ensure GRUB config is owned by root"
  file:
    path: /boot/grub2/grub.cfg
    owner: root
    group: root
    mode: '0600'
```
{% endcode %}

**Download Required**: [STIG Viewer v2.18](https://public.cyber.mil/stigs/srg-stig-tools/) for analyzing STIG requirements.
{% endtab %}

{% tab title="OpenSCAP" %}
#### Open Security Content Automation Protocol (OpenSCAP)

**What**: Open-source framework for automated security compliance assessment and remediation.

**Purpose**: Scan systems against security benchmarks (like STIGs) and generate reports showing compliance status.

**Capabilities**:

* Automated scanning of 401+ STIG rules in minutes
* Generation of compliance reports in multiple formats
* Creation of remediation scripts (Ansible and Bash)
* Before/after validation of security posture

**Installation**:

```bash
sudo dnf install -y openscap-scanner scap-security-guide
```

**Basic Usage**:

```bash
# Generate Ansible remediation playbook
oscap xccdf generate fix --profile stig \
  --output draft-remediate.yml \
  /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml
```

{% hint style="success" %}
**Efficiency Gain**: What would take weeks to manually check and document, OpenSCAP completes in minutes with full auditability.
{% endhint %}
{% endtab %}

{% tab title="Network Monitoring" %}
#### Command-Line Network Analysis Tools

**tcpdump**: Packet capture and analysis

```bash
# Capture traffic on interface eth0
sudo tcpdump -i eth0 -nn

# Save capture for later analysis
sudo tcpdump -i eth0 -w capture.pcap
```

**ngrep**: Network grep for packet payloads

```bash
# Search for specific strings in network traffic
sudo ngrep -q -W byline -d eth0 "password"
```

**ss**: Socket statistics (modern replacement for netstat)

```bash
# Show all listening TCP ports
ss -tlnp

# Show established connections
ss -tunap | grep ESTAB
```

**netstat**: Network statistics (legacy tool)

```bash
# Show listening ports and associated programs
sudo netstat -tulnp
```

{% hint style="info" %}
**Tool Selection**: Use `ss` for new work—it's faster and more feature-rich. Learn `netstat` because you'll encounter it in legacy systems.
{% endhint %}
{% endtab %}

{% tab title="Ansible Automation" %}
#### Ansible for Configuration Management

**What**: Automation platform for deploying configurations, managing systems, and orchestrating complex workflows.

**Why in This Course**: STIGs can be enforced through Ansible playbooks, allowing consistent, repeatable security configurations across fleets of systems.

**Structure**:

```yaml
# Ansible playbook structure
---
- name: Apply STIG configurations
  hosts: all
  become: yes
  
  vars:
    # Variables define values used in tasks
    sysctl_values:
      net.ipv4.conf.all.accept_source_route: 0
  
  tasks:
    # Tasks are actions to perform
    - name: Configure sysctl parameters
      sysctl:
        name: "{{ item.key }}"
        value: "{{ item.value }}"
        state: present
      loop: "{{ sysctl_values | dict2items }}"
```

**Key Files**:

* `defaults/main.yaml`: Variable definitions for configurations
* `tasks/main.yaml`: Actual remediation tasks to execute

{% hint style="warning" %}
**Learning Ansible**: If you're not familiar with Ansible, treat the generated playbooks as learning tools. They contain excellent examples of:

* Conditional logic (if/then/else)
* Loops and iteration
* File manipulation
* Service management
{% endhint %}
{% endtab %}
{% endtabs %}

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 2, you must know:

* **Command Line Proficiency**: Navigating the Linux filesystem, running commands, using pipes and redirects
* **Text Editor Skills**: Editing files with `vim` or another command-line editor
* **Package Management**: Installing software with `dnf`, `yum`, or `apt`
* **Basic Networking**: Understanding TCP/IP, DNS concepts, network interfaces
* **System Tools**: Using `grep`, `sysctl`, `systemctl`, and similar utilities
* **File Editing**: Creating and modifying configuration files

**Foundation Requirement**: Review these concepts if needed:

* Linux directory structure and file permissions
* How services and daemons work in Linux
* Basic shell scripting (reading, not necessarily writing)
* Understanding of IP addressing and subnetting
* SSH connectivity and authentication concepts
{% endhint %}

***

## 🎓 Learning Approach

<details>

<summary><strong>How This Session Works</strong></summary>

**Three-Phase Learning Model**:

1. **Conceptual Foundation**: Understanding control categories, STIG frameworks, and compliance philosophy
2. **Tool Exploration**: Installing and running OpenSCAP, examining STIG rules, generating remediation scripts
3. **Hands-On Application**: Analyzing generated configurations, understanding what they do, making informed decisions about implementation

**Not a "Copy-Paste" Course**: You won't blindly run remediation scripts. You'll learn to:

* Understand what each STIG rule does and why it matters
* Evaluate which controls are appropriate for your environment
* Read and modify Ansible playbooks and Bash scripts
* Verify that controls are working as intended

**Practice Philosophy**: "A little time every day beats five hours once a week." Consistency builds competence—treat this like going to the gym for your Linux skills.

</details>

***

## 🔗 Additional Resources

**Course Materials**:

* Discussion posts and peer responses (required weekly)
* Hands-on lab worksheets with guided exercises
* Optional advanced labs in KillerCoda environment
* Lab access: 44 dedicated lab devices (19 original course + 25 security engineering course)

**Community**:

* **Discord**: Primary support channel—ask questions, get help, collaborate
* **Live Sessions**: Weekly Saturday recordings + Sunday hands-on lab sessions
* **Annual Conference**: August 9-10 virtual event (free registration, 100+ attendees, industry guest speakers)

**Documentation**:

* [Online course book](https://professionallinuxusersgroup.github.io/psc/) (always in progress, free access)
* PDF versions of all materials in Discord
* STIG Viewer v2.18 for analyzing security requirements

{% hint style="info" %}
**Getting Help**: Discord is the fastest way to get answers. The community is active, supportive, and experienced. Don't hesitate to ask questions!
{% endhint %}
