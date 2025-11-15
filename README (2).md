---
cover: .gitbook/assets/sess1-ezgif.webp
coverY: 0
coverHeight: 241
layout:
  width: wide
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

_**Standards**_ and _**Compliance**_ keep systems locked down and audit-ready—reducing risk while meeting regulatory requirements. This session is about exploring how to implement real security controls using the industry frameworks that actually matter:&#x20;

* **STIGs (Security Technical Implementation Guide)**
* **NIST (National Institute of Standards and Technology) Cybersecurity Framework**.
* **CIS (Center for Internet Security) Controls** _(formerly known as Critical Security Controls)_



I'm documenting how to harden systems against threats while maintaining the documentation and accountability that compliance teams need.

{% hint style="info" %}
**Enterprise Context** Here's what I've learned about working in corporate environments: _**you're not operating in isolation**_. The security implementations and documentation you create directly feed into **Governance, Risk, and Compliance (GRC)** workflows.&#x20;



> **Translation**: _the controls you configure and the evidence you document enable their compliance reporting and risk assessments._
{% endhint %}



Getting straight to the hands-on, by applying these security controls to production-equivalent services like **MariaDB** to see how they work in practice.

***

## 📚 Learning Objectives

By the end of Session 1, we will have explored:

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

## 🛠️ Labs to Build Here

{% hint style="warning" %}
**🎓 Professional Development**

These labs mirror actual enterprise security engineering workflows—from baseline assessment through compliance validation to production hardening. You'll document findings, implement fixes, and verify remediation using the same tools and processes required in government and corporate environments.

> **Career Impact**: _Add "STIG remediation experience" and "DoD security compliance implementation" to your resume._
{% endhint %}

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

I'm learning that even in engineering roles outside dedicated security teams, my work fundamentally affects organizational security. I'm building systems that operate 24/7—which means I need to understand robust monitoring and handoff protocols to security and network operations teams.

**Documentation matters**: GRC teams depend on my security implementation reports to maintain compliance and manage organizational risk.&#x20;
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

***
