# sec1d1

{% stepper %}
{% step %}
### Describe the CIA Triad

{% hint style="info" %}
**Quick Definition**: The CIA Triad is like a tripod framework that keeps data "safe" and/or protected. The three legs are **Confidentiality** (_Who can see the data?_), **Integrity** (_Can you trust it?_), and **Availability** (_Can you get it when you need it?_).
{% endhint %}

<details>

<summary>Official Professional Definitions</summary>

"The three letters in "CIA triad" stand for Confidentiality, Integrity, and Availability. The CIA triad is a common model that forms the basis for the development of security systems. They are used for finding vulnerabilities and methods for creating solutions. Ideally, when all three standards have been met, the security profile of the organization is stronger and better equipped to handle threat incidents."Confidentiality "Confidentiality involves the efforts of an organization to make sure data is kept secret or private. To accomplish this, access to information must be controlled to prevent the unauthorized sharing of data—whether intentional or accidental."Integrity "Integrity involves making sure your data is trustworthy and free from tampering. The integrity of your data is maintained only if the data is authentic, accurate, and reliable."Availability "Even if data is kept confidential and its integrity maintained, it is often useless unless it is available to those in the organization and the customers they serve. This means that systems, networks, and applications must be functioning as they should and when they should. Also, individuals with access to specific information must be able to consume it when they need to, and getting to the data should not take an inordinate amount of time."What Is the CIA Triad?"Together, these three principles form the cornerstone of any organization's security infrastructure; in fact, they (should) function as goals and objectives for every security program."Confidentiality "Confidentiality can be violated in many ways, for example, through direct attacks designed to gain unauthorized access to systems, applications, and databases in order to steal or tamper with data."Integrity "Integrity is about ensuring that data has not been tampered with and, therefore, can be trusted. It is correct, authentic, and reliable."Availability "Availability means that networks, systems, and applications are up and running. It ensures that authorized users have timely, reliable access to resources when they are needed."What Is the CIA Triad?"The CIA Triad refers to three core principles: Confidentiality, Integrity, Availability"Confidentiality Focuses on limiting access to data based on need and risk. It prevents unauthorized exposure through controls like authentication, encryption, and role-based access.Integrity Ensures that data remains accurate and complete. That includes protecting it from unauthorized changes and validating that it hasn't been tampered with.Availability Ensures that data remains usable when needed. This requires resilient systems, backup plans, and monitoring to reduce downtime and service disruption.What is the CIA Triad's role in data security management?

</details>
{% endstep %}

{% step %}
### What is the relationship between Authority, Will, and Force as they relate to security?

{% hint style="success" %}
**Core Concept**: Authority, Will, and Force shape cybersecurity policies by determining how rules are created, prioritized, and enforced throughout an organization or across government structures.
{% endhint %}

Their relationship in practice is:

#### Authority — The Foundation

**Authority** gives the legal and organizational foundation for security.

* Authority in information security refers to the legitimate power to define security policies, standards, and controls—often resting with organizational leadership, governing bodies, or designated security officers.
* It establishes what should be protected, who has access, and the level of protection needed.
* Without clear authority, there can be confusion or lack of direction about priorities and acceptable risks.

#### Will — The Motivation

**Will** provides the motivation and prioritization to act.

* **Will** is the intention, determination, or organizational commitment to actually implement and support those security policies and strategies.
* Even with authority, if leadership or stakeholders lack the will to enforce policies or dedicate resources, security efforts may be overlooked or undermined.
* Will often manifests in the decision to move forward with necessary investments in technology, staffing, and training for security.

#### Force — The Enforcement

**Force** translates policy and motivation into effective enforcement and real-world consequences.

* **Force** is the tangible application of authority and will—it is the enforcement of policies, controls, and responses to noncompliance.
* This includes deploying technical controls (firewalls, monitoring), conducting audits, imposing sanctions for policy violations, or escalating responses to incidents and threats.
* In the legal and national security context, force can also mean regulatory fines, criminal prosecution, or even the use of state powers to defend against cyber adversaries.
{% endstep %}

{% step %}
### What are the types of controls and how do they relate to the above question?

{% hint style="warning" %}
**Big Picture Takeaway**: Not All Controls Are Created Equal. Security controls aren't just random technical configurations—they form a structured hierarchy that determines how effective your security posture actually is.
{% endhint %}

#### Technical Controls — The Automatic Stuff

**Technical Controls** are programmatic—they're enforced by code, configuration, or digital systems. These work even if people forget, don't care, or are trying to cause trouble.

**Examples in action:**

