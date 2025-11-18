---
cover: ../.gitbook/assets/session8-ezgif.webp
coverY: -14.470028426528357
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

# ⚠️ Session 8: Configuration Drift and Remediation

We will focus on **identifying, preventing, and correcting configuration drift** through automated detection and remediation strategies.

This session explores how to maintain system consistency and security by detecting unauthorized changes, enforcing configuration baselines, and implementing Infrastructure as Code practices that ensure systems remain in their intended state.

Configuration drift is the silent enemy of reliable infrastructure. When systems slowly deviate from their intended state—whether through manual changes, failed updates, or misconfigured automation—security risks increase and operational reliability suffers.

{% hint style="info" %}
**Enterprise Reality**: Configuration drift happens to all systems. The question isn't _if_ drift will occur, but whether you'll detect it and respond appropriately.

> _Our job is to_ **build systems that can identify and self-correct unauthorized changes** _while maintaining auditable change control for legitimate modifications._
{% endhint %}

***

## 📚 Learning Objectives

By the end of Session 8, you will have foundational knowledge and skills in:

{% tabs %}
{% tab title="💪 Core Competencies" %}
1. **Drift Detection**: Identifying configuration changes through file integrity monitoring
2. **Change Management**: Implementing CMDB practices and configuration baselines
3. **Automated Remediation**: Using Ansible to enforce desired system state
4. **Infrastructure as Code**: Managing configuration through version-controlled automation
{% endtab %}

{% tab title="🎓 Professional Skills" %}
5. **Audit Compliance**: Connecting drift management to compliance and incident response
6. **Risk Assessment**: Understanding when to check versus when to fix configuration drift
7. **System Lifecycle**: Managing build books, run books, and configuration documentation
8. **Immutable Infrastructure**: Implementing patterns that prevent unauthorized changes
{% endtab %}
{% endtabs %}

***

## 🎯 Relevance & Context

For security engineers, **controlling configuration state is foundational** for maintaining security posture and operational stability.

Configuration drift undermines both security and operational goals. Whether through silent config changes or forgotten test artifacts, drift introduces uncertainty and risk. When systems slowly deviate from their intended state, security risks increase and reliability suffers.

In enterprise environments, undocumented changes can void audits, invalidate incident investigations, or introduce vulnerabilities unnoticed. Every unauthorized modification represents a potential security gap and creates operational unpredictability.

By managing configuration as code and enforcing strong change control policies, security engineers can reduce attack surfaces, maintain auditability, and ensure long-term system consistency.

This session ties together principles of monitoring (Session 7), logging, and automation into a unified practice: **configuration control**. Security engineers must treat configuration as code and enforce strong change control policies. By learning to detect, document, and automatically remediate drift, you'll be equipped to reduce your organization's attack surface and ensure long-term consistency.

{% hint style="warning" %}
**Critical Understanding**: There are two types of configuration drift:

1. **Unauthorized Changes** (Bad): Manual modifications outside change management
2. **Entropy Over Time** (Inevitable): Normal system operations and aging

Both must be tracked, but unauthorized changes require immediate remediation.
{% endhint %}

#### What You'll Learn

<details>

<summary>🔧 <strong>Technical Skills</strong></summary>

* Implementing AIDE (Advanced Intrusion Detection Environment) for file integrity monitoring
* Using Ansible playbooks to detect and remediate configuration drift
* Creating and maintaining configuration baselines and build books
* Implementing hash verification for system files and configurations
* Building automated drift detection and correction workflows
* Understanding when to check drift versus when to fix drift

</details>

<details>

<summary>🎓 <strong>Professional Skills</strong></summary>

* Designing change management frameworks with CMDBs and configuration items
* Understanding the operational versus technical practices for drift management
* Balancing automatic remediation with human oversight
* Communicating configuration changes to stakeholders and audit teams
* Implementing compliance-driven configuration management
* Connecting monitoring, logging, and automation into unified configuration control

</details>

