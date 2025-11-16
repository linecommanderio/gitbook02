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

# session4\_gitbook

We will focus on **controlling system connectivity** through air-gapped architectures and bastion host implementations. This session explores how to limit exposure, create strong security boundaries, and establish controlled access points into isolated environments.

Air-gapped systems and bastion hosts are proven strategies for managing risk in enterprise environments. They focus on limiting exposure by creating physical and logical boundaries that support broader security architecture.

{% hint style="info" %}
**Enterprise Reality**: In production environments, completely isolating systems isn't practical—they need controlled access. Our job is to **design secure boundaries** and **implement controlled entry points** that maintain security while enabling necessary operations.

> **Key Insight**: _True air gaps are rare. Most "air-gapped" systems have defined, heavily monitored connection points. Bastions are how we enforce those controlled connections._
{% endhint %}

***

## 📚 Learning Objectives

By the end of Session 4, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="💪Core Competencies" %}
1. **Air-Gapped Systems**: Understanding physical isolation strategies and when they're appropriate
2. **Bastion Architecture**: Implementing controlled entry/egress points for isolated systems
3. **User Jailing**: Using chroot environments to restrict user capabilities
4. **Process Isolation**: Understanding namespaces, cgroups, and containment techniques
{% endtab %}

{% tab title="💡 Professional Skills" %}
5. **Security Architecture**: Designing network boundaries that balance security and operations
6. **Access Control**: Automating user restrictions and session management
7. **Compliance Integration**: Applying DISA STIGs to isolated system architectures
8. **Risk Management**: Making informed decisions about isolation vs. accessibility trade-offs
{% endtab %}
{% endtabs %}

***

## 🎯 Relevance & Context

Bastions and air gaps are strategies for controlling how systems connect—or don't connect—to the outside world. As organizations grow, protecting critical systems becomes more challenging. These proven architectural patterns offer ways to limit exposure and manage access securely.

#### What You'll Learn

<details>

<summary>🔧 <strong>Technical Skills</strong></summary>

* [ ] How to architect air-gapped network segments with defined boundaries
* [ ] Building bastion hosts that serve as controlled entry points
* [ ] Implementing chroot jails to restrict user and process capabilities
* [ ] Using network isolation to create defensible system boundaries
* [ ] Deploying automated jailing mechanisms for incoming connections

</details>

<details>

<summary>💡 <strong>Professional Skills</strong></summary>

* [ ] Designing security boundaries without creating operational barriers
* [ ] Creating network architecture documentation for stakeholders
* [ ] Understanding trade-offs between security and operational efficiency
* [ ] Implementing defense-in-depth strategies at the network level
* [ ] Communicating risk decisions to security and compliance teams

</details>

{% columns %}
{% column %}
**Why Controlled Isolation Matters**

**The Challenge**:

* Critical systems need protection from direct network exposure
* Complete isolation prevents necessary access and operations
* Unrestricted access creates unacceptable risk
* Multiple entry points increase attack surface

**The Solution**:

* **Air-gapped networks** physically separate critical systems
* **Bastions** provide the _only_ controlled entry/egress points
* **User jailing** restricts what authenticated users can do
* **Network boundaries** are clearly defined and monitored

**Real-World Application**: Think of Helm's Deep from _The Lord of the Rings_—there were only two entry points into a heavily fortified position. Defenders could focus resources on those choke points. That's exactly how bastion architecture works: force all traffic through defined, heavily monitored and restricted access points.
{% endcolumn %}

{% column %}
{% hint style="success" %}
**What You're Building**:

A complete bastion system that restricts incoming users to pre-defined actions, automatically jails their sessions, and provides controlled access to isolated networks.
{% endhint %}

{% hint style="warning" %}
**Important Context**:

"Air-gapped" systems in practice are rarely _completely_ isolated. Most have controlled connection points for necessary operations.
{% endhint %}

{% hint style="info" %}
**Career Reality**:

You'll design network boundaries and implement bastions more than you'll build truly air-gapped systems. Understanding both concepts helps you make informed architectural decisions and communicate risk appropriately.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 4, you must know:

* **Directory Navigation**: Working with Linux filesystem structure and paths
* **Configuration Management**: Editing system files with `vim` or other editors
* **SystemD Services**: Managing and restarting services with `systemctl`
* **SSH Configuration**: Understanding `/etc/ssh/sshd_config` structure and options
* **Bash Scripting**: Basic scripting with loops, conditionals, and case statements
* **Package Management**: Installing tools via `dnf`/`yum`/`apt`

**Foundation Requirement**: We should understand:

* User and group management fundamentals
* File permissions and ownership concepts
* Network connectivity and addressing basics
* Process management and troubleshooting
* STIG compliance assessment methodology
{% endhint %}

***
