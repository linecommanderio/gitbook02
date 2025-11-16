---
cover: ../../.gitbook/assets/lab-ezgif.png
coverY: -222.78811169728942
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

# 🛠️ session\_lab\_1\_gitbook

I'm building foundational skills in system exploration and security baseline implementation through hands-on Linux hardening.

<details>

<summary>🏗️ What We'll Build</summary>

A **security compliance validation workflow** for enterprise Linux systems:

* [ ] System reconnaissance capabilities to understand security-relevant configurations
* [ ] STIG-based hardening implementation for database services
* [ ] Professional security baseline documentation and remediation processes

</details>

### Lab Progression

{% stepper %}
{% step %}
**🔍 System Reconnaissance**
{% endstep %}

{% step %}
**🛡️ STIG Compliance**
{% endstep %}

{% step %}
**🎯 Database Hardening**
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**What Makes This Real**

These aren't simulations—we're performing actual system security assessments using Defense Information Systems Agency (DISA) Security Technical Implementation Guides (STIGs). These are the exact configuration standards used to secure DoD systems, financial infrastructure, and critical enterprise databases.

The techniques taught here directly map to:

* Security compliance auditor responsibilities
* Systems hardening engineer workflows
* Configuration management automation requirements
{% endhint %}

By the end, we'll understand:

* The **reconnaissance methodology** (system exploration, configuration discovery, security parameter validation)
* The **compliance architecture** (STIGs, security controls classification, remediation verification)

More importantly, we'll know how to systematically assess and harden Linux systems against documented security baselines.

***

### 📋 Required Materials

{% tabs %}
{% tab title="🛠️ Tools" %}
**SSH Client**:

* PuTTY (Windows)
* OpenSSH (Linux/macOS)
* Terminal emulator with SSH support

**Security Assessment**:

