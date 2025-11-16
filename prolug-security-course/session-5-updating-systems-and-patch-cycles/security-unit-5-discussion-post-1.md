---
cover: ../../.gitbook/assets/Screenshot from 2025-11-16 11-59-53.png
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

# 📢 Security Unit 5 Discussion Post 1

## Security Unit 5 Discussion Post 1

Review the Rocky documentation on Software management in Linux: [Software Management](https://docs.rockylinux.org/books/admin_guide/13-softwares/)

***

## 📚 Research References

1. [Software Management →](https://docs.rockylinux.org/books/admin_guide/13-softwares/)
2. [What's EPEL, and how do I use it? →](https://www.redhat.com/en/blog/whats-epel-and-how-do-i-use-it)
3. [How to use Extra Packages for Enterprise Linux (EPEL)? →](https://access.redhat.com/solutions/3358)
4. [EPEL (Extra Packages for Enterprise Linux) →](https://ciq.com/glossary/epel-extra-packages-for-enterprise-linux/)
5. [Understanding EPEL: Install and Use Extra Packages in Linux →](https://tuxcare.com/blog/epel-linux/)
6. [Extra Packages for Enterprise Linux (EPEL) →](https://docs.fedoraproject.org/en-US/epel/)
7. [EPEL Repo Explained: Installation and Usage on Linux →](https://linuxiac.com/epel-repo-installation-and-usage-on-linux/)

***

## 1️⃣ What do I already understand about the process?

{% hint style="info" %}
**Foundation Understanding**

The process of software management in Rocky Linux involves installing, removing, updating, and querying packages using robust command-line tools and repositories designed for enterprise environments.
{% endhint %}

### Core Concepts I Already Know

**Rocky Linux prefers RPM-packaged software for reliability**

Software can be managed in two primary ways in Rocky Linux: using packaged binaries (RPMs) or compiling applications from source code. The preferred method is using RPM packages because dependencies are resolved by the package manager, which increases reliability and reduces administrative overhead. Compiling from source is typically reserved for cases where pre-compiled packages are not available.

**RPM and DNF are the cornerstone tools for direct package management**

Rocky Linux leverages RPM for direct package manipulation and DNF (Dandified Yum), the modern command-line tool for interacting with RPM repositories, resolving dependencies, and managing software installations.

| Tool    | Purpose                | Use Case                                                   |
| ------- | ---------------------- | ---------------------------------------------------------- |
| **RPM** | Low-level operations   | Querying, installing, updating, removing packages directly |
| **DNF** | High-level abstraction | Automatic dependency resolution and repository management  |

**Modular and group operations allow for flexible and bulk installations**

Package managers use configuration files in `/etc/yum.repos.d/` to define repositories, which can contain official packages or additional ones like EPEL (for extra utilities not included in RHEL).

Modular packaging (streams and profiles) introduced in Rocky Linux 8 allows administrators to select multiple application versions from the same repo.

{% code title="Example: Module Management" overflow="wrap" %}
```bash
# Enable specific PostgreSQL version stream
dnf module enable postgresql:12
```
{% endcode %}

**EPEL and other repositories broaden package access but may affect support**

EPEL can be enabled for broader package availability, but is community-supported with less guarantee than official repositories.

**Plugins provide enterprise-grade package control features**

<details>

<summary>📦 DNF Plugin Capabilities</summary>

* **dnf-plugins-core**: Adds capabilities for repository and package management (config-manager, download-only mode, version locks, needs-restarting checks)
* **Version locking**: Prevents automatic upgrades of critical packages
* **Service restart detection**: Identifies services requiring restart after updates

These features improve enterprise operational stability and control.

</details>

***

## 2️⃣ What new things did I learn or pick up?

{% hint style="success" %}
**Key Discovery: EPEL Repositories**

I didn't know EPELs were a thing, and that you were strictly handcuffed to "official" repositories. Case-in-point, it's one of the main reasons why Arch Linux is not used in enterprise environments. The repositories are in the fringe of the Wild Wild West.
{% endhint %}

### Enterprise vs. Community Repository Philosophy

The strict repository control in enterprise Linux distributions provides:

* ✅ Stability and predictability
* ✅ Vendor support and SLAs
* ✅ Security update guarantees
* ✅ Long-term compatibility

This contrasts sharply with rolling-release distributions where package stability and support are less controlled.

***

## 3️⃣ What are the DNF plugins? What is the use of the versionlock plugin?

{% hint style="info" %}
**DNF Plugin System**

DNF plugins are additional Python-based modules that enhance DNF with capabilities that go beyond standard package operations. The core set of plugins is provided by the `dnf-plugins-core` package, which must be installed to access their features.
{% endhint %}

### Prominent DNF Plugins

{% tabs %}
{% tab title="🔧 config-manager" %}
#### config-manager

**Purpose**: Repository configuration management

**Capabilities**:

* Add new repositories
* Enable or disable existing repositories
* Modify repository configuration options

{% code title="Example Usage" overflow="wrap" %}
```bash
# Enable a disabled repository
dnf config-manager --enable epel

# Add a new repository
dnf config-manager --add-repo https://example.com/repo

# Disable a repository
dnf config-manager --disable epel
```
{% endcode %}
{% endtab %}

{% tab title="🏗️ copr" %}
#### copr

**Purpose**: Third-party Copr repository integration

**Capabilities**:

* Simplifies enabling third-party repositories
* Manages packages built in the Copr build system
* Provides access to community-maintained packages

{% code title="Example Usage" overflow="wrap" %}
```bash
# Enable a Copr repository
dnf copr enable user/project

# Install package from Copr
dnf install package-name
```
{% endcode %}

{% hint style="warning" %}
Copr repositories are community-maintained and not officially supported. Use with caution in production.
{% endhint %}
{% endtab %}

{% tab title="📥 download" %}
#### download

**Purpose**: Package download without installation

**Capabilities**:

* Download packages without installing
* Optional dependency resolution
* Useful for offline installations or package inspection

{% code title="Example Usage" overflow="wrap" %}
```bash
# Download package only
dnf download nginx

# Download with all dependencies
dnf download --resolve nginx

# Download to specific directory
dnf download --downloaddir=/tmp/packages nginx
```
{% endcode %}
{% endtab %}

{% tab title="🔄 needs-restarting" %}
#### needs-restarting

**Purpose**: Identify services requiring restart

**Capabilities**:

* Lists processes needing restart after updates
* Identifies services affected by library updates
* Helps maintain system security and stability

{% code title="Example Usage" overflow="wrap" %}
```bash
# List all processes needing restart
dnf needs-restarting

# List services needing restart
dnf needs-restarting -s

# List processes using old libraries
dnf needs-restarting -r
```
{% endcode %}

{% hint style="success" %}
**Best Practice**: Run after system updates to ensure all services are using updated libraries, especially for security patches.
{% endhint %}
{% endtab %}

{% tab title="🔒 versionlock" %}
#### versionlock

**Purpose**: Lock package versions

**Capabilities**:

* Lock specific packages at particular versions
* Prevent automated upgrades or downgrades
* Maintain stability for critical applications

{% code title="Example Usage" overflow="wrap" %}
```bash
# Lock a package at current version
dnf versionlock add nginx

# List all locked packages
dnf versionlock list

# Remove version lock
dnf versionlock delete nginx

# Clear all version locks
dnf versionlock clear
```
{% endcode %}

**Typical Use Cases**:

* ✅ Preventing production-critical application upgrades
* ✅ Allowing only tested and validated versions
* ✅ Avoiding known bugs in newer versions
* ✅ Maintaining compliance with specific version requirements
{% endtab %}

{% tab title="🔄 reposync" %}
#### reposync

**Purpose**: Repository synchronization

**Capabilities**:

* Synchronize entire repositories locally
* Create offline mirrors
* Support air-gapped environments

{% code title="Example Usage" overflow="wrap" %}
```bash
# Sync a repository
dnf reposync --repo=epel --download-path=/local/mirror

# Sync newest packages only
dnf reposync --repo=epel --newest-only
```
{% endcode %}
{% endtab %}
{% endtabs %}

***

### The versionlock Plugin: Deep Dive

{% hint style="success" %}
**Enterprise Stability Tool**

The **versionlock** plugin is specifically used to lock one or more packages at a particular version or set of versions, preventing automated upgrades or downgrades via DNF. This is useful in enterprise environments where stability and compatibility of critical applications are paramount, or where specific software versions are mandated for compliance reasons.
{% endhint %}

**Typical Use Cases**:

| Scenario                     | Why Version Lock?                                     | Example                                                |
| ---------------------------- | ----------------------------------------------------- | ------------------------------------------------------ |
| **Production Critical Apps** | Prevent inadvertent upgrades during routine updates   | Lock database server version during peak season        |
| **Tested Configurations**    | Only allow validated versions on managed systems      | Lock kernel version after extensive testing            |
| **Bug Avoidance**            | Avoid known issues in newer releases                  | Lock nginx version due to regression in latest release |
| **Compliance Requirements**  | Maintain specific versions for audit/regulatory needs | Lock OpenSSL version for FIPS compliance               |

{% code title="Real-World Version Lock Workflow" overflow="wrap" lineNumbers="true" %}
```bash
# Install dnf-plugins-core if not already installed
dnf install dnf-plugins-core

# Lock critical packages before maintenance window
dnf versionlock add kernel
dnf versionlock add postgresql-server
dnf versionlock add nginx

# Verify locks are in place
dnf versionlock list

# Perform system updates (locked packages won't upgrade)
dnf update

# After testing, unlock specific package
dnf versionlock delete nginx

# Update just that package
dnf update nginx

# Re-lock after verification
dnf versionlock add nginx
```
{% endcode %}

***

## 4️⃣ What is an EPEL? Why do I need to consider this when using one?

{% hint style="info" %}
**EPEL Definition**

EPEL (Extra Packages for Enterprise Linux) is a community-driven repository managed by the Fedora Project that provides a wide array of high-quality, additional open-source software packages for Enterprise Linux distributions such as Red Hat Enterprise Linux (RHEL), Rocky Linux, CentOS, and AlmaLinux.
{% endhint %}

### Why EPEL Exists

EPEL was created to fill the gap between the limited set of officially supported packages in enterprise distributions and the much broader package collection available in Fedora Linux.

**Key Characteristics**:

* ✅ Exclusively offers software **not present** in official repositories
* ✅ Packages built to **integrate cleanly** with RHEL and derivatives
* ✅ Resolves dependencies using both EPEL and official repos
* ✅ Includes utilities, language modules, and open-source tools

**Typical EPEL Packages**:

* `htop` - Interactive process viewer
* `ImageMagick` - Image manipulation tools
* `nginx` - Web server (when not in base repos)
* Programming language libraries (Python, Perl, Ruby modules)
* System utilities and monitoring tools

***

### Critical Considerations When Using EPEL

{% tabs %}
{% tab title="🛡️ Support & Stability" %}
#### Support & Stability

{% hint style="warning" %}
**No Official Vendor Support**

EPEL packages are **not officially supported** by Red Hat, Rocky, or other enterprise vendors—the Fedora community maintains them.
{% endhint %}

**Implications**:

* ❌ Less rigorous testing compared to vendor packages
* ❌ Shorter lifecycle and variable maintenance
* ❌ No SLA or support contracts
* ❌ Community-driven updates (may be slower or inconsistent)

**Risk Mitigation**:

* ✅ Test EPEL packages thoroughly before production use
* ✅ Monitor package update frequency and community activity
* ✅ Have fallback plans for critical EPEL dependencies
* ✅ Document all EPEL package usage for audit trails
{% endtab %}

{% tab title="⚙️ Enterprise Standards" %}
#### Enterprise Standards

EPEL software is built from Fedora sources and typically follows Fedora guidelines, which may differ from enterprise-grade standards.

**Areas of Difference**:

| Aspect                | Enterprise Repos        | EPEL                          |
| --------------------- | ----------------------- | ----------------------------- |
| **Support**           | Vendor-backed with SLAs | Community-driven, best-effort |
| **Update Cadence**    | Predictable, scheduled  | Variable, community-dependent |
| **Security Patching** | Guaranteed timelines    | Community-dependent timelines |
| **Testing**           | Rigorous QA processes   | Community testing             |
| **Lifecycle**         | Long-term (10+ years)   | Shorter, version-dependent    |

{% hint style="danger" %}
**Mission-Critical Workloads**

Avoid EPEL dependencies for mission-critical applications where vendor support and guaranteed SLAs are required.
{% endhint %}
{% endtab %}

{% tab title="🔄 Compatibility & Updates" %}
#### Compatibility & Updates

Using EPEL can introduce newer packages that may diverge in version or behavior from their enterprise counterparts.

**Compatibility Concerns**:

* ⚠️ Version mismatches with vendor packages
* ⚠️ Different default configurations
* ⚠️ Potential conflicts with future vendor updates
* ⚠️ Behavioral differences from enterprise equivalents

**Best Practices**:

* ✅ Verify package quality before deployment
* ✅ Test compatibility with existing infrastructure
* ✅ Monitor for conflicts during system updates
* ✅ Maintain documentation of EPEL package configurations
* ✅ Subscribe to EPEL mailing lists for update announcements

{% code title="Safe EPEL Installation" overflow="wrap" lineNumbers="true" %}
```bash
# Install EPEL repository
dnf install epel-release

# Verify EPEL is enabled but not default
dnf repolist

# Install EPEL package with confirmation
dnf install htop

# Check package origin
rpm -qi htop | grep "^From"
```
{% endcode %}
{% endtab %}

{% tab title="🔒 Security & Reliability" %}
#### Security & Reliability

Since these packages are not covered by official support agreements, any bugs or vulnerabilities may require community engagement or self-support to resolve.

**Security Considerations**:

| Risk                          | Impact                         | Mitigation                                          |
| ----------------------------- | ------------------------------ | --------------------------------------------------- |
| **Unpatched Vulnerabilities** | Exposure to known CVEs         | Monitor security advisories, apply patches promptly |
| **Delayed Updates**           | Extended vulnerability windows | Have contingency plans, consider alternatives       |
| **Unknown Provenance**        | Supply chain risks             | Verify package signatures, audit source code        |
| **Compatibility Issues**      | System instability             | Thorough testing before production deployment       |

{% hint style="warning" %}
**Security Vulnerability Response**

EPEL packages may have longer response times for security vulnerabilities compared to vendor-supported packages. Plan accordingly for security-sensitive applications.
{% endhint %}

**Reliability Impacts**:

* Bugs may require community engagement to resolve
* No guaranteed fix timelines
* Self-support may be necessary
* Potential for package abandonment
{% endtab %}

{% tab title="⚖️ Repository Priority" %}
#### Repository Priority

When EPEL is enabled, it is prudent to ensure repository priorities are set properly to avoid accidental upgrades or conflicts with vendor-supported packages.

{% hint style="success" %}
**Best Practice: Set Repository Priorities**

Configure repository priorities to ensure vendor packages take precedence over EPEL when conflicts arise.
{% endhint %}

{% code title="Setting Repository Priorities" overflow="wrap" lineNumbers="true" %}
```bash
# Install priority plugin (if needed)
dnf install dnf-plugin-priorities

# Set priority in repository files

# Edit /etc/yum.repos.d/epel.repo
[epel]
name=Extra Packages for Enterprise Linux $releasever - $basearch
baseurl=https://download.fedoraproject.org/pub/epel/$releasever/Everything/$basearch
enabled=1
gpgcheck=1
priority=10  # Lower priority than base repos (default is 99)

# Verify priority configuration
dnf repolist -v | grep -i priority
```
{% endcode %}

**Priority Guidelines**:

* 🥇 **Priority 1-10**: Vendor/official repositories (base, updates, extras)
* 🥈 **Priority 10-50**: Semi-trusted third-party repos (EPEL)
* 🥉 **Priority 50-99**: Untrusted or testing repositories

**Conflict Resolution**:

```bash
# Check which repo provides a package
dnf repoquery --info package-name

# Prefer package from specific repo
dnf install package-name --repo=baseos

# Exclude EPEL package if conflict exists
dnf install package-name --disablerepo=epel
```
{% endtab %}
{% endtabs %}

***

## 📝 Key Takeaways

{% hint style="success" %}
**Summary: Rocky Linux Software Management**

#### Package Management Tools

* **RPM**: Low-level package operations (install, query, verify, remove)
* **DNF**: High-level dependency resolution and repository management
* **Modular Packaging**: Multiple application versions from same repository

#### DNF Plugins Enhance Control

* **config-manager**: Repository configuration
* **versionlock**: Prevent unwanted package updates
* **needs-restarting**: Identify services requiring restart
* **download**: Offline package acquisition
* **reposync**: Local repository mirroring

#### EPEL Repository Considerations

**Advantages**:

* ✅ Broader package selection
* ✅ Access to tools not in official repos
* ✅ Community-maintained quality packages

**Risks**:

* ⚠️ No vendor support or SLAs
* ⚠️ Variable update cadence
* ⚠️ Potential compatibility issues
* ⚠️ Security patch delays

**Best Practices**:

1. Use EPEL selectively, not by default
2. Set repository priorities correctly
3. Test thoroughly before production use
4. Monitor for security updates
5. Document EPEL package dependencies
6. Have contingency plans for critical packages

#### Enterprise Stability Pattern

**Official repos for critical workloads → EPEL for utilities and tools → Version locks for production stability**
{% endhint %}

***

## 🔗 Additional Resources

**Official Documentation**

* [Rocky Linux Software Management →](https://docs.rockylinux.org/books/admin_guide/13-softwares/)
* [Red Hat: What's EPEL? →](https://www.redhat.com/en/blog/whats-epel-and-how-do-i-use-it)

**EPEL Deep Dives**

* [Fedora Project: EPEL Documentation →](https://docs.fedoraproject.org/en-US/epel/)
* [Understanding EPEL: Install and Use Extra Packages →](https://tuxcare.com/blog/epel-linux/)
* [EPEL Repo Installation and Usage →](https://linuxiac.com/epel-repo-installation-and-usage-on-linux/)

**Red Hat Knowledge Base**

* [How to use Extra Packages for Enterprise Linux (EPEL)? →](https://access.redhat.com/solutions/3358)
