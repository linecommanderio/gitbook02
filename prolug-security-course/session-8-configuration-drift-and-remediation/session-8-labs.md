---
cover: ../../.gitbook/assets/lab-ezgif.png
coverY: -192.80007058679104
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

# 🛠️ Session 8 Labs

I'm building a complete configuration drift detection and remediation system through progressively complex labs.

<details>

<summary>🏗️ What We'll Build</summary>

A **production-grade configuration management framework** with automated drift detection and enforcement:

* [ ] STIG-based change management policy framework
* [ ] AIDE file integrity monitoring system with baseline tracking
* [ ] Automated drift detection workflows with scheduled scanning
* [ ] Ansible-based drift remediation with continuous enforcement
* [ ] Enterprise configuration control demonstrating IaC principles

</details>

### ⏳ Lab Progression

{% stepper %}
{% step %}
**📋 Security Policy Review → Analyze DISA STIG change management controls**
{% endstep %}

{% step %}
**🔍 Drift Detection → Implement AIDE file integrity monitoring**
{% endstep %}

{% step %}
**🤖 Automated Remediation → Deploy Ansible drift correction workflows**
{% endstep %}

{% step %}
**🎯 Advanced Integration → Build comprehensive drift management systems**
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Real-World Workflow**

In enterprise environments, you'll manage drift detection systems and respond to configuration changes more often than you'll build monitoring infrastructure from scratch.

But you'll **absolutely** implement AIDE checks, write remediation playbooks, and enforce configuration baselines through automation.
{% endhint %}

This lab teaches you the complete configuration control pipeline so you understand how drift detection feeds automated remediation in production environments.

By the end, we'll understand:

* The **policy foundation** for change management (DISA STIG controls and operational practices)
* The **technical implementation** (AIDE monitoring, hash verification, baseline tracking)
* The **automation frameworks** (Ansible-based drift remediation with continuous enforcement)

And have a complete configuration management solution that demonstrates both proactive monitoring and reactive correction capabilities.

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

**Lab Environments**:

* KillerCoda Ubuntu Playground: [https://killercoda.com/playgrounds/scenario/ubuntu](https://killercoda.com/playgrounds/scenario/ubuntu)
* KillerCoda Ansible Labs: [https://killercoda.com/het-tanis/course/Ansible-Labs](https://killercoda.com/het-tanis/course/Ansible-Labs)
* Internet browser for interactive labs

**Command Line Access**:

* Root or sudo privileges in lab environments
* Basic text editor familiarity (`vi`, `nano`)
{% endtab %}

{% tab title="📚 Prerequisites" %}
<table data-full-width="true"><thead><tr><th width="375" valign="top">Required Knowledge</th><th valign="top">Required Skills from Previous Sessions</th></tr></thead><tbody><tr><td valign="top"><ul><li>Command line navigation and file operations</li><li>Basic Ansible playbook structure and execution</li><li>YAML syntax fundamentals</li><li>SystemD service management concepts</li><li>File permissions and ownership</li><li>SELinux context basics</li></ul></td><td valign="top"><ul><li>STIG Viewer operation and assessment methodology</li><li>Configuration file editing (<code>vi</code> or <code>nano</code>)</li><li>Service status verification and troubleshooting</li><li>Network connectivity testing (<code>curl</code>, <code>ping</code>)</li><li>Monitoring fundamentals from Session 7</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

***

### 🛠️ Labs to Build Here

{% stepper %}
{% step %}
**📋 Lab 1: Operational Activities - STIG Change Management Review**

Analyze DISA STIG controls for change management to understand the policy foundation for configuration drift management.

{% hint style="info" %}
**What You're Learning**

Before implementing technical drift management, you need to understand:

1. The policy requirements from DISA STIGs for change management
2. Why change control is considered a foundational security control
3. How technical implementations support operational change management practices

This lab establishes the regulatory and operational context for all configuration management activities.
{% endhint %}

**📖 Lab Instructions**

{% stepper %}
{% step %}
**Open STIG Viewer and Import RHEL 9 STIG**

1. Launch STIG Viewer 2.18
2. Import the Red Hat Enterprise Linux 9 STIG
3. Create a new checklist from the imported STIG

{% hint style="success" %}
**STIG Viewer Experience**

You've used STIG Viewer throughout the course—now you're analyzing change management controls that underpin all previous security configurations you've implemented.
{% endhint %}
{% endstep %}

{% step %}
**Filter for Change Management Controls**

1. In STIG Viewer, open the Filters panel
2. Set a filter for "**change management**"
3. Review the filtered results

🔬 Analysis Questions:

<details>

<summary><strong>Question 1: How many STIGs do you see?</strong></summary>

Count the number of STIG controls related to change management.

What to Notice: Change management appears across multiple control categories, not just in a single isolated STIG.

Expected Result: You should find several controls addressing different aspects of change management:

* Configuration change documentation requirements
* Change approval workflows
* Audit trail maintenance
* Baseline configuration management
* Security impact analysis requirements

Key Insight: Change management is pervasive across security controls because it's a foundational practice supporting many security objectives.

</details>

<details>

<summary><strong>Question 2: What is meant by a "robust change management process"?</strong></summary>

Review the control wording carefully. DISA STIGs reference "robust change management" in multiple contexts.

Definition Components:

* **Documented Procedures**: Written policies for how changes are requested, reviewed, approved
* **Approval Workflow**: Multi-level review ensuring appropriate authorization
* **Impact Assessment**: Analysis of security and operational implications before implementation
* **Testing Requirements**: Validation in lower environments before production deployment
* **Rollback Capabilities**: Procedures for reverting changes if issues arise
* **Audit Trail**: Comprehensive logging of all changes, approvers, and outcomes
* **Configuration Baselines**: Established reference points for detecting drift

Why "Robust"? The term emphasizes that change management isn't just paperwork—it's a comprehensive framework protecting system integrity, security, and availability.

Connection to Lab: The technical tools you'll implement (AIDE, Ansible) _support_ this robust process by providing automated detection and enforcement capabilities.

</details>

<details>

<summary><strong>Question 3: Can this be applied in just one STIG? Why or why not?</strong></summary>

Think about the scope and nature of change management.

Analysis:

Change management **cannot** be confined to a single STIG because:

* **Cross-Cutting Concern**: Change management affects all system components (OS, applications, network, storage)
* **Multiple Control Families**: Impacts configuration management (CM), audit (AU), identification and authentication (IA)
* **Enterprise-Wide Process**: Requires coordination across teams, not just technical controls
* **Policy and Technical Integration**: Combines operational procedures with technical enforcement

Evidence in STIGs: When you filtered, you likely found change management references in:

* Operating system configuration controls
* Application configuration controls
* Network device configuration controls
* Audit and accountability controls

Key Insight: Effective change management requires both:

1. **Policy/Operational** practices (approval workflows, documentation)
2. **Technical** controls (drift detection, automated enforcement)

This is why Session 8 emphasizes _both_ operational practices (CMDBs, build books) _and_ technical tools (AIDE, Ansible).

</details>

<details>

<summary><strong>Question 4: What type of control is being implemented with change management in these STIGs?</strong></summary>

Review the control categorization in STIG Viewer.

Control Type Analysis:

Change management controls are typically classified as:

* **Management Controls**: Policy-level controls requiring human oversight and decision-making
* **Operational Controls**: Process-based controls executed by people
* **Technical Controls**: Technology-enforced controls (like AIDE and Ansible)

In DISA STIGs:

Most change management controls fall under the **Configuration Management (CM)** family, which includes both:

* CM-2: Baseline Configuration
* CM-3: Configuration Change Control
* CM-6: Configuration Settings
* CM-7: Least Functionality

Why This Matters: Understanding control types helps you determine:

* What can be automated (technical controls)
* What requires human judgment (management controls)
* What needs procedural enforcement (operational controls)

Session 8 Connection: We implement _technical controls_ (AIDE, Ansible) that _support_ management and operational controls (change approval, documentation).

</details>

<details>

<summary><strong>Question 5: Is the control type different across the STIGs or all the same?</strong></summary>

Compare control types across the filtered results.

Expected Finding: Control types **vary** across different STIGs:

* Some are primarily **technical** (automated monitoring, enforcement)
* Some are primarily **management** (policy requirements, approval workflows)
* Some are **operational** (procedures, documentation practices)

Example Variations:

Control A: "The system must monitor configuration files for unauthorized changes"

* Type: **Technical Control**
* Implementation: AIDE, file integrity monitoring

Control B: "Changes must be approved by security team before implementation"

* Type: **Management Control**
* Implementation: Change advisory board (CAB), approval workflow

Control C: "System administrators must document all configuration changes"

* Type: **Operational Control**
* Implementation: Run books, change tickets, audit logs

Key Insight: Effective change management requires **all three control types** working together:

1. **Management**: Sets policy and provides oversight
2. **Operational**: Ensures processes are followed
3. **Technical**: Automates enforcement where possible

Lab Integration: Session 8 labs implement the technical controls (AIDE detection, Ansible remediation) that support the broader management and operational framework.

</details>
{% endstep %}

{% step %}
**Document Your Findings**

Create a summary document addressing:

1. The number and scope of change management controls in RHEL 9 STIG
2. Your understanding of "robust change management"
3. Why change management spans multiple STIGs and control families
4. The different control types and their relationships
5. How technical tools (like AIDE and Ansible) support policy objectives

**Professional Practice**: In enterprise environments, you'll frequently reference STIGs to justify technical implementations and demonstrate compliance alignment.
{% endstep %}
{% endstepper %}

{% hint style="success" %}
✅ **Outcome**:

* Understanding of DISA STIG change management requirements
* Recognition of change management as a cross-cutting security concern
* Awareness of different control types and their relationships
* Context for technical implementations in subsequent labs
* Foundation for justifying drift management investments to stakeholders
{% endhint %}
{% endstep %}

{% step %}
**🔍 Lab 2: Monitoring Configuration Drift with AIDE**

Implement AIDE (Advanced Intrusion Detection Environment) to detect unauthorized configuration changes through file integrity monitoring.

{% hint style="info" %}
**What You're Building**

AIDE creates a cryptographic baseline of your file system and detects any deviations from that baseline. This lab teaches you:

1. How to install and initialize AIDE monitoring
2. How to configure what AIDE tracks
3. How to interpret AIDE scan results and detect drift
4. How to update baselines after authorized changes
5. Real-world performance considerations for enterprise deployment

This is the **detection** component of drift management—the foundation for automated remediation.
{% endhint %}

**📖 Lab Instructions**

{% stepper %}
{% step %}
**Access KillerCoda Ubuntu Playground**

Navigate to: [https://killercoda.com/playgrounds/scenario/ubuntu](https://killercoda.com/playgrounds/scenario/ubuntu)

This provides an Ubuntu system where you have root access for installing and configuring AIDE.

{% hint style="warning" %}
**Lab Environment Note**

The KillerCoda playground provides a temporary environment. Your work will not persist after the session ends, so complete the lab in one sitting or be prepared to restart.
{% endhint %}
{% endstep %}

{% step %}
**Install AIDE and Observe Installation**

Install AIDE and watch what happens during installation:

```bash
apt -y install aide
```

🔬 Observation Questions:

<details>

<summary><strong>What is being put in the path <code>/etc/aide/aide.conf.d/</code>?</strong></summary>

While installation is running, monitor what's being placed in the AIDE configuration directory:

```bash
# During or after installation
ls -la /etc/aide/aide.conf.d/
```

Expected Contents:

The `/etc/aide/aide.conf.d/` directory contains modular configuration files that define:

* Which directories AIDE should monitor
* What attributes to check for each directory
* Exclusions for paths that change frequently (like logs)
* Different security levels for different filesystem areas

Common Files You'll See:

* `00_aide_conf_header` - Main configuration parameters
* `31_aide_binary_paths` - Monitoring for `/bin`, `/sbin`, `/usr/bin`
* `31_aide_library_paths` - Monitoring for `/lib`, `/usr/lib`
* `31_aide_systemd` - Monitoring for systemd files
* `31_aide_kernel` - Monitoring for kernel modules

Why Modular Configuration? Allows you to customize monitoring by adding/removing configuration snippets without editing a monolithic config file.

Count the Files:

```bash
ls /etc/aide/aide.conf.d/ | wc -l
```

You should see approximately 10-15 configuration files defining different monitoring scopes.

Professional Insight: In enterprise environments, you might add custom configuration files for application-specific directories, vendor software, or organizational security requirements.

</details>
{% endstep %}

{% step %}
**Check AIDE Version**

Verify the installed version:

```bash
aide -v
```

Why This Matters: Different AIDE versions have different capabilities. Knowing your version helps when:

* Reading documentation (features may vary by version)
* Troubleshooting issues (bugs may be version-specific)
* Planning upgrades (new features in newer versions)

Typical Output:

```
Aide 0.17.3

Compiled with the following options:
...
```
{% endstep %}

{% step %}
**Read AIDE Man Page**

Read the first page of the AIDE manual:

```bash
man aide
```

🔬 Analysis Questions:

<details>

<summary><strong>What does AIDE try to do, and how does it do it?</strong></summary>

Focus on the NAME and DESCRIPTION sections of the man page.

AIDE's Purpose:

AIDE (Advanced Intrusion Detection Environment) is a file and directory integrity checker. It:

1. **Creates a baseline database** of file attributes (checksums, permissions, ownership, timestamps)
2. **Compares current system state** against the baseline on subsequent checks
3. **Reports any deviations** that could indicate unauthorized changes or compromises

How It Works:

AIDE Workflow Step 1: Initialize

* Scans filesystem according to aide.conf
* Computes cryptographic hashes of files
* Records permissions, ownership, timestamps
* Stores baseline in database file

Step 2: Check

* Scans filesystem again
* Computes current hashes and attributes
* Compares to baseline database
* Reports additions, deletions, changes

Step 3: Update

* After authorized changes
* Create new baseline incorporating changes
* Replace old database with updated one

Key Capabilities (from man page):

* **Multiple hash algorithms**: MD5, SHA1, SHA256, SHA512 (more secure algorithms preferred)
* **Attribute checking**: Permissions, ownership, file type, inode, links, size, timestamps
* **Extended attributes**: SELinux contexts, ACLs
* **Flexible configuration**: Define different check levels for different directories

Security Foundation: AIDE detects unauthorized changes that could indicate:

* Rootkit installations (modified system binaries)
* Configuration tampering (altered config files)
* Malware persistence (added files in system directories)
* Privilege escalation (changed setuid binaries)

Connection to Session 8: AIDE provides the _detection_ capability that enables _automated remediation_ through Ansible. You can't fix drift you don't detect.

</details>
{% endstep %}

{% step %}
**Examine Daily AIDE Check Configuration**

Review the automated AIDE check configuration:

```bash
cat /etc/cron.daily/aide
```

🔬 Analysis Questions:

<details>

<summary><strong>What does this cron job attempt to do?</strong></summary>

Read through the script to understand the automated workflow.

Script Purpose:

The daily AIDE check attempts to:

1. **Run AIDE integrity check** automatically each day
2. **Email results** to system administrators
3. **Handle errors gracefully** if checks fail
4. **Maintain logs** of all integrity scans

Typical Workflow:

```bash
#!/bin/sh

# Daily AIDE integrity check

# Check if AIDE is configured
if [ ! -f /var/lib/aide/aide.db ]; then
    echo "AIDE database not initialized"
    exit 0
fi

# Run AIDE check
/usr/sbin/aide --check

# Email results if mail is configured

# Log output to /var/log/aide/
```

Why Daily? Balances:

* **Detection speed**: Daily checks mean unauthorized changes detected within 24 hours
* **System load**: Not so frequent that it impacts performance
* **Alert fatigue**: Not so frequent that administrators ignore results

Enterprise Adjustment: High-security environments might run hourly checks. Development environments might run weekly.

</details>

<details>

<summary><strong>What checks are there before execution?</strong></summary>

Look for conditional logic in the script.

Pre-Execution Checks:

The script typically validates:

1. **Database Exists**: Does `/var/lib/aide/aide.db` exist?
   * If not, AIDE hasn't been initialized—nothing to compare against
   * Script exits gracefully rather than failing
2. **AIDE Binary Available**: Is `/usr/sbin/aide` executable?
   * Ensures AIDE is actually installed
   * Prevents errors if package was removed
3. **Configuration Valid**: Is `/etc/aide/aide.conf` readable?
   * Ensures configuration file exists and is accessible
   * Prevents runtime errors during scan
4. **Capability Check** (if using `capsh`): Does process have required capabilities?
   * File integrity checking requires specific capabilities
   * Ensures script has permission to read all monitored files

Why These Checks Matter:

* **Graceful Degradation**: System doesn't generate error emails if AIDE simply isn't set up yet
* **Operational Safety**: Prevents cron job failures that would fill logs and email
* **Administrator Experience**: Only alerts when there's actual drift, not configuration issues

Error Handling Pattern:

```bash
if [ ! -f /var/lib/aide/aide.db ]; then
    # Database doesn't exist - AIDE not initialized
    exit 0  # Silent exit, not an error condition
fi

if [ ! -x /usr/sbin/aide ]; then
    # AIDE not installed
    logger "AIDE binary not found"
    exit 1  # This is an error - log it
fi
```

</details>

<details>

<summary><strong>What is <code>capsh</code> used for?</strong></summary>

Read the `capsh` man page:

```bash
man capsh
```

Capsh Purpose:

`capsh` (capability shell) is a utility for exploring and constraining capability sets in Linux.

Linux Capabilities Background:

Traditional Unix: Either root (UID 0, all privileges) or non-root (limited privileges)

Linux Capabilities: Fine-grained privileges that can be assigned individually

* `CAP_DAC_READ_SEARCH` - Bypass file read permission checks
* `CAP_SYS_ADMIN` - Perform system administration operations
* `CAP_CHOWN` - Change file ownership
* _Many others..._

Why AIDE Uses Capabilities:

AIDE needs to read ALL files on the system, including:

* Files you don't have normal read permission for
* Files in directories you can't normally access
* Protected system files

Without running as full root, AIDE can use specific capabilities:

```bash
# Run AIDE with specific capability
capsh --caps="cap_dac_read_search+ep" -- -c "/usr/sbin/aide --check"
```

This grants ONLY the read capability needed, following **principle of least privilege**.

Security Benefit:

* AIDE doesn't need full root privileges
* Reduces attack surface if AIDE process is compromised
* Limits potential damage from AIDE vulnerabilities

Enterprise Application: Capability-based security is increasingly common in containerized environments and security-hardened systems.

</details>
{% endstep %}

{% step %}
**Initialize AIDE Database**

Create the initial AIDE baseline database:

```bash
time aide -i -c /etc/aide/aide.conf
```

Command Breakdown:

* `time` - Measures execution duration
* `aide` - AIDE binary
* `-i` or `--init` - Initialize new database
* `-c /etc/aide/aide.conf` - Specify configuration file

🔬 Performance Questions:

<details>

<summary><strong>How long did initialization take?</strong></summary>

Expected Duration: Instructor's example took **5 minutes 8 seconds** on the lab system.

Your timing will vary based on:

* **Number of files**: More files = longer scan time
* **Hash algorithms**: SHA-256 is slower than MD5 but more secure
* **System resources**: CPU speed, disk I/O performance
* **Filesystem**: SSD vs HDD makes significant difference

Example Output:

```bash
real    5m8.342s
user    3m42.156s
sys     1m26.186s
```

Why Timing Matters:

* **Maintenance Windows**: Need to know if daily checks fit in available time
* **Resource Planning**: High CPU/disk usage might impact application performance
* **Scalability**: Enterprise environments with thousands of systems need capacity planning

</details>

<details>

<summary><strong>How much time was wall clock vs. system/user time?</strong></summary>

Compare the three time measurements:

Time Components:

* **real (wall clock)**: Actual elapsed time from start to finish
  * Example: 5 minutes 8 seconds
  * Includes all time—CPU, I/O wait, system overhead
* **user**: CPU time in user-space code
  * Example: 3 minutes 42 seconds
  * Time spent in AIDE program itself
* **sys**: CPU time in kernel-space (system calls)
  * Example: 1 minute 26 seconds
  * Time spent in kernel: file I/O, hash computation

Analysis:

If **user + sys ≈ real**: CPU-bound operation, little I/O wait

If **real >> user + sys**: I/O-bound operation, lots of disk waiting

Enterprise Consideration: On systems with slow storage (spinning disks, network storage), AIDE initialization might take significantly longer, predominantly in I/O wait.

</details>

<details>

<summary><strong>Why might you want to know this on your systems?</strong></summary>

Operational Reasons:

Maintenance Window Planning:

* Daily checks must complete within allocated time
* If AIDE takes 30 minutes and you have 1-hour windows, you have margin
* If AIDE takes 4 hours on some systems, you need longer windows or less frequent checks

Resource Capacity Planning:

* High CPU usage during scans might impact application performance
* If user+sys time is high, AIDE is CPU-intensive—consider scheduling during low-traffic periods
* If I/O wait is high, AIDE is disk-intensive—avoid running during backup windows

Detection Timing:

* If checks take hours, drift goes undetected longer
* Faster scans mean quicker detection of unauthorized changes
* Balance speed against resource impact

Scale Considerations:

* If you have 1,000 systems and checks take 5 minutes each: 5,000 minutes = 83 hours total
* Checks must be staggered to avoid overwhelming monitoring infrastructure
* Faster individual checks allow tighter staggering and quicker fleet-wide detection

Troubleshooting Performance Issues:

* Slow AIDE scans might indicate disk problems
* Excessive CPU time might indicate hash algorithm issues
* Monitoring timing changes over time can detect system degradation

Example Enterprise Decision:

> "AIDE checks take 10 minutes on average. We have 500 production systems. We'll stagger checks over 24 hours (one system every \~3 minutes) to distribute load. Maximum drift detection time: 24 hours."

</details>

<details>

<summary><strong>What do you notice about the output?</strong></summary>

Expected Output Characteristics:

```bash
AIDE, version 0.17.3

### AIDE database at /var/lib/aide/aide.db.new initialized.

Number of entries:      119847

---------------------------------------------------
The attributes of the (uncompressed) database(s):
---------------------------------------------------

/var/lib/aide/aide.db.new
  MD5      : abcdef123456...
  SHA256   : 1234567890abcdef...
  
End timestamp: 2024-01-15 10:42:13 -0500 (run time: 5m 8s)
```

Key Observations:

* **Massive Scale**: Example tracked \~119,847 entries (files/directories)
* **Database Location**: Created at `/var/lib/aide/aide.db.new` (not active yet)
* **Database Checksums**: Database file itself is hashed—protects baseline integrity

What You Need to Read About:

* **AIDE Configuration Syntax**: How to customize what's monitored
* **Hash Algorithms**: Choosing between MD5, SHA1, SHA256, SHA512
* **Rule Definitions**: Understanding `p+i+n+u+g+s+b+m+c+sha256` syntax
* **Performance Tuning**: Reducing scan time if needed
* **Database Management**: Backup, rotation, secure storage

</details>
{% endstep %}

{% step %}
**Activate AIDE Database**

Move the initialized database to the active location:

```bash
cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db
```

Why This Step:

* AIDE uses `/var/lib/aide/aide.db` for comparisons (without `.new`)
* Initialization creates `.new` to avoid overwriting existing database
* You explicitly promote new database to active status
* This two-step process prevents accidental baseline overwrites

{% hint style="warning" %}
**Production Consideration**:

In enterprise environments, you would:

1. Review the new database before activation
2. Backup the old database before replacement
3. Document the baseline update in change management
4. Schedule activation during maintenance window

Never blindly replace production baselines without review.
{% endhint %}
{% endstep %}

{% step %}
**Test AIDE with Controlled Changes**

Create test changes and verify AIDE detection:

```bash
# Create new directory
mkdir /root/prolug

# Create test files
touch /root/prolug/test1
touch /root/prolug/test2

# Run AIDE check
time aide -c /etc/aide/aide.conf --check
```

🔬 Analysis Questions:

<details>

<summary><strong>Did you see your new files created?</strong></summary>

Expected Output:

```bash
AIDE found differences between database and filesystem!!

Added entries:
--------------
added: /root/prolug
added: /root/prolug/test1
added: /root/prolug/test2

Number of entries:  119850
Added entries:      3
```

What This Demonstrates:

* AIDE successfully detected new files
* Comparison against baseline identified additions
* Three new entries: directory + two files

Drift Detection in Action: This is exactly how unauthorized changes are detected in production.

</details>

<details>

<summary><strong>How long did the check take to run?</strong></summary>

Compare check time to initialization time.

Expected Result: Checks are **significantly faster** than initialization.

* **Initialization**: 5+ minutes (computed hashes for all files)
* **Check**: Seconds to \~1 minute (selective checks and metadata comparisons)

Why Faster?

* Initialization: Compute hash for EVERY file
* Check: Only compute hashes for files with changed metadata
* Smart optimization: If size/timestamp unchanged, hash likely unchanged

Example:

```bash
real    0m42.123s
user    0m15.432s
sys     0m8.891s
```

Operational Significance:

* Daily checks have minimal performance impact
* Can run frequent checks without overloading systems
* Quick checks mean rapid drift detection in production

</details>

<details>

<summary><strong>What type of usage do you see against user/system space?</strong></summary>

Analyze the time breakdown:

Typical Pattern for Checks:

```bash
real    0m42s
user    0m15s
sys     0m8s
```

Resource Analysis:

* **Lower CPU Usage** during checks vs initialization
* **I/O Patterns**: checks are more I/O-bound but optimized
* **System Call Distribution**: stat() and metadata reads dominate

Why This Matters:

* Low resource usage means checks can run frequently
* Minimal performance impact on applications
* Scales to thousands of systems without infrastructure burden

</details>
{% endstep %}
{% endstepper %}

{% hint style="success" %}
✅ **Outcome**:

* Successfully installed and initialized AIDE monitoring
* Understanding of AIDE configuration and operation
* Experience with file integrity checking and drift detection
* Awareness of performance characteristics and resource usage
* Foundation for integrating AIDE into automated drift management workflows
* Recognition of scale (119,000+ files monitored) and capabilities
{% endhint %}
{% endstep %}

{% step %}
**🤖 Lab 3: Using Ansible to Fix Drift**

Implement Ansible-based automated remediation to correct configuration drift and enforce baseline configurations.

{% hint style="info" %}
**What You're Building**

This lab demonstrates the **remediation** component of drift management. You'll:

1. Deploy baseline configurations with Ansible playbooks
2. Simulate unauthorized changes (configuration drift)
3. Use Ansible to detect and correct drift automatically
4. Understand the implications of automated enforcement
5. Experience the "bulldozer effect" of continuous configuration enforcement

This integrates with AIDE (detection) to create a complete drift management system.
{% endhint %}

**📖 Lab Instructions**

{% stepper %}
{% step %}
**Complete Web Server Environment Deployment Lab**

Navigate to the KillerCoda Ansible lab:

[https://killercoda.com/het-tanis/course/Ansible-Labs/16-Ansible-Web-Server-Env-Deploy](https://killercoda.com/het-tanis/course/Ansible-Labs/16-Ansible-Web-Server-Env-Deploy)

Lab Overview:

This interactive lab walks you through:

* Deploying multiple web server environments (Dev, Test, QA) with Ansible
* Configuring each environment on different ports
* Verifying successful deployment
* Simulating unauthorized changes
* Using playbooks to remediate drift

Follow the Lab Instructions: Complete all steps in the KillerCoda environment.

{% hint style="warning" %}
**Critical Step**: Ensure you complete the lab through the drift simulation where one environment becomes broken. This sets up the remediation scenario.
{% endhint %}
{% endstep %}

{% step %}
**Verify Broken Environment**

After completing the drift simulation in the KillerCoda lab, verify the broken state:

```bash
curl node01:8081
```

Expected Result: Connection failure or error response.

What Happened:

<details>

<summary><strong>The Drift Scenario</strong></summary>

Backstory:

One of your development teams figured out they could modify the `test` and `qa` environments because a previous engineer left them in the sudoers file.

They've been "mucking around" (making unauthorized changes) in test/QA environments without following change management procedures.

Current State:

```bash
# Check all environments
curl node01:8080  # Dev - Working ✅
curl node01:8081  # Test - BROKEN ❌
curl node01:8082  # QA - Working ✅
```

Root Cause:

* Developer with sudo access deleted Test environment configuration
* Changes made outside proper change management procedures
* No documentation of what was changed or why

Your Mission: Restore the environment to baseline configuration using Ansible automation.

</details>
{% endstep %}

{% step %}
**Remediate Drift with Ansible**

You can address the unauthorized access separately with the security team, but for now you need to get those environments back to working state.

Run your original deployment playbook to see if it restores the environment:

```bash
ansible-playbook -i /root/hosts /root/web_environment.yaml
```

Watch the Execution: Observe Ansible's output as it:

* Detects drift (missing configuration)
* Redeploys missing components
* Restarts affected services
* Converges system to desired state

🔬 Analysis Questions:

<details>

<summary><strong>Did this force the system back into a working configuration?</strong></summary>

Verify all environments after playbook execution:

```bash
curl node01:8080  # Dev - Should work ✅
curl node01:8081  # Test - Should work now ✅
curl node01:8082  # QA - Should work ✅
```

Expected Result: Yes, all environments are working.

What Happened:

* Ansible compared current state to desired state defined in playbook
* Detected missing Test environment configuration
* Redeployed from template source
* Restarted web service
* System now matches baseline

Key Insight: Idempotent playbook design means running the same playbook:

* **On drift systems**: Fixes them (remediation)
* **On correct systems**: Makes no changes (validation)
* **Repeatedly**: Always converges to desired state

The "Bulldozer" Effect: Ansible laid the correct configuration over the top of whatever was there, forcing the system back to baseline regardless of what developers changed.

</details>

<details>

<summary><strong>If it worked, would it always work, or could they put this in a state that wouldn't work on?</strong></summary>

Short Answer: They could definitely break it in ways this playbook can't fix.

Detailed Analysis:

What This Playbook Can Fix:

* Missing configuration files → Redeploys from templates
* Deleted service configurations → Recreates them
* Stopped services → Restarts them
* Wrong file permissions → Corrects them
* Incorrect file content → Overwrites with templates

What Could Break This Playbook:

* File Path Changes (moving directories)
* Package Removal (uninstalling required packages)
* Firewall Blocks (ports blocked)
* Broken Dependencies (corrupting libraries)
* Permissions on Ansible Files (make playbook unreadable)

Professional Reality: Such extensive damage requires significantly more effort and clearly crosses into malicious or negligent territory.

</details>

<details>

<summary><strong>What is your test?</strong></summary>

Validation Commands:

```bash
# Test all three environments
curl node01:8080  # Dev environment
curl node01:8081  # Test environment
curl node01:8082  # QA environment
```

Each should return:

* HTTP 200 OK response
* Default web page content
* Confirmation that web service is running on expected port

Comprehensive Testing Approach:

```bash
# 1. Service Status
systemctl status httpd

# 2. Port Verification
ss -tlnp | grep -E '8080|8081|8082'

# 3. Configuration Files
ls -l /etc/httpd/conf.d/dev.conf
ls -l /etc/httpd/conf.d/test.conf
ls -l /etc/httpd/conf.d/qa.conf

# 4. Functionality Test
for port in 8080 8081 8082; do
    echo "Testing port $port:"
    curl -s node01:$port | head -5
done

# 5. Log Verification
tail /var/log/httpd/access_log
```

Enterprise Testing: In production, automated tests would run:

* Health checks for all services
* Response time verification
* Content validation (not just HTTP 200)
* Integration testing across dependent services
* Monitoring dashboard confirmation

</details>

<details>

<summary><strong>Could this cause potential problems in the environment?</strong></summary>

Absolutely YES. Automated drift remediation creates significant operational challenges.

Problems This Creates:

* Development Workflow Disruption (automation overwriting legitimate tests)
* Legitimate Configuration Testing blocked
* Emergency Response Complications (automation reverting emergency fixes)
* The False Positive Problem (automation can't distinguish authorized drift)
* Change Management Bypass Temptation (teams disable automation temporarily)

These are operational and process challenges that must be addressed before wide enforcement is enabled.

</details>

<details>

<summary><strong>Is the problem based on technology or operational practices? Why?</strong></summary>

The problem is fundamentally OPERATIONAL, not technical.

Technical Capability Works:

* Ansible detects drift and remediates reliably.

Operational Problems:

* Lack of Change Management Process
* Insufficient Environment Segregation
* Missing Communication and Training
* Inadequate Policy Framework

Enterprise Solution:

* Tiered Enforcement Strategy (looser in dev, stricter in prod)
* Change Management Integration (tickets, exemptions, maintenance windows)
* Communication and training to align teams

Lesson: Technology alone never solves organizational problems. Successful drift management requires technical capability + operational procedures + cultural alignment.

</details>
{% endstep %}

{% step %}
**Understand Continuous Enforcement**

Consider the implications of running this playbook automatically every 5 minutes:

```bash
# Hypothetical cron job (don't actually create this in lab)
*/5 * * * * ansible-playbook -i /root/hosts /root/web_environment.yaml
```

The Continuous Enforcement Pattern:

Every 5 minutes:

1. Ansible checks current state
2. Compares to desired state in playbook
3. Makes any necessary changes
4. Forces system back to baseline
5. Logs all changes made

Impact on Unauthorized Changes:

* Any manual modifications persist maximum 5 minutes
* Next scheduled run detects drift
* Playbook overwrites unauthorized changes

Communication to Development Teams:

> "Automated enforcement runs every 5 minutes on production systems. Any changes made outside proper change management will be automatically removed. Submit change requests through proper channels or your modifications will be continuously overwritten."

When Appropriate:

* Production systems requiring strict baseline adherence
* Security-critical configurations
* Compliance environments

When Problematic:

* Development/test environments needing experimentation
* Systems undergoing migration
* Troubleshooting situations requiring temporary modifications

{% hint style="danger" %}
**Professional Judgment Required**:

Automated enforcement is powerful but requires careful operational integration:

* Define clear policies for which systems have automated enforcement
* Establish proper change management procedures
* Provide exemption processes for legitimate needs
* Communicate enforcement policies clearly to all teams
* Monitor for patterns indicating operational friction

The technology enables the capability. Operations determines when and where it's appropriate.
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="success" %}
✅ **Outcome**:

* Hands-on experience with Ansible drift remediation
* Understanding of idempotent playbook design for drift correction
* Awareness of the "bulldozer effect" in automated enforcement
* Recognition of operational challenges with continuous enforcement
* Distinction between technical capability and operational appropriateness
* Foundation for designing drift management policies and procedures
{% endhint %}
{% endstep %}

{% step %}
**🎯 Lab 4: Digging Deeper Challenge (Optional)**

Advanced integration challenge for engineers ready to extend their drift management capabilities.

{% hint style="info" %}
**Professional Development**

This challenge isn't required for lab completion but represents real-world scenario integration you'll encounter in enterprise environments. It's an opportunity to:

* Combine multiple automation concepts into comprehensive solutions
* Practice system verification and reporting workflows
* Understand how drift management integrates with build automation
* Contribute to infrastructure automation capabilities
{% endhint %}

**📋 Challenge Tasks**

<details>

<summary><strong>Challenge: CSV Reporting for Build Process Verification</strong></summary>

**Objective**: Create an automated system to verify that systems are stamped according to your build process using custom facts and CSV reporting.

Lab Foundation:

Complete this KillerCoda lab first:

[https://killercoda.com/het-tanis/course/Ansible-Labs/19-Ansible-csv-report](https://killercoda.com/het-tanis/course/Ansible-Labs/19-Ansible-csv-report)

Then Integrate with Custom Facts lab:

[https://killercoda.com/het-tanis/course/Ansible-Labs/12-Ansible-System-Facts-Grouping](https://killercoda.com/het-tanis/course/Ansible-Labs/12-Ansible-System-Facts-Grouping)

Your Challenge:

Think Through the Design:

What metadata would you track in custom facts?

Example fields:

* Build date and time
* Build operator/automation system
* Build playbook version
* Base image or template used
* Security baseline applied (STIG checklist)
* Intended environment (dev/test/prod)
* Approved configuration baseline hash

Example Custom Fact:

```json
{
  "build_metadata": {
    "build_date": "2024-01-15T10:30:00Z",
    "build_operator": "ansible_automation_platform",
    "playbook_version": "v2.3.1",
    "base_template": "rhel9-minimal-2024.01",
    "stig_baseline": "RHEL9_STIG_V1R1",
    "environment": "production",
    "baseline_hash": "sha256:abcdef123456..."
  }
}
```

How would you generate the CSV report?

Ansible playbook snippet:

```yaml
- name: Generate build verification report
  hosts: all
  tasks:
    - name: Gather custom build facts
      setup:
        filter: ansible_local

    - name: Create CSV report
      template:
        src: build_verification.csv.j2
        dest: /tmp/build_verification_report.csv
      delegate_to: localhost
      run_once: yes
```

CSV Template (Jinja2):

```jinja2
hostname,build_date,build_version,environment,baseline_hash,verified
{% for host in groups['all'] %}
{{ hostvars[host]['ansible_hostname'] }},
{{ hostvars[host]['ansible_local']['build_metadata']['build_date'] }},
{{ hostvars[host]['ansible_local']['build_metadata']['playbook_version'] }},
{{ hostvars[host]['ansible_local']['build_metadata']['environment'] }},
{{ hostvars[host]['ansible_local']['build_metadata']['baseline_hash'] }},
{{ 'Yes' if hostvars[host]['ansible_local']['build_metadata']['stig_baseline'] == expected_baseline else 'No' }}
{% endfor %}
```

How would this help verify build process compliance?

Verification Capabilities:

* **Audit Trail**: Complete history of system builds
* **Consistency Validation**: All systems use approved baseline
* **Drift Detection**: Compare current state to build metadata
* **Compliance Reporting**: Generate evidence for audits
* **Troubleshooting**: Identify systems with incorrect builds

Integration with Drift Management:

Combined workflow:

1. Check custom facts: What was system supposed to be built as?
2. Run AIDE check: What is current system configuration?
3. Compare: Does current config match intended build?
4. Report: Generate CSV showing compliance status
5. Remediate: Fix any systems out of compliance

Example comprehensive verification playbook snippet:

```yaml
- hosts: all
  tasks:
    - name: Check build facts exist
      stat:
        path: /etc/ansible/facts.d/build_metadata.fact
      register: build_fact

    - name: Run AIDE integrity check
      command: aide --check
      register: aide_result
      failed_when: false

    - name: Verify baseline hash matches
      assert:
        that:
          - ansible_local.build_metadata.baseline_hash == expected_hash
        fail_msg: "System baseline does not match expected configuration"

    - name: Create verification report
      template:
        src: system_verification.csv.j2
        dest: /reports/verification_{{ ansible_date_time.date }}.csv
      delegate_to: localhost
      run_once: yes
```

Report Output Example:

```csv
hostname,build_compliant,aide_clean,baseline_match,last_checked
server01,Yes,Yes,Yes,2024-01-15 14:30:00
server02,Yes,No,Yes,2024-01-15 14:30:05
server03,No,Yes,No,2024-01-15 14:30:10
```

Enterprise Value:

* **Automated Compliance**: Continuous verification without manual checks
* **Audit Evidence**: CSV reports provide proof of configuration management
* **Drift Visualization**: Easy to see which systems need attention
* **Build Process Validation**: Confirms automation working correctly
* **Troubleshooting Aid**: Quickly identify systems with build issues

Professional Application:

This combined approach (custom facts + AIDE + CSV reporting) enables:

* **Configuration Management Database (CMDB)**: Automated population
* **Compliance Dashboards**: Import CSV into visualization tools
* **Automated Remediation**: Trigger fixes based on report results
* **Change Impact Analysis**: Compare before/after build changes

</details>

🔬 Advanced Thinking Questions:

1. How would you handle systems built before custom facts were implemented?
   * Legacy system handling, gradual adoption, baseline establishment for existing systems
2. What would you do if AIDE and custom facts disagree?
   * Define conflict resolution logic, investigation workflow, trust hierarchy
3. How would you scale this to 10,000 systems?
   * Report generation performance, storage and retention strategy, automated analysis/alerting

{% hint style="success" %}
✅ **Outcome**:

* Advanced integration of Ansible fact gathering and reporting
* Understanding of build process verification methods
* Practical experience with CSV report generation
* Foundation for comprehensive configuration management systems
* Skills combining multiple automation concepts into cohesive solutions
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
**Lab Environment Cleanup**

If you used the ProLUG lab environment, please reboot the machine from the command line when you are done:

```bash
sudo reboot
```

This ensures other students have the intended clean environment for their labs.
{% endhint %}

***

{% hint style="info" %}
🎓 **Professional Development**

These labs mirror actual enterprise configuration management workflows—from policy assessment through drift detection to automated remediation.

> _You've implemented AIDE file integrity monitoring, executed Ansible drift remediation, and experienced the operational challenges of automated enforcement using the same tools and processes required in corporate environments._

**Career Impact**: Add these skills to your project section on your resume:

* **"Enterprise configuration drift management"**
* **"AIDE file integrity monitoring"**
* **"Ansible automated remediation"**
* **"DISA STIG compliance assessment"**
{% endhint %}

***

Page Cover Recommendation: Full-width split-screen showing left side with chaotic drift indicators (red alerts, mismatched configs, AIDE warnings) and right side with clean automated remediation (green checkmarks, Ansible playbook execution, systems converging to baseline). Central divider showing the transition from "Drift Detected" to "Baseline Enforced" with visual workflow arrows.

Page Icon: 🧪 (test tube representing laboratory experimentation and hands-on learning)

Section Icons Strategic Placement:

* 🔧 for Technical implementation steps
* 🎓 for Professional skills and career impact
* ✅ for Successful completion outcomes
* ⚠️ for Warnings and important cautions
* 💡 for Key insights and concepts
* 🏢 for Enterprise context and real-world scenarios
* 🔍 for Analysis questions and deep-dive thinking
