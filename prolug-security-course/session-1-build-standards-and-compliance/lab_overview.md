# lab\_overview

```bash
mount | grep -i noexec
sysctl -a | grep -i ipv4 | grep -i forward
lsmod | grep -i tables
```

{% stepper %}
{% step %}
{% hint style="success" %}
**✅ Outcome**: Comprehensive understanding of existing system security controls and potential vulnerabilities
{% endhint %}
{% endstep %}

{% step %}
**🛠️ STIG Compliance Toolkit Setup**

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
**🔐 MariaDB Security Hardening**

**Implement hands-on security controls on live database infrastructure**

Real-world security engineering workflow:

{% stepper %}
{% step %}
#### Deploy MariaDB

Deploy MariaDB with secure installation procedures.
{% endstep %}

{% step %}
#### Analyze Security Gaps

Analyze security gaps using STIG benchmark requirements.
{% endstep %}

{% step %}
#### Remediate Critical Findings

Remediate critical findings such as:

* v-253666: User connection limit controls
* v-253677: Authentication security policies
* v-253678: Access control mechanisms
* v-253734: Database security configurations
{% endstep %}

{% step %}
#### Validate Implementation

Validate implementation using SQL queries and system verification.
{% endstep %}
{% endstepper %}

{% tabs %}
{% tab title="Installation" %}
{% code title="installation.sh" %}
```bash
dnf install mariadb-server
systemctl start mariadb
systemctl status mariadb
ss -ntulp | grep 3306
```
{% endcode %}
{% endtab %}

{% tab title="STIG Validation" %}
{% code title="stig-check.sql" %}
```sql
-- Example STIG check query
SELECT user, max_user_connections FROM mysql.user;
```
{% endcode %}
{% endtab %}

{% tab title="Verification" %}
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
**✅ Outcome**: Hardened database service meeting enterprise security standards with documented compliance evidence
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
**🎓 Professional Development**

These labs mirror actual enterprise security engineering workflows—from baseline assessment through compliance validation to production hardening. You'll document findings, implement fixes, and verify remediation using the same tools and processes required in government and corporate environments.

> **Career Impact**: _Add "STIG remediation experience" and "DoD security compliance implementation" to your resume._
{% endhint %}