{% columns %}
{% column width="58%" %}
**Why Configuration Control Matters**

**The Challenge**:

* Systems naturally drift from intended configuration over time
* Manual changes bypass change management and documentation
* Development teams may modify systems outside proper procedures
* Unauthorized modifications can invalidate audits and compliance
* Silent configuration changes introduce security vulnerabilities

**The Solution**:

* **Automated Detection**: Tools like AIDE monitor file system integrity continuously
* **Infrastructure as Code**: Configuration managed through version-controlled playbooks
* **Automated Remediation**: Ansible enforces desired state automatically
* **Change Management**: CMDBs track approved configuration items systematically
* **Immutable Patterns**: Systems designed to resist unauthorized modifications

**Real-World Application**: Think of quality control in pharmaceutical manufacturing—every step must be documented, every deviation investigated, every batch traceable.

* Regulators can audit the entire production chain and verify no unauthorized changes occurred.

That's exactly how enterprise configuration management works: _**ensure every system change is documented, authorized, and reversible, creating an auditable trail from initial build through entire system lifecycle.**_
{% endcolumn %}

{% column width="42%" %}
{% hint style="success" %}
**What You're Building**:

A comprehensive configuration management framework with automated drift detection, remediation playbooks, and change control procedures that maintain system integrity.
{% endhint %}

{% hint style="warning" %}
**Important Context**:

"Checking drift" versus "fixing drift" are fundamentally different operations. Checking is passive observation. Fixing is always disruptive—like dropping a cinder block in front of a race car to force it back to the center line.
{% endhint %}

{% hint style="info" %}
**Career Reality**:

You'll spend more time **preventing unauthorized changes** and **documenting legitimate ones** than you will troubleshooting individual configuration issues.

Understanding systematic approaches to configuration management is essential for enterprise operations.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

***

## Understanding System Lifecycle Management

{% hint style="info" %}
**Foundation Concept**: Systems have a lifecycle from initial build through ongoing operations to eventual replacement.
{% endhint %}

{% tabs %}
{% tab title="Build Phase 🏗️" %}
**Build Book Creation**:

* Document exact system construction steps
* Record initial configuration decisions
* Establish baseline configuration state
* Define approved configuration items (CIs)
* Create reference architecture documentation

**Purpose**: Establishes the "ground truth" of how the system _should_ be configured.

**Example Elements**:

```yaml
# Build Book Structure
- Hardware specifications
- Operating system installation
- Initial package selection
- Network configuration
- Security baseline (STIG compliance)
- User and access configuration
- Service configuration
- Integration points
```

**Critical Insight**: The build book becomes your reference point for detecting drift. If you don't document how you built it, you can't effectively manage how it changes.
{% endtab %}

{% tab title="Operations Phase 🔧" %}
**Run Book Maintenance**:

* Document operational procedures
* Track approved configuration changes
* Update baseline as changes occur
* Maintain change history and rationale
* Record troubleshooting procedures

**Purpose**: Provides the "owner's manual" for system operations and maintenance.

**Key Activities**:

```yaml
# Run Book Components
- Daily operational procedures
- Scheduled maintenance tasks
- Approved change procedures
- Troubleshooting guides
- Recovery procedures
- Escalation contacts
- Performance baselines
```

**Drift Management**: Run books document _authorized_ changes, distinguishing them from unauthorized drift.
{% endtab %}

{% tab title="Change Management 📋" %}
**CMDB Integration**:

* Configuration Management Database
* Configuration Items (CIs)
* Change approval workflows
* Audit trail maintenance
* Compliance verification

**Process Framework**:

```yaml
# Change Management Flow
1. Change request submitted
2. Impact assessment performed
3. Approval obtained
4. Implementation scheduled
5. Change executed
6. Verification performed
7. Documentation updated
8. CMDB records updated
```

**Professional Standard**: In enterprise environments, every configuration change must flow through change management—or it's considered unauthorized drift.
{% endtab %}
{% endtabs %}

***

