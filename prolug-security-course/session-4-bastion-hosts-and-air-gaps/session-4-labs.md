---
cover: ../../.gitbook/assets/lab-ezgif.png
coverY: -221.21941513552844
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

# 🛠️ Session 4 Labs

I'm building three progressively complex labs that take me from basic user jailing through complete bastion host implementation.

<details>

<summary>🏗️ What We'll Build</summary>

A **controlled access infrastructure** for air-gapped systems:

* [ ] Build a chroot jail that restricts a process to a limited filesystem environment.
* [ ] Deploy a complete bastion host that automatically jails incoming users, restricts available actions to a predefined menu, and provides controlled access to isolated networks.
* [ ] Test bastion effectiveness by attempting to escape restrictions, validate timeout mechanisms, and verify that users have _only_ the capabilities you've explicitly granted.

</details>

### ⏳ Lab Progression

{% stepper %}
{% step %}
#### 🔒 User Isolation → A restricted filesystem environment that isolates processes
{% endstep %}

{% step %}
#### 🏰 Bastion Implementation → The _single controlled entry point_ into an air-gapped network
{% endstep %}

{% step %}
#### 🎯 Enterprise Validation → Professional open-source contribution workflow
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**What Makes This Real**

The techniques taught here are used to secure classified systems, financial infrastructure, and critical operational technology.

> _We're building actual chroot jails with real filesystem isolation, implementing bastion hosts with automatic user jailing on SSH connection, and testing security controls that protect production air-gapped networks._&#x20;
{% endhint %}



By the end, we'll understand:&#x20;

* The **technical mechanisms** (`chroot`, SSH configuration, shell scripting)
* The **architectural patterns** (bastion hosts, controlled entry points, defense-in-depth).

More importantly, we'll know how to design and implement controlled access to isolated systems.

***

## 📋 Required Materials

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

* ProLUG Lab Server access
* Root or sudo privileges


{% endtab %}

{% tab title="📚 Prerequisites" %}
<table data-full-width="true"><thead><tr><th width="375.2265625" valign="top">Required Knowledge</th><th valign="top">Required Skills from Previous Sessions</th></tr></thead><tbody><tr><td valign="top"><ul><li>Directory navigation and file manipulation</li><li>SSH configuration (<code>/etc/ssh/sshd_config</code>)</li><li>User and group management</li><li>SystemD service control</li><li>Basic bash scripting (loops, conditionals, case statements)</li></ul></td><td valign="top"><ul><li>STIG Viewer operation and checklist management</li><li>Configuration file editing (<code>vim</code>/<code>nano</code>)</li><li>Service restart and validation procedures</li></ul></td></tr></tbody></table>


{% endtab %}
{% endtabs %}

***

## 🛠️ Labs to Build Here

{% stepper %}
{% step %}
#### 🔒 Lab 1: Building a chroot Jail

Learn process isolation by building a restricted filesystem environment from scratch.

{% hint style="info" %}
**What You're Building**

A chroot jail changes a process's view of the filesystem root. Instead of `/` being the actual root, the process sees `/var/chroot` as root. This restricts what files, binaries, and resources the process can access—fundamental to containerization and bastion security.

**Critical Insight**: If you don't provide necessary resources (binaries, libraries, device nodes), the jailed process will fail. You're not just restricting—you're building a minimal, functional environment.
{% endhint %}

**📖 Lab Instructions**

**Primary Lab**: [Building a chroot Jail on KillerCoda](https://killercoda.com/het-tanis/course/Linux-Labs/204-building-a-chroot-jail)

{% hint style="success" %}
**Interactive Environment**

The KillerCoda lab provides a pre-configured Ubuntu environment where you can practice building chroot jails without affecting your local system. All commands run in an isolated, disposable environment.
{% endhint %}

**🎯 Learning Objectives**

As you work through the lab, answer these critical questions:

<details>

<summary><strong>High-Level Architecture</strong></summary>

**Question**: If you were to write out the high-level steps of building a chroot jail, what would they be?

**Think About**:

* What directory structure must exist?
* What files and binaries are absolutely required?
* What configuration files enable network/system interaction?
* What device nodes are necessary for process operation?

**Professional Skill**: Being able to articulate the systematic process of building security controls demonstrates architectural understanding beyond rote command execution.

</details>

<details>

<summary><strong>Resource Requirements Analysis</strong></summary>

**Question**: Think about what you did in the lab and what extra (or less) you might give a user/process.

**Specific Analysis Questions**:

a) Directory Structure:

* What directories are needed?
* Why `/bin`, `/lib`, `/lib64`, `/usr/bin`?
* What happens if you omit `/etc`?

b) Executable Selection:

* What executables might you give the jailed user/process?
* Why did the lab include `bash`, `ssh`, and `curl`?
* What would you add for a web application? For a database client?

c) Link Library Dependencies:

* If you give an executable, why is it important to give the link libraries that it uses?
* How do you discover what libraries an executable requires?
* What happens when libraries are missing?

d) Special Device Files:

* What are the special files that you made with `mknod` and why must they be there?
* What is `/dev/null` used for?
* Why do cryptographic operations need `/dev/random` and `/dev/urandom`?
* What happens to SSH without these device nodes?

**Experimental Validation**: Try removing device nodes or redoing the lab without them. Document how it breaks. This hands-on failure analysis builds troubleshooting instincts.

</details>

**🔬 Key Technical Concepts**

{% tabs %}
{% tab title="🗂️ Filesystem Isolation" %}
**What chroot Does**:

* Changes apparent filesystem root for a process
* Process sees `/var/chroot` as `/`
* Cannot access files outside the jail (without privilege escalation)

**Directory Structure**:

```
/var/chroot/
├── bin/          # Essential executables
├── lib/          # Shared libraries (32-bit)
├── lib64/        # Shared libraries (64-bit)
├── usr/
│   └── bin/      # User executables
├── etc/          # Configuration files
│   ├── passwd    # User database
│   ├── group     # Group database
│   ├── hosts     # Name resolution
│   └── nsswitch.conf  # System databases
└── dev/          # Device nodes
    ├── null      # Bit bucket
    ├── zero      # Null source
    ├── random    # Random number generator
    └── urandom   # Non-blocking random
```
{% endtab %}

{% tab title="🔗 Link Libraries" %}
**Why Libraries Matter**:

Every executable depends on shared libraries (.so files). Without them, the binary cannot run.

**Discovery Command**:

```bash
ldd /usr/bin/ssh
```

**Example Output**:

```
linux-vdso.so.1 (0x00007fff...)
libssh.so.4 => /usr/lib/x86_64-linux-gnu/libssh.so.4
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6
```

**Critical Requirement**: Every library listed must be copied into the jail at the correct path, or the executable fails.

**Automation Pattern**:

```bash
# Copy all libraries for a binary
for lib in $(ldd /usr/bin/ssh | awk '{print $3}' | grep ^/); do
  cp --parents $lib /var/chroot/
done
```
{% endtab %}

{% tab title="⚙️ Device Nodes" %}
**Essential Device Files**:

| Device         | Purpose                              | Impact if Missing                              |
| -------------- | ------------------------------------ | ---------------------------------------------- |
| `/dev/null`    | Bit bucket (discard output)          | Programs cannot discard unwanted output        |
| `/dev/zero`    | Source of null bytes                 | Disk operations and memory initialization fail |
| `/dev/random`  | Blocking random data (entropy-based) | Cryptographic operations block or fail         |
| `/dev/urandom` | Non-blocking random data             | SSH key generation fails                       |
| `/dev/tty`     | Controlling terminal                 | Interactive sessions fail                      |

**Creation Command**:

```bash
mknod /var/chroot/dev/null c 1 3
mknod /var/chroot/dev/zero c 1 5
mknod /var/chroot/dev/random c 1 8
mknod /var/chroot/dev/urandom c 1 9
```

**Real-World Impact**: SSH will not work without `/dev/random` and `/dev/urandom` because it cannot generate cryptographic keys or session entropy.
{% endtab %}