* STIG Viewer 2.18
* Download: [https://public.cyber.mil/stigs/downloads/](https://public.cyber.mil/stigs/downloads/)

**Lab Environment**:

* ProLUG Lab Server access (Hammer server)
* Root or sudo privileges
* Internet connectivity for package installation

**STIG Checklist**:

* MariaDB Enterprise 10.x STIG (import into STIG Viewer)
{% endtab %}

{% tab title="📚 Prerequisites" %}
<table data-full-width="true"><thead><tr><th width="375.2265625" valign="top">Required Knowledge</th><th valign="top">Required Skills</th></tr></thead><tbody><tr><td valign="top"><ul><li>Basic command line navigation (<code>cd</code>, <code>ls</code>, <code>pwd</code>)</li><li>Text filtering with <code>grep</code> and pipes</li><li>Package management with <code>dnf</code>/<code>yum</code></li><li>SystemD service control (<code>systemctl</code>)</li><li>Basic SQL database concepts</li></ul></td><td valign="top"><ul><li>SSH connection establishment</li><li>Privilege escalation with <code>sudo</code></li><li>Configuration file editing (<code>vim</code>/<code>nano</code>)</li><li>Command output analysis and interpretation</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

***

### 🛠️ Labs to Build Here

{% stepper %}
{% step %}
#### **🔍 Module 1: System Reconnaissance and Security Discovery**

Learn to identify security-relevant system configurations through systematic exploration.

{% hint style="info" %}
**What You're Learning**

Before you can harden a system, you must understand its current security posture. This module teaches reconnaissance techniques used by security auditors and systems engineers to discover security-relevant configurations:

* Filesystem mount options that restrict code execution
* Kernel network parameters controlling traffic forwarding
* Loaded kernel modules providing firewall capabilities

**Critical Insight**: Security is not just about adding controls—it's about understanding what controls already exist and how they interact. Every `mount` option, every `sysctl` parameter, every kernel module represents a security decision (implicit or explicit).
{% endhint %}

**🎯 Learning Objectives**

This reconnaissance phase builds your ability to systematically inventory security-relevant configurations. As you execute commands, focus on understanding **why** each parameter matters for security.

<details>

<summary><strong>Exercise 1.1: Filesystem Security Options Analysis</strong></summary>

**Objective**: Discover how mounted filesystems restrict dangerous operations.

**Commands to Execute**:

```bash
# Find filesystems mounted with noexec (prevents code execution)
mount | grep -i noexec

# Find filesystems mounted with nodev (prevents device file interpretation)
mount | grep -i nodev

# Find filesystems mounted with nosuid (prevents SUID bit escalation)
mount | grep -i nosuid
```

**Analysis Questions**:

1. **Count**: Approximately how many of your mounted filesystems have each of these security options?
2. **Security Impact**:
   * What does `noexec` prevent? (Hint: Executables and scripts)
   * What does `nodev` prevent? (Hint: Device nodes like `/dev/sda`)
   * What does `nosuid` prevent? (Hint: Privilege escalation via SUID binaries)
3. **Real-World Application**:
   * Which filesystems should **always** have `noexec`? (Answer: `/tmp`, `/var/tmp`, `/home`)
   * Why might `/boot` have `nodev` and `nosuid`?
   * What's the risk if `/tmp` doesn't have `noexec`?

**Expected Output Pattern**:

```
/tmp on /tmp type ext4 (rw,nosuid,nodev,noexec,relatime)
/var/tmp on /var/tmp type ext4 (rw,nosuid,nodev,noexec,relatime)
```

**Professional Validation**: If `/tmp` or `/var/tmp` lack these options, they represent privilege escalation vectors. Attackers upload malicious code to world-writable temp directories and execute it.

</details>

<details>

<summary><strong>Exercise 1.2: Network Configuration Security Parameters</strong></summary>

**Objective**: Understand kernel-level network security controls and their configuration.

**Part A: IPv4 and IPv6 Parameter Discovery**

```bash
# List all IPv4 kernel parameters
sysctl -a | grep -i ipv4

# List all IPv6 kernel parameters
sysctl -a | grep -i ipv6
```

**Analysis Questions**:

1. **Enumeration**: How many IPv4 parameters exist? How many IPv6 parameters?
2. **Parameter Categories**: What categories of network controls do you see?
   * Forwarding controls (`forward`)
   * ICMP behavior (`icmp_*`)
   * TCP behavior (`tcp_*`)
   * Connection tracking and filtering

**Part B: IP Forwarding Analysis**

```bash
# Check if IPv4 forwarding is enabled on interfaces
sysctl -a | grep -i ipv4 | grep -i forward
```

**Critical Security Question**: Does IPv4 forwarding happen on your system's interfaces?

**Security Implications**:

| Forwarding State          | Meaning                   | Security Impact                                                                        |
| ------------------------- | ------------------------- | -------------------------------------------------------------------------------------- |
| `net.ipv4.ip_forward = 0` | System is an **endpoint** | Cannot route traffic between networks (secure default for workstations)                |
| `net.ipv4.ip_forward = 1` | System is a **router**    | Can forward packets between interfaces (required for gateways, dangerous on endpoints) |

**Real-World Scenario**: If an attacker compromises a system with IP forwarding enabled, they can pivot through it to reach other networks. Disabling forwarding on non-router systems is defense-in-depth.

**Part C: Netfilter Kernel Module Verification**

```bash
# Check what firewall-related kernel modules are loaded
lsmod | grep -i tables
```

**Analysis Questions**:

1. **Module Identification**: What types of "tables" modules exist?
   * `iptables` (IPv4 firewall)
   * `ip6tables` (IPv6 firewall)
   * `ebtables` (Ethernet bridge firewall)
   * `arptables` (ARP-level firewall)
2. **Architectural Understanding**: Why are these implemented as kernel modules rather than user-space programs?

**Professional Insight**: Netfilter operates at the kernel level because it must inspect every packet entering or leaving the system. User-space tools like `firewall-cmd` are just interfaces that configure these kernel modules.

</details>

**🔬 Key Technical Concepts**

{% tabs %}
{% tab title="🗂️ Mount Options Security" %}
**Defense-in-Depth Through Filesystem Restrictions**:

Mounting filesystems with security options provides defense layers that persist regardless of application-level permissions:

```bash
# View current mount options
mount | column -t

# View /etc/fstab for persistent mount configurations
cat /etc/fstab
```

**Critical Mount Options**:

| Option   | Prevents                           | Use Case                               |
| -------- | ---------------------------------- | -------------------------------------- |
| `noexec` | Executing binaries or scripts      | Temp directories, data partitions      |
| `nodev`  | Creating/interpreting device nodes | Any non-root filesystem                |
| `nosuid` | SUID/SGID privilege escalation     | Most filesystems except `/` and `/usr` |
| `ro`     | Writing to filesystem              | `/boot`, read-only data                |

**Enterprise Pattern**:

```bash
# /etc/fstab entry for secure /tmp
tmpfs /tmp tmpfs defaults,noatime,nosuid,nodev,noexec,mode=1777 0 0
```

**Validation Command**:

```bash
# Test if /tmp allows execution
cp /bin/ls /tmp/testexec
/tmp/testexec  # Should fail with "Permission denied" if noexec works
```
{% endtab %}

{% tab title="🌐 Sysctl Network Parameters" %}
**Kernel-Level Network Security Controls**:

`sysctl` manages kernel parameters at runtime. Network security relies heavily on correct kernel parameter configuration:

**Critical Security Parameters**:

```bash
# Disable IP forwarding (prevent routing)
net.ipv4.ip_forward = 0
net.ipv6.conf.all.forwarding = 0

# Enable SYN cookies (DDoS protection)
net.ipv4.tcp_syncookies = 1

# Disable ICMP redirects (prevent route poisoning)
net.ipv4.conf.all.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0

# Disable source packet routing (prevent spoofing)
net.ipv4.conf.all.accept_source_route = 0
net.ipv6.conf.all.accept_source_route = 0
```

**Persistence Mechanism**:

Runtime changes with `sysctl -w` don't survive reboot. Make persistent:

```bash
# Add to /etc/sysctl.conf or /etc/sysctl.d/*.conf
echo "net.ipv4.ip_forward = 0" >> /etc/sysctl.d/99-security.conf

# Apply immediately
sysctl -p /etc/sysctl.d/99-security.conf
```

**Verification Pattern**:

```bash
# Check current value
sysctl net.ipv4.ip_forward

# Check if persistent
grep ip_forward /etc/sysctl.conf /etc/sysctl.d/*
```
{% endtab %}

{% tab title="🔥 Netfilter Architecture" %}
**Kernel Packet Filtering Framework**:

Netfilter is the Linux kernel's firewall framework. User-space tools configure it:

```
User Space:    firewall-cmd  →  firewalld  →  iptables command
                                                      ↓
Kernel Space:  iptables/nftables kernel modules (netfilter)
```

**Module Verification**:

```bash
# Check loaded netfilter modules
lsmod | grep -E 'ip_tables|ip6_tables|nf_'

# Expected output includes:

# ip_tables    (IPv4 packet filtering)

# ip6_tables   (IPv6 packet filtering)

# nf_conntrack (connection tracking)

# nf_nat       (Network Address Translation)
```

**Architectural Insight**: The firewall operates in kernel space because:

* Every packet must be inspected (performance requirement)
* Decisions must be made before routing (security requirement)
* User-space processes could be killed or compromised (reliability requirement)

**Real-World Validation**:

```bash
# Check active firewall rules
iptables -L -n -v         # IPv4 rules
ip6tables -L -n -v        # IPv6 rules
firewall-cmd --list-all   # High-level firewalld configuration
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
✅ **Module 1 Outcome**:

* Systematic reconnaissance methodology for security-relevant configurations
* Understanding of filesystem mount security options
* Comprehension of kernel network parameters and their security implications
* Ability to identify loaded kernel modules and their purposes
* Foundation for security baseline validation and compliance assessment
{% endhint %}
{% endstep %}

{% step %}
**🛡️ Module 2: STIG Viewer Setup and Compliance Framework**

Prepare the professional security assessment tooling used across government and enterprise environments.

{% hint style="info" %}
**What You're Setting Up**

STIG Viewer is the official tool for working with Security Technical Implementation Guides (STIGs) published by the Defense Information Systems Agency (DISA). STIGs represent government-hardened security baselines for operating systems, applications, and network devices.

**Real-World Context**: Every DoD system, many financial institutions, and critical infrastructure operators use STIGs as their security baseline. Learning STIG compliance is learning enterprise security configuration management.
{% endhint %}

**🎯 Module Objectives**

<details>

<summary><strong>Understanding STIGs and Security Controls</strong></summary>

**What is a STIG?**

A Security Technical Implementation Guide (STIG) is a cybersecurity methodology for standardizing security protocols across:

* Operating systems (RHEL, Windows Server, etc.)
* Applications (databases, web servers, etc.)
* Network devices (routers, switches, firewalls)

**STIG Structure**:

Each STIG contains hundreds of "findings" (also called "rules" or "vulnerabilities"). Each finding specifies:

1. **Vulnerability ID** (e.g., V-253666)
2. **Severity** (CAT I, CAT II, CAT III)
3. **Rule Title** (descriptive name)
4. **Discussion** (why this matters)
5. **Check Text** (how to verify compliance)
6. **Fix Text** (how to remediate)

**Security Control Types**:

| Control Type    | Definition                                         | Example                            |
| --------------- | -------------------------------------------------- | ---------------------------------- |
| **Technical**   | Automated enforcement through system configuration | Firewall rules, file permissions   |
| **Operational** | Processes and procedures performed by personnel    | Backup procedures, access reviews  |
| **Management**  | Policies and oversight activities                  | Risk assessment, security planning |

Within technical controls:

| Subtype        | Purpose                               | Example                            |
| -------------- | ------------------------------------- | ---------------------------------- |
| **Preventive** | Stop attacks before they happen       | Disable unnecessary services       |
| **Detective**  | Identify attacks or policy violations | Audit logging, intrusion detection |
| **Corrective** | Respond to detected violations        | Automatic account lockout          |

</details>

**📖 Setup Instructions**

{% stepper %}
{% step %}
**Download STIG Viewer**

Navigate to the official DISA STIG repository:

**URL**: https://public.cyber.mil/stigs/downloads/

**Find**: "STIG Viewer 2.18" (or latest version)

**Download**: The appropriate package for your workstation OS:

* Windows: `.jar` file (requires Java Runtime Environment)
* Linux: `.jar` file (requires Java Runtime Environment)
* macOS: `.jar` file (requires Java Runtime Environment)

{% hint style="info" %}
**Java Requirement**: STIG Viewer is a Java application. Ensure you have JRE 8 or higher installed:

```bash
# Check Java version
java -version

# If not installed (RHEL/CentOS/Fedora):
sudo dnf install java-11-openjdk
```
{% endhint %}
{% endstep %}

{% step %}
**Download MariaDB STIG**

**Within the STIG Downloads Page**:

1. Navigate to **Application Security** → **Databases**
2. Find: **"MariaDB Enterprise 10.x Security Technical Implementation Guide"**
3. Download: The latest STIG package (ZIP file)

**Extract the STIG**:

```bash
# Example extraction
unzip U_MariaDB_10-x_V*_STIG.zip
```

**Files You'll Find**:

* XML file (for import into STIG Viewer)
* Manual (PDF with complete STIG content)
* XCCDF file (for automated scanning tools)
{% endstep %}

{% step %}
**Import STIG into STIG Viewer**

**Launch STIG Viewer**:

```bash
java -jar STIGViewer-2.18.jar
```

**Import the MariaDB STIG**:

1. **File** → **Import STIG** → Select the MariaDB XML file
2. STIG Viewer will parse and load all findings
3. Verify: You should see hundreds of MariaDB findings in the checklist

**Create a Checklist**:

1. **Checklist** → **Create Checklist** → Select "MariaDB Enterprise 10.x"
2. **Save** the checklist file (.ckl format)
3. This checklist will track your compliance assessment progress
{% endstep %}
{% endstepper %}

**🔬 STIG Viewer Workflow**

{% tabs %}
{% tab title="📋 Checklist Management" %}
**Professional Assessment Workflow**:

1. **Create Checklist**: Generate a new `.ckl` file for the system being assessed
2. **Work Through Findings**: Evaluate each STIG requirement
3. **Document Status**: Mark findings as:
   * **Open** (non-compliant)
   * **Not a Finding** (compliant)
   * **Not Applicable** (requirement doesn't apply to this system)
   * **Not Reviewed** (not yet assessed)
4. **Add Comments**: Document remediation steps, risk acceptances, or why something is N/A
5. **Export Results**: Generate compliance reports for management

**Finding Status Workflow**:

```
Start: Not Reviewed
    ↓
Run Check Text Commands
    ↓
Compliant? → Yes → Mark "Not a Finding"
           → No  → Mark "Open" → Document remediation plan
           → N/A → Mark "Not Applicable" → Document why
```
{% endtab %}

{% tab title="🔍 Finding Analysis" %}
**Systematic Finding Evaluation**:

For each STIG finding, answer:

1. **What is the problem?**
   * What security risk does this finding address?
   * What attack or misconfiguration does it prevent?
2. **What is the fix?**
   * What specific configuration changes are required?
   * What commands implement the remediation?
3. **What type of control is being implemented?**
   * Technical/Operational/Management?
   * Preventive/Detective/Corrective?
4. **Is it set properly on your system?**
   * Execute the Check Text commands
   * Compare output to expected compliance state
   * Document current state vs. required state

**Example Finding Breakdown**:

```
Finding: V-253666
Problem: Unlimited concurrent connections per user enable DoS
Fix: Set max_user_connections to a reasonable value (e.g., 50)
Control Type: Technical Preventive (resource exhaustion prevention)
Status: [Execute check to determine]
```
{% endtab %}

{% tab title="📊 Compliance Reporting" %}
**Professional Documentation Standards**:

Every finding requires documentation:

**For "Not a Finding" (Compliant)**:

```
Status: Not a Finding
Comments: Verified max_user_connections=50 in mysql.user table.
          Query executed: SELECT user, max_user_connections FROM mysql.user;
          Result: All users have max_user_connections=50 or less.
          Verified: 2024-01-15 by [Your Name]
```

**For "Open" (Non-Compliant)**:

```
Status: Open
Comments: Default installation has max_user_connections=0 (unlimited).
          Risk: Resource exhaustion DoS possible.
          Remediation Plan: Set max_user_connections=50 for all users.
          Scheduled Fix Date: 2024-01-20
          Responsible: [Your Name]
```

**For "Not Applicable"**:

```
Status: Not Applicable
Comments: Finding requires MariaDB Galera Cluster configuration.
          This system runs standalone MariaDB instance.
          Cluster-specific settings do not apply.
          Documented: 2024-01-15 by [Your Name]
```

**Export Compliance Report**:

* **Checklist** → **Export** → Choose format (CKL, HTML, CSV)
* Submit to security compliance team or auditors
{% endtab %}
{% endtabs %}

{% hint style="success" %}
✅ **Module 2 Outcome**:

* STIG Viewer installed and operational
* MariaDB STIG imported and checklist created
* Understanding of STIG structure and finding evaluation methodology
* Professional security assessment workflow established
* Foundation for systematic compliance validation
{% endhint %}
{% endstep %}

{% step %}
**🎯 Module 3: MariaDB Security Hardening Implementation**

Apply STIG-based hardening controls to a live MariaDB database service.

{% hint style="info" %}
**What You're Hardening**

MariaDB is an enterprise-grade relational database management system (RDBMS) used in production environments worldwide. Database security is critical because:

* Databases store sensitive data (credentials, financial records, PII)
* Misconfigured databases are prime targets for SQL injection attacks
* Database DoS attacks can cripple entire business operations
* Compliance regulations (PCI-DSS, HIPAA, SOX) require database hardening

**Real-World Context**: This lab implements actual production database security controls used by financial institutions, healthcare providers, and government agencies.
{% endhint %}

**🛠️ Lab Setup**

{% stepper %}
{% step %}
**Connect to Lab Environment**

```bash
# SSH into the ProLUG Hammer server
ssh student@hammer.prolug.lab

# Escalate to root privileges
sudo -i
```
{% endstep %}

{% step %}
**Install MariaDB**

```bash
# Install MariaDB server package
dnf install mariadb-server

# Verify installation
rpm -q mariadb-server
```

**Expected Output**:

```
mariadb-server-10.5.22-1.el9_2.x86_64
```
{% endstep %}

{% step %}
**Start and Verify MariaDB Service**

```bash
# Start the MariaDB service
systemctl start mariadb

# Verify service is running
systemctl status mariadb

# Check that MariaDB is listening on port 3306
ss -ntulp | grep 3306
```

**Expected Output**:

```
● mariadb.service - MariaDB 10.5 database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; disabled)
   Active: active (running)
   
