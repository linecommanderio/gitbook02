# session2 enhanced (1)

## Overview

Understanding and implementing network standards and compliance measures makes security controls of critical importance very effective. Networks represent one of the most common attack vectors in enterprise systems — and securing them is foundational to defensive security engineering.

This session focuses on **analyzing, configuring, and hardening** networking components using industry-standard tools and frameworks. You'll work with STIGs, OpenSCAP, DNS configurations, and network monitoring tools to reduce ingress risk and respond effectively to threats.

{% hint style="info" %}
**Defensive Security Focus** This course emphasizes the **defensive side** of security engineering — locking down servers and protecting systems against intrusion. We're building resilient infrastructure that can withstand real-world attacks.

> **Key Insight**: _Attackers either go **around** controls or **through** controls. It's much easier to go around them than through them. Your job is to eliminate paths around your technical controls._
{% endhint %}

We'll move from conceptual understanding to hands-on implementation, examining how security controls actually work in production Linux systems.

***

## 📚 Learning Objectives

By the end of Session 2, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="Core Competencies" %}
* **STIG Analysis**: Identifying and analyzing STIGs related to Linux networking
* **Name Resolution Security**: Understanding and configuring secure name resolution using `nsswitch.conf` and DNS
* **Network Monitoring**: Utilizing `tcpdump`, `ngrep`, `ss`, and `netstat` to monitor network behavior
{% endtab %}

{% tab title="Professional Skills" %}
* **Compliance Assessment**: Applying OpenSCAP and SCC tools for network compliance assessments
* **Threat Intelligence**: Exploring known network-based exploits using the Diamond Model of Intrusion Analysis
* **Security Trade-offs**: Understanding the balance between security hardening and system functionality
{% endtab %}
{% endtabs %}

***

## 🎯 Relevance & Context

Networks are one of the **most common attack vectors** in enterprise systems. Misconfigured name resolution, open ports, and insecure protocols are doorways to intrusion. This session teaches you to reduce ingress risk and respond effectively to threats.

### The Core Security Principle

Attackers either go **around** controls or **through** controls:

* **Through controls**: Defeating technical capabilities (very difficult)
* **Around controls**: Bypassing them entirely (much easier)

{% hint style="danger" %}
**Real Example**: Instead of brute-forcing SSH, attackers steal credentials or exploit MFA fatigue (going around). Your job is to eliminate paths around your technical controls.
{% endhint %}

### Why This Matters

**You'll learn to**:

* Reduce attack surface through STIG compliance and network hardening
* Detect intrusions early with monitoring tools
* Balance security with functionality using professional judgment
* Document security posture for compliance teams

{% hint style="warning" %}
**Critical Understanding**: Don't blindly apply all STIG remediations. Security engineers must understand **trade-offs** between security and productivity. Some controls may break applications. Your value is knowing which controls to implement and which to adapt.
{% endhint %}

### Professional Skills Development

{% hint style="success" %}
**What You're Building**: Understanding of complete data flows from application to physical layer, ability to monitor for anomalies, and defense-in-depth implementation skills that are required for network security engineering and SOC analyst roles.
{% endhint %}

{% hint style="info" %}
**Consistency Over Intensity**: Like physical fitness, you can't learn Linux security once a week. Spend a little time every day rather than cramming. Build hands-on experience incrementally.
{% endhint %}

### Key Resources

* **DISA STIGs**: https://public.cyber.mil/stigs/
* **OpenSCAP**: https://www.open-scap.org/
* **Diamond Model**: Threat analysis framework

### Key Terms

**sysctl** • **nsswitch.conf** • **DNS** • **OpenSCAP** • **CIS Benchmarks** • **ss/netstat** • **tcpdump** • **ngrep**

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
* **Required Software**: STIG Viewer v2.18 from https://public.cyber.mil/stigs/downloads

**Foundation Requirement**: This session builds on Session 1's security framework foundation. You must understand:

* Control types (technical, managerial, operational, physical)
* Control categories (preventative, detective, corrective, compensating)
* STIG structure and remediation workflows
* Professional security documentation standards
{% endhint %}

***

Page cover recommendation: Hero-width cover showing network security operations center (SOC) monitoring dashboard with firewall logs, packet captures, and STIG compliance metrics for immediate visual context of defensive network security engineering.

Page icon recommendation: 🛡️ (Shield representing defensive security focus and network protection).

Section icons applied: Strategic emoji use throughout for visual navigation (🔥 warm-up, 🔧 setup, 🌐 network config, 🔥 firewall, 🚪 port exposure, 🤖 automation).
