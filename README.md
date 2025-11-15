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

# Session 1: Build Standards and Compliance

## Overview

Building standards and compliance in cybersecurity engineering ensures systems adhere to industry best practices, regulatory requirements, and security frameworks—reducing organizational risk and vulnerability exposure.

This unit focuses on structured security implementation through industry-standard tools and frameworks: **STIGs** (Security Technical Implementation Guides), **CIS Controls**, and the **NIST Cybersecurity Framework**. Students will learn to maintain system resilience against evolving threats while ensuring accountability and regulatory alignment.

{% hint style="info" %}
**Enterprise Context**

You'll secure Linux systems in corporate environments where your work directly impacts organizational security posture. As a Linux engineer, you'll collaborate with **Governance, Risk, and Compliance (GRC)** teams—your technical implementations and documentation enable their compliance reporting and risk management.
{% endhint %}

Hands-on practice begins immediately with real enterprise services like **MariaDB**, applying security controls in production-equivalent scenarios.

***

## 🎯 What You'll Build This Week

{% stepper %}
{% step %}
#### 🔍 System Security Baseline Assessment

**Discover your system's current security posture**

Analyze critical security configurations by examining:

* **Filesystem Security**: Mount options (`noexec`, `nodev`, `nosuid`) preventing privilege escalation attacks
* **Network Configuration**: IPv4/IPv6 forwarding rules, routing capabilities, and interface hardening
* **Firewall Infrastructure**: Packet filtering tables and network isolation mechanisms

{% code overflow="wrap" %}
```bash
# Assessment commands you'll master
mount | grep -i noexec
sysctl -a | grep -i ipv4 | grep -i forward
lsmod | grep -i tables
```
{% endcode %}

{% hint style="success" %}
**✅ Outcome**: Comprehensive understanding of existing system security controls and potential vulnerabilities
{% endhint %}
{% endstep %}

{% step %}
#### 🛠️ STIG Compliance Toolkit Setup

**Build your security assessment environment**

Configure industry-standard security tools:

* **Install STIG Viewer 2.18**: Official DoD security assessment and validation tool
* **Import Security Benchmarks**: MariaDB STIG for database-specific compliance validation
* **Establish Professional Workflow**: Review → Remediate → Document → Verify

<details>

<summary>📋 Why STIG Viewer Matters</summary>

STIG Viewer is the **industry-standard tool** for government and high-security enterprise environments. Learning this tool makes you immediately productive in:

* DoD contractor positions
* Federal government IT roles
* Fortune 500 security engineering teams
* Compliance-heavy industries (finance, healthcare, defense)

</details>

{% hint style="success" %}
**✅ Outcome**: Production-ready security compliance assessment capability using enterprise tools
{% endhint %}
{% endstep %}

{% step %}
#### 🔐 MariaDB Security Hardening

**Implement hands-on security controls on live database infrastructure**

Real-world security engineering workflow implemented as a sequence:

{% stepper %}
{% step %}
### Deploy MariaDB

Secure installation procedures and service deployment.

{% code overflow="wrap" %}
```bash
dnf install mariadb-server
systemctl start mariadb
systemctl status mariadb
ss -ntulp | grep 3306
```
{% endcode %}
{% endstep %}

{% step %}
### Analyze Security Gaps

Compare the deployment against STIG benchmark requirements to identify gaps.

{% code overflow="wrap" %}
```sql
-- Example STIG check query
SELECT user, max_user_connections FROM mysql.user;
```
{% endcode %}
{% endstep %}

{% step %}
### Remediate Critical Findings

Address and remediate benchmark findings, examples include:

* v-253666: User connection limit controls
* v-253677: Authentication security policies
* v-253678: Access control mechanisms
* v-253734: Database security configurations
{% endstep %}

{% step %}
### Validate Implementation

Verify remediations with SQL queries and system checks.

{% code overflow="wrap" %}
```bash
# Confirm security controls active
systemctl status mariadb
mysql -e "SHOW VARIABLES LIKE '%connection%';"
```
{% endcode %}
{% endstep %}
{% endstepper %}

{% hint style="success" %}
**✅ Outcome**: Hardened database service meeting enterprise security standards with documented compliance evidence
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
**🎓 Professional Development**

These labs mirror actual enterprise security engineering workflows—from baseline assessment through compliance validation to production hardening. You'll document findings, implement fixes, and verify remediation using the same tools and processes required in government and corporate environments.

**Career Impact**: Add "STIG remediation experience" and "DoD security compliance implementation" to your resume.
{% endhint %}

***

## 📚 Learning Objectives

By the end of Unit 1, students will demonstrate competency in:

{% tabs %}
{% tab title="Core Competencies" %}
1. **Security Frameworks**: STIGs, CIS Controls, NIST Cybersecurity Framework
2. **Regulatory Compliance**: Industry standards for system administration and deployment
3. **STIG Remediation**: Hands-on security control implementation and validation
{% endtab %}

{% tab title="Professional Skills" %}
4. **Risk Management**: Understanding and addressing organizational risk vectors
5. **Documentation**: Security reporting practices for compliance teams
{% endtab %}
{% endtabs %}

***

## 🎯 Relevance & Context

{% columns %}
{% column width="60%" %}
#### Your Professional Responsibility

As administrators of sensitive data and critical systems, we carry both **ethical and legal obligations** to protect organizational assets from malicious actors.

**Success requires thorough understanding of**:

* The cybersecurity threat landscape
* Available security tools and frameworks
* Implementation methodologies
* Documentation and reporting requirements

#### Enterprise Impact

**These concepts enable you to**:

* Structure resilient security postures
* Reduce system attack surfaces systematically
* Implement comprehensive logging and monitoring
* Prepare proper incident documentation
* Collaborate effectively with security operations teams
{% endcolumn %}

{% column %}
{% hint style="success" %}
**Career Skills**

Even in engineering roles outside dedicated security teams, your work fundamentally affects organizational security. You'll build systems that operate 24/7—requiring robust monitoring and handoff protocols to security and network operations teams.

**Documentation matters**: GRC teams depend on your security implementation reports to maintain compliance and manage organizational risk.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

Students must possess Linux Administration competencies including:

* **Command Line Proficiency**: BASH shell operations and scripting
* **Package Management**: Installing and updating system packages via `dnf`/`yum`
* **System Tools**: `systemctl`, `mount`, `grep`, `ss`, and related utilities
* **Database Operations**: Basic SQL queries using MariaDB
* **Required Software**: STIG Viewer v2.18 (latest version)
{% endhint %}
