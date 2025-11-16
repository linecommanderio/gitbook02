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

By the end of Session 1, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="💪 Core Competencies" %}
1. **Security Frameworks**: Understanding STIGs, CIS Controls, and NIST Cybersecurity Framework
2. **Regulatory Compliance**: Industry standards when administering and building systems
3. **STIG Remediation**: Interacting with STIG remediation processes and tools
4. **Risk Management**: Understanding risk vectors to organizations and mitigation strategies
{% endtab %}

{% tab title="🔧 Professional Skills" %}
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

* How security frameworks provide structured approaches to system hardening
* Using STIG Viewer to review and assess security controls
* Conducting manual security reviews against established baselines
* Understanding STIG categorization (Cat 1, Cat 2, Cat 3) and priority
* Implementing security controls in database systems (MariaDB example)
* Documenting remediation actions and justifications

</details>

<details>

<summary>💼 <strong>Professional Skills</strong></summary>

* Reporting security posture to governance, risk, and compliance (GRC) teams
* Making informed decisions when controls cannot be fully implemented
* Documenting compensating controls and risk acceptance
* Understanding organizational security requirements and standards
* Communicating technical security concepts to non-technical stakeholders
* Building repeatable processes for security assessment and remediation

</details>

{% columns %}
{% column width="58%" %}
**Why Standards & Compliance Matter**

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

The concepts presented in this unit play a pivotal role in organizing and structuring a resilient security posture against threats to enterprise and organizational entities.

They provide processes and procedures that engineers and administrators can implement to **significantly reduce the attack surface** of systems while building a system of logging and documentation in the eventuality of a security incident.

{% hint style="success" %}
**Industry Reality**: Everything you do as a Linux engineer affects the security posture of your organization. Understanding frameworks and standards ensures your work aligns with organizational security objectives.
{% endhint %}
{% endcolumn %}

{% column width="42%" %}
{% hint style="warning" %}
**Important Context**:

STIGs tend to get out of date quickly as software evolves. You'll need to research current best practices and adapt controls to your specific environment.
{% endhint %}

{% hint style="info" %}
**STIG Categories**:

* **Cat 1**: Most critical—typically no open Cat 1s allowed
* **Cat 2**: Important—can be open but must be documented
* **Cat 3**: Lowest priority—often addressed with compensating controls
{% endhint %}

{% hint style="success" %}
**Career Value**:

Security engineering skills are highly valued. Every job posting for Linux administrators mentions concepts like identity and access management, compliance frameworks, and security hardening.

_These are the skills that make you worth what you're worth in the industry._
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

## 🔑 Key Terms & Definitions

Understanding these fundamental concepts is essential for success in enterprise security engineering:

{% tabs %}
{% tab title="Core Concepts" %}
**CIA Triad**\
The three pillars of information security:

* **Confidentiality**: Ensuring data is accessible only to authorized users
* **Integrity**: Maintaining accuracy and completeness of data
* **Availability**: Ensuring systems and data are accessible when needed

**Security Frameworks**\
Structured approaches to implementing security controls:

* **CIS** (Center for Internet Security): Community-developed security standards
* **STIG** (Security Technical Implementation Guide): DoD security configuration standards
* **NIST**: National Institute of Standards and Technology frameworks
{% endtab %}

{% tab title="Compliance" %}
**Regulatory Compliance**\
Legal and regulatory requirements for data protection and system security

**HIPAA** (Health Insurance Portability and Accountability Act)\
Healthcare data protection requirements

**PCI/DSS** (Payment Card Industry Data Security Standard)\
Requirements for organizations handling credit card transactions

**Industry Standards**\
Sector-specific security requirements and best practices
{% endtab %}

{% tab title="Risk Management" %}
**Risk Vectors**\
Potential pathways for security threats to exploit systems

**Compensating Controls**\
Alternative security measures when primary controls cannot be implemented

**Risk Acceptance**\
Documented decision to accept risk when mitigation isn't feasible

**Security Posture**\
Overall security status and readiness of an organization's systems
{% endtab %}
{% endtabs %}

***

## 🎓 Course Structure & Expectations

{% hint style="info" %}
**Course Delivery Format**

This course combines:

* **Weekly lectures** (recorded for flexibility)
* **Hands-on labs** with real systems
* **Discussion posts** (2 per week—required for certificate)
* **Practical projects** demonstrating applied skills

You don't have to attend live lectures, but you must:

* Watch all recordings
* Complete all labs and worksheets
* Participate in discussions
* Complete the final project
{% endhint %}

### Major Instructional Areas

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

## 📝 This Week's Activities

### Lab 1: MariaDB STIG Review & Remediation

{% hint style="info" %}
**Practical Focus**: You'll work with a real database service (MariaDB) to conduct security assessment and implement controls.
{% endhint %}

