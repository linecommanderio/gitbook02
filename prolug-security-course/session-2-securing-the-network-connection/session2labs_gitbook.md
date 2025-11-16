# session2labs\_gitbook

## What We'll Build

Build production-grade security engineering capabilities through six progressive labs that mirror real-world DoD and enterprise compliance workflows.

{% hint style="success" %}
**Real-World Impact**

These aren't simulations—you'll use actual RHEL 9 STIGs, official DISA remediation Ansible, STIG Viewer 2.18, and production firewalld configurations. The analysis methodology you'll develop is exactly how security engineers evaluate compliance requirements in government and enterprise environments.
{% endhint %}

### Core Competencies You'll Develop

{% tabs %}
{% tab title="🔍 Security Assessment" %}
**Baseline Security Reconnaissance**

Learn to assess system security posture by examining:

* Kernel security parameters (forwarding, martian packets, panic handling)
* Cryptographic settings and FIPS mode status
* SELinux enforcement levels
* Current security mechanism configurations

**Tool Mastery:** STIG Viewer 2.18 (official DoD compliance tool) configuration and RHEL 9 STIG benchmark import
{% endtab %}

{% tab title="🔧 Manual Remediation" %}
**Hands-On STIG Implementation**

Analyze and fix actual network security STIGs:

* TCP SYN cookie protection (V-257957)
* Martian packet handling (V-257958)
* Network security controls (V-257960, V-257961)
* Firewall configuration hardening

**Methodology:** Learn to categorize control types (preventative, detective, corrective) and understand what each STIG accomplishes
{% endtab %}

{% tab title="🤖 Automation at Scale" %}
**Enterprise Compliance Automation**

Generate and evaluate automated remediation using:

* Official DISA Ansible playbooks (985 configuration defaults, 2,991 remediation tasks)
* OpenSCAP tools for both Ansible and Bash script generation

**Critical Skill:** Learn when to apply automation and when manual judgment is required—this is what makes security engineers valuable
{% endtab %}

{% tab title="🚪 Practical Integration" %}
**Real-World Service Exposure**

Solve production scenarios like:

* Exposing Prometheus node\_exporter through firewalls
* Maintaining security while enabling monitoring
* Understanding firewall service definitions
* Implementing defense-in-depth at network boundaries

**Business Context:** Balance security requirements with operational needs
{% endtab %}
{% endtabs %}

### Career-Ready Skills

By completing these labs, you'll be able to add to your resume:

* ✅ STIG remediation experience with DoD security compliance
* ✅ OpenSCAP automation implementation
* ✅ Enterprise firewall configuration and management
* ✅ Security control categorization and analysis
* ✅ Production compliance workflow experience

***

## Lab Progression

This sequence mirrors the actual enterprise security engineering workflow—from assessment through manual hardening to scaled automation.

{% stepper %}
{% step %}
###


{% endstep %}

{% step %}
###


{% endstep %}
{% endstepper %}

### Phase 1: Foundation (Labs 1-2)

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>🔍 Security Reconnaissance</strong></td><td>Baseline your system's security posture by examining kernel parameters, cryptographic settings, and security mechanisms. Understand your starting point before hardening.</td><td></td></tr><tr><td><strong>📋 STIG Viewer Setup</strong></td><td>Configure the official DoD compliance tool (STIG Viewer 2.18) and import RHEL 9 STIG benchmarks. Learn the industry-standard assessment workflow.</td><td></td></tr></tbody></table>

### Phase 2: Manual Hardening (Labs 3-5)

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>🌐 Network STIG Remediation</strong></td><td>Analyze and fix actual network security STIGs (TCP SYN cookies, martian packets). Learn systematic STIG analysis: What's the problem? What's the fix? What control type?</td><td></td></tr><tr><td><strong>🔥 Firewall STIG Analysis</strong></td><td>Understand firewall security controls and categorize them (preventative, detective, corrective). Learn how these controls create defense-in-depth.</td><td></td></tr><tr><td><strong>🚪 Port Exposure Lab</strong></td><td>Real-world scenario: Expose Prometheus node_exporter through firewalld while maintaining security. Balance operational needs with security requirements.</td><td></td></tr></tbody></table>

### Phase 3: Automation at Scale (Lab 6)

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>🤖 Automated Remediation</strong></td><td>Generate automated scripts using DISA Ansible (985 defaults, 2,991 tasks) and OpenSCAP tools. Learn when to apply automation and when human judgment is required—your value as a security engineer.</td><td></td></tr></tbody></table>

{% hint style="warning" %}
**Professional Development Note**

Each phase builds on the previous one:

