---
cover: ../.gitbook/assets/sessionn6-ezgif.webp
coverY: 1.255407779243237
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

# 🔎 Session 6: Monitoring and parsing logs

We will focus on **monitoring and parsing logs** as one of the most essential security engineering practices in any production environment.

This session explores how logs are generated, formatted, collected, and analyzed across various layers of the infrastructure stack.&#x20;

You'll gain operational understanding of how to identify log sources, use modern tools for log aggregation and search, and develop awareness of log structure, integrity, and retention requirements.

{% hint style="danger" %}
**Critical Principle**: **Get your logs off your servers in real time**. This is arguably the most important thing you can do with logs.

> _**Even if attackers delete logs locally, the events are already captured remotely.**_

Logs are often the first and best source of truth when diagnosing an incident, auditing a system, or responding to a breach.
{% endhint %}

***

## 📚 Learning Objectives

By the end of Session 6, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="💪 Core Competencies" %}
1. **Log Types & Sources**: Understanding different types of logs (application, host, network, database) and their roles in system monitoring
2. **Log Structures**: Identifying log formats (RFC 3164 BSD Syslog, RFC 5424 IETF Syslog, systemd journal) and applying appropriate parsing techniques
3. **Log Aggregation**: Exploring and configuring log pipelines using modern tools like Grafana Loki, ELK Stack, and Splunk
4. **Real-World Analysis**: Analyzing security events using log data and query languages
5. **Log Integrity**: Understanding how log immutability and integrity contribute to reliable forensics and compliance
{% endtab %}

{% tab title="👔 Professional Skills" %}
6. **Operational Thinking**: Learning to think like operators and defenders—ensuring logs are complete, available, immutable, and actionable
7. **Security Investigation**: Using logs for troubleshooting, security auditing, and incident response
8. **Pattern Recognition**: Identifying indicators of compromise and predicting future incidents from log patterns
9. **Scalable Observability**: Creating observability strategies that support both security and performance goals
10. **Compliance Integration**: Applying DISA STIGs to logging requirements and audit trails
{% endtab %}
{% endtabs %}

***

## 🎯 Relevance & Context

Logs serve as your system's black box recorder—capturing every significant event, attempted action, and system state change. In the real world, the difference between a minor incident and a catastrophic breach often comes down to having the right logs at the right time.

#### What You'll Learn

<details>

<summary>🔍 <strong>Why Logs Matter</strong></summary>

**The Forensic Reality**:

* When a server crashes or gets compromised, logs tell you what happened
* Without logs, you're investigating blind—guessing at causes and missing critical evidence
* Logs capture both successful actions AND failed attempts (someone trying your front door)

**The Real-Time Imperative**: Scott's spaceship analogy from the lecture captures this perfectly: _"If you've seen any alien movie, they come to a deserted spaceship and everyone's dead. They have to go all the way into the middle of the ship to read the logs of what happened. Well, now you're all dead too. That's why we got a movie—because you had to go read the logs instead of scanning from the outside and realizing something killed everyone."_

Get logs off your servers in real time. This eliminates offensive cleanup strategies and provides continuous visibility even when systems go down.

</details>

<details>

<summary>🔧 <strong>Technical Skills</strong></summary>

* [ ] Identifying and understanding different log types and their sources
* [ ] Parsing structured log formats (RFC 3164, RFC 5424, journald)
* [ ] Configuring log rotation to manage disk space without losing data
* [ ] Setting up real-time log aggregation pipelines
* [ ] Using command-line tools (`journalctl`, `grep`, `less`, `awk`) for local log analysis
* [ ] Querying centralized log systems with modern tools
* [ ] Validating log integrity and ensuring immutability
* [ ] Analyzing logs for security incidents and troubleshooting

</details>

<details>

<summary>💡 <strong>Professional Skills</strong></summary>

* [ ] Thinking operationally about log completeness and availability
* [ ] Designing observability strategies that scale across enterprise environments
* [ ] Distinguishing between incidents (discrete events) and problems (recurring patterns)
* [ ] Conducting root cause analysis using log correlation
* [ ] Communicating findings from log analysis to security and operations teams
* [ ] Understanding compliance requirements for log retention and audit trails
* [ ] Making architectural decisions about log storage, aggregation, and analysis tools

</details>

{% columns %}
{% column width="58%" %}
**Why Log Management Is Critical**

**The Challenge**:

* Systems generate massive volumes of log data continuously
* Local log storage fills disks and becomes unmanageable
* Attackers routinely delete logs to cover their tracks
* Waiting for systems to recover before accessing logs delays incident response
* Without centralized aggregation, correlating events across systems is impossible

**The Solution**:

* **Real-time streaming** gets logs off servers immediately
* **Centralized aggregation** makes logs searchable across entire infrastructure
* **Structured formats** enable automated parsing and analysis
* **Immutability** ensures logs can't be tampered with after creation
* **Rotation and retention** balances storage costs with forensic needs

**Real-World Example**:\
Imagine 30 production servers suddenly go down. Do you:

* **A)** Wait for them to come back up to read local logs, then discover it was malicious and you've just brought compromised systems back into production
* **B)** Immediately query your centralized logging system, identify the attack pattern in the last events captured, and bring systems up quarantined and patched

The answer is obvious. But only one approach is possible if you've been streaming logs in real time.
{% endcolumn %}

{% column width="42%" %}
{% hint style="success" %}
**What You're Building**:

Operational understanding of log lifecycle—from generation through rotation, aggregation, storage, and analysis—with hands-on experience using modern tools for centralized logging and querying.
{% endhint %}

{% hint style="info" %}
**Career Context**:

Observability and log analysis skills are foundational for:

* Site Reliability Engineering (SRE)
* Security Operations Center (SOC) analysts
* DevOps engineers
* System administrators
* Incident responders

Tools like Prometheus, Grafana Loki, fluentd, and the ELK stack are industry standards in cloud-native and Kubernetes environments.
{% endhint %}

{% hint style="warning" %}
**Important Distinction**:

**Incidents** are discrete actions that happen (server crashes, authentication failure).

**Problems** are patterns of recurring issues identified through log analysis.

Logs help you solve both.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 6, you must be comfortable with:

* **Command-Line Tools**: Working with `journalctl`, `grep`, `less`, `cat`, `awk`, and related text processing utilities
* **SystemD Management**: Managing services with `systemctl` and understanding service states
* **File System Navigation**: Understanding standard log locations (`/var/log/`, `/var/log/messages`, journald storage)
* **Configuration Files**: Reading and editing YAML and JSON-formatted files
* **Standard I/O Streams**: Understanding stdin, stdout, stderr, and redirection operators
* **Text Editors**: Using `vim`, `nano`, or other editors to modify configuration files

**Foundation Requirement**: We should understand:

* Previous unit content on bastion hosts, air-gapped systems, and enterprise patching
* Basic networking concepts (for understanding network logs)
* STIG compliance framework and audit requirements
* The concept of syslog and log rotation
* How services generate output and where that output goes

**Tool Access**:

* STIG Viewer 2.18 installed for compliance reference
* Access to a Linux system for hands-on log analysis practice
{% endhint %}

***

## 📊 Key Terms and Definitions

<details>

<summary><strong>Types of Logs</strong></summary>

* **Application Logs**: Events generated by specific applications (web servers, databases, custom software)
* **Host Logs**: System-level events from the operating system (kernel messages, authentication, service status)
* **Network Logs**: Traffic data, firewall rules, connection attempts, packet captures
* **Database Logs**: Query execution, transaction records, replication events, performance metrics

</details>

<details>

<summary><strong>Log Structure Standards</strong></summary>

* **RFC 3164 (BSD Syslog)**: Traditional syslog format with priority, timestamp, hostname, and message
* **RFC 5424 (IETF Syslog)**: Enhanced syslog with structured data elements and UTF-8 support
* **systemd Journal**: Binary log format native to systemd-based systems, queryable with `journalctl`

</details>

<details>

<summary><strong>Log Management Concepts</strong></summary>

* **Log Rotation**: Automatic archival and compression of logs to manage disk space (logrotate, journald limits)
* **Log Aggregation**: Centralized collection of logs from multiple sources into a searchable system
* **Immutability**: Ensuring logs cannot be modified after creation (critical for forensics and compliance)
* **SIEM**: Security Information and Event Management—platforms that collect, analyze, and alert on security events

</details>

<details>

<summary><strong>Common Log Aggregation Tools</strong></summary>

* **ELK Stack**: Elasticsearch, Logstash, Kibana—popular open-source log aggregation and visualization
* **Grafana Loki**: Log aggregation designed for cloud-native and Kubernetes environments
* **Splunk**: Enterprise SIEM and log analysis platform
* **Graylog**: Open-source log management with centralized logging and analysis
* **Prometheus**: Metrics and monitoring system (often paired with Loki for logs)
* **fluentd**: Data collection and forwarding agent for unified logging layer

</details>

***

## 🔗 Connection to Previous Units

This session builds directly on concepts from previous units:

{% stepper %}
{% step %}
#### Unit 4: Bastion Hosts and Air-Gaps

Log aggregation becomes critical in isolated environments. How do you get logs off air-gapped systems? How do bastions facilitate controlled log export?
{% endstep %}

{% step %}
#### Unit 5: Enterprise Patching

Patching operations generate extensive logs. How do you validate successful patches? How do you identify systems that failed to update? Log analysis answers both.
{% endstep %}

{% step %}
#### Unit 6: Logs and Parsing

Now we close the loop: understanding what happens on your systems, why it happened, and how to respond. Logs are the foundation for everything that follows.
{% endstep %}
{% endstepper %}

***

## 🎓 What Makes This Unit Critical

{% hint style="danger" %}
**Security Reality Check**

Without proper logging:

* You cannot perform incident response or forensics
* You cannot prove compliance with security frameworks
* You cannot identify patterns of attack or system degradation
* You cannot troubleshoot system failures with any confidence

Logs are not optional. They are the foundation of operational security.
{% endhint %}

{% hint style="success" %}
**Career Accelerator**

If you tell someone you understand:

* Prometheus + Grafana Loki + fluentd
* Centralized logging architecture
* Log-based security analysis
* Cloud-native observability

...you become immediately valuable to any organization running Kubernetes or cloud infrastructure. These skills are in extremely high demand.
{% endhint %}

***

## 📚 Additional Resources

**Recommended Learning Paths**:

* https://landscape.cncf.io/ - CNCF Cloud Native Landscape - Observability & Analysis
* Grafana Loki documentation and tutorials
* Prometheus monitoring fundamentals
* ELK Stack setup and querying guides

**For Further Exploration**:

* Investigate how different tools handle log integrity validation
* Research log retention requirements for compliance frameworks (PCI-DSS, HIPAA, SOC 2)
* Explore advanced log parsing with regular expressions
* Study how SIEMs correlate events across distributed systems

***

**Page Cover Recommendation**: Hero-width image showing a visual representation of log streams flowing from multiple servers into a centralized aggregation point, with query results highlighting security events. This captures the core concept of "get logs off servers in real time."

**Page Icon Recommendation**: 📊 (chart/graph icon representing data analysis and observability)

**Section Icons**:

* 🔍 for forensic/analysis sections
* ⚠️ for security warnings
* 🔧 for technical implementation
* 💡 for career/professional guidance