tcp   LISTEN 0  80  *:3306  *:*  users:(("mysqld",pid=1234))
```

{% hint style="success" %}
**Verification Success**: If you see the service as `active (running)` and port 3306 in `LISTEN` state, MariaDB is operational.
{% endhint %}
{% endstep %}
{% endstepper %}

**🔍 STIG Implementation Exercises**

<details>

<summary><strong>Exercise 3.1: V-253666 - Max User Connections Control</strong></summary>

**STIG Finding**: V-253666

**Connect to MariaDB**:

```bash
# Local database connection
mysql
```

**Run the STIG Check Command**:

```sql
-- Check current max_user_connections settings
SELECT user, max_user_connections FROM mysql.user;
```

**Analysis Questions**:

1. **What is the problem?**
   * What security risk does unlimited user connections create?
   * How can an attacker exploit this? (Hint: Resource exhaustion)
2. **What is the fix?**
   * What SQL command sets `max_user_connections`?
   * What is a reasonable limit? (STIG recommends org-defined value)
3. **What is the fix?**
   * Technical/Operational/Management?
   * Preventive/Detective/Corrective?
   * What attack vector does this prevent?
4. **Is it set properly on your system?**
   * What does `max_user_connections=0` mean? (Unlimited)
   * What value would comply with the STIG? (Non-zero, org-defined)

**Remediation Steps**:

```sql
-- Set max_user_connections for all users
-- Replace 50 with your organization's defined limit
UPDATE mysql.user SET max_user_connections = 50 WHERE max_user_connections = 0;

