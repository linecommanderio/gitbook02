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

# session\_1\_gitbook

We will focus on **establishing security foundations** through industry standards, compliance frameworks, and practical STIG implementation.

This session introduces the cybersecurity landscape, regulatory requirements, and hands-on tools that form the foundation of enterprise Linux security engineering.

Building standards and compliance in cybersecurity engineering ensures that systems adhere to industry best practices, regulatory requirements, and security frameworks—reducing risks and vulnerabilities while maintaining accountability.

{% hint style="info" %}
**Course Philosophy**: This course is designed to secure Linux in corporate environments. You are a security administrator or engineer responsible for deploying and hardening systems to meet organizational standards.

> _Our job is to_ **implement structured security controls** _and_ **maintain compliance documentation** _that protects the organization while enabling necessary operations._
{% endhint %}

***

## 📚 Learning Objectives

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

## 🎯 Relevance & Context

As the shepherds of sensitive data and systems, it is the **ethical and legal duty** of individuals who administer and build these systems to protect them from malicious actors.

To be successful in securing systems, you need to thoroughly understand the cybersecurity landscape, its myriad potential threats, and the tools engineers and administrators have at their disposal.

### What You'll Learn

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

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 1, you must have:

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



Throughout this course, we'll cover these critical domains:

{% stepper %}
{% step %}
### Unit 1: Build Standards & Compliance

Foundation in security frameworks, STIG remediation, and compliance reporting
{% endstep %}

{% step %}
### Unit 2: Securing Network Connections

Network security architecture, firewall configuration, and traffic control
{% endstep %}

{% step %}
### Unit 3: User Access & System Integration

Identity and access management, LDAP integration, and trust authorities
{% endstep %}

{% step %}
### Unit 4: Bastion Hosts & Air Gaps

Controlled access architectures and network isolation strategies
{% endstep %}

{% step %}
### Unit 5: System Updates & Patch Cycles

Lifecycle management, vulnerability reduction, and change control
{% endstep %}

{% step %}
### Unit 6: Monitoring & Log Parsing

Log analysis, SIEM integration, and security event detection
{% endstep %}

{% step %}
### Unit 7: Monitoring & Alerting

Real-time alerting with Kafka, Loki, Grafana stack
{% endstep %}

{% step %}
### Unit 8: Configuration Drift

Baseline management, configuration monitoring, and remediation automation
{% endstep %}

{% step %}
### Unit 9: Certificates & Key Management

PKI, certificate lifecycle, and cryptographic fundamentals
{% endstep %}
{% endstepper %}

***

## 🏆 Course Objectives

By completing this course, you'll be able to demonstrate these industry-relevant competencies:

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Identity &#x26; Access Management</strong></td><td>Integrate Linux with enterprise IAM frameworks using SSSD, winbind, and trust authorities</td><td></td></tr><tr><td><strong>Security Hardening</strong></td><td>Implement industry-standard security controls and STIG compliance across Linux infrastructure</td><td></td></tr><tr><td><strong>Monitoring &#x26; Alerting</strong></td><td>Deploy enterprise monitoring solutions for 24/7 security operations support</td><td></td></tr><tr><td><strong>Configuration Management</strong></td><td>Prevent and remediate configuration drift across large-scale deployments</td><td></td></tr></tbody></table>

{% hint style="success" %}
**Resume Value**:

Instead of just listing "completed security course," you can now state specific competencies like:

* "Integrated Linux systems with enterprise identity and access management frameworks"
* "Implemented DISA STIG security baselines across production database infrastructure"

These specific skills match what employers actively seek in Linux security engineering roles.
{% endhint %}

***

## 📖 Instructional Methods

This course emphasizes **learner-centered activities** and hands-on practice:

{% columns %}
{% column width="50%" %}
**Practical Labs**

* Work with real systems and production-ready tools
* Implement security controls in actual environments
* Document your work using industry-standard formats

**Discussion Forums**

* Engage with peers and instructors
* Share different perspectives and approaches
* Learn from others' experiences and challenges

{% hint style="info" %}
**Why Discussions Matter**:

People think differently than you do. You'll pick up intangible knowledge that you can't get from blog posts or forums—you're working with actual people in an academic setting.
{% endhint %}
{% endcolumn %}

{% column width="50%" %}
**Collaborative Learning**

* Work individually or in study groups
* Join scheduled study sessions in Discord
* Get help from instructors and experienced peers

**Real-World Projects**

* Apply multiple concepts in integrated scenarios
* Build portfolio-worthy demonstrations of capability
* Solve problems similar to those in actual security roles

{% hint style="warning" %}
**Time Commitment**:

Plan for hands-on work. When you encounter problems (like missing plugins or outdated STIGs), you'll need to research solutions—this is the crux of why security engineers are valuable.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## 💻 Lab Environment Options

Choose the environment that works best for your learning style and resources:

{% tabs %}
{% tab title="Killer Coda (Recommended)" %}
**Free Online Linux Environments**

* No local setup required
* Access from any web browser
* Pre-configured with necessary tools
* Perfect for this unit's MariaDB lab

**Best For**: Quick start, minimal setup, learning on any device
{% endtab %}

{% tab title="Home Lab" %}
**Build Your Own Environment**

* Full control over configuration
* Persistent environment across sessions
* Learn infrastructure setup skills

**Recommended Distributions**:

* RHEL, Fedora, Rocky Linux, or AlmaLinux
* Red Hat family is \~85% of enterprise environments
* Government and large corporate infrastructures heavily use RHEL

**Best For**: Deep learning, building portfolio, career preparation
{% endtab %}

{% tab title="ProLUG Lab" %}
**Dedicated Course Infrastructure**

* 44 total lab machines available
* 25 systems for security engineering course
* Hammer Systems for various scenarios

**Access**: Request via Discord (not posted publicly)

**Best For**: Consistent environment, pre-configured systems, group learning
{% endtab %}
{% endtabs %}

***
