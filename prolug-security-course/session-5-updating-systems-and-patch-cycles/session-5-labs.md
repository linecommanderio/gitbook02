---
cover: ../../.gitbook/assets/lab-ezgif.png
coverY: -221.63013987743312
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

# 🛠️ Session 5 Labs

I'm building a complete enterprise package management and patching infrastructure through progressively complex labs.

<details>

<summary>🏗️ What We'll Build</summary>

A **production-grade software supply chain** with controlled repositories and automated patching:

* [ ] Security-hardened HTTP repository server meeting DISA STIGs
* [ ] Local package repository with GPG verification
* [ ] Network-shared repository infrastructure for air-gapped systems
* [ ] Automated enterprise patching framework with Ansible
* [ ] Staged deployment workflow with pre-checks and rollback capabilities

</details>

### Lab Progression

{% stepper %}
{% step %}
**📋 Security Assessment → Analyze Apache HTTP server security controls**
{% endstep %}

{% step %}
**📦 Repository Creation → Build local package repositories**
{% endstep %}

{% step %}
**🌐 Network Distribution → Share repositories across infrastructure**
{% endstep %}

{% step %}
**🤖 Enterprise Automation → Deploy systematic patching workflows**
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Real-World Workflow**

In corporate environments, you'll manage internal repositories more often than you'll build them from scratch.

But you'll **absolutely** configure systems to use internal repos, implement version locking, and execute patching procedures.
{% endhint %}

This lab teaches you the complete pipeline so you understand how repositories serve enterprise patching workflows.



By the end, we'll understand:

* The **security controls** for repository servers (Apache HTTP hardening with DISA STIGs)
* The **technical implementation** (local repos, network sharing, GPG verification)
* The **automation frameworks** (Ansible-based enterprise patching with staged deployment)

And have a complete package management solution that demonstrates both supply chain security and systematic patching capabilities.

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
* Pre-configured lab materials at `/lab_work/repos_and_patching/`
{% endtab %}

{% tab title="📚 Prerequisites" %}
<table data-full-width="true"><thead><tr><th width="375" valign="top">Required Knowledge</th><th valign="top">Required Skills from Previous Sessions</th></tr></thead><tbody><tr><td valign="top"><ul><li>Package management fundamentals (<code>dnf</code>/<code>yum</code>)</li><li>Repository configuration (<code>/etc/yum.repos.d/</code>)</li><li>HTTP server basics (Apache/httpd)</li><li>File system operations and mounting</li><li>SystemD service management</li><li>Basic Ansible concepts</li></ul></td><td valign="top"><ul><li>STIG Viewer operation and checklist management</li><li>Configuration file editing (<code>vim</code>/<code>nano</code>)</li><li>Network service validation (<code>ss</code>, <code>lsof</code>)</li><li>Firewall configuration (<code>firewall-cmd</code>)</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

***

### 🛠️ Labs to Build Here

{% stepper %}
{% step %}
#### **🔥 Pre-Lab: Security Tools and Vendor Software**

Prepare your assessment toolkit and understand vendor software distribution.

{% hint style="info" %}
**What You're Preparing**

Before building repositories, you need:

1. Security assessment tools to validate repository server hardening
2. Understanding of vendor software distribution (using Mellanox InfiniBand drivers as example)

This pre-lab establishes both the security baseline and real-world vendor software context.
{% endhint %}

**📖 Setup Instructions**

{% stepper %}
{% step %}
**Download STIG Viewer**