-- Apply changes
FLUSH PRIVILEGES;

-- Verify remediation
SELECT user, max_user_connections FROM mysql.user;
```

**Expected Compliant Output**:

```
+-------------+----------------------+
| user        | max_user_connections |
+-------------+----------------------+
| mariadb.sys |                    0 |
| mysql       |                    0 |
| root        |                   50 |
+-------------+----------------------+
```

{% hint style="warning" %}
**Special Consideration**: System accounts like `mariadb.sys` and `mysql` may legitimately have unlimited connections. The STIG applies to **regular user accounts**.
{% endhint %}

**Document in STIG Viewer**:

* **Status**: Not a Finding (after remediation)
* **Comments**: "Set max\_user\_connections=50 for all regular user accounts per organizational policy."

</details>

<details>

<summary><strong>Exercise 3.2: V-253677 - [STIG Finding Title]</strong></summary>

**STIG Finding**: V-253677

**Open STIG Viewer**:

1. Navigate to Finding V-253677
2. Read the **Discussion** section (understand the risk)
3. Review the **Check Text** (how to verify compliance)
4. Review the **Fix Text** (how to remediate)

**Analysis Questions**:

1. **What is the problem?**
   * Document the security vulnerability this finding addresses
   * What is the potential impact if left unmitigated?
2. **What is the fix?**
   * Identify the specific configuration change required
   * Document the commands to implement remediation
3. **What type of control is being implemented?**
   * Classify: Technical/Operational/Management
   * Classify: Preventive/Detective/Corrective
   * Explain why this classification is accurate
4. **Is it set properly on your system?**
   * Execute the Check Text commands
   * Compare output to STIG compliance criteria
   * Document compliant vs. non-compliant state

**Implementation Pattern**:

```bash
# Step 1: Run Check Text commands from STIG
[Insert commands from STIG Check Text]

