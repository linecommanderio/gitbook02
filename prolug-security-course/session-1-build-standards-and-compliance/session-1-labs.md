---
cover: ../../.gitbook/assets/lab-ezgif.png
coverY: -220.75630741768234
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

# 🛠️ Session 1 Labs

I'm building three progressively complex labs that take me from security assessment through compliance tooling to production hardening.

<details>

<summary>🏗️ What We'll Build</summary>

A **security compliance validation workflow** for enterprise Linux systems:

* [ ] System reconnaissance capabilities to understand security-relevant configurations
* [ ] STIG-based hardening implementation for database services
* [ ] Professional security baseline documentation and remediation processes

</details>

### ⏳ Lab Progression

{% stepper %}
{% step %}
#### **🔍 System Reconnaissance** → Discover your system's current security posture
{% endstep %}

{% step %}
#### **🛡️ STIG Compliance** → Build your security assessment environment
{% endstep %}

{% step %}
#### **🎯 Database Hardening** → Implement security controls on live database infrastructure
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**What Makes This Real**

These aren't simulations—I'm using STIG Viewer 2.18 (the official DoD tool), actual STIG benchmarks, and production-equivalent database infrastructure.

> _We're performing actual system security assessments using Defense Information Systems Agency (DISA) Security Technical Implementation Guides (STIGs)._
{% endhint %}

By the end, we'll understand:

* The **reconnaissance methodology** (system exploration, configuration discovery, security parameter validation)
* The **compliance architecture** (STIGs, security controls classification, remediation verification)

More importantly, we'll know how to systematically assess and harden Linux systems against documented security baselines.

***

### 🛠️️ Labs to Build Here

{% stepper %}
{% step %}
#### **🔍 System Security Baseline Assessment**

**Discover your system's current security posture.**

Analyze critical security configurations by examining:

* Filesystem Security: Mount options (`noexec`, `nodev`, `nosuid`) preventing privilege escalation attacks
* Network Configuration: IPv4/IPv6 forwarding rules, routing capabilities, and interface hardening
* Firewall Infrastructure: Packet filtering tables and network isolation mechanisms

{% code overflow="wrap" %}
```bash
# Assessment commands you'll master
mount | grep -i noexec
sysctl -a | grep -i ipv4 | grep -i forward
lsmod | grep -i tables
```
{% endcode %}

{% hint style="success" %}
✅ Outcome: Comprehensive understanding of existing system security controls and potential vulnerabilities
{% endhint %}
{% endstep %}

{% step %}
#### **🛠️ STIG Compliance Toolkit Setup**

**Build your security assessment environment.**

Configure industry-standard security tools:

* Install STIG Viewer 2.18: Official DoD security assessment and validation tool
* Import Security Benchmarks: MariaDB STIG for database-specific compliance validation
* Establish Professional Workflow: Review → Remediate → Document → Verify

<details>

<summary>📋 Why STIG Viewer Matters</summary>

STIG Viewer is the industry-standard tool for government and high-security enterprise environments. Learning this tool makes you immediately productive in:

* DoD contractor positions
* Federal government IT roles
* Fortune 500 security engineering teams
* Compliance-heavy industries (finance, healthcare, defense)

</details>

{% hint style="success" %}
✅ Outcome: Production-ready security compliance assessment capability using enterprise tools
{% endhint %}
{% endstep %}

{% step %}
#### **🔐 MariaDB Security Hardening**

**Implement hands-on security controls on live database infrastructure.**

Real-world security engineering workflow:

{% stepper %}
{% step %}
**Deploy MariaDB**

Deploy MariaDB with secure installation procedures.
{% endstep %}

{% step %}
**Analyze Security Gaps**

Analyze security gaps using STIG benchmark requirements.
{% endstep %}

{% step %}
**Remediate Critical Findings**

Remediate critical findings such as:

* v-253666: User connection limit controls
* v-253677: Authentication security policies
* v-253678: Access control mechanisms
* v-253734: Database security configurations
{% endstep %}

{% step %}
**Validate Implementation**

Validate implementation using SQL queries and system verification.
{% endstep %}
{% endstepper %}

{% tabs %}
{% tab title="📦 Installation" %}
{% code title="installation.sh" %}
```bash
dnf install mariadb-server
systemctl start mariadb
systemctl status mariadb
ss -ntulp | grep 3306
```
{% endcode %}
{% endtab %}

{% tab title="🛡️ STIG Validation" %}
{% code title="stig-check.sql" %}
```sql
-- Example STIG check query
SELECT user, max_user_connections FROM mysql.user;
```
{% endcode %}
{% endtab %}

{% tab title="✅ Verification" %}
{% code title="verification.sh" %}
```bash
# Confirm security controls active
systemctl status mariadb
mysql -e "SHOW VARIABLES LIKE '%connection%';"
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="success" %}
✅ Outcome: Hardened database service meeting enterprise security standards with documented compliance evidence
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="info" %}
🎓 Professional Development

These labs mirror actual enterprise security engineering workflows—from baseline assessment through compliance validation to production hardening.

> _You'll document findings, implement fixes, and verify remediation using the same tools and processes required in government and corporate environments._

**Career Impact**: Add these skills to your project section on your resume:

* **"STIG remediation experience"**
* **"DoD security compliance implementation"**
{% endhint %}