**Lab Objectives**

{% stepper %}
{% step %}
### Download STIG Viewer

Install the latest STIG Viewer tool and familiarize yourself with its interface
{% endstep %}

{% step %}
### Import MariaDB STIG

Load the appropriate STIG baseline for your MariaDB version
{% endstep %}

{% step %}
### Manual Review Process

Work through each control, checking current configuration against STIG requirements
{% endstep %}

{% step %}
### Implement Fixes

Apply remediation actions where technically feasible
{% endstep %}

{% step %}
### Document Exceptions

For controls that can't be implemented, document compensating controls or risk acceptance
{% endstep %}

{% step %}
### Generate Report

Produce compliance documentation suitable for GRC team review
{% endstep %}
{% endstepper %}

<details>

<summary><strong>Common Challenges You'll Encounter</strong></summary>

**Missing Plugins**\
You may find required PAM authentication plugins aren't present in your MariaDB installation. This is a real-world scenario—research the solution and document your approach.

**Version Mismatches**\
STIGs can be outdated. You'll need to adapt controls to your specific software version.

**Plugin Locations**\
Default plugin paths may differ from STIG expectations. Learn to locate plugins and create appropriate symlinks.

**Example Research Path**:

{% code title="Example commands" %}
```bash
# Search for missing plugin
find / -name "auth_pam.so" 2>/dev/null

# Check what package provides it
dnf whatprovides */auth_pam.so

# Verify MariaDB plugin directory
SELECT @@plugin_dir;
```
{% endcode %}

</details>

{% hint style="warning" %}
**Important Note**:

You're not required to fix all findings in this first lab. The goal is to **understand the process**:

1. Review security requirements
2. Assess current state
3. Implement what you can
4. Document why you can't implement certain controls
5. Report findings appropriately

_This is the same process you'll use in enterprise environments._
{% endhint %}

### Discussion Participation

**Week 1 Discussion Posts** (Required for Certificate):

1. **Initial Response**: Answer the weekly discussion question based on your learning
2. **Peer Engagement**: Respond meaningfully to at least one classmate's post

{% hint style="info" %}
**Discussion Value**:

You'll discover that others think differently than you do. You'll gain insights you can't get from documentation alone. This collaborative learning is essential in security engineering—you'll work with diverse teams throughout your career.
{% endhint %}

***

## 🎯 Success Criteria

By the end of Session 1, you should be able to:

* [ ] Explain the purpose and importance of security frameworks
* [ ] Navigate STIG Viewer to review security controls
* [ ] Conduct a manual security assessment of a Linux service
* [ ] Understand STIG categorization and prioritization
* [ ] Implement basic security controls where technically feasible
* [ ] Document findings, exceptions, and justifications
* [ ] Generate compliance reports for organizational review
* [ ] Articulate risk management decisions to stakeholders

{% hint style="success" %}
**Moving Forward**:

Session 1 establishes the foundation for all subsequent units. Master these concepts—they'll be applied throughout the course as we build increasingly sophisticated security architectures.
{% endhint %}

***

## 📚 External Resources

{% tabs %}
{% tab title="Community" %}
**Discord Server**

* Primary communication platform
* Study sessions and peer support
* Instructor Q\&A and office hours
* Course announcements and updates

Join the Professional Linux Users Group Discord for full course participation.
{% endtab %}

{% tab title="Tools" %}
**STIG Viewer**

* Download: https://public.cyber.mil/stigs/srg-stig-tools/
* Version: 2.18 (or latest available)
* Platforms: Windows, Linux (no Mac version)

**Lab Environments**

* https://killercoda.com/ - Free online Linux terminals
* ProLUG Lab access via Discord
{% endtab %}

{% tab title="Documentation" %}
**Security Frameworks**

* https://www.nist.gov/cyberframework
* https://www.cisecurity.org/controls
* https://public.cyber.mil/stigs/

**MariaDB Security**

* https://mariadb.com/kb/en/security/
* https://mariadb.com/kb/en/authentication-plugin-pam/
{% endtab %}
{% endtabs %}

***

## ⚡ Getting Started Checklist

Before beginning the lab work:

* [ ] Join the Discord server for course communication
* [ ] Download and install STIG Viewer v2.18
* [ ] Choose your lab environment (Killer Coda, Home Lab, or ProLUG Lab)
* [ ] Set up MariaDB on your chosen platform
* [ ] Review the course syllabus and schedule
* [ ] Introduce yourself in the Discord #introductions channel
* [ ] Review the first week's discussion questions

{% hint style="info" %}
**Next Steps**:

Once you've completed the checklist above, dive into Lab 1. Don't worry about getting everything perfect—focus on understanding the process. You'll improve with practice throughout the course.
{% endhint %}