# Step 2: Analyze output for compliance

# Step 3: If non-compliant, execute Fix Text commands
[Insert commands from STIG Fix Text]

# Step 4: Re-run Check Text to verify remediation

# Step 5: Document in STIG Viewer
```

{% hint style="info" %}
**Self-Directed Learning**: You're intentionally **not** given the specific STIG content here. Professional security engineers must be able to:

* Read STIG documentation independently
* Interpret Check Text and translate to commands
* Implement Fix Text accurately
* Verify remediation effectiveness

This mirrors real-world workflow where you receive a STIG checklist and must work through findings systematically.
{% endhint %}

</details>

<details>

<summary><strong>Exercise 3.3: V-253678 - [STIG Finding Title]</strong></summary>

**STIG Finding**: V-253678

**Systematic Evaluation Process**:

Follow the same methodology as Exercise 3.2:

Understand the ProblemRead STIG Discussion section. Document:What vulnerability or misconfiguration is being addressed?What is the attack scenario if unmitigated?What is the business/security impact?Identify the FixRead STIG Fix Text section. Document:What configuration changes are required?What files/settings must be modified?Are there any prerequisites or dependencies?Classify the ControlAnalyze the security control type:Technical (system enforced) vs. Operational (process) vs. Management (policy)?Preventive (stops attacks) vs. Detective (finds violations) vs. Corrective (responds to incidents)?Execute Check TextRun the compliance verification commands:# Execute commands from STIG Check Text\[Insert commands here]Document findings:Current configuration stateCompliant or non-compliant?Specific deviations from STIG requirementsImplement RemediationIf non-compliant, execute Fix Text:# Execute commands from STIG Fix Text\[Insert commands here]Critical: Always re-run Check Text after remediation to verify success.Document in STIG ViewerUpdate the checklist:Status: Not a Finding / Open / Not ApplicableComments: Detailed documentation of actions taken, verification results, and current state

**Professional Practice**: Maintain a remediation log with:

* Timestamp of each action
* Commands executed
* Output before and after changes
* Verification of successful remediation

</details>

<details>

<summary><strong>Exercise 3.4: V-253734 - [STIG Finding Title]</strong></summary>

**STIG Finding**: V-253734

**Advanced Analysis Challenge**:

This finding tests your ability to:

* Interpret complex STIG requirements independently
* Implement multi-step remediation procedures
* Verify compliance through systematic testing
* Document findings professionally

**Your Task**:

Complete the full STIG evaluation and remediation cycle:

1. **Problem Identification**:
   * What security control is this finding implementing?
   * Why does this matter in enterprise database environments?
2. **Technical Implementation**:
   * Execute Check Text commands
   * Analyze current compliance state
   * Implement Fix Text if non-compliant
   * Verify remediation success
3. **Control Classification**:
   * Document control type with justification
   * Explain how this control fits into defense-in-depth architecture
4. **Professional Documentation**:
   * Update STIG Viewer checklist
   * Write compliance comments that would satisfy an auditor
   * Include verification evidence (command outputs)

{% hint style="success" %}
**Self-Assessment Question**: Can you complete this exercise without step-by-step guidance?

* **Yes** → You're developing professional security engineering independence
* **No** → Review Exercises 3.2 and 3.3, then retry this exercise

The ability to work through STIGs independently is a critical professional skill. Auditors and compliance teams provide checklists, not instructions.
{% endhint %}

</details>

**🔬 Security Hardening Concepts**

{% tabs %}
{% tab title="🛡️ Database DoS Prevention" %}
**Resource Exhaustion Attacks**:

Databases are vulnerable to denial-of-service through resource exhaustion:

**Attack Vector: Connection Flooding**

```sql
-- Attacker opens unlimited connections
-- Each connection consumes:
-- - Memory (connection buffers)
-- - CPU (connection handling)
-- - File descriptors (network sockets)