1. **Assessment** gives you baseline understanding
2. **Manual hardening** develops deep security engineering intuition
3. **Automation** scales your capabilities to enterprise environments

The progression from manual to automated mirrors how you'll actually work in production—understanding the "why" before applying the "how at scale."
{% endhint %}

***

## Lab Exercises

### 🛠️ Labs to Build Here

{% stepper %}
{% step %}
**🔥 Pre-Lab Warm-Up: System Security Reconnaissance**

Quick system familiarization and security posture assessment.

Run through these commands to understand your system's current security configuration:

{% code title="security-recon.sh" overflow="wrap" lineNumbers="true" %}
```bash

# Check IPv4 forwarding configuration
sysctl -a | grep -i ipv4 | grep -i forward

# Question: Does this system appear to be set to forward? Why or why not?

# Check martian packet handling
sysctl -a | grep -i ipv4 | grep -i martian

# Question: What are martians and is this system allowing them?

# Check kernel panic behavior
sysctl -a | grep -i panic

# Question: How does this system handle panics?

# Check cryptographic settings
sysctl -a | grep -i crypto

# Question: What settings do you see? Is FIPS enabled?

# Verify FIPS and SELinux status
cat /proc/cmdline
fips-mode-setup --check
sestatus
cat /etc/selinux/config
```
{% endcode %}

<details>

<summary>🎯 What You're Looking For</summary>

Security Posture Indicators:

* SELinux status: Is it enforcing, permissive, or disabled?
* FIPS mode: Is Federal Information Processing Standard cryptography enabled?
* Forwarding settings: Is this system configured as a router or gateway?
* Martian packets: Are impossible source addresses being logged and dropped?
* Panic handling: How does the kernel respond to critical failures?

These baseline checks reveal your system's current security configuration before hardening.

</details>

{% hint style="success" %}
✅ **Outcome:** Comprehensive baseline understanding of system security configuration
{% endhint %}
{% endstep %}

{% step %}
**📋 STIG Viewer Setup and RHEL 9 STIG Import**

Configure your security assessment toolkit.

{% stepper %}
{% step %}
**Download STIG Viewer**

