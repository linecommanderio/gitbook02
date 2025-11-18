---
cover: ../../.gitbook/assets/Screenshot from 2025-11-16 23-44-21.png
coverY: 0
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

# 📢 Security Unit 1 Discussion Post 1

### 📚 Research References

1. [https://www.fortinet.com/resources/cyberglossary/cia-triad](https://www.fortinet.com/resources/cyberglossary/cia-triad)
2. [https://www.f5.com/labs/learning-center/what-is-the-cia-triad](https://www.f5.com/labs/learning-center/what-is-the-cia-triad)
3. [https://www.paloaltonetworks.co.uk/cyberpedia/what-is-data-security-management#what-is-the-cia-triads-role-in-data-security-management](https://www.paloaltonetworks.co.uk/cyberpedia/what-is-data-security-management#what-is-the-cia-triads-role-in-data-security-management)

***

### <mark style="color:$danger;">1️⃣ Describe the CIA Triad</mark>

{% hint style="success" %}
#### **KEY ANSWER**

#### The CIA Triad is like a tripod framework that keeps data "safe" and/or protected.&#x20;

> 👁️ _**Confidentiality (Who can see the data?)**_

> ✅ _**Integrity (Can you trust it?)**_

> ⚡ _**Availability (Can you get it when you need it?)**_
{% endhint %}

#### Official Professional Definitions

{% tabs %}
{% tab title="🏢 Fortinet" %}
[**Fortinet Definition**](https://www.fortinet.com/resources/cyberglossary/cia-triad)

"The three letters in "CIA triad" stand for Confidentiality, Integrity, and Availability. The CIA triad is a common model that forms the basis for the development of security systems.

They are used for finding vulnerabilities and methods for creating solutions. Ideally, when all three standards have been met, the security profile of the organization is stronger and better equipped to handle threat incidents."

**Confidentiality**

> _"Confidentiality involves the efforts of an organization to make sure data is kept secret or private._&#x20;
>
> * _To accomplish this, access to information must be controlled to prevent the unauthorized sharing of data—whether intentional or accidental."_

**Integrity**

> _"Integrity involves making sure your data is trustworthy and free from tampering._
>
> * _The integrity of your data is maintained only if the data is authentic, accurate, and reliable."_

**Availability**

> _"Even if data is kept confidential and its integrity maintained, it is often useless unless it is available to those in the organization and the customers they serve._
>
> * _This means that systems, networks, and applications must be functioning as they should and when they should._
> * _Also, individuals with access to specific information must be able to consume it when they need to, and getting to the data should not take an inordinate amount of time."_
{% endtab %}

{% tab title="🔐 F5" %}
[**F5 Definition**](https://www.f5.com/labs/learning-center/what-is-the-cia-triad)

"Together, these three principles form the cornerstone of any organization's security infrastructure; in fact, they (should) function as goals and objectives for every security program."

**Confidentiality**

> _"Confidentiality can be violated in many ways, for example, through direct attacks designed to gain unauthorized access to systems, applications, and databases in order to steal or tamper with data."_

**Integrity**

> _"Integrity is about ensuring that data has not been tampered with and, therefore, can be trusted. It is correct, authentic, and reliable."_

**Availability**

> _"Availability means that networks, systems, and applications are up and running._&#x20;
>
> * _It ensures that authorized users have timely, reliable access to resources when they are needed."_
{% endtab %}

{% tab title="🛡️ Palo Alto Networks" %}
[**Palo Alto Networks Definition**](https://www.paloaltonetworks.co.uk/cyberpedia/what-is-data-security-management#what-is-the-cia-triads-role-in-data-security-management)

"The CIA Triad refers to three core principles: Confidentiality, Integrity, Availability"

**Confidentiality**

> _Focuses on limiting access to data based on need and risk. It prevents unauthorized exposure through controls like authentication, encryption, and role-based access._

**Integrity**

> _Ensures that data remains accurate and complete. That includes protecting it from unauthorized changes and validating that it hasn't been tampered with._

**Availability**

> _Ensures that data remains usable when needed. This requires resilient systems, backup plans, and monitoring to reduce downtime and service disruption._
{% endtab %}
{% endtabs %}

### <mark style="color:$danger;">2️⃣ What is the relationship between Authority, Will, and Force as they relate to security?</mark>

{% hint style="success" %}
#### KEY ANSWER

#### _**Authority**_, _**Will**_, and _**Force**_ shape cybersecurity policies by determining how rules are created, prioritized, and enforced throughout an organization or across government structures.
{% endhint %}

#### The Three-Part Relationship

{% tabs %}
{% tab title="⚖️ Authority" %}
**Authority — The Foundation**

**Authority** gives the legal and organizational foundation for security.

**Key Aspects**:

* Authority in information security refers to the legitimate power to define security policies, standards, and controls—often resting with organizational leadership, governing bodies, or designated security officers
* It establishes what should be protected, who has access, and the level of protection needed
* Without clear authority, there can be confusion or lack of direction about priorities and acceptable risks

{% hint style="info" %}
**Authority provides the "what" and "who" of security** — defining what needs protection and who is responsible for implementing it.
{% endhint %}
{% endtab %}

{% tab title="💪 Will" %}
**Will — The Motivation**

**Will** provides the motivation and prioritization to act.

**Key Aspects**:

* **Will** is the intention, determination, or organizational commitment to actually implement and support those security policies and strategies
* Even with authority, if leadership or stakeholders lack the will to enforce policies or dedicate resources, security efforts may be overlooked or undermined
* Will often manifests in the decision to move forward with necessary investments in technology, staffing, and training for security

{% hint style="warning" %}
**Without Will, Authority becomes meaningless** — policies without commitment are just words on paper.
{% endhint %}
{% endtab %}

{% tab title="⚡ Force" %}
**Force — The Enforcement**

**Force** translates policy and motivation into effective enforcement and real-world consequences.

**Key Aspects**:

* **Force** is the tangible application of authority and will—it is the enforcement of policies, controls, and responses to noncompliance
* This includes deploying technical controls (firewalls, monitoring), conducting audits, imposing sanctions for policy violations, or escalating responses to incidents and threats
* In the legal and national security context, force can also mean regulatory fines, criminal prosecution, or even the use of state powers to defend against cyber adversaries

{% hint style="success" %}
**Force is where security becomes real** — it's the actual mechanism that prevents, detects, or responds to threats.
{% endhint %}
{% endtab %}
{% endtabs %}

### <mark style="color:$danger;">3️⃣ Types of Controls and Their Relation to Authority/Will/Force</mark>

{% hint style="success" %}
#### **KEY ANSWER**

Not All Controls Are Created Equal. Security controls aren't just random technical configurations—they form a structured hierarchy that determines how effective your security posture actually is.
{% endhint %}

#### Control Type Classifications

***

#### 🔄 How These Frameworks Work Together

{% hint style="success" %}
**Integration Pattern**

These frameworks work together to provide complete security understanding:

* **Control Categories** → What type of protection mechanism
* **Authority/Will/Force** → How the mechanism works on people
* **CIA Triad** → What you're protecting
{% endhint %}

#### Authority vs Force: Control Effectiveness Matrix

| Control Type | Enforcement Mechanism | Requires Compliance | Effectiveness Level |
| ------------ | --------------------- | ------------------- | ------------------- |
| Technical    | Force-Based           | ❌ No                | 🟢 High             |
| Physical     | Force-Based           | ❌ No                | 🟢 High             |
| Managerial   | Authority-Based       | ✅ Yes               | 🟡 Medium           |
| Operational  | Authority-Based       | ✅ Yes               | 🟡 Medium           |

#### Mapping Controls to CIA Triad

{% tabs %}
{% tab title="🔒 Confidentiality" %}
**Confidentiality (Access Control)**

**Force-Based Protection**:

* **Technical**: Encryption, authentication systems, network segmentation, access control lists
* **Physical**: Locked facilities, biometric scanners, secure disposal, visitor badges

**Authority-Based Governance**:

* **Managerial**: Privacy policies, data classification standards, need-to-know principles
* **Operational**: Access reviews, training, handling procedures, clearance verification

{% code title="Confidentiality Implementation Example" overflow="wrap" %}
```bash
# Technical Force: File encryption
gpg --encrypt --recipient user@example.com sensitive_data.txt

# Technical Force: Access control
chmod 600 sensitive_data.txt
chown admin:security sensitive_data.txt

# Technical Force: SELinux enforcement
semanage fcontext -a -t user_home_t "/secure/data(/.*)?"
restorecon -R /secure/data
```
{% endcode %}
{% endtab %}

{% tab title="✅ Integrity" %}
**Integrity (Data Accuracy)**

**Force-Based Protection**:

* **Technical**: Digital signatures, checksums, version control, immutable logs, blockchain
* **Physical**: Tamper-evident seals, secure hardware modules, write-once media

**Authority-Based Governance**:

* **Managerial**: Change management policies, quality standards, data validation requirements
* **Operational**: Backup procedures, validation processes, audit trails

{% code title="Integrity Implementation Example" overflow="wrap" %}
```bash
# Technical Force: Generate checksum
sha256sum critical_file.txt > critical_file.txt.sha256

# Technical Force: Verify integrity
sha256sum -c critical_file.txt.sha256

# Technical Force: Digital signature
gpg --sign --detach-sign critical_file.txt

# Technical Force: Verify signature
gpg --verify critical_file.txt.sig critical_file.txt
```
{% endcode %}
{% endtab %}

{% tab title="🔄 Availability" %}
**Availability (System Access)**

**Force-Based Protection**:

* **Technical**: Load balancers, failover systems, monitoring, RAID arrays, clustering
* **Physical**: Redundant power (UPS, generators), environmental controls, hardware redundancy

**Authority-Based Governance**:

* **Managerial**: Business continuity policies, SLAs, disaster recovery plans
* **Operational**: Maintenance schedules, incident response, capacity planning

{% code title="Availability Implementation Example" overflow="wrap" %}
```bash
# Technical Force: Service monitoring
systemctl enable --now monitoring_agent.service

# Technical Force: Automated failover
pcs resource create VirtualIP ocf:heartbeat:IPaddr2 ip=192.168.1.100 cidr_netmask=24

# Technical Force: RAID redundancy
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1

# Technical Force: Backup automation
systemctl enable --now backup.timer
```
{% endcode %}
{% endtab %}
{% endtabs %}

***

#### 📊 Security Control Architecture

<details>

<summary>🏗️ Layered Security Model</summary>

#### Defense in Depth Strategy

Effective security uses multiple layers of controls, combining both force-based and authority-based approaches:

| Layer       | Force-Based Controls           | Authority-Based Controls |
| ----------- | ------------------------------ | ------------------------ |
| Perimeter   | Firewalls, IDS/IPS             | Access policies          |
| Network     | Network segmentation, VLANs    | Network usage policies   |
| Host        | Antivirus, EDR, host firewalls | Configuration standards  |
| Application | Input validation, WAF          | Secure coding standards  |
| Data        | Encryption, DLP                | Data classification      |

**Key Principle**: **Force-based controls provide the reliable protection foundation**, while authority-based controls add governance and coverage for areas that can't be fully automated.

</details>

***

### 💡 Key Insights

{% hint style="success" %}
1. Force-based controls provide reliable protection because they work regardless of human behavior.
{% endhint %}

{% hint style="warning" %}
2. Authority-based controls require cooperation and should not be sole protection for critical assets.
{% endhint %}

{% hint style="info" %}
3. Effective security layers both approaches with force-based controls as foundation and authority-based controls for governance.
{% endhint %}

{% hint style="info" %}
4. Each CIA element needs multiple control types to ensure comprehensive protection.
{% endhint %}

{% hint style="danger" %}
5. Security effectiveness depends on how controls actually work on people, not just policy compliance.
{% endhint %}

***

### 🎯 Practical Application

#### Real-World Example: Database Security

{% code title="Layered Database Security Architecture" overflow="wrap" lineNumbers="true" %}
```
Force-Based Protection Layer:
├── Database encryption (at-rest and in-transit)
├── Network firewalls (port restrictions)
├── Locked server room (physical access control)
├── Authentication system (technical access control)
└── Monitoring and alerting (automated detection)

Authority-Based Governance Layer:
├── Access policies (who can access what)
├── User training (security awareness)
├── Audit procedures (compliance verification)
├── Change management (controlled modifications)
└── Incident response plan (breach procedures)

CIA Implementation:
├── Confidentiality
│   ├── Encryption (Force)
│   ├── Access control lists (Force)
│   └── Privacy policies (Authority)
├── Integrity
│   ├── Checksums and hashing (Force)
│   ├── Transaction logs (Force)
│   └── Data validation procedures (Authority)
└── Availability
    ├── Database replication (Force)
    ├── Load balancing (Force)
    └── Backup procedures (Authority)
```
{% endcode %}

{% hint style="success" %}
**Implementation Note**

The force-based controls work even if users ignore policies, while authority-based controls provide governance and coverage for areas that can't be fully automated.

This layered approach ensures:

* ✅ Protection continues even during policy violations
* ✅ Multiple failure points required for complete breach
* ✅ Governance framework for human decision-making
* ✅ Comprehensive coverage across CIA Triad elements
{% endhint %}

***

### 📝 Key Takeaways

{% hint style="success" %}
**Summary: Security Fundamentals**

* CIA Triad Foundation
  * Confidentiality: Who can see the data (access control)
  * Integrity: Can you trust it (data accuracy)
  * Availability: Can you get it when needed (system uptime)
* Authority, Will, and Force Relationship
  * Authority: Legal and organizational foundation (defines what should happen)
  * Will: Organizational commitment and motivation (drives implementation)
  * Force: Tangible enforcement and consequences (makes it happen)
* Security Control Hierarchy
  * Force-Based Controls (Automatic): Technical, Physical
  * Authority-Based Controls (Require Compliance): Managerial, Operational
* Critical Success Factors
  1. Layer multiple control types for defense in depth
  2. Use force-based controls for critical assets (don't rely solely on policies)
  3. Map controls to CIA Triad elements to ensure comprehensive coverage
  4. Understand enforcement mechanisms (how controls actually work)
  5. Balance automation with governance (technical controls + oversight)
  6. Test control effectiveness (verify they work as intended)

Enterprise Security Pattern: Force-Based Controls (Foundation) → Authority-Based Controls (Governance) → Comprehensive CIA Protection
{% endhint %}

***