## The Two Types of Configuration Drift

{% stepper %}
{% step %}
#### Unauthorized Configuration Changes

**Characteristics**:

* Manual modifications outside change management
* Development teams making "temporary" changes
* Security policy violations
* Undocumented system modifications
* Changes made without proper approval

**Impact**:

* Security vulnerabilities introduced
* Audit compliance violations
* System behavior becomes unpredictable
* Recovery procedures may fail
* Change history is incomplete

**Example Scenario**:

```bash
# Developer gains sudo access

# "Temporarily" modifies Apache configuration

# Changes port numbers for testing

# Forgets to document or reverse changes

# System now deviates from approved baseline
```

**Response**: Must be detected immediately and remediated through proper change management.
{% endstep %}

{% step %}
#### Natural System Entropy

**Characteristics**:

* Log files growing over time
* Cache files accumulating
* Temporary files being created
* System processes generating runtime data
* Normal wear and aging of components

**Impact**:

* Storage space consumption
* Performance degradation over time
* Resource exhaustion if unmanaged
* Expected and manageable drift

**Example**:

```bash
# Natural entropy examples
/var/log files growing
/tmp directory accumulating files
User home directories expanding
Package cache filling
Kernel modules loading/unloading
```

**Response**: Managed through routine maintenance, monitoring, and capacity planning rather than emergency remediation.
{% endstep %}

{% step %}
#### Detection Strategies

**For Unauthorized Changes**:

* File integrity monitoring (AIDE)
* Configuration auditing (Ansible check mode)
* Access logging and review
* Security event correlation
* Compliance scanning

**For Natural Entropy**:

* Disk space monitoring
* Log rotation policies
* Capacity trending
* Performance baselines
* Scheduled maintenance

**Key Distinction**: Unauthorized changes require immediate investigation and correction. Natural entropy requires planned management through operational procedures.
{% endstep %}
{% endstepper %}

***

## Operational vs. Technical Drift Management

{% columns %}
{% column width="50%" %}
#### Operational Practices

**Change Management Team**:

* Review and approve all configuration changes
* Maintain CMDB with configuration items
* Coordinate change schedules and impact
* Document approved modifications
* Track change success and rollback

**Configuration Items (CIs)**:

* System components requiring change control
* Critical files and configurations
* Service definitions and parameters
* Network settings and firewall rules
* Security policies and access controls

**Process Elements**:

```yaml
Change Management Flow:
- Change request submission
- Impact assessment
- Risk evaluation
- Approval workflow
- Scheduled implementation
- Post-change verification
- Documentation update
```

**Value**: Provides human oversight, coordination, and business context for technical changes.
{% endcolumn %}

{% column width="50%" %}
#### Technical Practices

**Automated Detection**:

* AIDE file integrity monitoring
* Ansible drift detection playbooks
* Hash verification scripts
* Configuration auditing tools
* Continuous compliance scanning

**Remediation Methods**:

* Ansible playbook enforcement
* Automated rollback procedures
* Self-healing configurations
* Immutable infrastructure patterns
* Version-controlled configurations

**Example Tools**:

```yaml
Technical Toolkit:
- AIDE (intrusion detection)
- Ansible (automation/remediation)
- Git (version control)
- rsync (replication/verification)
- Hash functions (integrity checks)
```

**Value**: Enables continuous, scalable monitoring and remediation without manual intervention.
{% endcolumn %}
{% endcolumns %}

{% hint style="danger" %}
**Critical Understanding**: Technical practices _enable_ operational procedures. Technology alone cannot make appropriate decisions about when and how to remediate drift. Human judgment and change management processes remain essential.
{% endhint %}

***

## AIDE: Advanced Intrusion Detection Environment

### What AIDE Does

AIDE monitors file system integrity by creating a baseline database of file attributes and detecting deviations from that baseline.

**Monitored Attributes**:

* File permissions (standard and extended ACLs)
* File ownership (user and group)
* File size and modification timestamps
* SELinux security contexts
* File content (through cryptographic hashes)
* Inode attributes and extended attributes

