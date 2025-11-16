---
cover: ../../.gitbook/assets/session2-ezgif.webp
coverY: -2.974083164680735
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

## 🎯 Relevance & Context

{% columns %}
{% column width="58.333333333333336%" %}
**Your Professional Responsibility**

Networks are one of the **most common attack vectors** in enterprise systems. Misconfigured name resolution, open ports, and insecure protocols are doorways to intrusion. This session teaches you to reduce ingress risk and respond effectively to threats.



{% hint style="danger" %}
**Real Example**: Instead of brute-forcing SSH, attackers steal credentials or exploit MFA fatigue (going around).

> _**Your job is to eliminate paths around your technical controls.**_
{% endhint %}



**Control Types Matter**:

Understanding control categories is **critical** to defensive security:

<table><thead><tr><th width="115.978515625">Control Type</th><th width="111.123046875">Description</th><th>Example</th></tr></thead><tbody><tr><td><strong>Technical</strong></td><td>Applied to systems, cannot be bypassed</td><td>SSH disallows root login</td></tr><tr><td><strong>Physical</strong></td><td>Real-world barriers</td><td>Locked server room</td></tr><tr><td><strong>Managerial</strong></td><td>Requires authority to enforce</td><td>Security policies</td></tr><tr><td><strong>Operational</strong></td><td>Procedures and practices</td><td>SOPs, change management</td></tr></tbody></table>

**Key Insight**: Technical and physical controls **prevent** actions. Managerial and operational controls require **authority** and \*\*will—if someone doesn't respect that authority, they can ignore the control entirely.
{% endcolumn %}

{% column width="43.333333333333336%" %}
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