-- Without max_user_connections limit:
-- Attacker can exhaust server resources
-- Legitimate users cannot connect
-- Database becomes unavailable
```

**Defense: Connection Limits**

```sql
-- Per-user connection limits
SET max_user_connections = 50;

-- Global connection limits (in my.cnf)
max_connections = 200

-- Connection timeout (in my.cnf)
wait_timeout = 300
interactive_timeout = 300
```

**Enterprise Pattern**:

* **Web Applications**: 10-50 connections per app user
* **Batch Jobs**: 5-10 connections per job scheduler
* **Admin Accounts**: 3-5 connections (for interactive use)
* **Service Accounts**: Org-defined based on workload

**Monitoring**:

```sql
-- Check current connections
SHOW PROCESSLIST;

-- Check connection statistics
SHOW STATUS LIKE 'Threads_connected';
SHOW STATUS LIKE 'Max_used_connections';
```
{% endtab %}

{% tab title="🔐 Authentication Security" %}
**Database Authentication Hardening**:

Databases are critical security boundaries requiring robust authentication:

**STIG-Required Controls**:

1. **Strong Password Policies**:

```sql
-- Enforce password complexity
INSTALL PLUGIN simple_password_check SONAME 'simple_password_check.so';
SET GLOBAL simple_password_check_minimal_length = 14;
```

2. **Account Lifecycle Management**:

```sql
-- Remove test accounts
DROP USER IF EXISTS 'test'@'localhost';

