---
cover: ../../.gitbook/assets/session5-ezgif.webp
coverY: -11.904238249272332
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

# 🔄 Session 5: Updating systems and patch cycles

We will focus on **controlling software origins and updates** through repository management and systematic patching strategies.

This session explores how to maintain system stability and security by controlling where software comes from, when it's updated, and how patches are applied across enterprise infrastructure.

Package repositories and patching lifecycles are proven strategies for managing risk in production environments. They focus on controlling the introduction of change by creating trusted software pipelines and systematic update procedures.

{% hint style="info" %}
**Enterprise Reality**: In production environments, controlling software sources is about **trust and stability**.

> _Our job is to_ **design trusted software pipelines** _and_ **implement systematic patching procedures** _that maintain security while ensuring operational continuity._
{% endhint %}

***

## 📚 Learning Objectives

By the end of Session 5, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="💪 Core Competencies" %}
1. **Package Integrity**: Understanding software supply chain security and verification
2. **Repository Management**: Creating, configuring, and securing local package repositories
3. **Enterprise Patching**: Implementing systematic patching strategies across infrastructure
4. **Lifecycle Engineering**: Managing software versions and update schedules
{% endtab %}

{% tab title="👔 Professional Skills" %}
5. **Supply Chain Security**: Protecting against compromised software sources
6. **Change Management**: Balancing stability with necessary updates
7. **Compliance Integration**: Applying DISA STIGs to package management practices
8. **Risk Assessment**: Making informed decisions about patching timing and scope
{% endtab %}
{% endtabs %}

***

## 🎯 Relevance & Context

For security engineers, **controlling the origin and integrity of software updates is foundational** for minimizing attack surfaces and maintaining system stability.

Every patch applied to a system must come from a known and trusted source, as introducing changes into a stable environment can have significant consequences.

By managing internal repositories and applying strict control over packages, organizations can enforce compliance, prevent supply chain attacks, and ensure only trusted, audited software enters production environments.

#### What You'll Learn

<details>

<summary>🔧 <strong>Technical Skills</strong></summary>

* How to create and share local package repositories over the network
* Configuring repository sources and GPG key verification
* Building enterprise patching frameworks with staged deployment
* Implementing version locking and dependency management
* Automating repository updates and synchronization
* Understanding package lifecycles and support considerations

</details>

<details>

<summary>🎓 <strong>Professional Skills</strong></summary>

* Designing software supply chain security controls
* Creating change management procedures for system updates
* Balancing security updates with operational stability
* Communicating patching schedules and risk to stakeholders
* Implementing defense-in-depth for software integrity
* Understanding support implications of third-party packages

</details>

{% columns %}
{% column width="58%" %}
#### **Why Software Control Matters**

**The Challenge**:

* Systems need regular security updates to address vulnerabilities
* Uncontrolled patching can introduce instability or breakage
* Direct internet access for updates creates security risks
* Third-party software may lack vendor support
* Large-scale patching requires systematic approaches

**The Solution**:

* **Local repositories** provide controlled, trusted software sources
* **Staged patching** tests updates before production deployment
* **Version locking** maintains consistency across infrastructure
* **Lifecycle management** balances security with stability
* **Automated workflows** enable safe large-scale updates



**Real-World Application**: Think of pharmaceutical supply chains—every drug must pass through verified distribution channels with tracked provenance.

* Regulatory authorities can audit the entire chain from manufacturer to patient.

That's exactly how enterprise software management works: _**ensure every package comes from trusted sources with verifiable integrity, creating an auditable path from upstream vendor to production deployment.**_
{% endcolumn %}

{% column width="42%" %}
{% hint style="success" %}
**What You're Building**:

A complete enterprise patching framework with local repositories, staged deployment procedures, and automated update workflows that balance security with stability.
{% endhint %}

{% hint style="warning" %}
**Important Context**:

"Enterprise patching" differs fundamentally from desktop updates.&#x20;

It requires testing, scheduling, rollback capabilities, and coordination across large-scale infrastructure.
{% endhint %}

{% hint style="info" %}
**Career Reality**:

You'll spend more time **managing patching schedules** and **controlling software sources** than you will troubleshooting individual packages.

Understanding systematic approaches and their trade-offs is essential for enterprise operations.
{% endhint %}

{% hint style="warning" %}
**Critical Insight**: When we patch systems, we're constantly balancing these two concepts:

* **Fit for Use**: Does the patch apply successfully?
* **Fit for Warranty**: Will the patched system continue working reliably?

This tension drives our need for **systematic testing** and **staged deployment** strategies.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 5, you must know:

* **Package Management Basics**: Understanding `dnf`/`yum`/`apt` package managers
* **Configuration File Editing**: Managing files in `/etc/yum.repos.d/`
* **HTTP Server Basics**: Understanding web server concepts for repository hosting
* **File System Navigation**: Working with mount points and directory structures
* **SystemD Services**: Managing and configuring system services
* **Network Configuration**: Understanding repository access over networks

**Foundation Requirement**: We should understand:

* User and group management fundamentals
* File permissions and ownership concepts
* Network connectivity and firewall basics
* STIG compliance assessment methodology
* Basic Ansible concepts and playbook structure
{% endhint %}

***