Download STIG Viewer 2.18 from [https://public.cyber.mil/stigs/downloads/](https://public.cyber.mil/stigs/downloads/)
{% endstep %}

{% step %}
**Import RHEL 9 STIG**

Download the RHEL 9 STIG and import it into STIG Viewer.
{% endstep %}

{% step %}
**Create Checklist**

Create a checklist from the opened RHEL 9 STIG for systematic assessment.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Tool Context:** STIG Viewer is the industry-standard tool for DoD and high-security enterprise environments. This is the same tool security engineers use in federal government and defense contractor positions.
{% endhint %}

{% hint style="success" %}
✅ **Outcome:** Operational STIG Viewer environment ready for network security assessment
{% endhint %}
{% endstep %}

{% step %}
**🌐 Network Service STIG Remediation**

Hands-on network configuration hardening.

Connect to your lab server and begin systematic STIG assessment:

{% code overflow="wrap" %}
```bash

# Filter STIGs by IPv4 to see network-related controls

# You should see multiple STIGs related to network hardening
```
{% endcode %}

**Examine and Remediate V-257957: TCP SYN Cookies**

{% tabs %}
{% tab title="🔍 Analysis" %}
What is the problem?

* System may be vulnerable to TCP SYN flood attacks

What is the fix?

* Enable TCP SYN cookies to protect against SYN floods

What type of control?

* Technical Preventative Control – system setting to prevent resource exhaustion attacks

Current Status Check:

{% code overflow="wrap" %}
```bash
sysctl -a | grep -i ipv4 | grep -i syncookies
```
{% endcode %}
{% endtab %}

{% tab title="🔧 Remediation" %}
If the value is not correctly set, create or update a sysctl conf:

{% code title="/etc/sysctl.d/00-remediate.conf" %}
```bash

# Enable TCP SYN cookie protection
net.ipv4.tcp_syncookies = 1
```
{% endcode %}

Apply the configuration:

{% code overflow="wrap" %}
```bash

# Reload sysctl configuration
sysctl --system

# Verify the change
sysctl net.ipv4.tcp_syncookies
```
{% endcode %}
{% endtab %}
{% endtabs %}

**Examine and Remediate V-257958**

{% hint style="warning" %}
**Assignment** – Follow the same analysis pattern:

{% stepper %}
{% step %}
What is the problem?
{% endstep %}

{% step %}
What is the fix?
{% endstep %}

{% step %}
What type of control is being implemented?
{% endstep %}

{% step %}
Is it set properly on your system?
{% endstep %}

{% step %}
How would you remediate this?
{% endstep %}
{% endstepper %}
{% endhint %}

**Examine V-257960 and V-257961 (Related STIGs)**

Key Question: How are these two STIGs related? What do they accomplish together?

{% hint style="success" %}
✅ **Outcome:** Systematic STIG analysis methodology with hands-on remediation experience
{% endhint %}
{% endstep %}

{% step %}
**🔥 Firewall STIG Analysis**

Understanding firewall security controls.

{% code overflow="wrap" %}
```bash

# Filter STIG Viewer by "firewall" keyword

# Observe the number of firewall-related STIGs
```
{% endcode %}

Analysis Questions:

* How many firewall STIGs do you see?
* What are these STIGs trying to accomplish?
* What types of controls are they? (Preventative, detective, corrective?)

<details>

<summary>🎯 Firewall Control Categories</summary>

Firewall STIGs typically implement:

* Technical Preventative Controls: Block unauthorized network access
* Technical Detective Controls: Log connection attempts for analysis
* Configuration Standards: Ensure consistent security posture across systems

These controls work together to create defense-in-depth at the network boundary.

</details>

{% hint style="success" %}
✅ **Outcome:** Understanding of firewall STIG requirements and control categorization
{% endhint %}
{% endstep %}

{% step %}
**🚪 Firewall Port Exposure Lab**

Real-world scenario: Exposing services securely.

{% hint style="info" %}
**Scenario:** Your team needs to use `node_exporter` with Prometheus to scrape system metrics for network monitoring. You're running a firewall, so you must expose the port that `node_exporter` uses to the network outside your system.
{% endhint %}

{% code title="firewall-port-exposure.sh" overflow="wrap" lineNumbers="true" %}
```bash

# Verify firewall is running
systemctl status firewalld

# Check if node_exporter service is defined
firewall-cmd --get-services | grep -i node
ls /usr/lib/firewalld/services | grep -i node

# Verify service is NOT currently enabled
firewall-cmd --list-services

# Examine the firewall service definition structure
cat /usr/lib/firewalld/services/prometheus-node-exporter.xml

# Enable the service through your firewall permanently
firewall-cmd --permanent --add-service=prometheus-node-exporter

# Reload firewall to apply changes
firewall-cmd --reload

# Verify the service is now enabled
firewall-cmd --list-services
```
{% endcode %}

<details>

<summary>📋 Understanding Firewall Service Definitions</summary>

When you examine `/usr/lib/firewalld/services/prometheus-node-exporter.xml`, you'll see:

* Port number: Which port the service uses
* Protocol: TCP or UDP
* Description: What the service does

This XML structure allows firewalld to manage services by name rather than memorizing port numbers.

</details>

{% hint style="success" %}
✅ **Outcome:** Practical experience exposing services through enterprise firewalls while maintaining security
{% endhint %}
{% endstep %}

{% step %}
**🤖 Automated STIG Remediation Tools**

Generate automated remediation scripts.

{% hint style="warning" %}
**Critical Understanding:** Security engineers don't blindly apply all STIG remediations. We must understand trade-offs between security and productivity. Some controls may break applications or cause operational issues.

**Your value:** Knowing which controls to apply and which to adapt based on business requirements.
{% endhint %}

**Download Official DISA STIG Remediation Ansible**

{% code title="download-stig-ansible.sh" overflow="wrap" lineNumbers="true" %}
```bash
cd /root
mkdir stigs
cd stigs

# Download official RHEL 9 STIG Ansible remediation
wget -O U_RHEL_9_V2R4_STIG_Ansible.zip \
  https://dl.dod.cyber.mil/wp-content/uploads/stigs/zip/U_RHEL_9_V2R4_STIG_Ansible.zip

# Extract the archive
unzip U_RHEL_9_V2R4_STIG_Ansible.zip

# Create working directory and extract role
mkdir ansible
cp rhel9STIG-ansible.zip ansible/
cd ansible
unzip rhel9STIG-ansible.zip
```
{% endcode %}

{% hint style="info" %}
**Note:** If lab downloads don't work, check `/labs` folder on your server for `[course]_[unit#].zip` backup files.
{% endhint %}

**Examine Default STIG Values**

{% code title="examine-defaults.sh" overflow="wrap" %}
```bash

# Navigate to defaults directory
cd /root/stigs/ansible/roles/rhel9STIG/defaults/

# Open default values file
vim main.yml

# Search for specific STIGs (use /257957 in vim to search)

# Example: /257957 to find that STIG's default configuration
```
{% endcode %}

What You'll See:

* Default values for each STIG rule
* Variables you can customize for your environment
* Boolean flags to enable/disable specific remediations

**Examine Ansible Playbook Implementation**

{% code title="examine-playbook.sh" overflow="wrap" %}
```bash

# View how STIGs are implemented in Ansible
vim /root/stigs/ansible/roles/rhel9STIG/tasks/main.yml

# Search for the same STIG (use /257957 in vim)

# Observe how the default values are applied to the system
```
{% endcode %}

<details>

<summary>🔍 Understanding Ansible Remediation Structure</summary>

The playbook shows:

* 985 configuration lines of defaults
* 2,991 Ansible playbook tasks for remediation
* Each task implements a specific STIG control

Key Insight: Modify `defaults/main.yml` to customize which STIGs apply and with what values, then run the playbook against your systems for automated compliance.

</details>

**Generate OpenSCAP Remediation Scripts**

{% code title="openscap-remediation.sh" overflow="wrap" lineNumbers="true" %}
```bash

# Install OpenSCAP tools
dnf -y install openscap-scanner openscap-utils scap-security-guide

# Create working directory
cd /root
mkdir openscap
cd openscap

# Generate Ansible remediation playbook
oscap xccdf generate fix \
  --profile ospp \
  --fix-type ansible \
  /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml \
  > draft-disa-remediate.yml

# Examine the generated playbook
vim draft-disa-remediate.yml

# Generate Bash script version
oscap xccdf generate fix \
  --profile ospp \
  --fix-type bash \
  /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml \
  > draft-disa-remediate.sh

# Examine the bash script
vim draft-disa-remediate.sh
```
{% endcode %}

{% tabs %}
{% tab title="📜 Ansible Version" %}
What You'll See:

* YAML playbook format with tasks for each remediation
* Variables at the top for customization
* Tags for selective remediation
* Ansible modules like `lineinfile`, `package`, `service`

**Use Case:** Automated deployment across multiple systems using Ansible infrastructure
{% endtab %}

{% tab title="💻 Bash Version" %}
What You'll See:

* Pure bash script with if/then/else logic
* Nested conditionals for complex remediations
* Direct system commands and file modifications

**Learning Opportunity:** If you want to improve your bash scripting, this is excellent reference code showing production-quality conditional logic and error handling.

**Use Case:** Single-system remediation without Ansible dependencies
{% endtab %}
{% endtabs %}

<details>

<summary>⚠️ Critical Warning: Don't Blindly Apply</summary>

Both methods generate comprehensive remediation scripts, but:

* Not all fixes apply to all environments
* Some remediations may break applications
* GRUB configurations won't work on certain boot systems
* Network settings may cause connectivity loss
* Service restrictions may impact functionality

Professional Approach:

1. Review generated scripts thoroughly
2. Test in non-production environment first
3. Apply selectively based on your threat model
4. Document any STIGs you cannot remediate and why
5. Create compensating controls for unimplemented STIGs

This is your value as a security engineer: understanding which controls to implement and which to adapt.

</details>

{% hint style="success" %}
✅ **Outcome:**

* Ability to generate automated STIG remediation scripts
* Understanding of Ansible and Bash remediation approaches
* Professional judgment for selective implementation
* Reference code for advanced bash scripting patterns
{% endhint %}
{% endstep %}
{% endstepper %}

***

## Key Takeaways

{% hint style="warning" %}
🎓 **Professional Development**

These labs mirror actual enterprise security engineering workflows—from baseline assessment through manual STIG remediation to automated compliance tooling. You'll analyze security controls, implement fixes manually to understand them deeply, then scale with automation using the same tools required in government and corporate environments.

**Career Impact:** Add "STIG remediation experience," "DoD security compliance implementation," and "OpenSCAP automation" to your resume.
{% endhint %}

### What Sets These Labs Apart

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><strong>🎯 Authentic Tools</strong></td><td>STIG Viewer 2.18, official DISA Ansible, OpenSCAP—the exact tools used in production DoD and enterprise environments</td></tr><tr><td><strong>🔍 Analysis Methodology</strong></td><td>Learn to ask: What's the problem? What's the fix? What control type? This is how security engineers think</td></tr><tr><td><strong>⚖️ Professional Judgment</strong></td><td>Understand when to automate and when human analysis is required—your value as a security engineer</td></tr><tr><td><strong>📈 Career Progression</strong></td><td>From manual security hardening to automated compliance at scale—the exact progression of real security engineering careers</td></tr></tbody></table>

**Page Cover Recommendation**: Hero-width cover showing a security operations center dashboard with STIG compliance metrics, security monitoring displays, and network diagrams for immediate visual context of enterprise security engineering environment.

**Page Icon Recommendation**: 🛡️ (shield representing security and protection—immediate recognition of security engineering content)