-- Remove anonymous users
DELETE FROM mysql.user WHERE User='';

-- Remove remote root access
DELETE FROM mysql.user WHERE User='root' AND Host NOT IN ('localhost', '127.0.0.1', '::1');

FLUSH PRIVILEGES;
```

3. **Authentication Method Restrictions**:

```sql
-- Disable plugin authentication (use native)
-- Verify no users have empty passwords
SELECT user, host, plugin FROM mysql.user WHERE password = '' OR authentication_string = '';
```

**Enterprise Best Practices**:

* Use SSL/TLS for all remote connections
* Implement external authentication (LDAP, Active Directory)
* Enable audit logging for authentication events
* Rotate service account credentials regularly
{% endtab %}

{% tab title="📊 Audit and Logging" %}
**Database Activity Monitoring**:

STIGs require comprehensive audit logging for security-relevant events:

**Required Audit Events**:

```sql
-- Enable audit plugin
INSTALL PLUGIN server_audit SONAME 'server_audit.so';

-- Configure audit logging
SET GLOBAL server_audit_logging = ON;
SET GLOBAL server_audit_events = 'CONNECT,QUERY_DDL,QUERY_DML_INSERT,QUERY_DML_UPDATE,QUERY_DML_DELETE';
```

**Log Review Requirements**:

```bash
# Audit log location
/var/lib/mysql/server_audit.log

