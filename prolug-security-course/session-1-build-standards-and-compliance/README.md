---
cover: ../../.gitbook/assets/sess1-ezgif.webp
coverY: 16.331368956931055
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

# 🛡️ Session 1: Build Standards and Compliance

We will focus on **establishing security foundations** through industry standards, compliance frameworks, and practical STIG implementation.

This session introduces the cybersecurity landscape, regulatory requirements, and hands-on tools that form the foundation of enterprise Linux security engineering.

Building standards and compliance in cybersecurity engineering ensures that systems adhere to industry best practices, regulatory requirements, and security frameworks—reducing risks and vulnerabilities while maintaining accountability.

{% hint style="info" %}
**Course Philosophy**: This course is designed to secure Linux in corporate environments. You are a security administrator or engineer responsible for deploying and hardening systems to meet organizational standards.

> _Our job is to_ **implement structured security controls** _and_ **maintain compliance documentation** _that protects the organization while enabling necessary operations._
{% endhint %}

***

### 📚 Learning Objectives

By the end of Session 1, we will have explored skills in:

{% tabs %}
{% tab title="💪 Core Competencies" %}
1. **Security Frameworks**: Understanding STIGs, CIS Controls, and NIST Cybersecurity Framework
2. **Regulatory Compliance**: Industry standards when administering and building systems
3. **STIG Remediation**: Interacting with STIG remediation processes and tools
4. **Risk Management**: Understanding risk vectors to organizations and mitigation strategies
{% endtab %}

{% tab title="👔 Professional Skills" %}
5. **Documentation**: STIG remediation and security reporting for GRC teams
6. **STIG Viewer**: Downloading, reviewing, and using the STIG Viewer tool
7. **Manual Review**: Conducting security assessments against STIG baselines
8. **Compliance Reporting**: Producing documentation for governance, risk, and compliance teams
{% endtab %}
{% endtabs %}

***

### 🎯 Relevance & Context

As the shepherds of sensitive data and systems, it is the **ethical and legal duty** of individuals who administer and build these systems to protect them from malicious actors.

To be successful in securing systems, you need to thoroughly understand the cybersecurity landscape, its myriad potential threats, and the tools engineers and administrators have at their disposal.

#### What You'll Learn

<details>

<summary>🔧 <strong>Technical Skills</strong></summary>

* [ ] How security frameworks provide structured approaches to system hardening
* [ ] Using STIG Viewer to review and assess security controls
* [ ] Conducting manual security reviews against established baselines
* [ ] Understanding STIG categorization (Cat 1, Cat 2, Cat 3) and priority
* [ ] Implementing security controls in database systems (MariaDB example)
* [ ] Documenting remediation actions and justifications

</details>

<details>

<summary>💼 <strong>Professional Skills</strong></summary>

* [ ] Reporting security posture to governance, risk, and compliance (GRC) teams
* [ ] Making informed decisions when controls cannot be fully implemented
* [ ] Documenting compensating controls and risk acceptance
* [ ] Understanding organizational security requirements and standards
* [ ] Communicating technical security concepts to non-technical stakeholders
* [ ] Building repeatable processes for security assessment and remediation

</details>

{% columns %}
{% column width="58.333333333333336%" %}
#### **Why Standards & Compliance Matter**

**The Challenge**:

* Organizations face evolving cybersecurity threats daily
* Systems must meet regulatory requirements (HIPAA, PCI/DSS, etc.)
* Security controls need consistent implementation across infrastructure
* Documentation must prove compliance for audits and assessments

**The Solution**:

* **Security Frameworks** provide structured, proven approaches to hardening
* **STIGs** offer specific, actionable security requirements
* **Compliance Standards** ensure legal and regulatory obligations are met
* **Documentation** creates accountability and supports incident response



**Real-World Application**:

The concepts presented in this unit provide processes and procedures that Engineers and Sysadmins can implement to **significantly reduce the attack surface** of systems while building a system of logging and documentation in the eventuality of a security incident.
{% endcolumn %}

{% column width="41.66666666666666%" %}
{% hint style="warning" %}
**Important Context**:

STIGs tend to get out of date quickly as software evolves.&#x20;

You'll need to research current best practices and adapt controls to your specific environment.
{% endhint %}



{% hint style="info" %}
**STIG Categories**:

* **Cat 1**: Most critical—typically no open Cat 1s allowed
* **Cat 2**: Important—can be open but must be documented
* **Cat 3**: Lowest priority—often addressed with compensating controls
{% endhint %}



{% hint style="success" %}
**Career Value**:

Every job posting for Linux Sysadmins or Engineers mention concepts like Identity and Access Management (IAM), compliance frameworks, and security hardening.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

### ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 1, you should have:

**Linux Administration Fundamentals**:

* The Command Line Interface and BASH shell skills
* Installing and updating Linux system packages
* Working with command-line tools: `systemctl`, `mount`, `grep`, `ss`
* Ability to interact with basic SQL queries using MariaDB
* Text editor proficiency (vim, nano, or similar)

**Required Software**:

* STIG Viewer v2.18 (latest version)
  * Note: No Mac version available—use Linux or Windows

**Foundation Concepts**:

* Basic understanding of system security concepts
* File permissions and ownership
* Service management with systemd
* Network connectivity fundamentals
{% endhint %}

***
