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

# 📢 Security Unit 6 Discussion Post 2

## Security Unit 6 Discussion Post 2

Read [Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)

***

### 📚 Research References

1. [Monitoring Distributed Systems →](https://sre.google/sre-book/monitoring-distributed-systems/)
2. [SRE Metrics: Core SRE Components, the Four Golden Signals & SRE KPIs →](https://www.splunk.com/en_us/blog/learn/sre-metrics-four-golden-signals-of-monitoring.html)
3. [4 SRE Golden Signals (What they are and why they matter) →](https://firehydrant.com/blog/4-sre-golden-signals-what-they-are-and-why-they-matter/)
4. [What are the Golden Signals? →](https://developer.cisco.com/articles/what-are-the-golden-signals/what-are-the-golden-signals-that-sre-teams-use-to-detect-issues/)
5. [Mastering Reliability: The 4 Golden Signals SRE Metrics →](https://pflb.us/blog/4-golden-signals-sre/)
6. [What are the Best Practices for Logging? →](https://www.parseable.com/blog/logging-best-practices)
7. [An Engineer's Checklist of Logging Best Practices →](https://www.dash0.com/guides/logging-best-practices)
8. [Logging in Distributed Systems →](https://www.geeksforgeeks.org/system-design/logging-in-distributed-systems/)
9. [9 Logging Best Practices You Should Know →](https://www.dash0.com/guides/logging-best-practices)

***

### 1️⃣ <mark style="color:$danger;">What interesting or new things do I learn in this reading?</mark>

{% hint style="info" %}
**Key Learning Areas**

This reading provides critical insights into Google's production monitoring philosophy, the Four Golden Signals framework, and best practices for managing alert fatigue in distributed systems.
{% endhint %}

#### Eight Core Insights

**1. The Four Golden Signals**

The chapter establishes "latency, traffic, errors, and saturation" as the critical four signals every system should monitor. This prioritization helps teams avoid drowning in metrics and focuses on what most reliably indicates overall system health and user experience.

**2. Symptoms vs. Causes Distinction**

Google's approach emphasizes paging for symptoms (what is broken from a user's perspective) over causes (why it is broken). This reduces alert noise—pages only go off when users are truly impacted, while dashboards and logs are used for deeper debugging and analysis.

**3. Alert Fatigue and Actionable Paging**

The text underlines the real cost of paging humans, highlighting how alert fatigue can lead to critical incidents being missed. Every page should be urgent, actionable, and novel; rote or non-actionable pages should be automated away or eliminated.

{% hint style="warning" %}
**Critical Principle**

Every page should be urgent, actionable, and novel. If it's not all three, it shouldn't page a human—it should be automated or removed.
{% endhint %}

**4. Combining Black-box and White-box Monitoring**

It clarifies the difference and utility of black-box (external, symptom-based) and white-box (internal, cause-based) monitoring, arguing for a mix. For most organizations, the prescriptive advice about leveraging both and emphasizing symptoms on alerts is not always the status quo.

<details>

<summary>📊 Black-box vs. White-box Monitoring</summary>

| Type          | Perspective             | Use Case                    | Example                                      |
| ------------- | ----------------------- | --------------------------- | -------------------------------------------- |
| **Black-box** | External, symptom-based | Alerting on user impact     | HTTP probe failing from customer perspective |
| **White-box** | Internal, cause-based   | Debugging and investigation | Database connection pool exhaustion          |

**Best Practice**: Alert on black-box symptoms, debug with white-box insights.

</details>

**5. Engineering for Simplicity**

The chapter warns against overly complex monitoring rules and hierarchies. It advocates for simple, robust rules for incident detection and maintaining separate, loosely coupled systems for monitoring versus deep debugging.

**6. Understanding "the Tail"**

Beyond mean values, the book spotlights the importance of monitoring the tail of distributions (e.g., 99th percentile latency), especially in distributed systems. This is critical for user experience but often overlooked in systems focused only on averages.

**7. Long-term Health Tradeoffs**

It acknowledges that sometimes lowering short-term alerting sensitivity or tolerating some availability risk is the only way to create space for systemic, long-term improvement. This is illustrated with real-world Google case studies (Bigtable, Gmail), showing how over-alerting leads to burnout and technical debt, and sometimes requires temporarily "backing off" to solve core problems.

**8. Guidance for Monitoring Rules**

The chapter provides a set of self-diagnostic questions to vet whether a monitoring rule should generate a page. These include whether the condition is urgent, actionable, user-visible, and not redundant with other alerts.

{% hint style="success" %}
**Self-Diagnostic Questions for Alerts**

Before creating an alert, ask:

* ✅ Is this condition **urgent**?
* ✅ Is it **actionable** (can someone do something about it now)?
* ✅ Is it **user-visible** (does it impact customers)?
* ✅ Is it **not redundant** with other alerts?

If the answer to any is "no," reconsider the alert.
{% endhint %}

***

### 2️⃣ <mark style="color:$danger;">What are the "4 golden signals"?</mark>

{% tabs %}
{% tab title="🚀 Latency" %}
#### Latency

**Definition**: The time it takes to serve a request, representing responsiveness and user experience.

**What It Measures**:

* Response time for successful requests
* Response time for failed requests (track separately!)

**Why It Matters**:

* Directly impacts user experience
* Slow responses often indicate deeper problems
* Tail latency (p99, p99.9) shows worst-case user experience

**Example Metrics**:

* HTTP request duration
* Database query latency
* API response time

{% hint style="warning" %}
**Important Distinction**

Track latency separately for successful vs. failed requests. A fast error is still an error, but the latency characteristics differ from slow success.
{% endhint %}
{% endtab %}

{% tab title="📊 Traffic" %}
#### Traffic

**Definition**: The amount of demand on the system, typically measured by the number of requests or the volume of data processed.

**What It Measures**:

* Request rate (requests per second)
* Bandwidth utilization
* Transaction volume

**Why It Matters**:

* Establishes baseline load patterns
* Detects unusual demand spikes or drops
* Helps capacity planning

**Example Metrics**:

* HTTP requests per second
* Database transactions per second
* Network I/O in bytes per second

{% hint style="info" %}
Traffic patterns reveal system behavior. Sudden drops can be as concerning as sudden spikes—both indicate something changed.
{% endhint %}
{% endtab %}

{% tab title="❌ Errors" %}
#### Errors

**Definition**: The rate of failed requests, capturing reliability and availability issues.

**What It Measures**:

* Explicit failures (HTTP 5xx, exceptions)
* Implicit failures (HTTP 200 with wrong content)
* Policy violations (requests taking too long)

**Why It Matters**:

* Directly indicates service reliability
* Impacts user trust and satisfaction
* Often the most actionable signal

**Example Metrics**:

* HTTP 5xx error rate
* Failed database queries
* Exception counts by type

{% hint style="danger" %}
**Critical Monitoring**

Errors are often the clearest signal that something is broken. Monitor error rates, not just counts—a 1% error rate at 1000 RPS is more critical than 100 errors at 10 RPS.
{% endhint %}
{% endtab %}

{% tab title="⚡ Saturation" %}
#### Saturation

**Definition**: How "full" your service is, indicating resource utilization and risk of overloading.

**What It Measures**:

* CPU utilization
* Memory usage
* Disk I/O capacity
* Network bandwidth
* Queue depths

**Why It Matters**:

* Predicts future problems before they impact users
* Guides capacity planning
* Indicates when scaling is needed

**Example Metrics**:

* CPU utilization percentage
* Memory utilization percentage
* Disk queue depth
* Thread pool occupancy

{% hint style="success" %}
**Leading Indicator**

Saturation is a leading indicator—it predicts problems before they become user-visible. Monitor it closely to prevent incidents.
{% endhint %}
{% endtab %}
{% endtabs %}

***

#### The Four Golden Signals: Quick Reference

| Signal           | What It Measures     | Alert When                    | Example                     |
| ---------------- | -------------------- | ----------------------------- | --------------------------- |
| 🚀 **Latency**   | Response time        | p99 latency exceeds threshold | API response time > 500ms   |
| 📊 **Traffic**   | Request volume       | Unusual spikes or drops       | Requests drop by 50%        |
| ❌ **Errors**     | Failure rate         | Error rate exceeds threshold  | 5xx errors > 1% of requests |
| ⚡ **Saturation** | Resource utilization | Resources near capacity       | CPU > 80% for 5+ minutes    |

***

### 3️⃣ <mark style="color:$danger;">Why is immutability so important to logging?</mark>

{% hint style="success" %}
**Core Principle**

**Immutability** is central to logging because it ensures logs remain a trustworthy, tamper-proof record of system events, enabling accurate troubleshooting, compliance auditing, and root-cause analysis.
{% endhint %}

#### The Critical Importance of Immutability

If logs could be edited or deleted after creation, incidents or security breaches could be hidden, and historical traces would lose credibility. Immutability preserves historical integrity and strongly supports forensic investigations by guaranteeing that each log entry reflects exactly what happened at a specific time.

**Why Immutability Matters**:

| Without Immutability               | With Immutability                    |
| ---------------------------------- | ------------------------------------ |
| ❌ Attackers can cover their tracks | ✅ Complete audit trail of all events |
| ❌ Incidents can be hidden          | ✅ Reliable forensic evidence         |
| ❌ Compliance audits fail           | ✅ Regulatory compliance achieved     |
| ❌ Root cause analysis compromised  | ✅ Trustworthy investigation data     |
| ❌ Historical data unreliable       | ✅ Accurate time-series analysis      |

***

#### 🔧 Other Required Items for Effective Logging

{% hint style="info" %}
**Holistic Approach**

Effective logging combines immutable records with these best practices so that when incidents occur, teams have reliable, actionable insights for rapid recovery and audit trails for accountability.
{% endhint %}

#### Seven Essential Logging Requirements

{% stepper %}
{% step %}
**Structured Logging**

Consistency (like using JSON or a standard schema) makes searching, filtering, and automated analysis feasible, especially across distributed systems.

{% code title="Example: Structured vs. Unstructured Logging" overflow="wrap" lineNumbers="true" %}
```json
// ❌ Unstructured (hard to parse)
"User john@example.com logged in from 192.168.1.100 at 2025-01-15 14:23:45"

// ✅ Structured (easily queryable)
{
  "timestamp": "2025-01-15T14:23:45.123Z",
  "event": "user_login",
  "user": "john@example.com",
  "source_ip": "192.168.1.100",
  "success": true,
  "session_id": "abc123xyz"
}
```
{% endcode %}
{% endstep %}

{% step %}
**Contextual Metadata**

Including timestamps, request IDs, user IDs, and service names enables event correlation and supports debugging distributed flows.

<details>

<summary>📋 Essential Metadata Fields</summary>

**Required in Every Log Entry**:

* ✅ **Timestamp** (with timezone, preferably UTC)
* ✅ **Service/Component Name**
* ✅ **Log Level** (DEBUG, INFO, WARN, ERROR, FATAL)
* ✅ **Request/Trace ID** (for distributed tracing)

**Recommended**:

* User ID or session identifier
* Source IP address
* Environment (production, staging, etc.)
* Version/build number
* Host/container identifier

</details>
{% endstep %}

{% step %}
**Centralization**

Aggregating logs in a single location simplifies search, visualization, and incident response.
{% endstep %}

{% step %}
**Log Rotation and Retention Policies**

Managing storage by archiving or deleting old logs prevents resource exhaustion and supports regulatory or business requirements for data history.

| Retention Period | Use Case               | Storage Tier           |
| ---------------- | ---------------------- | ---------------------- |
| 7-30 days        | Active investigation   | Hot (fast access)      |
| 30-90 days       | Recent troubleshooting | Warm (moderate access) |
| 90 days - 1 year | Compliance/audit       | Cold (slow access)     |
| 1+ years         | Long-term compliance   | Archive (rare access)  |
{% endstep %}

{% step %}
**Real-Time Monitoring and Alerting**

Streaming logs and setting up alerts lets teams detect anomalies or failures as they occur, speeding up problem detection and recovery.

{% hint style="warning" %}
**Integration with Four Golden Signals**

Log-based alerts should complement metric-based alerts. Use logs for context when metrics indicate a problem.
{% endhint %}
{% endstep %}

{% step %}
**Appropriate Log Levels**

Categorizing log entries by severity (INFO, WARN, ERROR, etc.) clarifies priority, allowing teams to focus on the most critical issues first.

| Level     | Use Case                               | When to Use                              |
| --------- | -------------------------------------- | ---------------------------------------- |
| **DEBUG** | Detailed diagnostic info               | Development and deep troubleshooting     |
| **INFO**  | General informational messages         | Normal operations, significant events    |
| **WARN**  | Potentially harmful situations         | Degraded performance, deprecated usage   |
| **ERROR** | Error events that don't stop execution | Handled exceptions, recoverable failures |
| **FATAL** | Severe errors causing termination      | Unrecoverable failures, system crashes   |
{% endstep %}

{% step %}
**Avoiding Sensitive Data**

Scrubbing or encrypting personally identifiable or secret information prevents leaks and supports compliance with privacy regulations.

{% hint style="danger" %}
**Never Log**

* ❌ Passwords or API keys
* ❌ Credit card numbers or PAN data
* ❌ Social security numbers
* ❌ Personal health information (PHI)
* ❌ Unencrypted authentication tokens

**Redact or Hash Instead**: If you must log user identifiers, use one-way hashing or tokenization.
{% endhint %}
{% endstep %}
{% endstepper %}

***

### 📝 Key Takeaways

{% hint style="success" %}
**Summary: Monitoring & Logging Best Practices**

#### Four Golden Signals

* 🚀 **Latency**: Response time and user experience
* 📊 **Traffic**: System demand and load patterns
* ❌ **Errors**: Reliability and failure rates
* ⚡ **Saturation**: Resource utilization and capacity

#### Immutability in Logging

* **Why**: Ensures trustworthy, tamper-proof records
* **Benefit**: Reliable forensics and compliance
* **Requirement**: Must be combined with other logging best practices

#### Effective Logging Requirements

1. Structured format (JSON, standard schema)
2. Rich contextual metadata (timestamps, request IDs)
3. Centralized aggregation
4. Retention and rotation policies
5. Real-time monitoring and alerting
6. Appropriate severity levels
7. Sensitive data protection

#### Alert Design Principles

* Page on **symptoms**, not causes
* Every alert must be **urgent, actionable, and novel**
* Avoid alert fatigue through careful rule design
* Use **black-box monitoring** for alerting, **white-box** for debugging
{% endhint %}

***

### 🔗 Additional Resources

**Google SRE Book**

* [Monitoring Distributed Systems →](https://sre.google/sre-book/monitoring-distributed-systems/)

**Four Golden Signals Deep Dives**

* [SRE Metrics: Core SRE Components →](https://www.splunk.com/en_us/blog/learn/sre-metrics-four-golden-signals-of-monitoring.html)
* [4 SRE Golden Signals Explained →](https://firehydrant.com/blog/4-sre-golden-signals-what-they-are-and-why-they-matter/)

**Logging Best Practices**

* [What are the Best Practices for Logging? →](https://www.parseable.com/blog/logging-best-practices)
* [Logging in Distributed Systems →](https://www.geeksforgeeks.org/system-design/logging-in-distributed-systems/)