{% tabs %}
{% tab title="Installation & Setup" %}
**Installing AIDE**:

```bash
# Install AIDE package
dnf install aide

# Initialize AIDE database
aide --init

# Move initialized database to production location
mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz
```

**Initial Configuration**:

```bash
# AIDE configuration file
/etc/aide.conf

# Define what to monitor
/etc p+i+n+u+g+s+b+m+c+sha256
/bin p+i+n+u+g+s+b+m+c+sha256
/sbin p+i+n+u+g+s+b+m+c+sha256
/usr/bin p+i+n+u+g+s+b+m+c+sha256
```

**Verification Attributes**:

* `p` = permissions
* `i` = inode
* `n` = number of links
* `u` = user ownership
* `g` = group ownership
* `s` = size
* `b` = block count
* `m` = modification time
* `c` = creation/status change time
* `sha256` = SHA-256 hash
{% endtab %}

{% tab title="Running Checks" %}
**Performing Integrity Checks**:

```bash
# Check system against baseline
aide --check

# Example output showing changes
Summary:
  Total number of entries:      119,847
  Added entries:                42
  Removed entries:              8
  Changed entries:              157
```

**Interpreting Results**:

```bash
# Changed file example
changed: /etc/ssh/sshd_config

# What changed
Size: 4321 -> 4358
Mtime: 2024-01-15 -> 2024-01-16
SHA256: abc123... -> def456...
```

**After Authorized Changes**:

```bash
# Update AIDE database after approved changes
aide --update

# Replace production database
mv /var/lib/aide/aide.db.new.gz \
   /var/lib/aide/aide.db.gz
```
{% endtab %}

{% tab title="Automation" %}
**Scheduled AIDE Checks**:

```bash
# Daily AIDE check via cron
cat /etc/cron.daily/aide-check
#!/bin/bash
/usr/sbin/aide --check | \
mail -s "AIDE Daily Report" admin@example.com
```

**Integration with Monitoring**:

```yaml
# Ansible playbook for AIDE monitoring
- name: Run AIDE integrity check
  command: /usr/sbin/aide --check
  register: aide_check
  changed_when: false
  
- name: Alert on changes detected
  debug:
    msg: "AIDE detected {{ aide_check.stdout_lines | length }} changes"
  when: aide_check.rc != 0
```

**Enterprise Pattern**: AIDE checks run automatically, results aggregate centrally, changes trigger alerts and change management reviews.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Scale Consideration**: In the example shown, AIDE tracked approximately **119,000 file system objects**. This comprehensive monitoring enables detection of even subtle unauthorized changes but requires appropriate storage and processing capacity.
{% endhint %}

***

## Checking Drift vs. Fixing Drift

{% hint style="danger" %}
**Critical Distinction**: The difference between _observing_ drift and _correcting_ drift has profound operational implications.
{% endhint %}

### Two Fundamentally Different Operations

**Checking Drift (Passive)**

**What It Does**:

* Observes system state
* Compares to baseline
* Reports deviations
* No system modifications
* Non-disruptive monitoring

**Analogy**: Like watching NASCAR race cars. You observe which lane they're in, track if they drift from center line, record their positions—but you never change anything about the race itself.

**Tools and Modes**:

```yaml
Passive Monitoring:
- AIDE integrity checks
- Ansible check mode (--check)
- Configuration audits
- Compliance scanning
- Log analysis
```

**Use Cases**:

* Continuous monitoring
* Compliance reporting
* Trend analysis
* Change detection
* Audit preparation

**Risk Level**: **Very Low** - No system changes occur



\*\*Fixing Drift (Active)\*\*

**What It Does**:

* Modifies system state
* Enforces baseline configuration
* Overwrites existing settings
* Potentially disruptive
* Always introduces change

**Analogy**: Like dropping a concrete pylon in front of a race car that's drifted out of lane. You _force_ it back to center line—but this is catastrophic for the car and highly disruptive to the race.

