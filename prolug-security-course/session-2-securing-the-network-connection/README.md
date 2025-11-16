---
cover: ../../.gitbook/assets/session2-ezgif.webp
coverY: 0
layout:
  width: default
  cover:
    visible: true
    size: full
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

# 🔐 Session 2: Securing the Network Connection

We will focus on **understanding and implementing network standards and compliance measures** that make security controls critically effective.

This session introduces foundational knowledge for analyzing, configuring, and hardening networking components using enterprise-grade tools and frameworks including STIGs, OpenSCAP, and secure DNS configurations.

Network security isn't about memorizing commands—it's about understanding how data flows through your systems and implementing controls that prevent unauthorized access while maintaining operational efficiency.

{% hint style="info" %}
**Course Context**: This is Week 2 of our defensive security focus. We're building on last week's control categories to **lock down server network connections** and establish secure communication boundaries.

> _Our job is to_ **reduce ingress risk** _through_ **structured standards and automated compliance tools** _that defend against network-based attacks._
{% endhint %}

***

### 📚 Learning Objectives

By the end of Session 2, the skillset we will have explored:

{% tabs %}
{% tab title="💪 Core Competencies" %}
1. **STIG Analysis**: Identifying and analyzing Security Technical Implementation Guides related to Linux networking
2. **Secure Name Resolution**: Understanding and configuring `nsswitch.conf` and DNS for secure hostname lookups
3. **Network Monitoring**: Using `tcpdump`, `ngrep`, `ss`, and `netstat` to monitor and analyze network behavior
{% endtab %}

{% tab title="👔 Professional Skills" %}
4. **Compliance Assessment**: Applying OpenSCAP and SCC tools for network compliance assessments
5. **Threat Intelligence**: Exploring known network-based exploits using the Diamond Model of Intrusion Analysis
6. **Security Trade-offs**: Understanding the balance between security hardening and system functionality
{% endtab %}

{% tab title="🔧 Technical Skills" %}
* **STIG Remediation**: Generating and applying Ansible playbooks and Bash scripts for compliance fixes
* **Control Implementation**: Distinguishing between technical, managerial, operational, and physical controls
* **Attack Vector Recognition**: Understanding how attackers go _around_ versus _through_ security controls
* **Configuration Validation**: Verifying network hardening measures with command-line tools
* **Automation Fundamentals**: Reading and modifying Ansible playbooks for system hardening
{% endtab %}
{% endtabs %}

***

### 🎯 Relevance & Context

**Networks represent one of the most common attack vectors in enterprise systems.**

Misconfigured name resolution, open ports, and insecure protocols create doorways for intrusion that defenders must close systematically.

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

**Real-World Application**: Think of network security like airport immigration control.&#x20;

* You don't just check one person's passport—you have _standardized procedures_, _automated scanning systems_, and _multiple layers of verification_ that process thousands of travelers efficiently while maintaining security.&#x20;

That's exactly what STIGs and OpenSCAP do for your Linux systems.
{% endcolumn %}

{% column width="40%" %}
{% hint style="success" %}
**What You're Building**:

An automated compliance system that assesses your network security posture against 401+ STIG rules, generates remediation playbooks, and provides verification of implemented controls.
{% endhint %}





{% hint style="warning" %}
**Critical Understanding**:

Understanding _what_ each control does and _why_ it matters is essential for making informed decisions about which controls to implement.
{% endhint %}





{% hint style="info" %}
**Career Reality**:

As system engineers, building resilient systems requires deep understanding of how data flows through network pathways and what tools can monitor and secure them.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

### ✅ Prerequisites

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