{% tab title="🌐 Network Configuration" %}
**Required Configuration Files**:

* `/etc/nsswitch.conf`: Tells the system how to resolve names (users, groups, hosts).
* `/etc/hosts`: Static hostname-to-IP mappings.
* `/etc/passwd` and `/etc/group`: User and group databases (even for jailed processes).

**Why They Matter**: Without these files, jailed processes cannot:

* Resolve hostnames to IP addresses
* Determine user/group ownership
* Perform network operations correctly
{% endtab %}
{% endtabs %}

{% hint style="success" %}
✅ **Outcome**:

* Functional chroot jail environment
* Understanding of minimal process requirements
* Ability to troubleshoot jail failures by analyzing missing resources
* Foundation for bastion host implementation
{% endhint %}
{% endstep %}

{% step %}
#### 🏰 Lab 2: Building a Bastion Host

Implement a complete bastion system that automatically jails users and restricts available actions.

{% hint style="info" %}
**What You're Building**

A bastion host is the _single controlled entry point_ into an isolated network. When users SSH in, they're automatically jailed and presented with a restricted menu—they cannot execute arbitrary commands. This is how classified systems, financial infrastructure, and critical operational technology enforce controlled access.

**Real-World Context**: The ProLUG lab environment you've been using _is_ a bastion system. When you connect, you're automatically jailed and given limited options. This lab teaches you how to build what you've been using.
{% endhint %}

**📖 Lab Instructions**