**Tools and Modes**:

```yaml
Active Remediation:
- Ansible playbook execution
- Configuration enforcement
- SELinux enforcing mode
- Automated rollback
- State convergence
```

**Use Cases**:

* Security incident response
* Compliance remediation
* Scheduled maintenance
* Disaster recovery
* Systematic updates

**Risk Level**: **High** - Always introduces change, potentially breaking existing functionality

````
{% endcolumn %}
{% endcolumns %}

### The SELinux Parallel

{% hint style="info" %}
**SELinux provides a perfect example of check versus fix:**

**Permissive Mode** (Checking):
* Observes policy violations  
* Logs what *would* be blocked  
* No enforcement actions taken  
* Safe for testing and development

**Enforcing Mode** (Fixing):
* Blocks policy violations actively  
* Prevents unauthorized actions  
* Enforces security boundaries  
* Production-ready but potentially disruptive

This same pattern applies to configuration drift management.
{% endhint %}

***

## Command Line Philosophy and Automation

{% hint style="success" %}
**Fundamental Insight**: Every command line operation falls into one of two categories.
{% endhint %}

### The Two Command Types

{% tabs %}
{% tab title="Setting Values ⚙️" %}
**What It Means**:
* Modifying system state  
* Writing configuration files  
* Starting/stopping services  
* Creating/deleting resources  
* Changing permissions or ownership

**Examples**:
```bash
# Setting operations
systemctl enable httpd
firewall-cmd --add-service=http
useradd newuser
echo "config" > /etc/app.conf
chmod 644 /etc/hosts
````

**Automation Equivalent**:

* Ansible tasks that _change_ system state
* Playbooks that _enforce_ configuration
* Scripts that _modify_ files or services

**Characteristics**:

* Always creates change
* Requires appropriate permissions
* Should be logged and tracked
* May require change management approval \{% endtab %\}

\{% tab title="Checking Values 🔍" %\} **What It Means**:

* Reading system state
* Verifying configuration
* Listing resources
* Inspecting permissions or ownership
* Monitoring service status

**Examples**:

```bash
# Checking operations
systemctl status httpd
firewall-cmd --list-services
id username
cat /etc/app.conf
ls -l /etc/hosts
```

**Automation Equivalent**:

* Ansible tasks in check mode (--check)
* Playbooks that _verify_ configuration
* Scripts that _audit_ system state

**Characteristics**:

* Never modifies system
* Read-only operations
* Safe to run repeatedly
* Forms basis for monitoring \{% endtab %\} \{% endtabs %\}

### Automation Mirrors Command Line

```yaml
# Ansible example - Setting value
- name: Ensure Apache is running
  service:
    name: httpd
    state: started
    enabled: yes
  # This SETS the service state

# Ansible example - Checking value
- name: Verify Apache is running
  service:
    name: httpd
    state: started
  check_mode: yes
  # This CHECKS the service state
```

\{% hint style="warning" %\} **Professional Practice**: Always test automation in check mode first. Understand what _would_ change before allowing actual changes. This prevents unexpected disruption while building confidence in automation logic. \{% endhint %\}

***

## Ansible for Drift Remediation

### Understanding Drift Remediation with Ansible

Ansible provides powerful capabilities for both detecting and correcting configuration drift through idempotent playbook execution.

\{% tabs %\} \{% tab title="Detection Mode" %\} **Check Mode Execution**:

```yaml
# Run playbook without making changes
ansible-playbook deploy.yml --check

# Shows what WOULD change

# Reports drift from desired state

# Safe to run against production
```

**Example Output**:

```yaml
TASK [Ensure Apache config is correct]
changed: [webserver01]
  # Would update /etc/httpd/conf/httpd.conf
  # Current differs from template

TASK [Ensure firewall rules are set]
ok: [webserver01]
  # Already in desired state
