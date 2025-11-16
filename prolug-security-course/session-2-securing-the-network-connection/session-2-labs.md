---
cover: ../../.gitbook/assets/lab-ezgif.png
coverY: -219.5096188830271
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

# 🛠️ Session 2 Labs

I'm building six progressively complex labs that take me from baseline security assessment through manual STIG remediation to automated compliance tooling. This isn't theoretical—I'm working with actual RHEL 9 STIGs, official DoD tools, and production firewall configurations used in government and enterprise environments.

The progression mirrors real-world security engineering:

<details>

<summary>🔍 Reconnaissance &#x26; Setup</summary>

> Baseline the system's current security posture by examining kernel parameters, cryptographic settings, and security mechanisms. Then configure STIG Viewer 2.18 (the official DoD compliance tool) with RHEL 9 STIG benchmarks—the same setup government and defense contractors use.

</details>

<details>

<summary>🔧 Manual Remediation</summary>

> Analyze and fix actual network security STIGs (TCP SYN cookies, martian packet handling, firewall controls), learning to categorize control types and understand what each STIG accomplishes. Solve real scenarios like exposing services through firewalls while maintaining security.

</details>

<details>

<summary>🤖 Automation at Scale</summary>

> Generate automated remediation scripts using both official DISA Ansible playbooks and OpenSCAP tools. Learn when to apply automation and when manual judgment is required—the critical skill that makes security engineers valuable.

</details>

{% hint style="info" %}
**What Makes This Real**

These aren't simulations—I'm using actual RHEL 9 STIGs, official DoD remediation Ansible, STIG Viewer 2.18, and production firewalld configurations. The analysis methodology (_**What's the problem? What's the fix? What control type?**_) is exactly how security engineers evaluate compliance requirements in government and enterprise environments.
{% endhint %}

By the end, we will understand both the manual security engineering workflow (critical for learning) and the automated compliance tooling (critical for scaling). More importantly, we'll learn **when to apply automation and when human judgment is required**—that's what makes a security engineer valuable.

***

## 🛠️ Labs to Build Here

{% stepper %}
{% step %}
### 🔥 Pre-Lab Warm-Up: System Security Reconnaissance

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
✅ Outcome: Comprehensive baseline understanding of system security configuration
{% endhint %}
{% endstep %}

{% step %}
### 🔧 STIG Viewer Setup and RHEL 9 STIG Import

Configure your security assessment toolkit.

{% stepper %}
{% step %}
**Download STIG Viewer**

Download STIG Viewer 2.18 from https://public.cyber.mil/stigs/downloads/
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
Tool Context: STIG Viewer is the industry-standard tool for DoD and high-security enterprise environments. This is the same tool security engineers use in federal government and defense contractor positions.
{% endhint %}

{% hint style="success" %}
✅ Outcome: Operational STIG Viewer environment ready for network security assessment
{% endhint %}
{% endstep %}

{% step %}
### 🌐 Network Service STIG Remediation

Hands-on network configuration hardening.

Connect to your lab server and begin systematic STIG assessment:

{% code overflow="wrap" %}
```bash
# Filter STIGs by IPv4 to see network-related controls
# You should see multiple STIGs related to network hardening
```
{% endcode %}

#### Examine and Remediate V-257957: TCP SYN Cookies

{% tabs %}
{% tab title="🔍 Analysis" %}
What is the problem?

* System may be vulnerable to TCP SYN flood attacks

What is the fix?

* Enable TCP SYN cookies to protect against SYN floods

What type of control?

* Technical Preventative Control — system setting to prevent resource exhaustion attacks

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

#### Examine and Remediate V-257958

{% hint style="warning" %}
Assignment — Follow the same analysis pattern:

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

#### Examine V-257960 and V-257961 (Related STIGs)

Key Question: How are these two STIGs related? What do they accomplish together?

{% hint style="success" %}
✅ Outcome: Systematic STIG analysis methodology with hands-on remediation experience
{% endhint %}
{% endstep %}

{% step %}
### 🔥 Firewall STIG Analysis

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
✅ Outcome: Understanding of firewall STIG requirements and control categorization
{% endhint %}
{% endstep %}

{% step %}
### 🚪 Firewall Port Exposure Lab

Real-world scenario: Exposing services securely.

{% hint style="info" %}
Scenario: Your team needs to use `node_exporter` with Prometheus to scrape system metrics for network monitoring. You're running a firewall, so you must expose the port that `node_exporter` uses to the network outside your system.
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
✅ Outcome: Practical experience exposing services through enterprise firewalls while maintaining security
{% endhint %}
{% endstep %}

{% step %}
### 🤖 Automated STIG Remediation Tools

Generate automated remediation scripts.

{% hint style="warning" %}
Critical Understanding: Security engineers don't blindly apply all STIG remediations. We must understand trade-offs between security and productivity. Some controls may break applications or cause operational issues.

Your value: Knowing which controls to apply and which to adapt based on business requirements.
{% endhint %}

#### Download Official DISA STIG Remediation Ansible

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
Note: If lab downloads don't work, check `/labs` folder on your server for `[course]_[unit#].zip` backup files.
{% endhint %}

#### Examine Default STIG Values

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

#### Examine Ansible Playbook Implementation

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

#### Generate OpenSCAP Remediation Scripts

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
{% tab title="🐍 Ansible Version" %}
What You'll See:

* YAML playbook format with tasks for each remediation
* Variables at the top for customization
* Tags for selective remediation
* Ansible modules like `lineinfile`, `package`, `service`

Use Case: Automated deployment across multiple systems using Ansible infrastructure
{% endtab %}

{% tab title="💻 Bash Version" %}
What You'll See:

* Pure bash script with if/then/else logic
* Nested conditionals for complex remediations
* Direct system commands and file modifications

Learning Opportunity: If you want to improve your bash scripting, this is excellent reference code showing production-quality conditional logic and error handling.

Use Case: Single-system remediation without Ansible dependencies
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
✅ Outcome:

* Ability to generate automated STIG remediation scripts
* Understanding of Ansible and Bash remediation approaches
* Professional judgment for selective implementation
* Reference code for advanced bash scripting patterns
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
🎓 Professional Development

These labs mirror actual enterprise security engineering workflows—from baseline assessment through manual STIG remediation to automated compliance tooling. You'll analyze security controls, implement fixes manually to understand them deeply, then scale with automation using the same tools required in government and corporate environments.

**Career Impact:** _Add "STIG remediation experience," "DoD security compliance implementation," and "OpenSCAP automation" to your resume._
{% endhint %}
