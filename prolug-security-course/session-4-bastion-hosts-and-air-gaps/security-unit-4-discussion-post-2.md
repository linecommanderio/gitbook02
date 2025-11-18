---
cover: ../../.gitbook/assets/Screenshot from 2025-11-17 21-43-45.png
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

# 📢 Security Unit 4 Discussion Post 2

## Security Unit 4 Discussion Post 2

> _**Do a Google or AI search of topics around jailing a user or processes in Linux.**_

***

## 📚 Research References

1. [How to set up Linux chroot jails - Red Hat →](https://www.redhat.com/en/blog/set-linux-chroot-jails)
2. [Taking a deeper dive into Linux chroot jails - Red Hat →](https://www.redhat.com/en/blog/deeper-linux-chroot-jails)
3. [Containers, Jails, and chroots; An introduction →](https://derg.nz/web/sysadmin/kb/blog/2022/11/07/Containers-an-introduction.html)
4. [Understanding Whether We Are in a chroot Jail - Baeldung →](https://www.baeldung.com/linux/chroot-jail)
5. [How to Restrict SSH User Access Using Chrooted Jail - TecMint →](https://www.tecmint.com/restrict-ssh-user-to-directory-using-chrooted-jail/)
6. [Difference Between chroot and Docker - Baeldung →](https://www.baeldung.com/ops/chroot-vs-docker)
7. [Setup User With SFTP/SCP Only Inside Chroot Jail - TechBlog →](https://techblog.dev/posts/2022/12/restrict-user-access-only-sftp-scp-inside-chroot-jail-linux/)
8. [Systemd: Advanced Features, Sandboxing, and Security - AlphaBravo →](https://blog.alphabravo.io/systemd-zero-to-hero-part-5-advanced-features-sandboxing-and-security-best-practices/)
9. [How To Sandbox Processes With Systemd - DigitalOcean →](https://www.digitalocean.com/community/tutorials/how-to-sandbox-processes-with-systemd-on-ubuntu-20-04)

***

## 📖 Introduction: User Jailing Concepts

{% hint style="info" %}
**What is User Jailing?**

User jailing (or process containment) refers to techniques that restrict a user's or process's access to system resources, limiting what they can see, modify, or execute. These security mechanisms prevent unauthorized access, contain potential security breaches, and enforce the principle of least privilege.
{% endhint %}

**Core Security Principles**:

* **Isolation**: Separate users/processes from sensitive system resources
* **Least Privilege**: Grant minimum access necessary for functionality
* **Defense in Depth**: Multiple security layers working together
* **Attack Surface Reduction**: Limit what attackers can reach if compromised

**Why Jail Users and Processes?**:

| Security Goal                    | Jailing Benefit                                               |
| -------------------------------- | ------------------------------------------------------------- |
| **Prevent Privilege Escalation** | Limits access to system resources even if process compromised |
| **Contain Breaches**             | Attacker gains access only to restricted environment          |
| **Multi-Tenancy**                | Multiple users share system without interfering               |
| **Compliance**                   | Meet regulatory requirements for data separation              |
| **Development/Testing**          | Safely test untrusted code in isolated environment            |

***

## 1️⃣ Methods of Jailing Users in Linux

{% hint style="success" %}
**Nine Major Jailing Methods**

Linux provides multiple complementary approaches to user and process confinement, each with different scopes, strengths, and use cases. Modern secure systems often combine several methods for defense-in-depth.
{% endhint %}

### Comparison Overview

| Method                  | Isolation Scope                              | Complexity  | Root Escape Risk                         | Best For                                        |
| ----------------------- | -------------------------------------------- | ----------- | ---------------------------------------- | ----------------------------------------------- |
| **chroot**              | Filesystem only                              | Low         | High (if root)                           | SFTP, simple file access restriction            |
| **SELinux**             | System-wide (files, processes, capabilities) | High        | Very Low                                 | Enterprise RHEL/Fedora mandatory access control |
| **AppArmor**            | Application-level                            | Medium      | Low                                      | SUSE/Ubuntu/Debian app restrictions             |
| **systemd**             | Service/process                              | Medium      | Low                                      | Service sandboxing, system daemons              |
| **Firejail/Bubblewrap** | Process/app                                  | Medium      | Low (Bubblewrap), Medium (Firejail SUID) | Desktop app isolation, browser sandboxing       |
| **Podman Rootless**     | Container                                    | Medium-High | Very Low (rootless)                      | Multi-tenant workloads, dev environments        |
| **Snap/Flatpak**        | Application                                  | Low-Medium  | Low                                      | Desktop application sandboxing                  |
| **PAM Modules**         | Session/auth                                 | Low-Medium  | N/A (access control)                     | Login restrictions, policy enforcement          |
| **cgroups/Quotas**      | Resources only                               | Low-Medium  | N/A (resource limits)                    | Multi-user resource management, DoS prevention  |

### Detailed Method Analysis

{% tabs %}
{% tab title="📁 chroot Jail" %}
**chroot Jail - Filesystem Isolation**

{% hint style="info" %}
**Core Concept**

`chroot` changes a process's apparent root directory, restricting filesystem access to a subtree. The process cannot access files outside the chroot jail.
{% endhint %}

**How It Works**:

{% code title="Basic chroot Example" overflow="wrap" lineNumbers="true" %}
```bash
# Create jail directory structure
mkdir -p /jail/{bin,lib64,home}

# Copy essential binaries
cp /bin/bash /jail/bin/
cp /bin/ls /jail/bin/

# Copy required libraries
ldd /bin/bash | grep -o '/lib.*\.[0-9]' | xargs -I {} cp {} /jail/lib64/
ldd /bin/ls | grep -o '/lib.*\.[0-9]' | xargs -I {} cp {} /jail/lib64/

# Chroot into jail
chroot /jail /bin/bash

# Inside jail: root directory is now /jail
ls /

# Output: bin  home  lib64

# Cannot access actual system root
ls /etc

# ls: cannot access '/etc': No such file or directory
```
{% endcode %}

**Technical Details**:

<details>

<summary>🔍 chroot Implementation Details</summary>

**System Call**:

```c
int chroot(const char *path);
```

**What Changes**:

* Process's root directory (`/`) becomes the specified path
* All absolute paths resolved relative to new root
* `..` cannot escape beyond new root

**What Doesn't Change**:

* Process's current working directory (must `chdir()` after `chroot()`)
* Open file descriptors from before chroot
* Process capabilities and user ID
* Network access
* Device access

**Security Requirements**:

* Requires root privileges (CAP\_SYS\_CHROOT)
* Must drop privileges after chroot for security
* Jail must contain all necessary libraries and binaries

</details>

**Characteristics**:

* **Scope**: Filesystem only. Limits accessible files but leaves processes, users, and networks unconfined.
* **Strengths**:
  * ✅ Simple to implement and understand
  * ✅ Compatible with all Linux systems (no special kernel features)
  * ✅ Commonly used for SFTP/SSH restriction
  * ✅ Useful for system recovery environments
* **Limitations**:
  * ❌ No process or network isolation
  * ❌ Vulnerable to root escapes (if process runs as root)
  * ❌ Requires careful jail setup with all dependencies
  * ❌ Must drop privileges for real security
* **Recent Usage Examples**:
  * SFTP/SSH user jails
  * System recovery environments
  * Package building (mock, pbuilder)
  * FTP servers (vsftpd chroot)

**SSH/SFTP chroot Example**:

{% code title="/etc/ssh/sshd_config - SFTP Chroot" overflow="wrap" lineNumbers="true" %}
```bash
# Match specific users or groups
Match Group sftpusers
    # Force SFTP subsystem
    ForceCommand internal-sftp
    # Chroot to user's home directory
    ChrootDirectory /home/%u
    # Disable forwarding
    AllowTcpForwarding no
    X11Forwarding no
    # Limit permissions
    PermitTunnel no

# Directory structure requirements:

# /home/username must be owned by root:root

# /home/username permissions must be 755 or more restrictive

# User writes to /home/username/uploads (owned by user)
```
{% endcode %}

**Setup SFTP Chroot Jail**:

{% stepper %}
{% step %}
#### Create User and Directory Structure

{% code overflow="wrap" lineNumbers="true" %}
```bash
# Create SFTP-only group
groupadd sftpusers

# Create user
useradd -g sftpusers -d /home/sftpuser -s /sbin/nologin sftpuser
passwd sftpuser

# Create directory structure
mkdir -p /home/sftpuser/uploads

# Set ownership (chroot directory must be root-owned)
chown root:root /home/sftpuser
chmod 755 /home/sftpuser

# Upload directory owned by user
chown sftpuser:sftpusers /home/sftpuser/uploads
chmod 755 /home/sftpuser/uploads
```
{% endcode %}
{% endstep %}

{% step %}
#### Configure SSH

{% code overflow="wrap" %}
```bash
# Edit /etc/ssh/sshd_config

# Add Match block shown above

# Test configuration
sshd -t

# Restart SSH
systemctl restart sshd
```
{% endcode %}
{% endstep %}

{% step %}
#### Test SFTP Access

{% code overflow="wrap" %}
```bash
# Connect as chrooted user
sftp sftpuser@localhost

# Inside SFTP session
sftp> pwd
Remote working directory: /

sftp> ls
uploads

sftp> cd uploads
sftp> put testfile.txt
Uploading testfile.txt to /uploads/testfile.txt

sftp> ls /
uploads  # Cannot see /etc, /var, etc.
```
{% endcode %}
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
**Security Warning**: `chroot` is NOT a security boundary for root users. A process running as root inside a chroot can escape using various techniques. Always drop privileges after chrooting.
{% endhint %}

<details>

<summary>🔓 chroot Escape Techniques (Why Root in chroot is Dangerous)</summary>

**Common Escape Methods**:

1. **Create Device Files**:

```bash
# Inside chroot as root
mknod /dev/hda1 b 3 1
mount /dev/hda1 /mnt
chroot /mnt

# Now outside original chroot
```

2. **Use ptrace on Process Outside chroot**:

```bash
# Attach to process outside chroot

# Inject shellcode to execute commands
```

3. **Create New chroot**:

```bash
# As root, can call chroot() again
mkdir /tmp/newroot
chroot /tmp/newroot

# Breaks out of original chroot
```

**Mitigation**: Drop root privileges immediately after chroot.

</details>
{% endtab %}

{% tab title="🔒 SELinux Confined User" %}
**SELinux Confined User - Mandatory Access Control**

{% hint style="info" %}
**Core Concept**

SELinux (Security-Enhanced Linux) applies security contexts and policies to users, providing mandatory access control that restricts what processes can do regardless of traditional Unix permissions.
{% endhint %}

**How It Works**:

SELinux assigns **security contexts** (labels) to:

* Users (user identities)
* Processes (domain types)
* Files (file types)
* Network ports, devices, etc.

Policies define what interactions are allowed between these contexts.

{% code title="SELinux Context Format" overflow="wrap" %}
```
user:role:type:level

Example:
user_u:user_r:user_t:s0

Components:
- user_u     - SELinux user (maps to Linux user)
- user_r     - Role (defines what domains can be entered)
- user_t     - Type/Domain (defines permissions)
- s0         - Security level (MLS/MCS)
```
{% endcode %}

**Characteristics**:

* **Scope**: System-wide. Controls processes, file access, and privilege transitions with fine granularity.
* **Strengths**:
  * ✅ Extremely granular policy enforcement
  * ✅ Strong separation on Fedora/RHEL
  * ✅ Protects against privilege escalation
  * ✅ Confines even root-owned processes
  * ✅ Defense against zero-day exploits
* **Limitations**:
  * ❌ Complex to set up and maintain
  * ❌ Some desktop apps need policy tweaks
  * ❌ Only effective with SELinux enabled
  * ❌ Requires proper user context mapping
  * ❌ Steep learning curve
* **Recent Usage Examples**:
  * Desktop and server user confinement on Fedora and RHEL
  * Improved KDE support in Fedora 40
  * Android (SELinux-based app sandboxing)
  * Container isolation (SELinux contexts per container)

**SELinux User Types**:

| SELinux User      | Purpose              | Access Level                                  |
| ----------------- | -------------------- | --------------------------------------------- |
| **unconfined\_u** | Full system access   | Unrestricted (like root)                      |
| **user\_u**       | Regular user         | Cannot su/sudo, cannot X11 forwarding         |
| **staff\_u**      | Administrative user  | Can sudo to specific roles                    |
| **sysadm\_u**     | System administrator | Full administrative access                    |
| **guest\_u**      | Restricted guest     | No network access, no X11, very limited       |
| **xguest\_u**     | Guest with browser   | Web browser only, no file access outside home |

**Confine a User with SELinux**:

{% stepper %}
{% step %}
#### View Current User Mapping

{% code overflow="wrap" %}
```bash
# View SELinux user mappings
semanage login -l

Login Name           SELinux User         MLS/MCS Range
__default__          unconfined_u         s0-s0:c0.c1023
root                 unconfined_u         s0-s0:c0.c1023
```
{% endcode %}
{% endstep %}

{% step %}
#### Map Linux User to SELinux User

{% code overflow="wrap" lineNumbers="true" %}
```bash
# Create regular Linux user
useradd restricteduser
passwd restricteduser

# Map to confined SELinux user (user_u)
semanage login -a -s user_u restricteduser

# Verify mapping
semanage login -l | grep restricteduser
restricteduser       user_u               s0

# Relabel user's home directory
restorecon -R -F -v /home/restricteduser
```
{% endcode %}
{% endstep %}

{% step %}
#### Test Confined User

{% code overflow="wrap" %}
```bash
# Login as confined user
ssh restricteduser@localhost

# Check current context
id -Z
user_u:user_r:user_t:s0

# Try to become root (denied by SELinux)
su -

# SELinux denies even if password correct

# Try to execute in user's home
cd ~
./my-script.sh

# Only works if file has correct SELinux context

# Check what's denied
ausearch -m avc -ts recent
```
{% endcode %}
{% endstep %}
{% endstepper %}

**Guest User Confinement**:

{% code title="Create Extremely Restricted Guest User" overflow="wrap" lineNumbers="true" %}
```bash
# Create guest account
useradd -s /bin/bash guestuser
passwd guestuser

# Map to guest_u (most restrictive)
semanage login -a -s guest_u guestuser

# Relabel home directory
restorecon -R -F -v /home/guestuser

# Guest user limitations:

# ❌ Cannot execute most binaries

# ❌ No network access

# ❌ No X11 forwarding

# ❌ Cannot read other users' files

# ❌ Very limited system interaction
```
{% endcode %}

**Web Browser-Only User (xguest)**:

{% code title="User Can Only Browse Web" overflow="wrap" lineNumbers="true" %}
```bash
# Create xguest user
useradd xguestuser
semanage login -a -s xguest_u xguestuser

# xguest_u can:

# ✅ Run web browser (Firefox, Chrome)

# ✅ Access home directory

# ❌ Execute downloaded files

# ❌ Access USB devices

# ❌ Network access except browser

# ❌ Read files outside home
```
{% endcode %}

{% hint style="success" %}
**Enterprise Benefit**: SELinux confinement provides defense-in-depth even if an attacker compromises a user account. The SELinux policy prevents lateral movement and privilege escalation.
{% endhint %}

<details>

<summary>🔧 Troubleshooting SELinux Denials</summary>

**Check for Denials**:

```bash
# View recent AVC denials
ausearch -m avc -ts recent

# Or use audit2why for human-readable explanations
ausearch -m avc -ts recent | audit2why

# Generate policy module to allow (if legitimate)
ausearch -m avc -ts recent | audit2allow -M mymodule
semodule -i mymodule.pp
```

**Common Issues**:

* User home directory not labeled correctly → `restorecon -R -F /home/username`
* Application needs additional permissions → Create custom policy module
* File copied from wrong location → Relabel with `chcon` or `restorecon`

</details>
{% endtab %}

{% tab title="🛡️ AppArmor Confined User" %}
**AppArmor Confined User - Path-Based Security Profiles**

{% hint style="info" %}
**Core Concept**

AppArmor restricts user programs using path-based security profiles. It can be integrated with PAM to apply profiles on user login, controlling what system resources programs can access.
{% endhint %}

**How It Works**:

AppArmor uses **profiles** that define:

* Which files a program can read, write, or execute
* Which capabilities the program can use
* Network access permissions
* IPC (Inter-Process Communication) permissions

Profiles are based on **file paths** (not labels like SELinux).

{% code title="AppArmor Profile Example" overflow="wrap" lineNumbers="true" %}
```
# /etc/apparmor.d/usr.bin.firefox

#include <tunables/global>

/usr/bin/firefox {
  #include <abstractions/base>
  #include <abstractions/dbus>
  
  # Allow reading user's home directory
  owner @{HOME}/** r,
  
  # Allow writing to downloads
  owner @{HOME}/Downloads/** rw,
  
  # Allow reading system libraries
  /usr/lib/** r,
  /lib/** r,
  
  # Allow network access
  network inet stream,
  network inet6 stream,
  
  # Deny access to sensitive files
  deny /etc/shadow r,
  deny /root/** rw,
  
  # Allow executing specific binaries
  /usr/bin/firefox ix,
  /usr/lib/firefox/** ix,
}
```
{% endcode %}

**Characteristics**:

* **Scope**: Application-level. Constrains how programs run and what system resources they access.
* **Strengths**:
  * ✅ Easier to write/edit profiles than SELinux policies
  * ✅ Can be tied to usernames/groups with pam\_apparmor
  * ✅ Path-based matching is intuitive
  * ✅ Good for desktop application sandboxing
  * ✅ Less overhead than SELinux
* **Limitations**:
  * ❌ Less granular for user-level control compared to SELinux
  * ❌ Path-based (can be bypassed with hard links in some cases)
  * ❌ Not default on all distributions (SUSE, Ubuntu, Debian)
  * ❌ Smaller community/policy library than SELinux
* **Recent Usage Examples**:
  * Enforced app restrictions on SUSE, Ubuntu, and Debian systems
  * Snap package confinement (Ubuntu)
  * Server application sandboxing (web servers, databases)

**AppArmor Modes**:

| Mode         | Behavior                               | Use Case            |
| ------------ | -------------------------------------- | ------------------- |
| **Enforce**  | Deny operations not explicitly allowed | Production          |
| **Complain** | Allow everything, log violations       | Profile development |
| **Disabled** | No restrictions                        | Not recommended     |

**Confine User Applications with pam\_apparmor**:

{% stepper %}
{% step %}
#### Install pam\_apparmor

{% code overflow="wrap" %}
```bash
# Ubuntu/Debian
apt install libpam-apparmor

# SUSE
zypper install pam_apparmor

# Verify AppArmor is active
systemctl status apparmor
aa-status
```
{% endcode %}
{% endstep %}

{% step %}
#### Create AppArmor Profile for User Shell

{% code title="/etc/apparmor.d/bin.bash.restricted" overflow="wrap" lineNumbers="true" %}
```
#include <tunables/global>

/bin/bash flags=(complain) {
  #include <abstractions/base>
  #include <abstractions/bash>
  
  # Allow user's home directory access
  owner @{HOME}/** rw,
  
  # Allow reading common system files
  /etc/passwd r,
  /etc/group r,
  /etc/nsswitch.conf r,
  
  # Allow executing common utilities
  /bin/* ix,
  /usr/bin/* ix,
  
  # Deny sensitive directories
  deny /root/** rw,
  deny /etc/shadow r,
  deny /var/log/** rw,
  deny /proc/sys/** w,
  
  # Network access
  network inet stream,
  network inet6 stream,
}
```
{% endcode %}

Load profile:

```bash
apparmor_parser -r /etc/apparmor.d/bin.bash.restricted
```
{% endstep %}

{% step %}
#### Configure PAM to Apply Profile on Login

{% code title="/etc/pam.d/login" overflow="wrap" lineNumbers="true" %}
```
# At the end of session section
session optional pam_apparmor.so order=user,group,default
```
{% endcode %}

Create user-specific profile mapping:

{% code title="/etc/security/apparmor.d/users/restricteduser" overflow="wrap" %}
```
/bin/bash.restricted
```
{% endcode %}
{% endstep %}

{% step %}
#### Test Confined User

{% code overflow="wrap" %}
```bash
# Login as restricted user
login restricteduser

# Check current AppArmor profile
cat /proc/self/attr/current
/bin/bash.restricted (enforce)

# Try accessing denied path
cat /etc/shadow

# Permission denied (AppArmor blocked)

# Check AppArmor denials
dmesg | grep -i apparmor

# or
journalctl | grep apparmor
```
{% endcode %}
{% endstep %}
{% endstepper %}

**Per-Application Confinement**:

{% code title="Confine Firefox per User" overflow="wrap" lineNumbers="true" %}
```bash
# Create profile
cat > /etc/apparmor.d/usr.bin.firefox << 'EOF'
#include <tunables/global>

/usr/bin/firefox {
  #include <abstractions/base>
  #include <abstractions/gnome>
  
  owner @{HOME}/.mozilla/** rw,
  owner @{HOME}/Downloads/** rw,
  
  # Deny access to SSH keys
  deny @{HOME}/.ssh/** rw,
  
  # Deny access to other sensitive files
  deny @{HOME}/.gnupg/** rw,
  deny @{HOME}/.password-store/** rw,
  
  /usr/lib/firefox/** ix,
  /usr/bin/firefox ix,
}
EOF

# Load profile
apparmor_parser -r /etc/apparmor.d/usr.bin.firefox

# Set to enforce mode
aa-enforce /usr/bin/firefox

# Launch Firefox (now confined)
firefox &
```
{% endcode %}

{% hint style="info" %}
**Profile Development Tip**: Start profiles in **complain mode** to log what the application does without blocking. Review logs, then switch to **enforce mode**.
{% endhint %}
{% endtab %}

{% tab title="⚙️ systemd Sandboxing" %}
**systemd Sandboxing - Service and Process Isolation**

{% hint style="info" %}
**Core Concept**

systemd provides native sandboxing directives that restrict services or user sessions using kernel features (namespaces, capabilities, seccomp, filesystem restrictions).
{% endhint %}

**How It Works**:

systemd unit files include directives that configure isolation:

* **Filesystem**: Control access to directories, make paths read-only
* **Network**: Isolate network namespace
* **User**: Run as dynamic user, prevent privilege escalation
* **Capabilities**: Drop Linux capabilities
* **System Calls**: Filter allowed syscalls with seccomp

{% code title="systemd Sandboxing Directives" overflow="wrap" %}
```ini
[Service]

# Filesystem restrictions
PrivateTmp=yes                      # Private /tmp and /var/tmp
PrivateHome=yes                     # Hide /home, /root, /run/user
ProtectSystem=strict                # Make / read-only
ProtectHome=yes                     # Make /home inaccessible
ReadOnlyPaths=/                     # Make specific paths read-only
InaccessiblePaths=/root             # Make paths completely inaccessible

# User restrictions
DynamicUser=yes                     # Create ephemeral user (UID at runtime)
PrivateUsers=yes                    # User namespace isolation

# Network restrictions
PrivateNetwork=yes                  # Private network namespace (no network)
RestrictAddressFamilies=AF_INET AF_INET6  # Only IPv4/IPv6

# Kernel restrictions
ProtectKernelTunables=yes           # Make /proc and /sys read-only
ProtectKernelModules=yes            # Deny loading kernel modules
ProtectKernelLogs=yes               # Deny reading kernel logs
ProtectControlGroups=yes            # Make cgroup filesystem read-only

# Capability restrictions
NoNewPrivileges=yes                 # Prevent gaining new privileges
CapabilityBoundingSet=CAP_NET_BIND_SERVICE  # Only allow specific capabilities

# System call filtering
SystemCallFilter=@system-service    # Allow only system service calls
SystemCallErrorNumber=EPERM         # Error to return for blocked syscalls
```
{% endcode %}

**Characteristics**:

* **Scope**: Process/service. Controls filesystem, kernel, IPC, and networking on a per-service basis.
* **Strengths**:
  * ✅ Native, kernel-integrated sandboxing
  * ✅ No containers required
  * ✅ Options like PrivateHome, ProtectSystem, DynamicUser
  * ✅ Works with existing services (just edit unit file)
  * ✅ Fine-grained control over system resources
* **Limitations**:
  * ❌ Somewhat advanced configuration
  * ❌ Not retroactive to running user shells (unless specifically set up)
  * ❌ May break services that need broad access
  * ❌ Requires understanding of service dependencies
* **Recent Usage Examples**:
  * Sandboxed background services on Ubuntu, RHEL, SUSE
  * Desktop applications via systemd user units
  * Hardened web servers (nginx, apache)
  * Database services (PostgreSQL, MySQL)

**Sandbox a systemd Service**:

{% stepper %}
{% step %}
#### Create Sandboxed Service

{% code title="/etc/systemd/system/webapp.service" overflow="wrap" lineNumbers="true" %}
```ini
[Unit]
Description=Sandboxed Web Application
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/webapp

# Filesystem isolation
PrivateTmp=yes
PrivateHome=yes
ProtectSystem=strict
ProtectHome=yes
ReadWritePaths=/var/www/webapp    # Only writable path

# User isolation
DynamicUser=yes
User=webapp
Group=webapp

# Network restrictions (web app needs network)
RestrictAddressFamilies=AF_INET AF_INET6
PrivateNetwork=no  # Needs network for web server

# Kernel protections
ProtectKernelTunables=yes
ProtectKernelModules=yes
ProtectKernelLogs=yes
ProtectControlGroups=yes

# Capability restrictions
NoNewPrivileges=yes
CapabilityBoundingSet=CAP_NET_BIND_SERVICE CAP_SETUID CAP_SETGID
AmbientCapabilities=CAP_NET_BIND_SERVICE

# System call filtering
SystemCallFilter=@system-service
SystemCallErrorNumber=EPERM
SystemCallArchitectures=native

# Resource limits
MemoryLimit=512M
CPUQuota=50%

[Install]
WantedBy=multi-user.target
```
{% endcode %}
{% endstep %}

{% step %}
#### Analyze Sandbox Configuration

{% code overflow="wrap" %}
```bash
# Analyze security settings
systemd-analyze security webapp.service

# Output shows security score and recommendations

# Example output:

# PASS: PrivateTmp=yes

# PASS: ProtectSystem=strict

# WARN: User= is not set

# Score: 8.5/10
```
{% endcode %}
{% endstep %}

{% step %}
#### Test Service Isolation

{% code overflow="wrap" lineNumbers="true" %}
```bash
# Start service
systemctl start webapp.service

# Check process isolation
ps aux | grep webapp

# Shows process running as dynamic user

# Try to access filesystem from inside service
systemctl cat webapp.service

# Shows restricted filesystem view

# Check what service can access
systemd-run --unit=test-access \
  --property=PrivateHome=yes \
  --property=ProtectSystem=strict \
  ls /home

# Output: (empty) - /home is hidden
```
{% endcode %}
{% endstep %}
{% endstepper %}

**Sandbox User Session**:

{% code title="~/.config/systemd/user/sandboxed-app.service" overflow="wrap" lineNumbers="true" %}
```ini
[Unit]
Description=Sandboxed User Application

[Service]
ExecStart=/usr/bin/myapp

# Filesystem restrictions
PrivateTmp=yes
ProtectSystem=strict
ReadWritePaths=%h/Documents/myapp-data  # Only allow writing here

# Network restrictions
PrivateNetwork=yes  # No network access at all

# Kernel protections
ProtectKernelTunables=yes
ProtectKernelModules=yes

# Prevent privilege escalation
NoNewPrivileges=yes

[Install]
WantedBy=default.target
```
{% endcode %}

Start as user:

```bash
systemctl --user daemon-reload
systemctl --user start sandboxed-app.service
```

{% hint style="success" %}
**Best Practice**: Use `systemd-analyze security` to assess and improve service security posture. Aim for high security scores (8+/10).
{% endhint %}

<details>

<summary>🔐 Advanced systemd Sandboxing Options</summary>

**Additional Restrictions**:

```ini
[Service]

# Deny specific system calls
SystemCallFilter=~@clock @mount @swap @reboot

# ~@ means deny this syscall group

# Restrict namespaces
RestrictNamespaces=yes
RestrictNamespaces=~user net  # Deny user and net namespaces

# Lock down hostname
ProtectHostname=yes

# Prevent access to real-time clock
ProtectClock=yes

# Restrict address families more strictly
RestrictAddressFamilies=none  # No network at all

# Lock personality (prevent 32-bit execution on 64-bit)
LockPersonality=yes

# Deny execution of memory-mapped files
MemoryDenyWriteExecute=yes  # W^X protection

# Remove access to /proc
ProcSubset=pid  # Only see own process in /proc

# Make /dev private
PrivateDevices=yes  # Minimal /dev with only pseudo-devices
```

**Systemd Syscall Groups**:

* `@system-service` - Common for system services
* `@file-system` - Filesystem operations
* `@network-io` - Network operations
* `@ipc` - Inter-process communication
* `@privileged` - Privileged operations
* `@resources` - Resource management
* `@mount` - Mounting filesystems
* `@reboot` - System reboot/shutdown

</details>
{% endtab %}

{% tab title="🔬 Firejail/Bubblewrap/Nsjail" %}
**Firejail, Bubblewrap, Nsjail - Namespace-Based Sandboxes**

{% hint style="info" %}
**Core Concept**

These tools sandbox user shells or applications using Linux namespaces, seccomp filters, and other kernel features. They provide flexible, on-demand isolation for individual processes.
{% endhint %}

**Comparison**:

| Tool           | SUID Required     | Complexity | Primary Use Case                  |
| -------------- | ----------------- | ---------- | --------------------------------- |
| **Firejail**   | Yes (SUID binary) | Low        | Desktop app sandboxing            |
| **Bubblewrap** | No (unprivileged) | Medium     | Flatpak backend, custom sandboxes |
| **Nsjail**     | No                | High       | Security competitions, testing    |

**Characteristics**:

* **Scope**: Process/application
* **Strengths**:
  * ✅ Flexible; can be invoked on a per-app basis
  * ✅ Unprivileged user sandboxing (Bubblewrap/Nsjail)
  * ✅ Strong isolation capabilities
  * ✅ Can sandbox untrusted code quickly
  * ✅ Good for testing and development
* **Limitations**:
  * ❌ Firejail's SUID may introduce risks
  * ❌ Bubblewrap/Nsjail require explicit configuration
  * ❌ Typically focused on single processes/apps (not entire user sessions)
  * ❌ May break applications expecting full system access
* **Recent Usage Examples**:
  * Isolating browsers, communication apps
  * Sandboxes for CLI experimentation
  * Security competitions (CTF challenges)
  * Development environments

**Firejail - Easy Desktop Sandboxing**:

{% stepper %}
{% step %}
#### Install Firejail

{% code overflow="wrap" %}
```bash
# RHEL/Fedora
dnf install firejail

# Ubuntu/Debian
apt install firejail

# Verify installation
firejail --version
```
{% endcode %}
{% endstep %}

{% step %}
#### Sandbox Firefox

{% code overflow="wrap" lineNumbers="true" %}
```bash
# Run Firefox in sandbox
firejail firefox

# Check what's isolated

# - Private /tmp

# - Limited /home access (profile-dependent)

# - Filtered syscalls (seccomp)

# - Network namespace options

# Check sandbox status
firejail --list
PID  User  Sandbox
1234 alice /usr/bin/firefox

# Check what file paths are accessible inside sandbox
firejail --tree
└─1234:alice:/usr/bin/firefox
  ├─/home/alice/.mozilla  (read-write)
  ├─/home/alice/Downloads (read-write)
  └─(rest of /home blocked)
```
{% endcode %}
{% endstep %}

{% step %}
#### Create Custom Profile

{% code title="~/.config/firejail/custom-app.profile" overflow="wrap" lineNumbers="true" %}
```bash
# Include base restrictions
include /etc/firejail/default.profile

# Private /tmp
private-tmp

# Whitelist specific directories
whitelist ${HOME}/Documents/work
whitelist ${HOME}/Downloads

# Blacklist sensitive files
blacklist ${HOME}/.ssh
blacklist ${HOME}/.gnupg

# Disable network
net none

# Restrict system calls
seccomp

# Drop capabilities
caps.drop all

# Run as non-root
noroot

# Disable X11 if not needed

# x11 none
```
{% endcode %}

Use profile:

```bash
firejail --profile=~/.config/firejail/custom-app.profile /path/to/app
```
{% endstep %}
{% endstepper %}

**Bubblewrap - Unprivileged Containers**:

{% code title="Sandbox Shell with Bubblewrap" overflow="wrap" lineNumbers="true" %}
```bash
# Install bubblewrap
dnf install bubblewrap  # or apt install bubblewrap

# Create minimal sandbox
bwrap \
  --ro-bind /usr /usr \
  --ro-bind /lib /lib \
  --ro-bind /lib64 /lib64 \
  --ro-bind /bin /bin \
  --ro-bind /sbin /sbin \
  --dir /tmp \
  --dir /var \
  --proc /proc \
  --dev /dev \
  --ro-bind /etc/resolv.conf /etc/resolv.conf \
  --ro-bind /etc/passwd /etc/passwd \
  --ro-bind /etc/group /etc/group \
  --bind ~/sandbox-home /home/user \
  --unshare-all \
  --share-net \
  --hostname sandbox \
  --die-with-parent \
  --new-session \
  /bin/bash

# Inside sandbox:

# - Root filesystem read-only

# - Private /tmp

# - Custom /home

# - All namespaces unshared (except network)

# - Cannot see other processes

# - Cannot access /home outside sandbox-home
```
{% endcode %}

**Nsjail - Advanced Isolation**:

{% code title="Nsjail Configuration" overflow="wrap" lineNumbers="true" %}
```bash
# nsjail.cfg
name: "sandbox"

mode: ONCE
hostname: "sandbox"

cwd: "/home/user"

time_limit: 600

# Mount points
mount {
  src: "/bin"
  dst: "/bin"
  is_bind: true
  rw: false
}

mount {
  src: "/usr"
  dst: "/usr"
  is_bind: true
  rw: false
}

mount {
  src: "/lib"
  dst: "/lib"
  is_bind: true
  rw: false
}

mount {
  src: "/lib64"
  dst: "/lib64"
  is_bind: true
  rw: false
}

mount {
  dst: "/tmp"
  fstype: "tmpfs"
  rw: true
}

mount {
  src: "/home/user/sandbox"
  dst: "/home/user"
  is_bind: true
  rw: true
}

# Resource limits
rlimit_as: 512
rlimit_cpu: 1000
rlimit_fsize: 1024
rlimit_nofile: 32

# Security
clone_newnet: true
clone_newuser: true
clone_newns: true
clone_newpid: true
clone_newipc: true
clone_newuts: true

# Run as specific user inside
inside_uid: 1000
inside_gid: 1000

exec_bin {
  path: "/bin/bash"
}
```
{% endcode %}

Run:

```bash
nsjail --config nsjail.cfg
```

{% hint style="warning" %}
**Firejail SUID Warning**: Firejail runs as a SUID binary, which has historically had security vulnerabilities. Consider Bubblewrap for unprivileged sandboxing or disable Firejail SUID if possible.
{% endhint %}
{% endtab %}

{% tab title="📦 Podman Rootless Container" %}
**Podman Rootless Container - User Namespace Isolation**

{% hint style="info" %}
**Core Concept**

Podman runs user environments or applications in rootless containers using user namespaces. Containers cannot escalate privileges on the host, providing strong isolation.
{% endhint %}

**How It Works**:

Podman uses **user namespaces** to map container UIDs to unprivileged host UIDs:

* Container UID 0 (root) → Host UID 100000 (unprivileged)
* Container UID 1 → Host UID 100001
* Etc.

This means even if a process inside the container becomes root, it has no privileges on the host.

{% code title="User Namespace UID Mapping" overflow="wrap" %}
```bash
# View UID mapping for rootless podman
cat /etc/subuid
alice:100000:65536

# Container UID 0 maps to host UID 100000

# Container UID 1 maps to host UID 100001

# Container UID 65535 maps to host UID 165535

# Host UID alice (1000) appears as nobody inside container
```
{% endcode %}

**Characteristics**:

* **Scope**: Container-level (process, network, filesystem within container)
* **Strengths**:
  * ✅ Rootless—containers can't escalate privileges on host
  * ✅ Tight integration with SELinux or AppArmor
  * ✅ Compatible with Docker images
  * ✅ Multi-tenant safe (users can't interfere with each other)
  * ✅ No daemon required (unlike Docker)
* **Limitations**:
  * ❌ Not for interactive user logins (not a replacement for login shells)
  * ❌ Mostly for running isolated workloads/workspaces
  * ❌ Some limitations on networking (no privileged port binding <1024)
  * ❌ May require subuid/subgid configuration
* **Recent Usage Examples**:
  * Multi-user secure app hosting with Podman rootless on RHEL, Fedora, Ubuntu, SUSE
  * Development environments (per-project containers)
  * CI/CD pipelines (build in isolated containers)
  * Desktop containerized applications

**Run Rootless Container**:

{% stepper %}
{% step %}
#### Setup Rootless Podman

{% code overflow="wrap" lineNumbers="true" %}
```bash
# Install Podman
dnf install podman  # RHEL/Fedora

# apt install podman  # Ubuntu/Debian

# Verify subuid/subgid ranges assigned
cat /etc/subuid
alice:100000:65536

cat /etc/subgid
alice:100000:65536

# If not present, add:
usermod --add-subuids 100000-165535 alice
usermod --add-subgids 100000-165535 alice

# Test rootless podman
podman run --rm -it fedora:latest echo "Hello from container"
```
{% endcode %}
{% endstep %}

{% step %}
#### Run Persistent Development Environment

{% code overflow="wrap" lineNumbers="true" %}
```bash
# Create persistent container for development
podman run -d \
  --name devenv \
  --userns=keep-id \
  -v ~/projects:/workspace:Z \
  -p 8080:8080 \
  fedora:latest \
  sleep infinity

# Execute commands in container
podman exec -it devenv bash

# Inside container:

# Install development tools
dnf install -y git vim python3 nodejs

# Work on projects
cd /workspace

# Files persist in ~/projects on host

# Stop container
podman stop devenv

# Start again later
podman start devenv
```
{% endcode %}
{% endstep %}

{% step %}
#### Multi-User Isolation

{% code overflow="wrap" %}
```bash
# User alice runs container
alice$ podman run -d --name alice-app nginx

# User bob runs container
bob$ podman run -d --name bob-app nginx

# Containers completely isolated:

# - Separate user namespaces

# - Separate network namespaces

# - Cannot see each other's containers

# - Cannot interfere with each other

# alice cannot see bob's container
alice$ podman ps -a
CONTAINER ID  IMAGE  COMMAND  CREATED  STATUS  NAMES
abc123        nginx           Running  alice-app

# bob cannot see alice's container
bob$ podman ps -a
CONTAINER ID  IMAGE  COMMAND  CREATED  STATUS  NAMES
def456        nginx           Running  bob-app
```
{% endcode %}
{% endstep %}
{% endstepper %}

**Systemd Integration - Container as Service**:

{% code title="~/.config/systemd/user/container-app.service" overflow="wrap" lineNumbers="true" %}
```ini
[Unit]
Description=Rootless Container Application
After=network.target

[Service]
Type=simple
Restart=always

# Generate unit file from existing container

# podman generate systemd --new --name container-name

ExecStart=/usr/bin/podman run \
  --rm \
  --name myapp \
  --userns=keep-id \
  -v %h/data:/data:Z \
  -p 8080:8080 \
  myapp:latest

ExecStop=/usr/bin/podman stop -t 10 myapp

[Install]
WantedBy=default.target
```
{% endcode %}

Enable:

```bash
systemctl --user daemon-reload
systemctl --user enable --now container-app.service
systemctl --user status container-app.service
```

{% hint style="success" %}
**Rootless Security**: Container root cannot become host root. Even if attacker gains root inside container, they have no privileges on the host system.
{% endhint %}
{% endtab %}

{% tab title="📱 Snap/Flatpak Sandboxing" %}
**Snap/Flatpak Application Sandboxing**

{% hint style="info" %}
**Core Concept**

Snap (Ubuntu) and Flatpak (Fedora/others) use LSM (Linux Security Modules) and container features for application sandboxing. They provide fine-grained controls via portals and clear permission prompts.
{% endhint %}

**How They Work**:

Both systems package applications with their dependencies and run them in isolated environments:

* **Snap**: Uses AppArmor for confinement, snapd daemon for management
* **Flatpak**: Uses Bubblewrap for namespace isolation, portals for host access

{% code title="Isolation Features" overflow="wrap" %}
```
Application Sandbox:
├─ Filesystem: Limited access, no /home by default
├─ Network: Controlled via permissions
├─ Display: Isolated X11/Wayland
├─ Audio: Portal-based access
├─ Files: User must explicitly grant access
└─ Devices: Controlled access (webcam, USB, etc.)
```
{% endcode %}

**Characteristics**:

* **Scope**: Application. Only affects packaged apps, not user shells.
* **Strengths**:
  * ✅ Fine-grained controls via portals and LSM
  * ✅ Clear permission prompts (similar to mobile apps)
  * ✅ Snap: AppArmor, Flatpak: Bubblewrap namespace
  * ✅ Easy to install sandboxed apps
  * ✅ Automatic updates with security fixes
* **Limitations**:
  * ❌ Only relevant to packaged GUI apps
  * ❌ Underlying system/users not confined
  * ❌ Some apps need broad permissions (IDE, terminal)
  * ❌ Classic snaps (non-confined) bypass sandboxing
* **Recent Usage Examples**:
  * Desktop app isolation on Ubuntu (Snap), Fedora (Flatpak)
  * Browser sandboxing (Chrome, Firefox as Snap/Flatpak)
  * Communication apps (Slack, Discord, Teams)

**Snap Confinement Levels**:

| Confinement | Isolation                          | Use Case                 |
| ----------- | ---------------------------------- | ------------------------ |
| **strict**  | Full isolation, AppArmor enforced  | Most applications        |
| **classic** | No confinement, full system access | Development tools (IDEs) |
| **devmode** | Debugging mode, violations logged  | Development/testing      |

**Install and Manage Snap Applications**:

{% stepper %}
{% step %}
#### Install Snap Application

{% code overflow="wrap" %}
```bash
# Install snap daemon (if not present)
apt install snapd  # Ubuntu
dnf install snapd  # Fedora

# Search for applications
snap find firefox

# Install application
snap install firefox

# View installed snaps
snap list

# Check application confinement
snap connections firefox

# Shows interfaces connected and their permissions
```
{% endcode %}
{% endstep %}

{% step %}
#### Manage Snap Permissions

{% code overflow="wrap" lineNumbers="true" %}
```bash
# View available interfaces
snap interfaces

# Grant permission (interface connection)
snap connect firefox:camera

# Allows Firefox to access webcam

# Revoke permission
snap disconnect firefox:camera

# Check what an app can access
snap connections firefox
Interface        Plug                  Slot
network          firefox:network       :network
home             firefox:home          :home
camera           firefox:camera        -  (disconnected)

# View AppArmor profile
cat /var/lib/snapd/apparmor/profiles/snap.firefox.*
```
{% endcode %}
{% endstep %}
{% endstepper %}

**Flatpak Permissions**:

{% stepper %}
{% step %}
#### Install Flatpak Application

{% code overflow="wrap" %}
```bash
# Install Flatpak
dnf install flatpak  # Fedora
apt install flatpak  # Ubuntu

# Add Flathub repository
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Install application
flatpak install flathub org.mozilla.firefox

# Run application
flatpak run org.mozilla.firefox
```
{% endcode %}
{% endstep %}

{% step %}
#### Manage Flatpak Permissions

{% code overflow="wrap" lineNumbers="true" %}
```bash
# View application permissions
flatpak info --show-permissions org.mozilla.firefox

# Override permissions with Flatseal (GUI tool)
flatpak install flathub com.github.tchx84.Flatseal
flatpak run com.github.tchx84.Flatseal

# Or use command line

# Grant filesystem access
flatpak override --user --filesystem=~/Downloads org.mozilla.firefox

# Revoke network access
flatpak override --user --unshare=network org.mozilla.firefox

# View overrides
flatpak override --user --show org.mozilla.firefox
```
{% endcode %}
{% endstep %}
{% endstepper %}

**Permission Examples**:

| Permission         | Snap                       | Flatpak                    |
| ------------------ | -------------------------- | -------------------------- |
| **Home directory** | `home` interface           | `--filesystem=home`        |
| **Network access** | `network` interface        | `--share=network`          |
| **Webcam**         | `camera` interface         | `--device=all`             |
| **Audio**          | `audio-playback` interface | `--device=dri` (for audio) |
| **X11 display**    | `x11` interface            | `--socket=x11`             |
| **Wayland**        | `wayland` interface        | `--socket=wayland`         |

{% hint style="info" %}
**User Experience**: Snap and Flatpak make sandboxing accessible to desktop users. Permission prompts similar to mobile apps help users understand what applications can access.
{% endhint %}
{% endtab %}

{% tab title="🔑 PAM Modules" %}
**PAM Modules for Session Restriction**

{% hint style="info" %}
**Core Concept**

PAM (Pluggable Authentication Modules) can apply access controls and security policies during authentication and session setup. Modules like `pam_apparmor` and `pam_selinux` integrate with LSMs to restrict user sessions.
{% endhint %}

**How It Works**:

PAM modules are inserted into the authentication stack:

{% code title="/etc/pam.d/login" overflow="wrap" lineNumbers="true" %}
```bash
# Authentication
auth       required     pam_unix.so
auth       required     pam_faillock.so

# Account verification
account    required     pam_unix.so
account    required     pam_time.so

# Session setup - Apply LSM policy
session    required     pam_selinux.so open
session    optional     pam_apparmor.so order=user,group,default
session    required     pam_unix.so
session    required     pam_selinux.so close
```
{% endcode %}

**Characteristics**:

* **Scope**: Auth/session. Controls what resources sessions may access, or applies AppArmor/SELinux on login.
* **Strengths**:
  * ✅ Tied to authentication (automatic application)
  * ✅ Integrates with system login
  * ✅ Can enforce access control in multi-user environments
  * ✅ Works with existing user management
* **Limitations**:
  * ❌ Does not provide filesystem or process isolation by itself
  * ❌ Depends on other mechanisms (SELinux, AppArmor) for stronger jailing
  * ❌ Configuration errors can lock out users
  * ❌ Complex to troubleshoot when misconfigured
* **Recent Usage Examples**:
  * SUSE with `pam_apparmor`
  * RHEL with `pam_selinux`
  * Access restrictions via PAM policies (time-based, group-based)

**Common PAM Restriction Modules**:

| PAM Module        | Purpose                         | Configuration                  |
| ----------------- | ------------------------------- | ------------------------------ |
| **pam\_selinux**  | Apply SELinux context on login  | `/etc/selinux/...` policies    |
| **pam\_apparmor** | Apply AppArmor profile on login | `/etc/security/apparmor.d/...` |
| **pam\_time**     | Time-based access restrictions  | `/etc/security/time.conf`      |
| **pam\_access**   | Host/user/group access control  | `/etc/security/access.conf`    |
| **pam\_limits**   | Resource limits                 | `/etc/security/limits.conf`    |
| **pam\_exec**     | Execute custom scripts          | Inline in PAM config           |

**Restrict Login Times**:

{% code title="/etc/security/time.conf" overflow="wrap" lineNumbers="true" %}
```bash
# Allow alice to login only during business hours
login;*;alice;Wk0800-1700

# Deny bob from login on weekends
login;*;bob;!Wk

# Allow devs group only during core hours
login;*;@developers;Al0900-1700
```
{% endcode %}

Enable in PAM:

{% code title="/etc/pam.d/login" overflow="wrap" %}
```bash
account    required     pam_time.so
```
{% endcode %}

**Restrict Access by Host/Group**:

{% code title="/etc/security/access.conf" overflow="wrap" lineNumbers="true" %}
```bash
# Format: permission:users:origins

# Deny all except wheel group
-:ALL EXCEPT wheel:ALL

# Allow admin from specific network
+:admin:192.168.1.0/24

# Deny user bob from all origins
-:bob:ALL

# Allow developers from specific hosts
+:@developers:dev1.example.com dev2.example.com
```
{% endcode %}

Enable in PAM:

{% code title="/etc/pam.d/login" overflow="wrap" %}
```bash
account    required     pam_access.so
```
{% endcode %}

**Execute Custom Script on Login**:

{% code title="/etc/pam.d/sshd" overflow="wrap" lineNumbers="true" %}
```bash
# Execute script on successful login
session    optional     pam_exec.so /usr/local/bin/login-notify.sh

# Example script: /usr/local/bin/login-notify.sh
#!/bin/bash
echo "User $PAM_USER logged in from $PAM_RHOST at $(date)" >> /var/log/custom-logins.log
```
{% endcode %}

{% hint style="warning" %}
**Configuration Care**: PAM configuration errors can lock all users out of the system. Always keep a root shell open when testing PAM changes, and test in a VM first.
{% endhint %}
{% endtab %}

{% tab title="📊 Resource Quotas/cgroups" %}
**Resource Quotas and cgroups - Resource Limitation**

{% hint style="info" %}
**Core Concept**

cgroups (control groups) limit user or group resource consumption (CPU, memory, I/O, network). This prevents denial-of-service and resource hogging in multi-user environments.
{% endhint %}

**How It Works**:

cgroups organize processes into hierarchical groups and apply resource controllers:

{% code title="cgroup Hierarchy" overflow="wrap" %}
```
/sys/fs/cgroup/
├─ system.slice/          # System services
│  ├─ sshd.service
│  └─ systemd-journald.service
├─ user.slice/            # User sessions
│  ├─ user-1000.slice/    # User alice
│  │  ├─ session-1.scope  # Login session 1
│  │  └─ session-2.scope  # Login session 2
│  └─ user-1001.slice/    # User bob
│     └─ session-3.scope
└─ machine.slice/         # Virtual machines/containers
```
{% endcode %}

**Resource Controllers**:

* **cpu**: CPU time allocation
* **cpuset**: CPU core affinity
* **memory**: Memory usage limits
* **io** (blkio): Block I/O bandwidth
* **pids**: Maximum number of processes
* **net\_cls/net\_prio**: Network bandwidth/priority

**Characteristics**:

* **Scope**: Resources only
* **Strengths**:
  * ✅ Prevents DoS and resource hogging
  * ✅ Granular limits via systemd slices
  * ✅ Fair resource sharing in multi-user environments
  * ✅ Real-time monitoring of resource usage
  * ✅ Hierarchical allocation
* **Limitations**:
  * ❌ No filesystem or process isolation
  * ❌ Doesn't prevent access, only limits resource use
  * ❌ Requires understanding of resource controllers
  * ❌ May impact legitimate workloads if misconfigured
* **Recent Usage Examples**:
  * systemd service/user slices to cap compute/memory/network on RHEL, Ubuntu, SUSE
  * Kubernetes resource limits (based on cgroups)
  * Multi-tenant compute environments

**Limit User Resources with systemd**:

{% stepper %}
{% step %}
#### Create Slice for Users

{% code title="/etc/systemd/system/user-limited.slice" overflow="wrap" lineNumbers="true" %}
```ini
[Unit]
Description=Limited Resources for Specific Users
Before=slices.target

[Slice]

# CPU quota: 50% of one CPU core
CPUQuota=50%

# Memory limit: 2GB
MemoryMax=2G
MemoryHigh=1800M

# Maximum tasks (processes/threads)
TasksMax=100

# I/O weight (lower = less priority)
IOWeight=100

# Control group accounting
CPUAccounting=yes
MemoryAccounting=yes
TasksAccounting=yes
IOAccounting=yes
```
{% endcode %}

Enable:

```bash
systemctl daemon-reload
```
{% endstep %}

{% step %}
#### Assign Users to Slice

{% code overflow="wrap" lineNumbers="true" %}
```bash
# Create drop-in directory
mkdir -p /etc/systemd/system/user@.service.d/

# Create override for specific user UID
cat > /etc/systemd/system/user@1000.service.d/slice.conf << EOF
[Service]
Slice=user-limited.slice
EOF

# Reload systemd
systemctl daemon-reload

# When user 1000 (alice) logs in:

# Her session automatically joins user-limited.slice

# All her processes inherit the resource limits
```
{% endcode %}
{% endstep %}

{% step %}
#### Monitor Resource Usage

{% code overflow="wrap" %}
```bash
# View resource usage by slice
systemctl status user-limited.slice

# Detailed cgroup information
systemd-cgtop

# View specific user slice
systemctl status user-1000.slice

# Check if limits are being hit
journalctl -u user@1000.service | grep -i "limit\|oom"
```
{% endcode %}
{% endstep %}
{% endstepper %}

**Per-Service Resource Limits**:

{% code title="/etc/systemd/system/webapp.service" overflow="wrap" lineNumbers="true" %}
```ini
[Service]
ExecStart=/usr/local/bin/webapp

# CPU limits
CPUQuota=200%         # 2 full CPU cores maximum
CPUWeight=100         # Relative weight for CPU scheduling

# Memory limits
MemoryMax=1G          # Hard limit (OOM kill if exceeded)
MemoryHigh=900M       # Soft limit (throttle if exceeded)
MemorySwapMax=0       # Disable swap for this service

# Task limits
TasksMax=50           # Maximum 50 processes/threads

# I/O limits
IOWeight=100          # I/O scheduling weight
IOReadBandwidthMax=/dev/sda 10M   # 10 MB/s read limit
IOWriteBandwidthMax=/dev/sda 5M   # 5 MB/s write limit

# Enable accounting
CPUAccounting=yes
MemoryAccounting=yes
TasksAccounting=yes
IOAccounting=yes
```
{% endcode %}

**Traditional ulimit (Legacy)**:

{% code title="/etc/security/limits.conf" overflow="wrap" lineNumbers="true" %}
```bash
# Format: domain type item value

# domain: username, @group, *

# type: soft, hard, -

# item: nofile, nproc, cpu, memlock, etc.

# Limit max open files
*           soft    nofile      4096
*           hard    nofile      65536

# Limit max processes for regular users
@users      soft    nproc       100
@users      hard    nproc       200

# Limit CPU time (minutes)
@students   hard    cpu         60

# Limit memory (KB)
@guests     hard    memlock     65536

# No core dumps for users
*           hard    core        0
```
{% endcode %}

Apply:

```bash
# ulimits apply at next login

# Current user won't see changes until re-login

# Check current limits
ulimit -a
```

{% hint style="info" %}
**Modern Approach**: systemd cgroups are preferred over ulimits for resource control. They provide more granular control and better integration with modern Linux systems.
{% endhint %}

<details>

<summary>📈 Resource Controller Details</summary>

**CPU Controller**:

* `CPUQuota=` - Percentage of CPU time (100% = 1 core)
* `CPUWeight=` - Relative weight for CPU scheduling (100 = default)
* `CPUAccounting=` - Enable CPU usage tracking

**Memory Controller**:

* `MemoryMax=` - Hard limit (OOM kill if exceeded)
* `MemoryHigh=` - Soft limit (throttle if exceeded)
* `MemorySwapMax=` - Maximum swap usage
* `MemoryAccounting=` - Enable memory usage tracking

**I/O Controller**:

* `IOWeight=` - Relative weight for I/O scheduling
* `IOReadBandwidthMax=` - Read bandwidth limit per device
* `IOWriteBandwidthMax=` - Write bandwidth limit per device
* `IOAccounting=` - Enable I/O usage tracking

**Task Controller**:

* `TasksMax=` - Maximum number of tasks (processes/threads)
* `TasksAccounting=` - Enable task counting

**Best Practices**:

* Set `MemoryHigh` to 90% of `MemoryMax` for graceful throttling
* Use `CPUWeight` for proportional CPU sharing
* Enable accounting for monitoring
* Test limits with realistic workloads

</details>
{% endtab %}
{% endtabs %}

***

## 2️⃣ Real-World Jailing Examples

{% hint style="success" %}
**Personal Experience: Interactive Lab Environments**
{% endhint %}

### Killercoda Sandbox Environments

> When I use the Killercoda environments, I am confined and jailed to only the sandbox environments in order to complete the exercises.

**What's Happening Behind the Scenes**:

Killercoda uses **container-based isolation** (likely Kubernetes pods or similar) combined with multiple restriction layers:

{% stepper %}
{% step %}
#### Container Isolation

Each user gets their own isolated container with:

* Private filesystem (no access to other users' environments)
* Private network namespace (cannot interfere with other users)
* Resource limits (CPU, memory quotas via cgroups)
* Time-limited sessions (containers destroyed after timeout)

{% code title="Similar to:" overflow="wrap" %}
```bash
# Kubernetes pod-based isolation
kubectl run killercoda-session-abc123 \
  --image=killercoda/ubuntu-lab:latest \
  --limits="cpu=1,memory=2Gi" \
  --restart=Never \
  --command -- /bin/bash

# User connects to this specific pod

# Cannot see or access other pods

# Pod deleted after session ends
```
{% endcode %}
{% endstep %}

{% step %}
#### Read-Only Filesystem

Base system is read-only; user can only write to specific directories:

* `/tmp` - Temporary scratch space
* `/workspace` - User's working directory
* Everything else read-only or inaccessible

**Prevents**:

* Tampering with system binaries
* Installing persistent malware
* Breaking the environment for the next user
{% endstep %}

{% step %}
#### Network Restrictions

Carefully controlled network access:

* Allow: Specific exercise resources (APIs, databases for labs)
* Deny: Other users' containers
* Rate-limited: Prevent abuse
* Monitored: Detect malicious activity

**Implemented via**:

* Kubernetes NetworkPolicies
* iptables/nftables rules
* Service mesh (Istio, Linkerd)
{% endstep %}

{% step %}
#### Capability Restrictions

Stripped-down privileges inside container:

* Cannot load kernel modules
* Cannot mount filesystems
* Cannot change system time
* Cannot access raw sockets (unless exercise requires)

**Using**:

* Dropped Linux capabilities
* seccomp profiles (blocked syscalls)
* AppArmor/SELinux policies
{% endstep %}
{% endstepper %}

**Security Architecture**:

{% code title="Killercoda-Style Architecture" overflow="wrap" %}
```
User Browser
    ↓
Load Balancer (Rate Limiting)
    ↓
Kubernetes Cluster
    ↓
Dedicated Pod per User
├─ Container: Ubuntu Lab Image
│  ├─ Filesystem: Read-only + /tmp + /workspace
│  ├─ Network: NetworkPolicy restrictions
│  ├─ Resources: CPU/Memory limits (cgroups)
│  ├─ Security: seccomp + capabilities dropped
│  └─ Lifetime: Auto-delete after timeout
└─ User sees only their environment
   • Cannot escape container
   • Cannot see other users
   • Cannot persist malicious changes
   • Clean slate for next user
```
{% endcode %}

### Red Hat Instruqt Labs

> Also in web apps that pose interactive lab environments, like in Red Hat's Instruqt labs.

**Similar but Enhanced**:

Red Hat Instruqt uses **multi-VM environments** with orchestration:

{% stepper %}
{% step %}
#### Multiple VMs per Lab

Complex scenarios require multiple systems:

* Bastion host VM
* Application server VM
* Database server VM
* Load balancer VM

All isolated per user, orchestrated as a single "lab instance"
{% endstep %}

{% step %}
#### Nested Virtualization

For OpenShift labs:

* Outer VM (hypervisor)
* Inner VMs/containers (OpenShift cluster)
* User confined to this nested environment

**Isolation Layers**:

1. Cloud provider isolation (AWS, GCP, Azure)
2. VM isolation (KVM, QEMU)
3. Container isolation (Kubernetes, OpenShift)
4. User permissions (RBAC, namespaces)
{% endstep %}

{% step %}
#### Automated Provisioning and Teardown

* Lab environment created on-demand (IaC - Terraform, CloudFormation)
* User gets temporary access (time-limited credentials)
* Environment completely destroyed after session
* Zero persistence between users

**Benefits**:

* Clean environment every time
* No contamination from previous users
* Cost-efficient (pay only for active labs)
* Scalable (spin up thousands of labs)
{% endstep %}
{% endstepper %}

### Common Use Cases for Jailing

**Where You've Likely Been Jailed (Unknowingly)**:

| Environment                        | Jailing Method           | Purpose                          |
| ---------------------------------- | ------------------------ | -------------------------------- |
| **Shared Hosting**                 | chroot + resource limits | Isolate websites from each other |
| **Online Code Editors**            | Containers + seccomp     | Safe code execution              |
| **CTF Competitions**               | Nsjail + namespaces      | Isolated challenge environments  |
| **University Labs**                | SELinux + quotas         | Multi-user compute access        |
| **Git Forge SSH** (GitHub, GitLab) | chroot + forced commands | Restrict to Git operations only  |
| **SFTP File Servers**              | chroot                   | Restrict to specific directories |
| **Cloud Shell** (AWS, GCP, Azure)  | Containers               | Temporary admin environment      |
| **Jupyter Hub**                    | Container per user       | Data science notebooks           |

### Useful Ways to Use Jails

**Practical Applications**:

{% tabs %}
{% tab title="🔒 Security Isolation" %}
**Isolate Untrusted Code**:

* Run downloaded scripts in Firejail
* Test potentially malicious binaries in containers
* Analyze malware in isolated VM/container

**Example**:

```bash
# Test untrusted script safely
firejail --net=none --private /path/to/untrusted-script.sh

# Any malicious actions contained to sandbox
```
{% endtab %}

{% tab title="👥 Multi-User Systems" %}
**Share Server Among Users**:

* University compute servers (SELinux + quotas)
* Shared development environments (Podman per user)
* Multi-tenant hosting (chroot + cgroups)

**Benefits**:

* Users can't interfere with each other
* Fair resource sharing
* Prevent one user consuming all resources
* Isolation for security and stability
{% endtab %}

{% tab title="🧪 Development & Testing" %}
**Isolated Development Environments**:

* Per-project containers (different language versions)
* Test in clean environment (no system contamination)
* Reproducible builds (containerized toolchains)

**Example**:

```bash
# Project 1: Python 3.8
podman run -it --rm -v ~/project1:/workspace python:3.8 bash

# Project 2: Python 3.12
podman run -it --rm -v ~/project2:/workspace python:3.12 bash

# No conflicts between versions
```
{% endtab %}

{% tab title="🌐 Web Service Sandboxing" %}
**Confine Web Applications**:

* systemd sandboxing for web servers
* Container-based app isolation
* Prevent compromised app from attacking system

**Example**:

```ini
[Service]

# Sandboxed nginx
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
ReadWritePaths=/var/www/html
NoNewPrivileges=yes
```
{% endtab %}

{% tab title="🎓 Education & Training" %}
**Hands-On Labs**:

* Killercoda-style interactive tutorials
* CTF competition challenges
* Safe environment for learning destructive commands

**Benefits**:

* Students can break things without consequences
* Learn recovery procedures safely
* Realistic but isolated environment
{% endtab %}

{% tab title="🔐 Compliance & Audit" %}
**Meet Regulatory Requirements**:

* SELinux for government/military (mandatory access control)
* Container isolation for PCI DSS (payment systems)
* User confinement for HIPAA (healthcare data)

**Audit Benefits**:

* Demonstrate technical controls
* Prove user separation
* Log confined user actions
{% endtab %}
{% endtabs %}

***

## 📊 Comprehensive Comparison Matrix

| Method           | Filesystem | Process | Network | User/IPC | Resource | Capabilities | Complexity | Best Use Case                       |
| ---------------- | ---------- | ------- | ------- | -------- | -------- | ------------ | ---------- | ----------------------------------- |
| **chroot**       | ✅          | ❌       | ❌       | ❌        | ❌        | ❌            | Low        | SFTP, simple file restriction       |
| **SELinux**      | ✅          | ✅       | ✅       | ✅        | ❌        | ✅            | High       | Enterprise mandatory access control |
| **AppArmor**     | ✅          | ✅       | ✅       | ⚠️       | ❌        | ✅            | Medium     | Desktop app sandboxing              |
| **systemd**      | ✅          | ✅       | ✅       | ✅        | ✅        | ✅            | Medium     | Service sandboxing                  |
| **Firejail**     | ✅          | ✅       | ✅       | ✅        | ⚠️       | ✅            | Low        | Desktop app isolation               |
| **Bubblewrap**   | ✅          | ✅       | ✅       | ✅        | ❌        | ✅            | Medium     | Unprivileged sandboxes              |
| **Nsjail**       | ✅          | ✅       | ✅       | ✅        | ✅        | ✅            | High       | Security competitions               |
| **Podman**       | ✅          | ✅       | ✅       | ✅        | ✅        | ✅            | Medium     | Multi-tenant workloads              |
| **Snap/Flatpak** | ✅          | ✅       | ✅       | ✅        | ❌        | ✅            | Low        | Desktop app packaging               |
| **PAM**          | ❌          | ❌       | ❌       | ❌        | ❌        | ❌            | Low        | Access control policies             |
| **cgroups**      | ❌          | ❌       | ❌       | ❌        | ✅        | ❌            | Low        | Resource limiting                   |

**Legend**:

* ✅ Full support
* ⚠️ Partial support
* ❌ No support

***

## 📝 Key Takeaways

{% hint style="success" %}
**Summary: User and Process Jailing in Linux**

**Core Concepts**

**User Jailing**: Techniques that restrict a user's or process's access to system resources, implementing:

* **Isolation** - Separate from sensitive resources
* **Least Privilege** - Minimum access required
* **Defense in Depth** - Multiple security layers
* **Attack Surface Reduction** - Limit compromise impact

**Nine Jailing Methods**

**1. chroot Jail** (Filesystem Only):

* ✅ Simple, compatible, widely used (SFTP/SSH)
* ❌ No process/network isolation, root escape vulnerable
* **Use**: SFTP jails, system recovery, package building

**2. SELinux Confined User** (System-Wide Mandatory Access Control):

* ✅ Extremely granular, protects against privilege escalation
* ❌ Complex to configure, steep learning curve
* **Use**: Enterprise RHEL/Fedora, Android, container isolation

**3. AppArmor Confined User** (Application-Level Path-Based):

* ✅ Easier than SELinux, intuitive path-based policies
* ❌ Less granular than SELinux
* **Use**: SUSE/Ubuntu/Debian app restrictions, Snap confinement

**4. systemd Sandboxing** (Service/Process Isolation):

* ✅ Native kernel-integrated, no containers needed
* ❌ Advanced configuration, not for user shells
* **Use**: Service hardening, background daemons

**5. Firejail/Bubblewrap/Nsjail** (Namespace-Based Sandboxes):

* ✅ Flexible per-app isolation, Bubblewrap unprivileged
* ❌ Firejail SUID risks, requires explicit configuration
* **Use**: Browser isolation, security competitions, testing

**6. Podman Rootless Container** (User Namespace Containers):

* ✅ Rootless (no privilege escalation), multi-tenant safe
* ❌ Not for interactive logins, some network limitations
* **Use**: Multi-tenant workloads, dev environments, CI/CD

**7. Snap/Flatpak** (Application Packaging with Sandboxing):

* ✅ Easy installation, clear permissions, automatic updates
* ❌ Only packaged GUI apps, not system-wide
* **Use**: Desktop app isolation, distribution-independent apps

**8. PAM Modules** (Session/Authentication Restrictions):

* ✅ Tied to authentication, integrates with login
* ❌ No isolation itself, depends on other mechanisms
* **Use**: Time-based access, login policies, LSM integration

**9. cgroups/Resource Quotas** (Resource Limitation Only):

* ✅ Prevents DoS, fair resource sharing
* ❌ No filesystem/process isolation
* **Use**: Multi-user resource management, service limits

**Method Selection Guidance**

**For Maximum Security** (Government, Critical Infrastructure):

* SELinux Confined User + systemd Sandboxing + cgroups

**For Desktop Application Isolation**:

* Firejail/Flatpak/Snap (user-friendly)
* AppArmor (distribution default on Ubuntu/SUSE)

**For Multi-Tenant Hosting**:

* Podman Rootless + SELinux/AppArmor + cgroups

**For SFTP/File-Only Access**:

* chroot (simple, effective for limited use case)

**For Service Hardening**:

* systemd sandboxing directives (native integration)

**For Testing/Development**:

* Bubblewrap/Podman (flexible, unprivileged)

**Real-World Applications**

**Already Jailed (Likely)**:

* Interactive labs (Killercoda, Instruqt) - Containers + resource limits
* Shared hosting - chroot + quotas
* Online code editors - Containers + seccomp
* University compute servers - SELinux + cgroups
* Git SSH access - chroot + forced commands

**Useful Jailing Scenarios**:

* **Security**: Isolate untrusted code, analyze malware safely
* **Multi-User**: Fair resource sharing, prevent interference
* **Development**: Per-project isolated environments
* **Compliance**: Meet regulatory separation requirements
* **Education**: Safe learning environments, break without consequences

**Defense-in-Depth Strategy**

**Best Practice**: Combine multiple methods:

* SELinux/AppArmor (system-wide mandatory access control)
* systemd sandboxing (service-specific restrictions)
* cgroups (resource limits)
* Containers (isolation when needed)

**Example Layered Defense**:

```
User Login
    ↓
SELinux applies user_u context
    ↓
PAM applies resource limits
    ↓
User runs application
    ↓
AppArmor/SELinux confines application
    ↓
systemd limits resources via cgroups
    ↓
Application sandboxed in container (if needed)
    ↓
Result: Multiple security boundaries
```

**Critical Principles**

1. **No Single Solution**: Different methods for different use cases
2. **chroot ≠ Security Boundary**: Not safe for root users
3. **Layer Security**: Combine multiple mechanisms
4. **Test Thoroughly**: Jails can break legitimate functionality
5. **Monitor**: Log and audit confined user actions
6. **Keep Simple**: Only add complexity when needed
7. **User Experience**: Balance security with usability
{% endhint %}

***

## 🔗 Additional Resources

**chroot Jails**

* [How to set up Linux chroot jails - Red Hat →](https://www.redhat.com/en/blog/set-linux-chroot-jails)
* [Taking a deeper dive into Linux chroot jails - Red Hat →](https://www.redhat.com/en/blog/deeper-linux-chroot-jails)
* [Restrict SSH User Access Using Chrooted Jail - TecMint →](https://www.tecmint.com/restrict-ssh-user-to-directory-using-chrooted-jail/)

**SELinux and AppArmor**

* [RHEL 9 Security Hardening - Using SELinux →](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/using_selinux/index)
* [Fedora SELinux User Guide →](https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-selinux/)
* [AppArmor Documentation - Ubuntu →](https://ubuntu.com/server/docs/security-apparmor)

**systemd Sandboxing**

* [Systemd Sandboxing and Security Best Practices - AlphaBravo →](https://blog.alphabravo.io/systemd-zero-to-hero-part-5-advanced-features-sandboxing-and-security-best-practices/)
* [How To Sandbox Processes With Systemd - DigitalOcean →](https://www.digitalocean.com/community/tutorials/how-to-sandbox-processes-with-systemd-on-ubuntu-20-04)
* [systemd.exec Manual Page →](https://www.freedesktop.org/software/systemd/man/systemd.exec.html)

**Containers and Namespaces**

* [Containers, Jails, and chroots; An Introduction →](https://derg.nz/web/sysadmin/kb/blog/2022/11/07/Containers-an-introduction.html)
* [Podman Rootless Containers →](https://github.com/containers/podman/blob/main/docs/tutorials/rootless_tutorial.md)
* [Difference Between chroot and Docker - Baeldung →](https://www.baeldung.com/ops/chroot-vs-docker)

**Linux Namespaces and Security**

* [Linux Namespaces - man7.org →](https://man7.org/linux/man-pages/man7/namespaces.7.html)
* [Linux Capabilities - man7.org →](https://man7.org/linux/man-pages/man7/capabilities.7.html)
* [seccomp - Secure Computing Mode →](https://www.kernel.org/doc/html/latest/userspace-api/seccomp_filter.html)