```

**Benefits**:

* Non-disruptive assessment
* Identifies configuration drift
* Validates playbook logic
* Safe production scanning \{% endtab %\}

\{% tab title="Remediation Mode" %\} **Active Enforcement**:

```yaml
# Execute playbook with changes
ansible-playbook deploy.yml

# Converges system to desired state

# Overwrites drift configurations

# Enforces baseline
```

**Example Playbook**:

```yaml
---
- name: Enforce web server configuration
  hosts: webservers
  become: yes
  
  tasks:
    - name: Ensure Apache package present
      package:
        name: httpd
        state: present
    
    - name: Deploy Apache configuration
      template:
        src: httpd.conf.j2
        dest: /etc/httpd/conf/httpd.conf
      notify: restart apache
    
    - name: Ensure Apache is running
      service:
        name: httpd
        state: started
        enabled: yes
  
  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted
```

**Characteristics**:

* Enforces desired state
* Overwrites unauthorized changes
* Idempotent execution
* Self-documenting configuration \{% endtab %\}

\{% tab title="Continuous Enforcement" %\} **Scheduled Remediation**:

```bash
# Cron job for continuous enforcement
*/5 * * * * ansible-playbook /etc/ansible/enforce.yml

# Every 5 minutes:
# - Checks configuration state
# - Corrects any drift detected
# - Maintains baseline automatically
```

**Orchestration Platform**:

```yaml
# Ansible Automation Platform schedule
Schedule: Every 5 minutes
Playbook: /ansible/enforce_baseline.yml
Inventory: production_servers
Credentials: automation_user

# Provides:
# - Central logging
# - Change tracking
# - Drift reporting
# - Automated correction
```

**Enterprise Pattern**: Like a bulldozer that runs every few minutes, laying fresh configuration over any unauthorized changes, forcing the system back to baseline regardless of what developers or operators modified.

\{% hint style="danger" %\} **Operational Impact**: This approach is _extremely disruptive_ to unauthorized changes. Anyone modifying systems outside change management will have their changes automatically reversed within minutes. This is intentional but must be communicated clearly. \{% endhint %\} \{% endtab %\} \{% endtabs %\}

***

## Practical Drift Remediation Scenario

### Real-World Example: Environment Tampering

\{% hint style="info" %\} **Scenario Setup**: Development team gained sudo access on test/QA environments through leftover permissions. They've been making unauthorized modifications to web server configurations, creating system instability. \{% endhint %\}

\{% stepper %\} \{% step %\}

### The Situation

**Initial Deployment**:

```yaml
# Original Ansible playbook deployed three environments
- Dev environment: Port 8080
- Test environment: Port 8081  
- QA environment: Port 8082

# All services verified working
```

**Verification Commands**:

```bash
# Check all environments running
curl http://server:8080  # Dev - Working
curl http://server:8081  # Test - Working
curl http://server:8082  # QA - Working
```

**Everything functioning correctly initially.** \{% endstep %\}

\{% step %\}

### Unauthorized Changes Detected

**The Discovery**:

```bash
# Testing environments after a few days
curl http://server:8080  # Dev - Working ✅
curl http://server:8081  # Test - FAILED ❌
curl http://server:8082  # QA - Working ✅
```

**Investigation**:

```yaml
Problem: Test environment on 8081 no longer responds
Cause: Developer with sudo access deleted configuration
Impact: Night shift teams unable to access test environment
Status: System broken, outside change management
```

**Root Cause**: Previous engineer left developers in sudoers file. They've been "mucking around" in test/QA environments without authorization. \{% endstep %\}

\{% step %\}

### Automated Remediation

**Response**:

```bash
# Re-run original deployment playbook
ansible-playbook web_environments.yml

# Playbook execution:
# - Detects missing test environment config
# - Redeploys from template
# - Restarts services as needed
# - Converges to desired state
```

**Verification After Remediation**:

```bash
curl http://server:8080  # Dev - Working ✅
curl http://server:8081  # Test - Working ✅ (Fixed)
curl http://server:8082  # QA - Working ✅
```

**Result**: Environment automatically restored to baseline configuration. Unauthorized changes completely overwritten. \{% endstep %\}

\{% step %\}

### Continuous Enforcement Implementation

**Prevention Strategy**:

```

