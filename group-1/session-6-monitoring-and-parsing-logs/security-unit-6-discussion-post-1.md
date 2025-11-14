---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
  metadata:
    visible: true
---

# Security Unit 6 Discussion Post 1

## Security Unit 6 Discussion Post 1

Review chapter 15 of the SRE book: [Chapter 15 Investigating Systems](https://google.github.io/building-secure-and-reliable-systems/raw/ch15.html#collect_appropriate_and_useful_logs)

There are 14 references at the end of the chapter. Follow them for more information.

{% hint style="info" %}
**Key Resource**

[What does debugging a program look like?](https://jvns.ca/blog/2019/06/23/a-few-debugging-resources/) should be reviewed for question "3".
{% endhint %}

***

## 📚 Research References

1. [Chapter 15 Investigating Systems →](https://google.github.io/building-secure-and-reliable-systems/raw/ch15.html#collect_appropriate_and_useful_logs)
2. [Logging Cheat Sheet →](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
3. [OWASP Top 10 Security Logging and Monitoring Failures Explained →](https://www.securityjourney.com/post/owasp-top-10-security-logging-and-monitoring-failures-explained)
4. [C9: Implement Security Logging and Monitoring →](https://top10proactive.owasp.org/the-top-10/c9-security-logging-and-monitoring/)
5. [A09:2021 – Security Logging and Monitoring Failures →](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/)
6. [NIST SP 800-92r1 Cybersecurity Log Management Planning Guide →](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-92r1.ipd.pdf)
7. [NIST Special Publication 800-92 Guide to Computer Security Log Management →](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-92.pdf)
8. [Guidance on NIST 800-171 log retention →](https://consultdts.com/article/nist-800-171-log-retention/)
9. [What does debugging a program look like? →](https://jvns.ca/blog/2019/06/23/a-few-debugging-resources/)

***

## 1️⃣ <mark style="color:$danger;">What are some concepts that are new to you?</mark>

{% hint style="success" %}
**Key Learning**

Learning that influential institutions have created frameworks that establish different philosophies of doing things, it provides different vantage aspects that drive how operation is to produce.
{% endhint %}

Understanding that major institutions like Google (SRE), OWASP (web application security), and NIST (federal/enterprise standards) have developed complementary yet distinct approaches to logging and investigation provides valuable perspectives on operational excellence.

Each framework reflects its organizational context:

* 🏢 **Google SRE**: Cloud-scale operations and reliability engineering
* 🌐 **OWASP**: Application security and vulnerability prevention
* 🏛️ **NIST**: Government compliance and enterprise governance

***

## 2️⃣ <mark style="color:$danger;">There are 5 conclusions drawn, do you agree with them? Would you add or remove anything from the list?</mark>

{% hint style="info" %}
**Analysis Overview**

All five conclusions are well aligned with industry best practices for security-relevant logging and investigations, including recommended approaches by SRE, security engineering, and compliance frameworks. Each addresses a core challenge found in investigating, debugging, and incident response missions.
{% endhint %}

### The Five Core Conclusions

{% stepper %}
{% step %}
#### Design your logging to be immutable

* **Principle**: Logging systems should make it difficult (ideally, impossible) to alter existing log entries, and all modifications must be traceable via audit trails.
* **Implementation**: Remote and centralized logging hardens security against attackers seeking to cover their tracks.
* **Why It Matters**: Without immutable logs, attackers can erase evidence of compromise, making incident investigation and forensic analysis impossible.
{% endstep %}

{% step %}
#### Take privacy into consideration

* **Principle**: Logging design must include privacy protection measures, comply with regulations, and engage organizational stakeholders.
* **Balance Required**: Logs should balance investigative needs with legal and privacy obligations.
* **Critical Consideration**: Improper logging of PII (Personally Identifiable Information) can create compliance violations and legal liability.
{% endstep %}

{% step %}
#### Determine which security logs to retain

* **Selection Criteria**: Choose which logs are most useful for investigations according to their signal-to-noise ratio.
* **Avoid Logging Noise**: Exclude excess unrewarding logs such as routine firewall blocks that provide no investigative value.
* **Retention Requirements**: Understand retention requirements to ensure you have the data necessary to investigate security incidents.
{% endstep %}

{% step %}
#### Budget for logging

* **Resource Reality**: Logging consumes storage, computation resources, and budget.
* **Planning Required**: Organizations must explicitly plan for log storage, processing, and retention.
* **Optimization Strategies**: Use strategies like warm/cold storage tiers and log rotation to manage costs effectively.
{% endstep %}

{% step %}
#### Robust, secure debugging access

* **Security Imperative**: Debugging systems are privileged and must be secured against unauthorized access or misuse, especially in security investigations.
* **Emergency Access Design**: Emergency access mechanisms should be designed carefully and alert on use.
* **Monitoring Requirement**: All debug access must be logged and monitored for potential abuse.
{% endstep %}
{% endstepper %}

***

## Framework Comparison: Google SRE vs OWASP vs NIST

{% hint style="success" %}
**Key Finding**

The five conclusions from Google's logging guidance overlap strongly with both OWASP and NIST recommendations, though each framework emphasizes some areas differently.
{% endhint %}

### Detailed Comparison by Principle

{% tabs %}
{% tab title="🔒 Immutable Logging" %}
#### Immutable Logging & Integrity

**Google SRE**

* Immutability and audit trails are essential for reliable logs
* Focus on centralized, remote logging
* Emphasis on tamper-proof architectures

**OWASP**

* Recommends protecting log integrity (against tampering/modification)
* Ensures only authorized personnel can alter logs
* Focus on application-level log protection

**NIST**

* Advocates storage/transfer safeguards
* Explicit access control requirements
* Architecture for entire log lifecycle

**Common Ground**: All three stress strong controls to ensure logs cannot be altered undetected, supporting audit trails and forensic accountability.
{% endtab %}

{% tab title="🔐 Privacy" %}
#### Privacy & Data Protection

**Google SRE**

* Must protect privacy
* Comply with legal obligations
* Handle PII carefully in logging design

**OWASP**

* Don't log sensitive/illegal data
* Follow legal/compliance requirements for PII
* Protect sensitive fields explicitly

**NIST**

* Explicit requirement to address sensitive information
* PII handling in use/baseline
* Regulated data compliance focus

**Common Ground**: Each warns about sensitive data exposure—the need to avoid logging PII/credentials unless authorized, and to comply with existing laws/regulations.
{% endtab %}

{% tab title="📅 Retention Strategy" %}
#### Log Retention & Selection

**Google SRE**

* Log data selection guided by investigative value
* Retention driven by compliance needs
* Signal-to-noise ratio optimization

**OWASP**

* Log only what's needed for security/investigation
* Adhere to legal retention periods
* Securely dispose after retention

**NIST**

* Process for defining what to log and why
* Retention periods driven by risk
* Legal and operational needs balance

**Common Ground**: Log what is useful (not everything), retain as needed for investigations and compliance, and securely purge when no longer required.
{% endtab %}

{% tab title="💰 Budget & Resources" %}
#### Budget & Resource Planning

**Google SRE**

* Plan for log volume, storage, operational costs
* Warm/cold storage tiers
* Explicit cost modeling

**OWASP**

* Acknowledges risks related to log growth/exhaustion
* Suggests log rotation
* Alerting for full storage

**NIST**

* Detailed inventory of infrastructure
* Explicit planning for storage growth
* Cloud migration and cold storage strategies

**Common Ground**: All acknowledge that logging has real costs and requires explicit planning for storage, processing, and retention resources.
{% endtab %}

{% tab title="🔧 Debug Access" %}
#### Debugging & Emergency Access

**Google SRE**

* Limits emergency/debug access strongly
* Monitors use for investigation reliability
* Focus on operational security

**OWASP**

* Restricts debug logging to avoid sensitive exposure
* Reviewing/alerting on critical events
* Production environment protection

**NIST**

* Structured inventory of roles
* Policies for emergency access
* Incident/forensic handling support

**Common Ground**: All demand monitoring systems to detect suspicious events and log failures, and emergency or debug access is noted as a sensitive area for abuse.
{% endtab %}
{% endtabs %}

***

### Key Similarities Across All Frameworks

<details>

<summary>🎯 Comprehensive Commonalities</summary>

**1. Integrity/Immutability**

All three stress strong controls to ensure logs cannot be altered undetected, supporting audit trails and forensic accountability.

**2. Confidentiality/Privacy**

Each warns about sensitive data exposure—the need to avoid logging PII/credentials unless authorized, and to comply with existing laws/regulations.

**3. Event Selection/Retention**

Log what is useful (not everything), retain as needed for investigations and compliance, and securely purge when no longer required.

**4. Monitoring/Alerting**

All demand monitoring systems to detect suspicious events and log failures, and emergency or debug access is noted as a sensitive area for abuse.

**5. Comprehensive Policy**

Each encourages organizations to set explicit policies for log management, including incident response, storage, and resource planning.

</details>

***

### Nuances and Distinctions

{% hint style="warning" %}
**Framework-Specific Emphases**

While the frameworks agree on fundamentals, each brings unique perspectives based on their organizational context.
{% endhint %}

**Google SRE Framework**

The Google framework is more explicit about **centralized logging** and storage optimization (budgeting, cold/warm storage tiers), while OWASP/NIST discuss these as operational best practices.

* Focus on **scale** and **operational efficiency**
* Emphasis on **cost optimization** at cloud scale
* Practical guidance for **distributed systems**

**NIST Guidance**

NIST guidance stands out for championing organization-wide policy/process maturity, detailed inventories, and explicit, iterative improvement cycles—taking log management beyond mere tooling implementation toward resilient, governed practices.

* Emphasis on **governance** and **compliance**
* **Enterprise-wide** policy frameworks
* **Federal/regulated** environment focus
* Detailed **audit requirements**

**OWASP Approach**

OWASP is more focused on web/app log guidance and specific anti-patterns; NIST sets the broader context for enterprise/federal adoption.

* **Application security** perspective
* **Web-specific** vulnerability prevention
* **Developer-focused** implementation guidance
* **Attack pattern** awareness

***

## 3️⃣ <mark style="color:$danger;">In Julia Evan’s debugging blog, which shows that debugging is just another form of troubleshooting: What useful things do you learn about the relationship between these topics? /</mark> <mark style="color:$danger;"></mark><mark style="color:$danger;">Are there any techniques you already do that this helps solidify for you?</mark>

{% hint style="success" %}
**Core Insight**

**Logging standards exist to enable the debugging workflow at scale**. The relationship is symbiotic: effective debugging requires good logs, and understanding debugging workflows informs what logs should capture.
{% endhint %}

### Key Relationships Between Debugging and Logging

**1. Assumptions vs. Reality**

Debugging is largely about discovering that assumptions you were "sure" were true turn out to be false. Without tamper-proof logs, you can't confidently answer "what actually happened?" versus "what I think happened".

{% code title="Example: Authentication Failure Investigation" overflow="wrap" lineNumbers="true" %}
```bash

# Assumption: "SSH works fine, must be user error"

# Reality discovered through logs:

# /var/log/secure
Jan 15 14:23:45 server sshd[1234]: Failed password for admin from 10.0.1.50
Jan 15 14:23:45 server sshd[1234]: PAM 2 more authentication failures
Jan 15 14:23:46 server sshd[1234]: maximum authentication attempts exceeded

# /var/log/audit/audit.log
type=USER_AUTH msg=audit(1642262625.123:456): user pid=1234 uid=0 auid=4294967295 
ses=4294967295 subj=system_u:system_r:sshd_t:s0-s0:c0.c1023 
msg='op=PAM:authentication grantors=? acct="admin" exe="/usr/sbin/sshd" 
hostname=10.0.1.50 addr=10.0.1.50 terminal=ssh res=failed'

# Discovery: Account locked due to failed attempts, not user credentials
```
{% endcode %}

**2. Reproducibility Requirements**

The debugging workflow starts with reproduction, ideally reproducible quickly. If you can't reproduce an incident because logs were purged or never captured, the debugging loop fails at step one.

{% hint style="danger" %}
**Critical Failure Mode**

Without sufficient log retention, intermittent issues become uninvestigable. The debugging loop requires data from the failure point—if logs are purged or never captured, you're debugging blind.
{% endhint %}

**3. Explicit Error Messages**

Debuggable code provides clear error messages explaining exactly what happened.

| Silent Failure          | Debuggable Logging                                                                             |
| ----------------------- | ---------------------------------------------------------------------------------------------- |
| "Error occurred"        | "Connection refused to LDAP server dc1.example.com:389 - timeout after 5s"                     |
| "Authentication failed" | "PAM authentication failed for user 'admin': SSSD backend timeout (error code 110)"            |
| "System error"          | "Failed to mount /dev/sdb1 at /data: filesystem type 'xfs' not supported by kernel"            |
| "Cannot connect"        | "TLS handshake failed with api.service.com:443 - certificate expired (valid until 2024-12-31)" |

Silent failures are investigation dead-ends; explicit logging enables the debugging loop.

**4. Single Variable Experiments**

Debugging experiments require changing one variable and observing results. Without precise ordering, correlating "I changed X" with "Y happened" becomes guesswork, especially in distributed systems.

{% code title="Example: Timestamp Correlation" overflow="wrap" lineNumbers="true" %}
```bash

# Change made at precise timestamp
2025-01-15 14:23:42.123 [CONFIG] firewalld: zone public updated - added port 8080/tcp

# Immediate effect observed
2025-01-15 14:23:42.456 [NETFILTER] iptables: ACCEPT tcp dpt:8080 src:10.0.1.0/24
2025-01-15 14:23:43.001 [APP] nginx: successfully bound to 0.0.0.0:8080

# Timestamp correlation enables causal analysis
```
{% endcode %}

**5. Scale: Development vs. Production**

The debugging techniques (experiments, checking assumptions, information gathering) are exactly what security operations and incident response perform using logs. The difference is scale: development debugging uses print statements and debuggers; production "debugging" uses centralized log management, SIEM correlation, and automated detection.

***

### Techniques This Solidifies

{% stepper %}
{% step %}
#### Why Context in Logs Matters

**Problem**: Minimal logs ("error occurred") are useless for debugging

**Solution**: Rich context enables hypothesis formation

{% code title="Minimal vs. Rich Logging" overflow="wrap" lineNumbers="true" %}
```python

# ❌ Useless minimal logging
logging.error("Database operation failed")

# ✅ Rich contextual logging
logging.error(
    "Database operation failed",
    extra={
        "operation": "INSERT",
        "table": "users",
        "user_id": user.id,
        "database": "production",
        "error_code": e.pgcode,
        "error_message": str(e),
        "transaction_id": txn_id,
        "duration_ms": elapsed_time * 1000,
        "retry_count": retry_attempts
    }
)
```
{% endcode %}
{% endstep %}

{% step %}
#### Correlation as a Debugging Tool

**Principle**: Observability correlation shows how metrics, logs, and traces together enable the debugging loop in production.

**Implementation**: You're building the "experiments" infrastructure for future-you investigating incidents.

**Three Pillars of Observability**:

* 📊 **Metrics**: What's happening (quantitative)
* 📝 **Logs**: Why it's happening (qualitative)
* 🔍 **Traces**: How it's happening (causal)

{% hint style="info" %}
Modern observability platforms correlate these three data types using shared identifiers (trace IDs, transaction IDs) to enable rapid root cause analysis.
{% endhint %}
{% endstep %}

{% step %}
#### Error Handling Design = Investigation Design

**Pattern Recognition**: The `failure` library pattern (error chains with context) is what application developers should log, and what your PAM/SSSD/SELinux investigations depend on.

{% code title="Error Chain Example" overflow="wrap" lineNumbers="true" %}
```bash

# Authentication failure chain - each layer adds context
SSH login rejected 
  ↳ PAM auth failed (pam_sss.so)
    ↳ SSSD timeout 
      ↳ LDAP connection refused to dc1.example.com:389
        ↳ Network unreachable (no route to host)
```
{% endcode %}

When authentication fails, you need the complete chain of causation. This is debugging-informed logging design.
{% endstep %}

{% step %}
#### "It's Probably Your Code" = "Log Your Systems, Not Just Vendors"

**Developer Wisdom**: It's usually your code, not the library.

**Infrastructure Translation**: For infrastructure, this means logging your configurations, policy decisions, and orchestration logic—not just relying on vendor logs (RHEL audit, Cisco syslog, etc.).

**What to Log**:

* ✅ Your automation scripts and their decisions
* ✅ Your configuration management state changes
* ✅ Your policy enforcement actions
* ✅ Your orchestration layer operations
* ❌ Just vendor default logs without context
{% endstep %}

{% step %}
#### "Understand Error Messages" = Know Your Log Schemas

**Developer Requirement**: Developers must learn what Python's `NameError` means.

**Operator Requirement**: Operators must understand what "upstream connect error" or "PAM error: System error" actually indicate.

{% hint style="success" %}
**Validation**: This validates investing time in vendor documentation (Red Hat, IBM, Cisco—your research pattern).

Understanding log schemas and error codes is not optional—it's the foundation of effective investigation.
{% endhint %}

**Example: Common PAM Error Codes**

* `PAM_SUCCESS (0)`: Successful function return
* `PAM_OPEN_ERR (1)`: Cannot open dynamic library
* `PAM_SYMBOL_ERR (2)`: Symbol not found
* `PAM_SERVICE_ERR (3)`: Error in service module
* `PAM_SYSTEM_ERR (4)`: System error
* `PAM_AUTH_ERR (7)`: Authentication failure
* `PAM_PERM_DENIED (6)`: Permission denied
{% endstep %}

{% step %}
#### Fast Feedback Loops = Accessible Logs

**Developer Experience**: Fast feedback enables rapid iteration

**Operations Translation**: If it takes 20 minutes to query last hour's logs, your investigation "experiment loop" is crippled.

{% hint style="warning" %}
**Performance Requirement**

This is why you evaluate SIEM architectures, data lakes, and cold storage strategies. Log accessibility directly impacts MTTD (Mean Time To Detect) and MTTR (Mean Time To Resolve).
{% endhint %}

**Target Performance**:

* 🎯 Query last 1 hour of logs: **< 5 seconds**
* 🎯 Query last 24 hours: **< 30 seconds**
* 🎯 Query last 7 days: **< 2 minutes**
* 🎯 Query last 30 days: **< 5 minutes** (acceptable for historical analysis)
{% endstep %}
{% endstepper %}

***

## 📝 Key Takeaways

{% hint style="info" %}
**Summary: Security Logging & Debugging Integration**

#### Framework Alignment

All major frameworks (Google SRE, OWASP, NIST) agree on core principles:

* 🔒 **Immutability**: Logs must be tamper-proof
* 🔐 **Privacy**: Protect PII and comply with regulations
* 📊 **Selectivity**: Log what matters, not everything
* 💰 **Budget**: Plan for storage and processing costs
* 🔧 **Security**: Control and monitor privileged access

#### The Debugging Connection

**Logging exists to enable debugging at production scale:**

* Rich context enables hypothesis formation
* Immutability ensures reliable evidence
* Correlation supports causal analysis
* Fast access enables rapid iteration
* Error chains guide investigation paths

#### Practical Application

**For effective logging and investigation:**

✅ **DO:**

* Design logs with debugging workflows in mind
* Include rich context in every log entry
* Correlate across metrics, logs, and traces
* Understand vendor log schemas and error codes
* Optimize for fast query response times
* Log your orchestration logic and decisions

❌ **DON'T:**

* Log sensitive data without explicit authorization
* Rely solely on vendor default logging
* Sacrifice context for brevity
* Allow logs to be modified without audit trails
* Neglect storage planning and cost management
* Skip correlation identifiers (trace IDs, transaction IDs)

#### The Golden Rule

**"Good logging enables good debugging; good debugging teaches good logging."**

The relationship is circular and reinforcing—understanding one improves the other.
{% endhint %}

***

## 🔗 Additional Resources

**Google SRE**

* [Building Secure and Reliable Systems - Chapter 15 →](https://google.github.io/building-secure-and-reliable-systems/raw/ch15.html)

**OWASP Security Logging**

* [Logging Cheat Sheet →](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
* [OWASP Top 10: A09:2021 Security Logging Failures →](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/)

**NIST Guidelines**

* [NIST SP 800-92r1 Cybersecurity Log Management →](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-92r1.ipd.pdf)
* [NIST SP 800-92 Guide to Computer Security Log Management →](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-92.pdf)

**Debugging Resources**

* [What does debugging a program look like? - Julia Evans →](https://jvns.ca/blog/2019/06/23/a-few-debugging-resources/)