{% code overflow="wrap" %}
```bash
# File permissions: only the owner can read/write, period
chmod 600 sensitive_file
```
{% endcode %}

* SELinux policies that prevent processes from accessing certain files
* Database connection limits that reject new connections after the threshold
* Encryption that makes data unreadable without the proper keys

#### Physical Controls — The Real-World Barriers

**Physical Controls** work in the material world but follow the same "force, not authority" principle: They create physical obstacles. Like technical controls, these work regardless of whether people want to cooperate. To get past them, someone has to break something or find a way around them.

**Examples in action:**

* Locked server cabinets that prevent hardware tampering
* Security cameras that record what happens (detective physical control)
* Biometric scanners that verify identity through physical characteristics
* Cable locks that prevent equipment theft

#### Managerial vs Operational Controls — The "Please Follow the Rules"

These depend entirely on people choosing to follow them. These only work if people respect the rules and choose to follow them.

{% tabs %}
{% tab title="Managerial Controls" %}
**Managerial Controls** are high-level directives from leadership.

* All employees must use strong passwords
* No personal devices on the corporate network
* Data classification must follow our three-tier system
{% endtab %}

{% tab title="Operational Controls" %}
**Operational Controls** are the day-to-day processes that implement those directives.

* Password complexity training sessions
* IT procedures for device registration
* Step-by-step data classification workflows
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

***

### How These Frameworks Work Together

These frameworks work together to provide complete security understanding:

* **Control Categories** → What type of protection mechanism
* **Authority/Will/Force** → How the mechanism works on people
* **CIA Triad** → What you're protecting

#### Authority vs Force: How Controls Actually Work

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Force-Based Controls</strong></td><td>Automatic Enforcement: Technical and Physical controls that work regardless of human behavior</td><td></td></tr><tr><td><strong>Authority-Based Controls</strong></td><td>Require Compliance: Managerial and Operational controls that depend on people following rules</td><td></td></tr></tbody></table>

<details>

<summary>Detailed Control Breakdown by Type</summary>

**Force-Based Controls (Automatic Enforcement)**

* **Technical Controls:** Firewalls, encryption, access controls, automated systems
* **Physical Controls:** Locks, cameras, environmental systems, barriers

**Authority-Based Controls (Require Compliance)**

* **Managerial Controls:** Policies, directives, governance frameworks
* **Operational Controls:** Procedures, training, daily processes

</details>

#### Mapping Controls to CIA Triad

<details>

<summary>Confidentiality (Access Control)</summary>

**Force-Based:**

* Technical: Encryption, authentication systems, network segmentation
* Physical: Locked facilities, biometric scanners, secure disposal

**Authority-Based:**

* Managerial: Privacy policies, data classification standards
* Operational: Access reviews, training, handling procedures

</details>

<details>

<summary>Integrity (Data Accuracy)</summary>

**Force-Based:**

* Technical: Digital signatures, checksums, version control
* Physical: Tamper-evident seals, secure hardware modules

**Authority-Based:**

* Managerial: Change management policies, quality standards
* Operational: Backup procedures, validation processes

</details>

<details>

<summary>Availability (System Access)</summary>

**Force-Based:**

* Technical: Load balancers, failover systems, monitoring
* Physical: Redundant power, environmental controls

**Authority-Based:**

* Managerial: Business continuity policies, SLAs
* Operational: Maintenance schedules, incident response

</details>

***

### Key Insights

{% hint style="success" %}
**1. Force-based controls provide reliable protection** because they work regardless of human behavior.
{% endhint %}

{% hint style="warning" %}
**2. Authority-based controls require cooperation** and should not be sole protection for critical assets.
{% endhint %}

{% hint style="info" %}
**3. Effective security layers both approaches** with force-based controls as foundation and authority-based controls for governance.
{% endhint %}

{% hint style="info" %}
**4. Each CIA element needs multiple control types** to ensure comprehensive protection.
{% endhint %}

{% hint style="danger" %}
**5. Security effectiveness depends on how controls actually work on people**, not just policy compliance.
{% endhint %}

***

### Practical Application

#### Real-World Example: Database Security

{% code title="Layered Security Approach" %}
```
Force-Based Protection:
├── Database encryption
├── Network firewalls
└── Locked server room

Authority-Based Governance:
├── Access policies
├── User training
└── Audit procedures

CIA Implementation:
├── Confidentiality → Encryption
├── Integrity → Checksums
└── Availability → Redundancy
```
{% endcode %}

{% hint style="success" %}
**Implementation Note**: The force-based controls work even if users ignore policies, while authority-based controls provide governance and coverage for areas that can't be fully automated.
{% endhint %}
