---
cover: ../../.gitbook/assets/session3-ezgif.webp
coverY: 8.280315541431698
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

# 🪪 Session 3: User Access and System Integration

We will focus on **integrating Linux systems with centralized authentication** using SSSD (System Security Services Daemon) and understanding PAM (Pluggable Authentication Modules).

Active Directory (AD) and Lightweight Directory Access Protocol (LDAP) have become the industry standards because they offer centralized, scalable, and secure ways of controlling access across enterprise systems.

{% hint style="info" %}
**Enterprise Reality** As Linux administrators, we don't want to be in the business of managing users directly. Identity and Access Management (IAM) engineers handle user accounts and policies. Our job is to **integrate systems** with their centralized authentication infrastructure and **provide secure access** to our systems.

> **Key Insight**: _Local authentication doesn't scale. It's difficult to maintain and gets out of date quickly. Centralized authentication via LDAP/AD is the enterprise standard._
{% endhint %}

***

## 📚 Learning Objectives

By the end of Session 3, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="💪Core Competencies" %}
1. **Directory Services**: Understanding how LDAP and Active Directory work and why they're beneficial
2. **PAM Architecture**: Gaining foundational understanding of Pluggable Authentication Modules
3. **SSSD Integration**: Connecting Linux systems to centralized authentication using SSSD
{% endtab %}

{% tab title="👔 Professional Skills" %}
4. **Enterprise Integration**: Implementing authentication that integrates with organizational IAM infrastructure
5. **STIG Compliance**: Applying relevant STIGs to directory service integration and authentication
6. **Security Reporting**: Documenting security posture for Governance, Risk, and Compliance (GRC) teams
{% endtab %}
{% endtabs %}

***

## 🎯 Relevance & Context

&#x20;LDAP and Active Directory provide centralized authentication, authorization, and account management capabilities that far surpass local account management methods.

#### What You'll Learn

<details>

<summary>🔧 <strong>Technical Skills</strong></summary>



* [ ] How SSSD provides NSS and PAM interfaces for remote directory access
* [ ] Integrating Rocky Linux (RHEL derivative) with LDAP/Active Directory
* [ ] Implementing authentication that meets DISA STIGs and CIS Benchmarks
* [ ] Troubleshooting SSSD and directory service connectivity

</details>

<details>

<summary>👔 <strong>Professional Skills</strong></summary>

* [ ] Working with IAM teams to implement organizational authentication
* [ ] Reporting security posture to GRC teams using standardized tools
* [ ] Understanding compliance requirements for identity management
* [ ] Making informed decisions about authentication trade-offs

</details>



{% columns %}
{% column width="66.66666666666666%" %}
**Why Centralized Authentication Matters**

**The Problem with Local Authentication**:

* Gets out of date quickly
* No centralized policy enforcement
* Doesn't scale in enterprise environments
* Difficult to maintain across multiple systems





**The Solution**:

* LDAP/Active Directory provide **single source of truth** for user identities
* IAM engineers manage users and policies centrally
* Linux administrators focus on **system integration** and **secure access**
* Changes propagate automatically across all connected systems


{% endcolumn %}

{% column width="35.000000000000014%" %}
{% hint style="success" %}
**What You're Building**:&#x20;

Connecting Linux systems to organizational identity infrastructure while maintaining security compliance.&#x20;
{% endhint %}



{% hint style="info" %}
**Career Reality**: You won't spend much time setting up LDAP servers as a Linux administrator. IAM teams handle directory infrastructure.&#x20;

Your focus is on **integrating your systems** with their services and **maintaining secure access** while meeting compliance requirements.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 3, you must know:

* **Directory Navigation**: Working with Linux file system and configuration directories
* **Configuration Management**: Editing system configuration files with `vim` or other editors
* **SystemD Services**: Managing services with `systemctl` and understanding service dependencies
* **System Controls**: Using `sysctl` command for kernel parameter management
* **STIG Viewer**: Basic knowledge of STIG Viewer for security compliance assessment
* **Package Management**: Installing and managing packages via `dnf`/`yum`

**Foundation Requirement**: We should understand:

* STIG structure and remediation workflows
* Security control types and categories
* Network security and system hardening fundamentals
* Professional security documentation standards
{% endhint %}

***