# Monitor for suspicious activity
tail -f /var/lib/mysql/server_audit.log | grep -i 'failed\|denied\|error'
```

**Security Event Categories**:

| Event Type     | Examples                  | Security Significance          |
| -------------- | ------------------------- | ------------------------------ |
| **Connection** | Login/logout, failed auth | Account compromise detection   |
| **DDL**        | CREATE, ALTER, DROP       | Schema tampering detection     |
| **DML**        | INSERT, UPDATE, DELETE    | Data modification tracking     |
| **DCL**        | GRANT, REVOKE             | Privilege escalation detection |

**Enterprise Integration**:

* Forward logs to SIEM (Security Information and Event Management)
* Alert on suspicious patterns (e.g., repeated failed logins)
* Retain logs per compliance requirements (often 1 year minimum)
{% endtab %}

{% tab title="🔒 Privilege Minimization" %}
**Least Privilege Implementation**:

STIGs enforce the security principle of least privilege for database access:

**Privilege Analysis**:

```sql
-- Audit current privileges
SELECT user, host, Select_priv, Insert_priv, Update_priv, Delete_priv, 
       Create_priv, Drop_priv, Grant_priv, Super_priv 
FROM mysql.user;

-- Find users with excessive privileges
SELECT user, host FROM mysql.user WHERE Super_priv = 'Y' AND user NOT IN ('root', 'admin');
```

**Role-Based Access Control (RBAC)**:

```sql
-- Create application-specific roles
CREATE ROLE 'app_reader';
GRANT SELECT ON app_db.* TO 'app_reader';

CREATE ROLE 'app_writer';
GRANT SELECT, INSERT, UPDATE, DELETE ON app_db.* TO 'app_writer';

-- Assign roles to users
GRANT 'app_reader' TO 'webapp_readonly'@'localhost';
GRANT 'app_writer' TO 'webapp'@'localhost';
```

**Privilege Validation Pattern**:

For each database user, ask:

1. What is this user's business function?
2. What minimum privileges are required for that function?
3. Does this user have any excessive privileges?
4. Can privileges be further restricted?

**Red Flags**:

* Application accounts with `SUPER` privilege
* Users with `FILE` privilege (can read/write OS files)
* Wildcard grants (`GRANT ALL ON *.* TO ...`)
* Users with `GRANT OPTION` who aren't DBAs
{% endtab %}
{% endtabs %}

{% hint style="success" %}
✅ **Module 3 Outcome**:

* Functional MariaDB service installation and verification
* Systematic STIG finding evaluation methodology applied
* Multiple security controls implemented and verified
* Security control classification skills developed
* Professional compliance documentation practices established
* Real-world database hardening experience gained
{% endhint %}
{% endstep %}
{% endstepper %}

***

## 🎓 Professional Skills Development

{% hint style="info" %}
**What You've Actually Built**

This wasn't just a lab exercise—you've developed professional security engineering competencies used across government and enterprise environments:

#### Technical Skills Acquired:

* **System Reconnaissance**: Systematic discovery of security-relevant configurations
* **STIG-Based Hardening**: Industry-standard compliance assessment and remediation
* **Database Security**: Enterprise database hardening and access control implementation
* **Security Controls Classification**: Technical, operational, management; preventive, detective, corrective



#### Career-Relevant Competencies:

* Security baseline implementation for production systems
* Regulatory compliance validation (DoD, PCI-DSS, HIPAA patterns)
* Risk mitigation through defense-in-depth configuration
* Professional security documentation and reporting
{% endhint %}

**📋 Resume-Ready Project Descriptions**:

```
Implemented DISA STIG-based security hardening for MariaDB database systems, 
evaluating 4+ security findings across authentication controls, resource 
management, and privilege minimization. Classified security control types 
(technical/operational, preventive/detective/corrective) and documented 
compliance evidence using STIG Viewer for auditor review.

Performed Linux system security reconnaissance to identify filesystem mount 
options, kernel network parameters, and loaded security modules. Validated 
defense-in-depth configurations including noexec/nodev/nosuid mount restrictions, 
IP forwarding controls, and netfilter kernel module implementation.
```

***
