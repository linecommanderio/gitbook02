# session01 gb

## Overview

Building standards and compliance in cybersecurity engineering ensures systems adhere to industry best practices, regulatory requirements, and security frameworks—reducing organizational risk and vulnerability exposure.

This unit focuses on structured security implementation through industry-standard tools and frameworks: **STIGs** (Security Technical Implementation Guides), **CIS Controls**, and the **NIST Cybersecurity Framework**. Students will learn to maintain system resilience against evolving threats while ensuring accountability and regulatory alignment.

{% hint style="info" %}
**Enterprise Context**

You'll secure Linux systems in corporate environments where your work directly impacts organizational security posture. As a Linux engineer, you'll collaborate with **Governance, Risk, and Compliance (GRC)** teams—your technical implementations and documentation enable their compliance reporting and risk management.
{% endhint %}

Hands-on practice begins immediately with real enterprise services like **MariaDB**, applying security controls in production-equivalent scenarios.

***

## 🎯 What Labs You'll Build This Week

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

{% stepper %}
{% step %}
**Deploy MariaDB**

Install and start the service using your system package manager and verify it's listening.

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
**Analyze Security Gaps**

Use STIG benchmark requirements to identify deviations and inspect database-level settings.

{% code overflow="wrap" %}
```sql
-- Example STIG check query
SELECT user, max_user_connections FROM mysql.user;
```
{% endcode %}
{% endstep %}

{% step %}
**Remediate Critical Findings**

Address identified issues such as:

* `v-253666`: User connection limit controls
* `v-253677`: Authentication security policies
* `v-253678`: Access control mechanisms
* `v-253734`: Database security configurations

(Apply configuration and access control changes according to your STIG remediation plan and organizational policies.)
{% endstep %}

{% step %}
**Validate Implementation**

Confirm remediations and security controls are active using system and SQL verification.

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

* **The CIA Triad**: Confidentiality, Integrity, and Availability as foundational security principles
* **Security Control Types**: Administrative, technical, and physical controls working in concert
* **Risk Management Frameworks**: NIST RMF and quantitative/qualitative risk assessment methodologies
* **Regulatory Standards**: HIPAA, PCI/DSS, and industry-specific compliance requirements
* **Implementation methodologies** and documentation standards

#### Enterprise Impact

**These concepts enable you to**:

* **Structure resilient security postures** using defense-in-depth principles
* **Reduce system attack surfaces** systematically through STIG compliance and benchmarking
* **Implement comprehensive monitoring** with proper logging and incident documentation
* **Make risk-informed decisions** using ALE (Annual Loss Expectancy), SLE (Single Loss Expectancy), and ARO (Annual Rate of Occurrence)
* **Collaborate effectively** with security operations teams through clear documentation

#### Critical Thinking in Security

Security engineering requires **evaluating and defending technical decisions**. You'll practice:

* Analyzing STIG requirements to understand their protective intent
* Identifying control categories (preventive, detective, corrective, compensating)
* Defending implementation choices with risk-based justifications
* Understanding the relationship between authority, will, and force in security enforcement
{% endcolumn %}

{% column %}
{% hint style="success" %}
**Career Skills**

Even in engineering roles outside dedicated security teams, your work fundamentally affects organizational security. You'll build systems that operate 24/7—requiring robust monitoring and handoff protocols to security and network operations teams.

**Documentation matters**: GRC teams depend on your security implementation reports to maintain compliance and manage organizational risk.
{% endhint %}

{% hint style="info" %}
**Key Resources**

* **NIST Risk Management Framework**: [csrc.nist.gov/projects/risk-management](https://csrc.nist.gov/projects/risk-management/about-rmf)
* **SANS Security Policies**: Industry-standard templates and best practices
* **OpenSCAP**: Automated compliance scanning and validation
* **Cyber.mil**: Official DISA STIG repository

**Professional Development**: Understanding these frameworks positions you for roles requiring security clearances and government compliance expertise.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

Students must possess Linux Administration competencies including:

* **Command Line Proficiency**: BASH shell operations and scripting
* **Package Management**: Installing and updating system packages via DNF/YUM
* **System Tools**: `systemctl`, `mount`, `grep`, `ss`, and related utilities
* **Database Operations**: Basic SQL queries using MariaDB
* **Required Software**: STIG Viewer v2.18 (latest version)
{% endhint %}