yaml
# Schedule periodic enforcement
Schedule: Every 5 minutes
Playbook: web_environments.yml
Action: Check and enforce baseline
```

**Impact**:

* Any unauthorized changes detected
* Configuration automatically restored
* Drift corrected within 5 minutes
* System always matches baseline

**The Bulldozer Effect**:

```bash
# If developers modify configs again:
1. Changes persist for up to 5 minutes
2. Next scheduled run detects drift
3. Playbook overwrites unauthorized changes
4. System forced back to baseline
5. Changes completely eliminated
```

**Communication to Teams**:

> "The automation will restore baseline configuration every 5 minutes. Any changes made outside proper change management will be automatically removed. Submit change requests through proper channels or your modifications will be continuously overwritten."

````

<div data-gb-custom-block data-tag="hint" data-style='warning'>

**Limitations**: This approach doesn't handle all possible breakage. Sophisticated tampering (changing file paths, breaking automation itself) could defeat remediation. However, such extensive damage requires significantly more effort and clearly crosses into termination-worthy violations.

</div>

</div>

</div>

***

## Immutable Infrastructure Concepts

<details>
<summary><strong>What Is Immutable Infrastructure?</strong></summary>

**Definition**: Infrastructure that is never modified after deployment. When changes are needed, components are replaced entirely rather than updated in place.

**Characteristics**:
* Servers deployed, never patched  
* Configuration baked into images  
* Changes deployed as new instances  
* Old instances destroyed, not updated  
* Version-controlled infrastructure code

**Benefits**:
* Eliminates configuration drift entirely  
* Consistent, predictable deployments  
* Easy rollback (revert to previous image)  
* Simplified testing and validation  
* Clear audit trail of all changes

**Example**: Container-based deployments where each update produces a new container image. Old containers are destroyed, new ones deployed. No drift possible because containers are immutable.

</details>

<details>
<summary><strong>Infrastructure as Code (IaC)</strong></summary>

**Definition**: Managing infrastructure through code files instead of manual configuration.

**Key Principles**:
* Configuration stored in version control (Git)  
* Automated deployment through code execution  
* Changes peer-reviewed before implementation  
* Complete infrastructure reproducible from code  
* Audit trail through version control history

**Example Technologies**:
```yaml
Ansible: Configuration management and orchestration
Terraform: Infrastructure provisioning
Kubernetes: Container orchestration
CloudFormation: AWS infrastructure management
````

**Benefit for Drift Management**: When infrastructure is code, drift becomes a version control problem. Unauthorized changes are immediately visible as differences from version-controlled source.

<details>

<summary><strong>Configuration Hashing and Verification</strong></summary>

**Purpose**: Cryptographically verify configuration files haven't changed.

**Implementation**:

```bash
# Generate hash of critical config file
sha256sum /etc/ssh/sshd_config > sshd_config.sha256

# Later, verify file hasn't changed
sha256sum -c sshd_config.sha256

# If modified, hash won't match:
/etc/ssh/sshd_config: FAILED
```

**Automation Use**:

```yaml
# Ansible task to verify configuration
- name: Verify SSH config integrity
  command: sha256sum -c /root/sshd_config.sha256
  register: hash_check
  failed_when: hash_check.rc != 0
```

**Enterprise Pattern**: Store configuration hashes in version control. Automated checks verify production files match approved hashes. Mismatches trigger alerts and investigation.

</details>

<details>

<summary><strong>Orchestration vs. Automation</strong></summary>

**Automation**: Running specific tasks automatically

```yaml
# Example: Automated backup
cron job: Run backup script nightly
Result: Backups happen automatically
```

**Orchestration**: Coordinating multiple automated tasks into workflows