**Primary Lab**: [Building a Bastion Host on KillerCoda](https://killercoda.com/het-tanis/course/Linux-Labs/210-building-a-bastion-host)

{% hint style="success" %}
**Architecture Overview**

The lab simulates this architecture:

```
[Outside System]  →  SSH  →  [Bastion (node01)]  →  SSH  →  [Air-Gapped Network]
  controlplane            Jailed User          free_user      controlplane
                          (restricted)                        (unjailed)
```

You're forcing all access through one controlled point where users are automatically restricted.
{% endhint %}

**🎯 Learning Objectives**

As you work through the lab, analyze these architectural decisions:

<details>

<summary><strong>Bastion Architecture</strong></summary>

**Question**: If you were to write out the high-level steps of building a bastion host, what would they be?

**Think About**:

* How do you automatically jail users on SSH connection?
* How do you restrict their shell to a custom script?
* How do you prevent them from executing arbitrary commands?
* How do you provide controlled access to specific destinations?

**Professional Skill**: Understanding the systematic process of implementing defense-in-depth access controls.

</details>

<details>

<summary><strong>Restriction Validation</strong></summary>

**Question**: When you jump into the bastion host, do you have any options other than the one you have given yourself?

**Analysis Points**:

* Can you execute shell commands directly?
* Can you background processes?
* Can you escape to a normal shell?
* What happens when you try unexpected input?

**Security Principle**: Effective security controls must be tested by attempting to bypass them. If you don't try to break your own controls, attackers will.

</details>

<details>

<summary><strong>Jail Escape Testing</strong></summary>

**Question**: How did you test that you couldn't leave the jailed environment?

**Test Cases**:

* Timeout Test: Wait 20 seconds without input
* Invalid Input Test: Enter characters other than menu options
* Exit Option Test: Select the exit option
* Valid Connection Test: Select the connection option and reach destination

**Critical Analysis**: How effective do you think this is as a technical preventative control against user breakout in the jail, having a 20 second timeout?

**Trade-off Consideration**:

* Security: Timeout prevents indefinite idle sessions
* Usability: 20 seconds may be too short for reading options
* Risk: Is timeout effective against determined attackers?

</details>

**🔬 Implementation Deep Dive**

{% tabs %}
{% tab title="🔐 SSH Configuration" %}
**Automatic User Jailing**:

Edit `/etc/ssh/sshd_config` to automatically chroot specific users:

```bash
# At the end of /etc/ssh/sshd_config
Match User jailed_user
    ChrootDirectory /var/chroot
```

**How This Works**:

1. User `jailed_user` SSHs into the bastion
2. SSH daemon matches the username
3. Automatically executes `chroot /var/chroot` for this session
4. User never sees the real filesystem

**Critical Requirement**: The chroot directory must be owned by root and not writable by the jailed user, or SSH refuses the connection.

**Service Restart**:

```bash
systemctl restart sshd
```
{% endtab %}

{% tab title="🛡️ Restricted Shell" %}
**Replace Default Shell with Custom Script**:

Edit `/etc/passwd` (and copy to `/var/chroot/etc/passwd`):

```
jailed_user:x:1001:1001::/home/jailed_user:/bin/bastion.sh
```

\*\*Instead of `/bin/bash`, the user gets `/bin/bastion.sh`—a custom menu script.

**Bastion Script** (`/var/chroot/bin/bastion.sh`):

```bash
#!/bin/bash

echo "Welcome to Bastion Host"
echo "Select your destination:"
echo "1) Connect to Control Plane as free_user"
echo "2) Exit"
read -n2 -t 20 -p "Choice [timeout: 20s]: " input

case $input in
  1)
    ssh free_user@controlplane
    ;;
  2)
    echo "Exiting..."
    exit 0
    ;;
  *)
    echo "Invalid selection or timeout. Disconnecting."
    exit 1
    ;;
esac
```

**What This Enforces**:

* Users cannot run arbitrary commands
* Only predefined actions available
* Timeout prevents idle sessions
* Any invalid input disconnects the session
{% endtab %}

{% tab title="🔗 Multi-Hop Architecture" %}
**Connection Flow**:

```
User Workstation
     ↓ (SSH)
[Bastion Host: node01]
  ↓ (jailed_user - restricted menu)
  ↓ (Select option 1)
     ↓ (SSH as free_user)
[Air-Gapped Network: controlplane]
  ↓ (free_user - full shell access)
```

**Security Layers**:

1. Physical: Must have network access to bastion
2. Authentication: Must know `jailed_user` password
3. Restriction: Automatically jailed, limited menu
4. Second Authentication: Must know `free_user` password
5. Access: Finally reach air-gapped network

**Emergency Disconnect**: Shutting down the bastion host (`systemctl stop sshd`) instantly disconnects _all_ users from the air-gapped network—a critical "break glass" security control.
{% endtab %}

{% tab title="🧪 Testing Methodology" %}
**Systematic Validation**:

{% stepper %}
{% step %}
#### Test 1: Timeout Mechanism

```bash
ssh jailed_user@node01

# Wait 20 seconds without input

# Expected: Automatic disconnection
```
{% endstep %}

{% step %}
#### Test 2: Invalid Input

```bash
ssh jailed_user@node01

# Enter: asdf

# Expected: Immediate disconnection
```
{% endstep %}

{% step %}
#### Test 3: Exit Option

```bash
ssh jailed_user@node01

# Enter: 2

# Expected: Clean exit message and disconnection
```
{% endstep %}

{% step %}
#### Test 4: Valid Connection

```bash
ssh jailed_user@node01

# Enter: 1

# Enter free_user password

# Expected: SSH session to controlplane as free_user

# Verify: Can I execute arbitrary commands now?
```
{% endstep %}

{% step %}
#### Test 5: Jail Escape Attempts

```bash
ssh jailed_user@node01

# Try: Ctrl+C, Ctrl+Z, Ctrl+\

# Try: Command injection in menu

# Try: Background processes

# Expected: All attempts fail or disconnect
```
{% endstep %}
{% endstepper %}

**Documentation Requirement**: Record results of each test case. Failed controls must be remediated before deployment.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Security Trade-off Analysis**

**Strengths**:

* Single entry point simplifies monitoring
* Automatic jailing prevents filesystem exploration
* Restricted menu eliminates arbitrary command execution
* Timeout prevents indefinite idle sessions

**Weaknesses**:

* 20-second timeout may be too short for legitimate use
* Menu script must be carefully coded (avoid injection vulnerabilities)
* SSH key-based authentication bypass would circumvent controls
* Bastion itself is a single point of failure

**Professional Consideration**: No security control is perfect. Understanding limitations is as important as understanding capabilities.
{% endhint %}

{% hint style="success" %}
✅ **Outcome**:

* Fully functional bastion host implementation
* Automatic user jailing on SSH connection
* Restricted menu-based access control
* Multi-hop authentication architecture
* Understanding of bastion security trade-offs
{% endhint %}
{% endstep %}

{% step %}
#### 🎯 Optional Challenge: Digging Deeper

Contribute to the open-source lab infrastructure and practice professional development workflows.

{% hint style="info" %}
**Professional Skill Development**

This challenge isn't just about fixing a diagram—it's about practicing the GitHub workflow used in professional software and documentation development: fork, edit, commit, pull request. These skills are essential for contributing to enterprise infrastructure-as-code repositories.
{% endhint %}

**📋 Challenge Tasks**

<details>

<summary><strong>Task 1: Fix the Lab Diagram</strong></summary>

**Problem**: The bastion lab diagram shows "unjailed user" but the actual lab uses "free\_user".

**Objective**: Create an accurate diagram using Excalidraw that matches the actual lab implementation.

**Diagram Requirements**:

* Three systems: Outside controlplane, Bastion (node01), Air-gapped controlplane
* User flow: SSH → jailed\_user (menu) → SSH → free\_user (shell)
* Visual indication of jail boundary
* Clear labeling of authentication points

**Tool**: [Excalidraw](https://excalidraw.com/) - open-source diagramming tool

</details>

<details>

<summary><strong>Task 2: GitHub Contribution Workflow</strong></summary>

**Professional Git Workflow**:

Fork the Repository Navigate to: https://github.com/het-tanis/prolug-labsClick Fork to create your own copy.Clone Your Forkgit clone https://github.com/YOUR\_USERNAME/prolug-labs.gitcd prolug-labs/Linux-Labs/210-building-a-bastion-hostReplace the Diagram# Export your Excalidraw diagram as PNG# Replace the existing diagram filecp \~/Downloads/your-bastion-diagram.png ./bastion-diagram.pngCommit Your Changesgit add bastion-diagram.pnggit commit -m "Fix bastion diagram: Update user labels to match lab implementation"git push origin mainCreate Pull RequestGo to your fork on GitHubClick Pull RequestWrite a clear description of your changesSubmit for review

**Pull Request Description Template**:

```markdown
## Fix bastion lab diagram

### Changes
- Updated user label from "unjailed user" to "free_user"
- Clarified jail boundary visualization
- Improved authentication flow indicators

### Testing
- Verified diagram matches actual lab implementation
- Confirmed all system names are accurate

### Screenshots
[Attach before/after screenshots]
```

</details>

<details>

<summary><strong>Task 3: Documentation Improvement Ideas</strong></summary>

**Beyond the Diagram**:

Once you're comfortable with the Git workflow, consider contributing:

1. **Troubleshooting Section**: Common failure points and resolutions
2. **Advanced Configurations**: Multi-destination bastion menu
3. **Security Hardening Guide**: Additional controls beyond basic implementation
4. **Automation Scripts**: Ansible playbook for bastion deployment

**Professional Impact**: Open-source contributions demonstrate initiative, technical writing skills, and collaborative development capabilities—all valued in hiring decisions.

</details>

{% hint style="success" %}
✅ **Outcome**:

* Hands-on experience with professional Git workflows
* Practice contributing to open-source projects
* Technical documentation and diagramming skills
* GitHub profile evidence of community contribution

**Career Impact**: Add "Open-source contributor" to your resume with link to pull request.
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Professional Development:** These labs mirror actual security engineering workflows—from process isolation fundamentals through defense-in-depth access control implementation.&#x20;

> _You've built chroot jails used in containerization, bastion hosts protecting classified networks, and practiced Git workflows used in enterprise infrastructure-as-code development._

**Career Impact**: Add these skills to your project section on your resume:

* **"Implemented chroot jail environments for process isolation"**
* **"Deployed bastion host architecture for air-gapped network access control"**
* **"Contributed to open-source security lab infrastructure via GitHub"**
{% endhint %}

***