Download STIG Viewer 2.18 from [https://public.cyber.mil/stigs/downloads/](https://public.cyber.mil/stigs/downloads/)

{% hint style="success" %}
**Tool Experience**

You've used STIG Viewer in previous sessions—now you're applying it to HTTP server security, critical for hosting package repositories.
{% endhint %}
{% endstep %}

{% step %}
**Import Apache 2.4 STIG**

1. Download the Apache 2.4 STIG from the same downloads page
2. Import it into STIG Viewer
3. Create a checklist from the opened STIG

**Why Apache?** Your repository server will use HTTP (via Apache/httpd) to distribute packages over the network. Security hardening is mandatory for infrastructure services.
{% endstep %}

{% step %}
**Review Vendor Software Distribution**

Navigate to [Mellanox InfiniBand Drivers](https://network.nvidia.com/products/infiniband-drivers/linux/mlnx_ofed/)

**Analysis Questions**:

* Can you find the currently available LTS for Rocky 9.5 x86\_64?
* How do they provide SHA-256 checksums for integrity verification?
* What ISO size considerations exist for air-gapped environments?

<details>

<summary>🔍 Understanding Vendor Distribution</summary>

**What You're Learning**:

* Vendors provide software as ISOs for offline installation
* SHA-256 checksums verify integrity (detect tampering)
* Different distributions require different packages
* Architecture-specific builds (x86\_64 vs. ARM)

**Real-World Application**: When building internal repositories, you'll download vendor ISOs, verify their integrity, mount them, and serve their contents to your infrastructure—exactly what this lab teaches.

</details>

{% hint style="info" %}
**Lab Environment Note**

The Mellanox ISO is already provided in `/lab_work/repos_and_patching/` for your convenience. You won't need to download it, but understanding the vendor distribution process is critical for enterprise work.
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="success" %}
✅ **Outcome**:

* STIG Viewer environment ready for Apache HTTP assessment
* Understanding of vendor software distribution processes
* Foundation for repository server security hardening
{% endhint %}
{% endstep %}

{% step %}
#### **🔐 Lab Part 1: Apache HTTP Server STIG Assessment**

Analyze security controls for the repository server before building the repository infrastructure.

{% hint style="info" %}
**Why This Matters**

Your repository server will distribute software packages across your infrastructure. If an attacker compromises this server, they could inject malicious packages into your entire environment—supply chain attacks at the infrastructure level.

**Critical Security**: HTTP server hardening is foundational before hosting repositories.
{% endhint %}

**🎯 Apache Installation**

{% code title="install-apache.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Stop warewulf client (if running - prevents conflicts)
systemctl stop wwclient

# Install Apache HTTP server
dnf install -y httpd

# Start the HTTP service
systemctl start httpd

# Verify service is running
systemctl status httpd
```
{% endcode %}

**📋 STIG Analysis Methodology**

For each STIG finding, systematically analyze using this framework:

{% stepper %}
{% step %}
**What is the problem?**

Identify the security vulnerability or compliance gap.
{% endstep %}

{% step %}
**What is the fix?**

Determine the remediation action required.
{% endstep %}

{% step %}
**What type of control is being implemented?**

Classify: Technical/Administrative, Preventative/Detective/Corrective
{% endstep %}

{% step %}
**Is it set properly on your system?**

Validate current configuration state.
{% endstep %}

{% step %}
**Can you remediate this finding?**

Implement the fix and verify compliance.
{% endstep %}
{% endstepper %}

**🔍 STIG Examinations**

{% tabs %}
{% tab title="🔐 TLS Configuration STIGs" %}
**Assignment**: Look at the 4 STIGs for "tls"

Filter STIG Viewer using the "tls" keyword.

**Analysis Question**:

* What file is fixed for all of them to be remediated?

<details>

<summary>🔑 Understanding TLS Controls</summary>

**What TLS STIGs Address**:

* **Transport Layer Security**: Encrypting HTTP traffic to prevent eavesdropping
* **Protocol Versions**: Disabling outdated SSL/TLS versions vulnerable to attacks
* **Cipher Suites**: Restricting to strong cryptographic algorithms
* **Certificate Validation**: Ensuring proper PKI implementation

**Common Remediation File**: `/etc/httpd/conf.d/ssl.conf`

All TLS-related STIGs typically modify the SSL configuration file because that's where Apache defines:

* Enabled TLS protocol versions
* Allowed cipher suites
* Certificate paths
* SSL engine parameters

</details>

{% hint style="warning" %}
**Real-World Impact**

Unencrypted HTTP repository traffic allows attackers to:

* Intercept package downloads (confidentiality breach)
* Modify packages in transit (integrity compromise)
* Inject malicious packages (supply chain attack)

TLS encryption is **mandatory** for production repository infrastructure.
{% endhint %}
{% endtab %}

{% tab title="🛡️ V-214234: Error Logging" %}
**Assignment**: Check STIG V-214234

Follow the systematic analysis framework:

{% stepper %}
{% step %}
**What is the problem?**

Examine the STIG requirement and identify the security concern.
{% endstep %}

{% step %}
**What is the fix?**

Determine the configuration change required.
{% endstep %}

{% step %}
**What type of control is being implemented?**

Consider: Is this preventative, detective, or corrective? Technical or administrative?
{% endstep %}

{% step %}
**Is it set properly on your system?**

Verify current configuration:

```bash
# Check Apache configuration
grep -i "ErrorLog" /etc/httpd/conf/httpd.conf
```
{% endstep %}

{% step %}
**Can you remediate?**

Implement the required configuration changes.
{% endstep %}
{% endstepper %}

<details>

<summary>🔍 Understanding Error Logging Controls</summary>

**Why Error Logging Matters**:

* **Security Event Detection**: Logs capture attack attempts
* **Troubleshooting**: Diagnose service failures
* **Compliance Auditing**: Demonstrate security monitoring
* **Forensic Analysis**: Investigate incidents

**Common Issues**:

* Logs not enabled
* Insufficient logging detail
* Logs not sent to centralized SIEM
* Inadequate log retention

</details>
{% endtab %}

{% tab title="🔒 V-214248: Directory Permissions" %}
**Assignment**: Check STIG V-214248

Follow the systematic analysis framework with additional security considerations:

{% stepper %}
{% step %}
**What is the problem?**
{% endstep %}

{% step %}
**What is the fix?**
{% endstep %}

{% step %}
**What type of control is being implemented?**
{% endstep %}

{% step %}
**Is it set properly on your system?**

Verify current permissions:

```bash
# Check Apache directory ownership and permissions
ls -ld /etc/httpd
ls -l /etc/httpd/
```
{% endstep %}

{% step %}
**SELinux Impact Analysis**

**Critical Thinking Question**: How do you think SELinux will help implement this control in an enforcing state? Or will it not affect it?

<details>

<summary>🛡️ SELinux and Apache Security</summary>

**SELinux Context for Apache**:

```bash
# Check SELinux contexts
ls -laZ /etc/httpd

# Expected contexts:

# httpd_config_t - Configuration files

# httpd_sys_content_t - Web content
```

**SELinux Enforcement**:

* **Mandatory Access Control**: Even if file permissions are wrong, SELinux can prevent unauthorized access
* **Process Isolation**: httpd process restricted to specific contexts
* **Defense-in-Depth**: Additional layer beyond traditional DAC permissions

**Real-World Impact**: SELinux enforcing mode provides defense even if file permissions are misconfigured—attacker needs to bypass _both_ DAC and MAC.

</details>
{% endstep %}
{% endstepper %}

**Validation Commands**:

{% code title="verify-permissions.sh" overflow="wrap" %}
```bash
# Check directory ownership
ls -ld /etc/httpd

# Should be: drwxr-xr-x root root

# Check SELinux context (if enforcing)
ls -ldZ /etc/httpd

# Should include: system_u:object_r:httpd_config_t:s0
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="success" %}
✅ **Outcome**:

* Systematic STIG assessment methodology for HTTP servers
* Understanding of TLS security controls
* Apache configuration hardening experience
* SELinux integration with traditional file permissions
* Foundation for secure repository server operation
{% endhint %}
{% endstep %}

{% step %}
#### **📦 Lab Part 2: Building Local Package Repositories**

Create a local package repository from the Rocky Linux DVD ISO.

{% hint style="info" %}
**What You're Building**

A **local repository** hosted on your Hammer server that:

* Contains all packages from the Rocky 9.5 DVD ISO
* Uses GPG verification for package integrity
* Serves as the **only** package source for your system
* Simulates air-gapped environment repository infrastructure

**Real-World Context**: Air-gapped networks can't access internet repositories. Local repos are the **only** way to provide software.
{% endhint %}

**🔧 Implementation Steps**

{% stepper %}
{% step %}
**Remove Existing Repositories**

**Why?** To simulate air-gapped environment and force system to use only your local repository.

{% code title="remove-repos.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Navigate to repository configuration directory
cd /etc/yum.repos.d

# Archive existing repository configurations
mkdir old_archive
mv *.repo old_archive

# Verify repositories are gone
dnf repolist

# Expected output: No repositories available
```
{% endcode %}

**What Just Happened**: Your system now has **no package sources**. You can't install anything until you create the local repository.

{% hint style="danger" %}
**Critical State**

Your system is now in a vulnerable state—if you need to install packages for troubleshooting, you can't until the local repo is configured. In production, you'd stage new repositories _before_ removing old ones.
{% endhint %}
{% endstep %}

{% step %}
**Mount the Rocky Linux ISO**

**Purpose**: Access the DVD ISO contents as a filesystem to expose package directories.

{% code title="mount-iso.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Mount the ISO to /mnt using loop device
mount -o loop /lab_work/repos_and_patching/Rocky-9.5-x86_64-dvd.iso /mnt

# Verify mount succeeded
df -h

# Expected output:

# /dev/loop0  11G  11G  0  100%  /mnt

# Examine ISO contents
ls -l /mnt

# You should see: BaseOS/ and AppStream/ directories
```
{% endcode %}

<details>

<summary>🔍 Understanding Rocky Linux Repository Structure</summary>

**Two Repository Types**:

| Repository | Purpose                                                 | Example Packages                        |
| ---------- | ------------------------------------------------------- | --------------------------------------- |
| BaseOS     | Core operating system components                        | `kernel`, `glibc`, `systemd`, `openssh` |
| AppStream  | Applications, programming languages, additional tooling | `httpd`, `nginx`, `python3`, `git`      |

**Why Separate?**

* **BaseOS**: Minimal, stable, long-term support
* **AppStream**: Rapid updates, multiple versions, modular packages

**Enterprise Benefit**: You can mirror just BaseOS for critical systems, or both for full functionality.

</details>

**Verification**:

{% code title="verify-mount.sh" overflow="wrap" %}
```bash
# Check for repository metadata
ls -l /mnt/BaseOS/repodata/

# Should see: repomd.xml and other repo metadata files
```
{% endcode %}
{% endstep %}

{% step %}
**Create Repository Configuration**

**Purpose**: Tell `dnf` where to find packages and how to verify their integrity.

{% code title="/etc/yum.repos.d/rocky9.repo" %}
```ini
[BaseOS]
name=BaseOS Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/BaseOS/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[AppStream]
name=AppStream Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/AppStream/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
```
{% endcode %}

**Configuration Analysis Questions**

<details>

<summary>❓ Do the paths make sense?</summary>

**Analyze the Configuration**:

* `baseurl=file:///mnt/BaseOS/`
  * **Protocol**: `file://` = local filesystem access
  * **Triple slashes**: `file:///` = absolute path from root
  * **Path**: `/mnt/BaseOS/` = where we mounted the ISO
* Why this works:
  * You mounted the ISO to `/mnt`
  * Inside the ISO are `BaseOS/` and `AppStream/` directories
  * `dnf` can now access `file:///mnt/BaseOS/` directly

**What Happens Next**: When you run `dnf install`, it will:

1. Read `/etc/yum.repos.d/rocky9.repo`
2. Access `file:///mnt/BaseOS/repodata/repomd.xml`
3. Download package lists
4. Install packages from `/mnt/BaseOS/Packages/` or `/mnt/AppStream/Packages/`

</details>

**Set Proper Permissions**:

{% code title="configure-repo.sh" overflow="wrap" %}
```bash
# Set repository configuration to standard permissions
chmod 644 /etc/yum.repos.d/rocky9.repo

# Clear package manager cache
dnf clean all

# Rebuild repository cache
dnf repolist
```
{% endcode %}
{% endstep %}

{% step %}
**Test Local Repository**

**Purpose**: Validate repository functionality and understand package availability.

**Test BaseOS Repository**:

{% code title="test-baseos.sh" overflow="wrap" %}
```bash
# List all available packages from BaseOS
dnf --disablerepo="*" --enablerepo="BaseOS" list available

# Count available packages
dnf --disablerepo="*" --enablerepo="BaseOS" list available | nl

# View first 10 packages
dnf --disablerepo="*" --enablerepo="BaseOS" list available | nl | head
```
{% endcode %}

**Analysis Question**: Approximately how many packages are available in BaseOS?

**Test AppStream Repository**:

{% code title="test-appstream.sh" overflow="wrap" %}
```bash
# List all available packages from AppStream
dnf --disablerepo="*" --enablerepo="AppStream" list available

# Count available packages
dnf --disablerepo="*" --enablerepo="AppStream" list available | nl

# View first 10 packages
dnf --disablerepo="*" --enablerepo="AppStream" list available | nl | head
```
{% endcode %}

**Analysis Question**: Approximately how many packages are available in AppStream?

<details>

<summary>📊 Expected Results</summary>

**Typical Package Counts**:

* **BaseOS**: \~1,000-1,500 packages (core system)
* **AppStream**: \~6,000-8,000 packages (applications and tools)

**Why AppStream is Larger**: Contains multiple versions of languages (Python 3.9, 3.11, 3.12), web servers, databases, and development tools.

</details>
{% endstep %}

{% step %}
**Test Package Installation**

**Purpose**: Validate repository can resolve dependencies and install packages.

{% code title="test-install.sh" overflow="wrap" %}
```bash
# Attempt to install GIMP (graphics editor with many dependencies)
dnf --disablerepo="*" --enablerepo="BaseOS AppStream" install gimp

# CRITICAL: Hit "n" to cancel - do not actually install
```
{% endcode %}

**Analysis Questions**:

1. **How many packages does it want to install?**
   * Count the total packages listed
   * Understand GIMP's dependency tree
2. **How can you tell they're from different repos?**

<details>

<summary>🔍 Reading Repository Indicators</summary>

**Package Repository Identification**:

```
Installing:
 gimp                x86_64  2.99.8-3.el9    AppStream    33 M
Installing dependencies:
 babl                x86_64  0.1.88-1.el9    AppStream   219 k
 gegl04              x86_64  0.4.34-1.el9    AppStream   890 k
 cairo               x86_64  1.17.4-7.el9    BaseOS      789 k
```

**Repository Column**: The fourth column shows package source:

* `AppStream` = from AppStream repository
* `BaseOS` = from BaseOS repository

**Why This Matters**: Dependencies may span multiple repositories. Your repository configuration must enable both to resolve complex dependency trees.

</details>

{% hint style="success" %}
**Validation Success**: If `dnf` can calculate dependencies and list packages, your local repository is functioning correctly.
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="success" %}
✅ **Outcome**:

* Functional local repository serving Rocky 9.5 packages
* Understanding of Rocky Linux repository structure (BaseOS vs. AppStream)
* Experience with GPG verification and repository metadata
* Ability to simulate air-gapped package management
* Foundation for network-shared repository infrastructure
{% endhint %}
{% endstep %}

{% step %}
#### **🌐 Lab Part 3: Network-Shared Repository Infrastructure**

Share your local repository over HTTP so other systems can access it.

{% hint style="info" %}
**What You're Building**

Transform your local repository into **network-shared infrastructure**:

* Configure Apache HTTP to serve repository directories
* Modify repository URLs from `file://` to `http://`
* Enable other systems to install packages from your Hammer server
* Simulate enterprise repository server architecture

**Enterprise Relevance**: Internal repository servers provide:

* **Bandwidth optimization**: Download once, serve to thousands
* **Air-gap support**: No external internet required
* **Version control**: Lock to tested package versions
* **Compliance**: Centralized audit trail
{% endhint %}

**🔧 Implementation Steps**

{% stepper %}
{% step %}
**Verify HTTP Server Readiness**

**Purpose**: Confirm Apache is running and listening before configuring repository sharing.

{% code title="verify-httpd.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Verify httpd package is installed
rpm -qa | grep -i httpd

# Check service status
systemctl status httpd

# Verify listening on port 80
ss -ntulp | grep 80

# Alternative verification
lsof -i :80
```
{% endcode %}

<details>

<summary>🔍 Understanding Service Validation</summary>

**Multiple Validation Methods**:

1. **Package Check** (`rpm -qa`): Confirms installation
2. **Service Status** (`systemctl status`): Confirms service is active
3. **Port Listen** (`ss -ntulp`): Confirms network binding
4. **Process Port** (`lsof`): Identifies specific process on port

**Why Multiple Checks?** Each reveals different failure modes:

* Package missing? Installation failed
* Service inactive? Configuration error preventing start
* Port not listening? Firewall or binding issue
* Wrong process on port? Port conflict

**Professional Practice**: Never assume—verify each layer independently.

</details>
{% endstep %}

{% step %}
**Configure Apache to Serve Repository**

**Purpose**: Create Apache configuration that maps HTTP URL to ISO mount point.

{% code title="/etc/httpd/conf.d/repos.conf" %}
```apache
<Directory "/mnt">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

Alias /repo /mnt

<Location /repo>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Location>
```
{% endcode %}

<details>

<summary>🔍 Understanding Apache Configuration Directives</summary>

**Configuration Breakdown**:

```apache
<Directory "/mnt">
```

* **Purpose**: Define access controls for filesystem path `/mnt`
* **Scope**: Physical directory on server

```apache
Options Indexes FollowSymLinks
```

* **Indexes**: Generate directory listings (allows browsing)
* **FollowSymLinks**: Traverse symbolic links (if present)

```apache
Require all granted
```

* **Access Control**: Allow all clients to access this directory

```apache
Alias /repo /mnt
```

* **URL Mapping**: `http://server/repo` → `/mnt` on filesystem
* **Purpose**: Create clean URL structure

```apache
<Location /repo>
```

* **Purpose**: Define access controls for URL path `/repo`
* **Scope**: HTTP URL space

**How This Works**:

1. Client requests: `http://hammer25/repo/BaseOS/`
2. Apache sees: `Alias /repo /mnt`
3. Apache serves: `/mnt/BaseOS/` (the mounted ISO)

</details>

**Apply Configuration**:

{% code title="restart-httpd.sh" overflow="wrap" %}
```bash
# Restart Apache to load new configuration
systemctl restart httpd

# Verify restart succeeded
systemctl status httpd
```
{% endcode %}
{% endstep %}

{% step %}
**Reconfigure Repository to Use HTTP**

**Purpose**: Change repository URLs from local `file://` to network `http://`.

{% code title="/etc/yum.repos.d/rocky9.repo" %}
```ini
###USE YOUR HAMMER MACHINE IN BASEURL###

[BaseOS]
name=BaseOS Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
#baseurl=file:///mnt/BaseOS/
baseurl=http://hammer25/repo/BaseOS/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[AppStream]
name=AppStream Packages Rocky Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
#baseurl=file:///mnt/AppStream/
baseurl=http://hammer25/repo/AppStream/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
```
{% endcode %}

**Configuration Analysis Questions**

<details>

<summary>❓ Do the modified paths make sense?</summary>

**Analyze the URL Transformation**:

**Before** (local filesystem):

```ini
baseurl=file:///mnt/BaseOS/
```

* Protocol: `file://`
* Path: `/mnt/BaseOS/` (local filesystem)

**After** (network HTTP):

```ini
baseurl=http://hammer25/repo/BaseOS/
```

* Protocol: `http://`
* Hostname: `hammer25` (your lab server)
* Path: `/repo/BaseOS/` (Apache virtual path)

**How This Works**:

1. `dnf` makes HTTP request: `GET http://hammer25/repo/BaseOS/repodata/repomd.xml`
2. Apache receives request for `/repo/BaseOS/`
3. Apache consults `Alias /repo /mnt` configuration
4. Apache serves file from `/mnt/BaseOS/repodata/repomd.xml`
5. Client receives repository metadata

**Critical Understanding**: The URL path `/repo/BaseOS/` doesn't exist on the filesystem. Apache **maps** it to `/mnt/BaseOS/` using the Alias directive.

**If You Don't Understand**:

* Review Apache Alias directive documentation
* Test URL manually: `curl http://hammer25/repo/BaseOS/`
* Ask for clarification in the Discord community

</details>

**Apply Changes**:

{% code title="reconfigure-repos.sh" overflow="wrap" %}
```bash
# Clear cached repository data
dnf clean all

# Rebuild cache from HTTP sources
dnf repolist

# Should show: BaseOS and AppStream via HTTP
```
{% endcode %}
{% endstep %}

{% step %}
**Validate Network Repository**

**Purpose**: Confirm packages can be installed via HTTP instead of local filesystem.

{% code title="test-http-repo.sh" overflow="wrap" %}
```bash
# Attempt to install GIMP via HTTP repositories
dnf --disablerepo="*" --enablerepo="BaseOS AppStream" install gimp

# CRITICAL: Hit "n" to cancel - do not actually install
```
{% endcode %}

**What Changed?**

* **Before**: Packages read from `/mnt/` (local filesystem)
* **After**: Packages download from `http://hammer25/repo/` (network)

**Validation Success Indicators**:

1. `dnf` successfully contacts `http://hammer25`
2. Repository metadata downloads
3. Package dependencies calculate correctly
4. Installation would proceed (if you answered "y")

{% hint style="success" %}
**Achievement Unlocked**: Your Hammer server is now a **repository server**. Any system with network access to Hammer can install packages from it—no internet required.
{% endhint %}
{% endstep %}
{% endstepper %}

<details>

<summary>🏢 Enterprise Architecture Context</summary>

**What You've Built**:

```
┌─────────────────────────────────────────────┐
│     Repository Server (Hammer25)            │
│                                             │
│  ┌─────────────┐        ┌─────────────┐   │
│  │ Rocky ISO   │───────▶│   Apache    │   │
│  │  /mnt/      │        │   httpd     │   │
│  └─────────────┘        └──────┬──────┘   │
│                                 │          │
└─────────────────────────────────┼──────────┘
                                  │
                                  │ HTTP
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
            ┌───────▼────────┐         ┌───────▼────────┐
            │  Client Sys 1  │         │  Client Sys 2  │
            │                │         │                │
            │ dnf install... │         │ dnf install... │
            └────────────────┘         └────────────────┘
```

**Enterprise Benefits**:

* **Centralized Control**: Single source of truth for approved packages
* **Bandwidth Savings**: Download once, serve to thousands
* **Air-Gap Support**: No external internet connectivity required
* **Version Locking**: Freeze packages at tested versions
* **Audit Trail**: All installations from known, logged source

</details>

{% hint style="success" %}
✅ **Outcome**:

* Network-accessible HTTP repository server
* Apache configuration for repository hosting
* Understanding of file-to-HTTP URL mapping
* Multi-system package management infrastructure
* Foundation for enterprise repository architecture
{% endhint %}
{% endstep %}

{% step %}
#### **🤖 Lab Part 4: Enterprise Patching Automation**

Implement systematic patching workflows with Ansible automation and staged deployment procedures.

{% hint style="info" %}
**What You're Building**

A **production-grade patching framework** that:

* **Pre-checks** system state before patching
* **Custom facts** gather system-specific information
* **Stages deployment** to prevent mass failures
* **Automates reboot** when kernel/critical updates require it
* **Post-validates** system health after patching

**Real-World Context**: This is how enterprise infrastructure patches thousands of systems safely. One-off `dnf update` commands don't scale—you need systematic workflows with validation and rollback capabilities.
{% endhint %}

**📖 Lab Instructions**

**Primary Lab**: [Enterprise Ansible Patching on KillerCoda](https://killercoda.com/het-tanis/course/Ansible-Labs/102-Enterprise-Ansible-Patching)

{% hint style="success" %}
**Interactive Environment**

KillerCoda provides a pre-configured environment with:

* Ansible control node
* Multiple target systems
* Pre-written roles demonstrating enterprise patterns
* Safe environment to experiment with patching automation

All commands run in an isolated, disposable environment—perfect for learning patching workflows without risk.
{% endhint %}

**🎯 Learning Objectives**

As you work through the lab, analyze these architectural decisions:

<details>

<summary><strong>Ansible Role Structure</strong></summary>

**Question**: Look at the roles, in the order they are run in the playbook.

**Systematic Analysis**:

Identify Role Execution OrderList roles in playbook execution sequence:First role? (e.g., gather\_custom\_facts)Second role? (e.g., pre\_patch\_checks)Third role? (e.g., apply\_patches)Fourth role? (e.g., reboot\_if\_needed)Fifth role? (e.g., post\_patch\_validation)Understand Role DependenciesWhy must roles execute in this order?Can you apply patches before gathering facts? (No—need system info)Can you reboot before pre-checks? (No—need baseline validation)Can you skip post-validation? (Bad practice—no success confirmation)Identify Enterprise PatternsWhat makes this "enterprise-grade"?Separation of concerns (each role has one responsibility)Idempotency (safe to re-run)Validation at each stageClear rollback points

**Professional Skill**: Understanding role orchestration demonstrates architectural thinking beyond individual task execution.

</details>

<details>

<summary><strong>Custom Facts Deep Dive</strong></summary>

**Question**: Does it make sense how the custom facts are used? What other custom facts might you use?

**Analysis Points**:

**What Are Custom Facts?**

* Ansible facts: System information gathered before task execution
* Custom facts: Additional information you define programmatically

**Common Custom Facts in Patching**:

```yaml
# Example custom facts
ansible_local:
  patching:
    last_patch_date: "2025-01-15"
    patch_window: "maintenance"
    critical_services:
      - postgresql
      - nginx
      - redis
    reboot_required: false
```

**How They're Used**:

```yaml
# Skip patching outside maintenance window
- name: Apply patches
  dnf:
    name: '*'
    state: latest
  when: ansible_local.patching.patch_window == "maintenance"
```

**What Other Custom Facts Might You Use?**

Brainstorm scenarios:

* **Application version** (ensure compatibility before patching)
* **Database replication status** (don't reboot primary while replicating)
* **Load balancer membership** (remove from pool before patching)
* **Compliance tier** (different patch schedules for different systems)

**Professional Insight**: Custom facts enable **context-aware automation**—decisions based on system-specific attributes, not one-size-fits-all commands.

</details>

<details>

<summary><strong>Pre-Check Validation</strong></summary>

**Question**: What are the prechecks doing? What other ones might you add?

**Common Pre-Checks**:

Purpose: Ensure sufficient space for package downloads and installation.- name: Check available disk space  shell: df -h / | tail -1 | awk '{print $5}' | sed 's/%//'  register: disk\_usage- name: Fail if disk space insufficient  fail:    msg: "Insufficient disk space (\{{ disk\_usage.stdout \}}% used)"  when: disk\_usage.stdout|int > 85Why Critical: Package installation fails if disk is full, leaving system in broken state.Purpose: Record critical service state before patching.- name: Check critical services  systemd:    name: "\{{ item \}}"  register: service\_status  loop:    - httpd    - postgresql    - redis- name: Record service states  set\_fact:    pre\_patch\_services: "\{{ service\_status \}}"Why Critical: Enables post-patch validation—did services restart correctly?Purpose: Verify system can reach package repositories.- name: Test repository connectivity  command: dnf repolist  register: repo\_check  failed\_when: "'BaseOS' not in repo\_check.stdout"Why Critical: Prevents starting patch process when repos are unreachable.Purpose: Confirm recent backup exists before making changes.- name: Check last backup date  stat:    path: /var/backup/latest  register: backup\_stat- name: Fail if backup too old  fail:    msg: "No recent backup found"  when:     - backup\_stat.stat.exists    - (ansible\_date\_time.epoch|int - backup\_stat.stat.mtime) > 86400Why Critical: Rollback capability depends on recent backups.

**What Other Pre-Checks Might You Add?**

Consider enterprise scenarios:

* **Configuration drift detection** (system matches Ansible state)
* **Pending reboot check** (finish previous patching first)
* **Load balancer status** (confirm system is in rotation)
* **Database replication lag** (ensure replica is current)
* **Active user sessions** (warn before disruptive reboot)

**Professional Insight**: Pre-checks prevent **predictable failures**. Investing time in validation saves hours of troubleshooting failed patch runs.

</details>

<details>

<summary><strong>Reboot Logic Analysis</strong></summary>

**Question**: What does the reboot task do, and how does it check for reboot to be needed?

**Reboot Decision Logic**:

```yaml
- name: Check if reboot required
  stat:
    path: /var/run/reboot-required
  register: reboot_needed

- name: Reboot system if necessary
  reboot:
    msg: "Rebooting for kernel updates"
    connect_timeout: 5
    reboot_timeout: 600
    pre_reboot_delay: 0
    post_reboot_delay: 30
  when: reboot_needed.stat.exists

- name: Wait for system to become reachable
  wait_for_connection:
    connect_timeout: 20
    sleep: 5
    delay: 5
    timeout: 300
```

**How Reboot Detection Works**:

Detection:Check for /var/run/reboot-required fileCreated by package manager when kernel/critical libs updateSimple, reliable indicatorExample:# After kernel updatels -l /var/run/reboot-required# File exists? Reboot needed.Detection:Compare running kernel vs. installed kernelReboot if newer kernel installed- name: Get running kernel  command: uname -r  register: running\_kernel- name: Get installed kernel  shell: rpm -q kernel | tail -1 | sed 's/kernel-//'  register: installed\_kernel- name: Determine reboot need  set\_fact:    reboot\_required: "\{{ running\_kernel.stdout != installed\_kernel.stdout \}}"Detection:Use needs-restarting utilityIdentifies processes using old libraries- name: Check for services needing restart  command: needs-restarting -r  register: needs\_restart  failed\_when: false- name: Reboot if needed  reboot:  when: needs\_restart.rc == 1Why This Matters: System libraries can update while processes still use old versions. Reboot ensures all processes use updated code.

**Reboot Safety Mechanisms**:

```yaml
reboot:
  connect_timeout: 5      # How long to wait for initial connection
  reboot_timeout: 600     # Maximum time for reboot (10 minutes)
  pre_reboot_delay: 0     # Delay before issuing reboot command
  post_reboot_delay: 30   # Wait after reboot before continuing
```

**Why Each Parameter Matters**:

* **connect\_timeout**: Detects hung systems quickly
* **reboot\_timeout**: Prevents infinite waits
* **pre\_reboot\_delay**: Allows graceful service shutdown
* **post\_reboot\_delay**: Ensures services fully initialize

**Professional Consideration**: Rebooting thousands of systems simultaneously can overwhelm infrastructure. Enterprise patching **staggers reboots** across time windows and availability zones.

</details>

**🔬 Advanced Analysis Questions**

After completing the KillerCoda lab:

1. **How would you modify this workflow for database servers that can't reboot during business hours?**
   * Consider: Maintenance windows, replication status, connection draining
2. **What rollback mechanisms are missing from this playbook?**
   * Consider: Snapshot restoration, package downgrade, configuration rollback
3. **How would you adapt this for a 1,000-system infrastructure?**
   * Consider: Staging (dev → staging → production), percentage-based deployment, canary systems

{% hint style="success" %}
✅ **Outcome**:

* Understanding of enterprise patching architecture
* Experience with Ansible role orchestration
* Knowledge of pre-check and post-validation patterns
* Appreciation for systematic deployment workflows
* Foundation for building production patching infrastructure
{% endhint %}
{% endstep %}

{% step %}
#### **🎯 Optional Challenge: Digging Deeper**

Advanced challenges for engineers ready to extend their repository and patching knowledge.

{% hint style="info" %}
**Professional Development**

These challenges aren't required for lab completion but represent real-world scenarios you'll encounter in enterprise environments. They're opportunities to:

* Apply learned concepts to novel problems
* Practice troubleshooting and documentation
* Contribute to open-source infrastructure projects
{% endhint %}

**📋 Challenge Tasks**

<details>

<summary><strong>Challenge 1: Air-Gap Repository Justification</strong></summary>

**Question**: You've set up a local repository and you've shared that repo out to other systems that might want to connect. Why might you need this if you're going to fully air-gap systems? Is it still necessary even if your enterprise patching solution is well designed? Why or why not?

**Analysis Framework**:

Define "Fully Air-Gapped"What does "fully air-gapped" actually mean?No internet connectivity? (Yes, but...)No network connectivity at all? (Usually no—internal networks exist)Physical separation from external networks? (Yes)Identify Software Update RequirementsAir-gapped systems still need:Security patches for vulnerabilitiesBug fixes for stabilityNew software installationsDependency updatesQuestion: If systems have no internet, how do they get updates?Repository Solutions for Air-GapsScenario A: Truly Isolated SystemsPhysically disconnected from all networksUpdates via: Removable media (USB drives, DVDs)Process: Mount ISO → Install packages locallyScenario B: Air-Gapped NetworkInternal network exists, but isolated from internetUpdates via: Internal repository server (what you built!)Process: Sync packages to internal repo → Distribute to systemsWhich Scenario is More Common? Scenario B—most "air-gapped" systems operate on isolated networks, not standalone machines.Repository Value in Air-Gapped EnvironmentsEven with Enterprise Patching Solution, Repositories Provide:Bandwidth optimization: Download once, serve to thousands (even internally)Version control: Lock to specific tested versionsRollback capability: Keep previous package versions availableOffline operation: Patching doesn't depend on external sync timingCompliance audit: Central source of truth for approved softwareAnswer the Question: Is it still necessary?Yes, because:Enterprise patching solutions use repositories—they don't replace themAnsible/Satellite/Spacewalk all require package repositories to distribute updatesAir-gapped networks need internal repositories since external ones are unreachable

**Write Your Analysis**: Document your reasoning in the Discord discussion forum.

</details>

<details>

<summary><strong>Challenge 2: Add Mellanox ISO as Repository</strong></summary>

**Objective**: Can you add the Mellanox ISO that is included in the `/lab_work/repos_and_patching` section to be a repository that your systems can access?

**Implementation Steps**:

Mount Mellanox ISO# Create mount pointmkdir -p /mnt/mellanox# Mount the Mellanox ISOmount -o loop /lab\_work/repos\_and\_patching/MLNX\_OFED\_LINUX-\<version>.iso /mnt/mellanox# Verify mountls -l /mnt/mellanoxExplore ISO Structure# Find repository directoriescd /mnt/mellanoxfind . -name "repodata" -type d# Understand directory layout# Mellanox ISOs often have RPMS/ or different structureCreate Repository Configuration# Create Mellanox repo configvi /etc/yum.repos.d/mellanox.repoConfiguration Template:\[mellanox-ofed]name=Mellanox OFED Repositorybaseurl=file:///mnt/mellanox/RPMS/enabled=1gpgcheck=0Note: GPG check disabled for third-party repos unless you import their signing key.Configure Apache to Serve Mellanox RepoExtend repos.conf:# Add to /etc/httpd/conf.d/repos.confAlias /mellanox /mnt/mellanox\<Location /mellanox>    Options Indexes FollowSymLinks    AllowOverride None    Require all granted\</Location>Restart Apache:systemctl restart httpdUpdate Repository for Network Access\[mellanox-ofed]name=Mellanox OFED Repositorybaseurl=http://hammer25/mellanox/RPMS/enabled=1gpgcheck=0Test:dnf clean alldnf --disablerepo="\*" --enablerepo="mellanox-ofed" list available

**Troubleshooting Tips**:

* Can't find repodata? Mellanox ISOs may require `createrepo` to generate metadata
* Permission errors? Check Apache can read `/mnt/mellanox`
* 404 errors? Verify Alias path matches filesystem path

**If You Have Trouble**: Ask in the Discord group. Troubleshooting vendor ISOs is a valuable real-world skill.

</details>

<details>

<summary><strong>Challenge 3: Contribute to Enterprise Patching Tool</strong></summary>

**Objective**: Make a pull request to improve the enterprise patching tool that you just used. Write up, for the group, why you need that change and how it improves the efficacy of the patching.

**Professional Git Workflow**:

Identify Improvement OpportunityPotential Enhancements:Additional pre-checks: Database status, backup verificationEnhanced rollback: Automated snapshot restorationBetter reporting: Generate patch compliance reportsMulti-stage deployment: Canary systems before full rolloutNotification integration: Slack/email alerts on completionDocument Your Rationale:What problem does this solve?What enterprise scenario requires this enhancement?How does it improve patching efficacy?Fork the RepositoryNavigate to: https://github.com/het-tanis/ansible-labsClick Fork to create your own copy.Clone Your Forkgit clone https://github.com/YOUR\_USERNAME/ansible-labs.gitcd ansible-labs/Ansible-Labs/102-Enterprise-Ansible-PatchingImplement Your EnhancementExample: Add disk space pre-check# roles/pre\_patch\_checks/tasks/main.yml- name: Check available disk space  shell: df -h / | tail -1 | awk '{print $5}' | sed 's/%//'  register: disk\_usage- name: Fail if insufficient disk space  fail:    msg: "Insufficient disk space (\{{ disk\_usage.stdout \}}% used). Require <85%."  when: disk\_usage.stdout|int > 85Commit Your Changesgit add roles/pre\_patch\_checks/tasks/main.ymlgit commit -m "Add disk space pre-check to prevent failed installations"git push origin mainCreate Pull RequestGo to your fork on GitHubClick Pull RequestWrite clear description of enhancementSubmit for reviewPull Request Description Template:## Enhancement: Add Disk Space Pre-Check### Problem StatementPackage installations fail when disk space is insufficient, leaving systemsin broken states requiring manual intervention.### SolutionAdded pre-check role task that:- Measures root filesystem usage- Fails playbook if >85% utilized- Provides clear error message to operator### Benefits- Prevents predictable installation failures- Reduces troubleshooting time- Improves patching reliability### Testing- Tested on systems with 90% disk usage (correctly failed)- Tested on systems with 70% disk usage (correctly proceeded)- Verified error message clarity### Enterprise JustificationIn production environments with thousands of systems, a small percentagewith disk issues can create significant operational overhead. This pre-checkprevents wasted patching attempts and reduces incident response burden.

**Career Impact**: Open-source contributions demonstrate:

* Initiative and professional development
* Ansible automation expertise
* Collaborative development workflow
* Technical writing and documentation skills

Add to resume: "Contributed to open-source Ansible patching framework"

</details>

{% hint style="success" %}
✅ **Outcome**:

* Deep analysis of air-gap repository requirements
* Hands-on experience with vendor ISO integration
* Professional Git workflow practice
* Open-source contribution to infrastructure automation
* Evidence of advanced troubleshooting and documentation capabilities
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="info" %}
🎓 **Professional Development**

These labs mirror actual enterprise infrastructure workflows—from security assessment through repository infrastructure deployment to automated patching frameworks.

> _You've built Apache-based repository servers, configured package distribution infrastructure, and implemented Ansible automation using the same tools and processes required in corporate environments._

**Career Impact**: Add these skills to your project section on your resume:

* **"Enterprise Linux repository management"**
* **"Apache HTTP configuration"**
* **"Ansible patching automation"**
{% endhint %}

***