```yaml
# Example: Orchestrated deployment
1. Pull latest code from Git
2. Run automated tests
3. Build container image
4. Push to registry
5. Deploy to staging
6. Run smoke tests
7. Deploy to production
8. Verify health checks
9. Update monitoring
10. Send notifications
```

**For Drift Management**: Orchestration coordinates drift detection, change management approval, remediation execution, and verification—creating comprehensive automated governance.

</details>

***

## Connection to Previous Sessions

{% hint style="success" %}
**Building on Course Foundation**: Session 8 integrates concepts from across the entire course into comprehensive configuration management.
{% endhint %}

<details>

<summary><strong>Integration with Session 7: Monitoring and Alerting</strong></summary>

**From Session 7, We Learned**:

* The three pillars of observability (metrics, traces, logs)
* How to decide what to monitor and when to alert
* Building monitoring infrastructure with Prometheus and Grafana
* Alert engineering fundamentals and avoiding alert fatigue

**Connection to Session 8**: Configuration drift is _what we monitor for_. Session 7 taught the monitoring infrastructure; Session 8 teaches what specific configuration states to monitor and how to respond when drift is detected.

**Combined Practice**:

```yaml
# Monitoring infrastructure detects drift
Prometheus: Tracks AIDE check status
Grafana: Visualizes drift over time
Alertmanager: Notifies on unauthorized changes

# Automation responds to drift
Ansible: Remediates detected drift
AIDE: Provides detailed change information
CMDB: Documents approved changes
```

This session unifies monitoring, logging, and automation into **configuration control**.

</details>

<details>

<summary><strong>Foundation from Sessions 1-6</strong></summary>

**Skills Built Throughout Course**:

* Session 1-3: System security fundamentals and network hardening
* Session 4: Identity management and access control
* Session 5: Package management and trusted software sources
* Session 6: Log parsing and security event correlation

**Application in Session 8**: All these foundational skills converge in configuration management:

* Security baselines define what to enforce
* Network configurations are critical drift targets
* User access controls must be monitored continuously
* Package sources represent critical configuration items
* Log analysis reveals unauthorized changes

Configuration drift management is the _operational discipline_ that maintains everything we've built in previous sessions.

</details>

***

## ✅ Prerequisites

{% hint style="warning" %}
**📋 Required Foundation**

To be successful in Session 8, you must know:

* **Command Line Proficiency**: Navigation, editing with `vi`, file operations, `grep` searches
* **Ansible Basics**: Playbook structure, YAML syntax, task execution, check mode
* **STIG Compliance**: Understanding STIG framework and using STIG Viewer
* **Service Management**: SystemD basics, service status verification
* **File Permissions**: Understanding standard and extended ACLs
* **SELinux Context**: Basic SELinux concepts and label management
* **Monitoring Fundamentals**: Concepts from Session 7 on observability and alerting

**Foundation Requirement**: You should understand:

* Basic networking and connectivity verification (Sessions 1-3)
* System logging and log file locations (Session 6)
* Change management principles and workflows
* Package management and software installation (Session 5)
* File integrity and hash verification concepts
* Identity and access management (Session 4)
{% endhint %}

***

Page cover recommendation: Wide hero-width image showing a system lifecycle diagram—from initial build book creation through operational run book maintenance to automated drift detection and remediation cycles, with clear indicators of authorized versus unauthorized change paths. This visualizes the complete configuration management lifecycle central to this session.

Page icon: 🔄 (circular arrows symbol for immediate recognition of configuration cycle management and drift correction)

Section icons strategic placement suggestions:

* 🔧 for Technical Skills and Tools sections
* 🎓 for Professional Skills and Processes sections
* ✅ for Validation/Verification content
* ⚠️ for Warning/Risk content
* 💡 for Key Insights/Concepts
* 🏢 for Enterprise Context and Real-World Scenarios
* 🔍 for Detection and Monitoring topics
* ⚙️ for Setting/Configuration operations
* 🛡️ for Security and Protection elements
