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

# Session 5: Updating systems and patch cycles

## Overview

Where software originates—and how and when it is updated (patched)—is essential to maintaining system stability and security. Every patch applied to a system must come from a known and trusted source, as introducing changes into a stable environment can have significant consequences. Administrators and engineers ensure that patching is planned and scheduled using verified, trackable repositories and resources.

In this unit, I will examine how this process is implemented in adjacent distributions, where administrators can apply granular control to Red Hat Package Manager (RPM) packages and maintain internal repositories of vetted packages.

{% hint style="warning" %}
**Control is Everything**

If I can't control what software gets onto my systems and how it gets there, I don't have a security posture—I have hope. Control over software sources is the foundation of system stability and security.
{% endhint %}

***

## 🎯 Learning Objectives

By the end of Unit 5, I will:

{% stepper %}
{% step %}
### Understand the importance of package integrity and controlled software sources

* Why control over software sources is paramount for security
* Cryptographic verification of packages (GPG keys)
* Supply chain security and trusted sources
* Fit for use vs. fit for warranty concepts
{% endstep %}

{% step %}
### Understand patching techniques and routines for enterprise environments

* How enterprise patching differs from desktop "update now"
* Life cycle engineering and staged deployments
* Testing methodologies across dev → test → production
* Risk-based patch prioritization
{% endstep %}

{% step %}
### Understand automated methods of patching at scale

* Ansible automation for fleet-scale patch deployment
* Pre-checks, fact gathering, and validation
* Reboot management and service verification
* Rollback capabilities and error handling
{% endstep %}

{% step %}
### Understand custom internal package repositories and their management

* Creating ISO-based and RPM-based repositories
* Sharing repositories over HTTP with Apache
* Configuring systems to use controlled repo sources
* EPEL considerations in enterprise environments
{% endstep %}
{% endstepper %}

<details>

<summary>📋 What Each Objective Covers (expanded)</summary>

**1. Package Integrity & Control**

* Why control over software sources is paramount for security
* Cryptographic verification of packages (GPG keys)
* Supply chain security and trusted sources
* Fit for use vs. fit for warranty concepts

**2. Patching Techniques & Routines**

* How enterprise patching differs from desktop "update now"
* Life cycle engineering and staged deployments
* Testing methodologies across dev → test → production
* Risk-based patch prioritization

**3. Automated Patching Methods**

* Ansible automation for fleet-scale patch deployment
* Pre-checks, fact gathering, and validation
* Reboot management and service verification
* Rollback capabilities and error handling

**4. Custom Internal Repositories**

* Creating ISO-based and RPM-based repositories
* Sharing repositories over HTTP with Apache
* Configuring systems to use controlled repo sources
* EPEL considerations in enterprise environments

</details>

***

## 🔍 Relevance and Context

{% hint style="success" %}
**Security Engineering Foundation**

For security engineers, controlling the origin and integrity of software updates is a foundational practice for minimizing attack surfaces. Repository control isn't just convenience—it's a security function.
{% endhint %}

### Why This Matters

Think of repository management like a bastion host for software. Just as I control who can access systems through a bastion, I control what software can enter systems through controlled repositories.

**In enterprise environments:**

* 🏢 Air-gapped systems require local repositories (government, classified environments)
* 🔒 Controlled entry points prevent unauthorized software
* 📋 Auditable changes maintain compliance and security posture
* ⚖️ Staged deployments protect production systems from untested changes

By managing internal repositories and applying strict control over RPM packages, organizations can:

| Capability                           | Security Benefit                             |
| ------------------------------------ | -------------------------------------------- |
| 📦 **Enforce compliance**            | Ensure only approved packages enter systems  |
| 🛡️ **Prevent supply chain attacks** | Control and verify all software sources      |
| 🔒 **Audit software provenance**     | Track complete history of installed packages |
| ✅ **Maintain production integrity**  | Only trusted, vetted software in production  |

***

## 🏗️ What I'll Build This Week

This unit has two major hands-on components:

**1. Build and Manage Repositories**

* Create local repositories from ISO files
* Share repositories over HTTP using Apache
* Configure systems to use controlled repo sources
* Understand repository metadata and structure

**2. Enterprise Patching Framework**

* Implement automated patching with Ansible
* Manage fleet-scale updates (hundreds to thousands of systems)
* Control patching life cycles across environments
* Handle pre-checks, validation, and reboots

***

## 💡 Key Concepts I'll Learn

### Fit for Use vs. Fit for Warranty

{% tabs %}
{% tab title="✅ Fit for Use" %}
**The Engineer's Checklist**

Fit for use is about initial functionality—does it work right now?

For a web server:

* ✅ Is the server serving?
* ✅ Is it running on ports 80 and 443?
* ✅ Is it serving from the correct document root?
* ✅ Does it respond to requests?

**Engineers love this**: Check, check, check, check. Nice and structured.
{% endtab %}

{% tab title="🏆 Fit for Warranty" %}
**The Weekend Test**

Fit for warranty is about sustained operational excellence:

**Do I bet my weekend that this won't fail?**

* Will it handle the 5,001st connection?
* Will it keep serving under load?
* Will it continue working without intervention?
* Am I confident enough to go home?

**This is what matters**: A system might pass all initial checks but fail under real-world conditions.
{% endtab %}
{% endtabs %}

***

### Enterprise Patching vs. Desktop "Update Now"

| Desktop Patching                 | Enterprise Patching                       |
| -------------------------------- | ----------------------------------------- |
| Click "Update Now"               | Inventory 980+ systems before starting    |
| Single device                    | Fleet of hundreds/thousands               |
| Hope it works                    | Controlled life cycle across environments |
| Reboot when convenient           | Scheduled maintenance windows             |
| One-off process                  | Automated orchestration required          |
| Minimal testing                  | Dev → Test → QA → Staging → Production    |
| Personal inconvenience if broken | Business-critical impact if broken        |

{% hint style="info" %}
**The Fundamental Difference**

One-off patching is "click and hope." Enterprise patching is about inventory management, state tracking, controlled life cycles, staged deployments, comprehensive testing, and automated orchestration across hundreds or thousands of systems.
{% endhint %}

***

## 📚 Prerequisites

To be successful, I should have a working understanding of:

{% hint style="info" %}
**Required Foundation Skills**

* ✅ Basic directory navigation in Linux
* ✅ Ability to edit and manage configuration files
* ✅ Basic knowledge of STIG (Security Technical Implementation Guides)
* ✅ Basic knowledge of Ansible automation
* ✅ Understanding of network concepts (HTTP, client-server)
{% endhint %}
