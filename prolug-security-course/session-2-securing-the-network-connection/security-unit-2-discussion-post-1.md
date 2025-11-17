---
layout:
  width: default
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

# Security Unit 2 Discussion Post 1

> _**There are 401 STIGs for RHEL 9. If you filter in your STIG viewer for**_ `sysctl` _**there are 33 (mostly network focused),**_ `ssh` _**- 39, and network - 58. Now there are some overlaps between those, but review them and answer these questions.**_

***

### 📚 Research References

1. [DISA STIG Viewer - RHEL 9 →](https://public.cyber.mil/stigs/downloads/)
2. [Red Hat Enterprise Linux 9 Security Technical Implementation Guide →](https://www.stigviewer.com/stig/red_hat_enterprise_linux_9/)
3. [Understanding sysctl and Kernel Parameters →](https://www.kernel.org/doc/Documentation/sysctl/)
4. [OpenSSH Security Best Practices →](https://www.ssh.com/academy/ssh/security)
5. [NIST SP 800-123: Guide to General Server Security →](https://csrc.nist.gov/publications/detail/sp/800-123/final)

***

### 🎯 STIG Filtering Overview

{% hint style="info" %}
**RHEL 9 STIG Landscape**

Out of 401 total STIGs for RHEL 9, network-related configurations dominate the security baseline:

* **sysctl**: 33 STIGs (mostly network-focused kernel parameters)
* **ssh**: 39 STIGs (authentication, encryption, and protocol hardening)
* **network**: 58 STIGs (general network security configurations)

_Note: Some overlap exists between these categories_
{% endhint %}

### STIG Category Breakdown

| Category    | STIG Count | Primary Focus          | Configuration Target                 |
| ----------- | ---------- | ---------------------- | ------------------------------------ |
| **sysctl**  | 33         | Network stack behavior | Kernel parameters (`/proc/sys/net`)  |
| **ssh**     | 39         | Remote access security | SSH daemon and client configuration  |
| **network** | 58         | Network infrastructure | Firewall, routing, protocol settings |

***

### <mark style="color:$danger;">1️⃣ As systems engineers, why are we focused on protecting the network portion of our server builds?</mark>

{% hint style="success" %}
**Key Answer**

The network is the primary location where security vulnerabilities are exploited. So as Systems engineers our priority is to configure the interaction between the system and the network, protecting the critical boundry where the systems interact with the outside world.

> _**The point of hardening a system is to lockdown the server to only do the things it should do.**_

**Availability** of systems is the number KPI in operations. So, making the system not be compromised as easily, we maintain a higher availability.
{% endhint %}

#### The Network as Primary Attack Vector

{% tabs %}
{% tab title="🎯 Attack Surface Reality" %}
**The Network Is Where Exploitation Happens**

**Fundamental Security Principle**:

The network represents the most significant attack surface for modern server infrastructure. Every network-facing service, open port, and protocol implementation is a potential entry point for adversaries.

**Why Network Security Dominates**:

* 🌐 **External Connectivity**: Servers must communicate with the outside world to provide services
* 🔓 **Open Ports**: Each listening service creates a potential vulnerability
* 📡 **Protocol Complexity**: Network protocols (TCP/IP, HTTP, SSH) have inherent complexity that can be exploited
* 🔄 **Constant Traffic**: Network activity is continuous, providing persistent attack opportunities

{% hint style="danger" %}
**Security Reality**

Studies consistently show that **over 90% of successful breaches** involve network-based attack vectors—remote exploitation, credential theft over the network, or man-in-the-middle attacks.
{% endhint %}

**Common Network Attack Vectors**:

<details>

<summary>🚨 Network-Based Attack Methods</summary>

**Remote Code Execution (RCE)**:

* Exploiting vulnerable network services (Apache, Nginx, SSH)
* Buffer overflows in network protocols
* Deserialization vulnerabilities in web applications

**Credential-Based Attacks**:

* SSH brute force and password spraying
* Man-in-the-middle attacks on authentication
* Session hijacking and credential theft

**Denial of Service (DoS)**:

* SYN floods, UDP floods, ICMP floods
* Application-layer attacks (Slowloris, HTTP floods)
* DNS amplification attacks

**Network Reconnaissance**:

* Port scanning and service enumeration
* Banner grabbing for version information
* Network topology mapping

**Data Exfiltration**:

* Encrypted tunnels through allowed protocols
* DNS tunneling for covert channels
* Protocol abuse for data smuggling

</details>
{% endtab %}

{% tab title="🛡️ Critical Boundary Protection" %}
**Securing the System-Network Interface**

**The Boundary Concept**:

The network interface is the **critical security boundary** between:

* **Trusted internal system** (your hardened server)
* **Untrusted external environment** (the internet, other networks)

{% code title="Security Boundary Visualization" overflow="wrap" %}
```
External Network (Untrusted)
         ↓
    [Firewall Layer]
         ↓
  [Network Interface] ← CRITICAL BOUNDARY
         ↓
  [Network Stack - sysctl configurations]
         ↓
  [System Services - SSH, Web, etc.]
         ↓
    [Operating System]
         ↓
  [Data and Applications]
```
{% endcode %}

**Why This Boundary Matters**:

* 🔒 **First Line of Defense**: Network controls prevent attacks from ever reaching the system
* ⚡ **Performance Impact**: Network misconfigurations can cause DoS without any exploitation
* 🎯 **Precise Control**: Network hardening allows granular control over what traffic is permitted
* 📊 **Measurable Security**: Network controls provide clear, auditable security posture

{% hint style="info" %}
**Systems Engineering Perspective**

We configure the interaction between the system and network because **prevention at the network boundary is exponentially more effective than detection and response after penetration**.
{% endhint %}
{% endtab %}

{% tab title="🔐 Server Hardening Philosophy" %}
**Lockdown: Only Do What Should Be Done**

**Core Hardening Principle**:

> "The point of hardening a system is to lock down the server to only do the things it should do."

**Network-Focused Hardening Strategy**:

{% stepper %}
{% step %}
#### Identify Legitimate Functions

**Determine Required Network Behavior**:

* What services must the server provide?
* Which ports need to be open?
* What protocols are necessary?
* Who should be allowed to connect?

{% code title="Example: Web Server Requirements" overflow="wrap" %}
```bash
# Required network functions for a web server
Incoming: TCP/80 (HTTP), TCP/443 (HTTPS)
Outgoing: TCP/443 (package updates), UDP/53 (DNS)
Management: TCP/22 (SSH from admin network only)
```
{% endcode %}
{% endstep %}

{% step %}
#### Deny Everything Else

**Default Deny Posture**:

* Block all network traffic by default
* Explicitly permit only required communications
* Remove or disable unnecessary network services

{% code title="Firewall Default Deny" overflow="wrap" %}
```bash
# Implement default deny
firewall-cmd --set-default-zone=drop
firewall-cmd --zone=public --add-service=https --permanent
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload
```
{% endcode %}
{% endstep %}

{% step %}
#### Harden Required Functions

**Configure Secure Operation**:

* Apply STIGs to required services
* Use sysctl to harden network stack
* Implement defense-in-depth controls

{% code title="Network Stack Hardening" overflow="wrap" %}
```bash
# Example sysctl hardening
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
```
{% endcode %}
{% endstep %}

{% step %}
#### Monitor and Maintain

**Continuous Security**:

* Monitor network connections and traffic patterns
* Review logs for anomalies
* Update configurations as requirements change
* Audit compliance with hardening standards
{% endstep %}
{% endstepper %}

{% hint style="success" %}
**Hardening Outcome**: A properly hardened server has **minimal network attack surface**, performs only its intended functions, and actively resists common exploitation techniques.
{% endhint %}
{% endtab %}

{% tab title="📈 Availability as KPI" %}
**Availability Through Security**

**Key Performance Indicator (KPI)**:

> _**"Availability of systems is the number one KPI in operations."**_

**Security-Availability Relationship**:

| Security Posture            | Availability Impact                          | Business Outcome                         |
| --------------------------- | -------------------------------------------- | ---------------------------------------- |
| **Weak Network Security**   | Frequent compromises, DoS attacks, incidents | ❌ Poor uptime, service disruptions       |
| **Strong Network Security** | Resilient against attacks, stable operation  | ✅ High availability, business continuity |

**How Network Security Maintains Availability**:

**1. Prevention of Service Disruption**:

{% code title="DoS Prevention Example" overflow="wrap" %}
```bash
# sysctl settings to prevent SYN flood DoS
net.ipv4.tcp_syncookies = 1                    # Enable SYN cookies
net.ipv4.tcp_max_syn_backlog = 2048           # Increase SYN queue size
net.ipv4.tcp_synack_retries = 2               # Reduce SYN-ACK retries
net.core.netdev_max_backlog = 5000            # Increase network device backlog
```
{% endcode %}

These configurations **prevent network-based DoS attacks** from overwhelming the server and causing downtime.

**2. Faster Incident Detection and Response**:

* Hardened systems have predictable behavior
* Anomalies are easier to detect
* Response time is reduced

**3. Prevention of Compromise-Induced Downtime**:

* Compromised systems require incident response and recovery
* Recovery time can range from hours to days
* Network hardening prevents the compromise in the first place

**Availability Math**:

{% code title="Uptime Calculation" overflow="wrap" %}
```
Scenario 1: Unhardened Server
- Compromised 3 times per year
- Average recovery time: 24 hours
- Annual downtime: 72 hours
- Availability: 99.18%

Scenario 2: Network-Hardened Server  
- Compromised 0 times per year
- No compromise-related downtime
- Annual downtime: 8 hours (planned maintenance)
- Availability: 99.91%

Difference: 0.73% improvement = ~64 hours additional uptime
```
{% endcode %}

{% hint style="success" %}
**Operations Excellence**: By making the system more difficult to compromise through network hardening, we **maintain higher availability** and achieve operational excellence.
{% endhint %}
{% endtab %}
{% endtabs %}

***

### <mark style="color:$danger;">2️⃣ Why is it important to understand all the possible ingress points to our servers that exist?</mark>

{% hint style="success" %}
**Key Answer**

Most compromises exploit a connection point that was left unsecured.&#x20;

> _**Inventory of ingress points is a necessary prerequisite for effective defense.**_

By being able to identify every ingress point we can:

1. Effectively harden the server
2. Prevent unauthorized access
3. Make informed decisions about managing risk.
{% endhint %}

#### The Ingress Point Inventory Imperative

{% tabs %}
{% tab title="🚪 What Are Ingress Points?" %}
**Defining Ingress Points**

An **ingress point** is any network-accessible interface, service, protocol, or port through which data can enter a system from external sources.

**Common Ingress Point Categories**:

| Category                        | Examples                                          | Risk Level     |
| ------------------------------- | ------------------------------------------------- | -------------- |
| **Network Services**            | SSH (22), HTTP (80), HTTPS (443), Database (3306) | 🔴 High        |
| **Management Interfaces**       | IPMI, BMC, Remote Console                         | 🔴 Critical    |
| **File Transfer**               | FTP (21), SFTP (22), SCP, rsync                   | 🟡 Medium-High |
| **Monitoring/Logging**          | SNMP (161), Syslog (514), Metrics exporters       | 🟡 Medium      |
| **Inter-Service Communication** | APIs, Message queues, RPC                         | 🟡 Medium      |
| **ICMP**                        | Ping, Traceroute responses                        | 🟢 Low-Medium  |

{% code title="Discovering Ingress Points" overflow="wrap" %}
```bash
# List all listening network services
ss -tulpn

# Show all open ports
nmap -sT -p- localhost

# List systemd services with network sockets
systemctl list-units --type=socket --state=active

# Check firewall allowed services
firewall-cmd --list-all

# View running processes with network connections
lsof -i -P -n

# Check for services listening on all interfaces (0.0.0.0)
netstat -tulpn | grep "0.0.0.0"
```
{% endcode %}

{% hint style="danger" %}
**Unknown Ingress Point = Unmanaged Risk**

Every ingress point you don't know about is a potential vulnerability you can't protect against.
{% endhint %}
{% endtab %}

{% tab title="🎯 Why Complete Inventory Matters" %}
**The Case for Comprehensive Enumeration**

**Security Principle**: **You cannot protect what you do not know exists.**

**Benefits of Complete Ingress Point Inventory**:

**1. Effective Server Hardening**:

* ✅ Close unnecessary ports and services
* ✅ Apply appropriate STIG configurations to each service
* ✅ Implement defense-in-depth for critical ingress points
* ✅ Verify security posture is complete

{% code title="Hardening Based on Inventory" overflow="wrap" %}
```bash
# After inventory, disable unnecessary services
systemctl disable --now cups.service        # Print service not needed
systemctl disable --now bluetooth.service    # Bluetooth not needed on server
systemctl disable --now avahi-daemon.service # mDNS not needed

# Close firewall ports for disabled services
firewall-cmd --remove-service=ipp --permanent  # CUPS printing
firewall-cmd --remove-service=mdns --permanent # Avahi
firewall-cmd --reload
```
{% endcode %}

**2. Prevent Unauthorized Access**:

* 🔒 Each ingress point is authenticated and authorized
* 🔒 Access control lists (ACLs) applied appropriately
* 🔒 Network segmentation enforced
* 🔒 Monitoring and logging enabled

**3. Informed Risk Management**:

<details>

<summary>📊 Risk Assessment Framework</summary>

For each identified ingress point, assess:

| Risk Factor               | Assessment Questions                                       |
| ------------------------- | ---------------------------------------------------------- |
| **Exposure**              | Is this exposed to the internet or only internal networks? |
| **Necessity**             | Is this service absolutely required for business function? |
| **Authentication**        | What authentication mechanisms protect this service?       |
| **Encryption**            | Is communication encrypted in transit?                     |
| **Vulnerability History** | Does this service have known vulnerabilities?              |
| **Update Frequency**      | How often is this service updated and patched?             |
| **Monitoring**            | Do we have logging and alerting for this service?          |

**Risk-Based Prioritization**:

```
High Risk Ingress Points (Immediate Attention):
├── Internet-exposed SSH with password authentication
├── Unencrypted HTTP serving sensitive data
├── Database ports exposed to public networks
└── Management interfaces (IPMI) on untrusted networks

Medium Risk Ingress Points (Scheduled Hardening):
├── Internal SSH with key authentication
├── Monitoring services on management networks
└── Encrypted internal APIs

Low Risk Ingress Points (Standard Hardening):
├── ICMP responses (limited)
├── NTP services (restricted access)
└── Authenticated, encrypted inter-service communication
```

</details>

{% hint style="success" %}
**Risk Management Outcome**: Complete inventory enables **data-driven decisions** about security investments and priorities.
{% endhint %}
{% endtab %}

{% tab title="🔍 Discovery Methodology" %}
**How to Build Your Ingress Point Inventory**

{% stepper %}
{% step %}
#### Network-Level Discovery

**Identify listening ports and services:**

{% code title="Network Discovery Commands" overflow="wrap" lineNumbers="true" %}
```bash
# Socket Statistics - most reliable
ss -tulpn | tee ingress_inventory.txt

# Network connections with process info
lsof -i -P -n | tee -a ingress_inventory.txt

# Netstat alternative (if ss unavailable)
netstat -tulpn | tee -a ingress_inventory.txt

# Firewall configuration
firewall-cmd --list-all-zones | tee firewall_config.txt

# SELinux port labeling
semanage port -l | tee selinux_ports.txt
```
{% endcode %}
{% endstep %}

{% step %}
#### Service-Level Discovery

**Identify systemd services with network components:**

{% code title="Service Discovery Commands" overflow="wrap" lineNumbers="true" %}
```bash
# List all active services
systemctl list-units --type=service --state=active

# List all socket units
systemctl list-units --type=socket

# Identify services with network dependencies
systemctl show -p After,Before,Requires,Wants '*' | grep -i network

# Find services with network capabilities
getcap -r / 2>/dev/null | grep -i net
```
{% endcode %}
{% endstep %}

{% step %}
#### Process-Level Discovery

**Identify processes with network activity:**

{% code title="Process Network Analysis" overflow="wrap" lineNumbers="true" %}
```bash
# All processes with network connections
lsof -i | awk '{print $1}' | sort -u

# Processes listening on specific interfaces
ss -tlp | grep -v "127.0.0.1"

# Find processes with raw socket access
ps aux | grep -E '(CAP_NET_RAW|CAP_NET_ADMIN)'
```
{% endcode %}
{% endstep %}

{% step %}
#### Configuration Review

**Review configuration files for network bindings:**

{% code title="Configuration File Analysis" overflow="wrap" lineNumbers="true" %}
```bash
# Search for listen directives
grep -r "listen" /etc/httpd/ /etc/nginx/ /etc/ssh/

# Find IP bindings
grep -r "bind" /etc/ 2>/dev/null | grep -v "Binary"

# Check inetd/xinetd services (if present)
cat /etc/inetd.conf /etc/xinetd.d/* 2>/dev/null
```
{% endcode %}
{% endstep %}

{% step %}
#### Documentation and Tracking

**Create formal ingress point register:**

{% code title="Ingress Point Documentation Template" overflow="wrap" %}
```
Ingress Point Register - [Hostname]
Last Updated: [Date]

| Port | Protocol | Service | Process | Auth Method | Encryption | Exposure | Business Justification |
|------|----------|---------|---------|-------------|------------|----------|----------------------|
| 22   | TCP      | SSH     | sshd    | Key-based   | Yes        | Internal | Remote administration |
| 443  | TCP      | HTTPS   | httpd   | TLS Cert    | Yes        | Internet | Public web service |
| 3306 | TCP      | MySQL   | mysqld  | Password    | Yes        | Internal | Application database |

Risk Assessment: [Date]
Next Review: [Date + 90 days]
Responsible: [System Owner]
```
{% endcode %}
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Best Practice**: Maintain ingress point inventory as a **living document**, reviewed quarterly and updated whenever system changes occur.
{% endhint %}
{% endtab %}

{% tab title="⚠️ Real-World Example" %}
**Case Study: The Overlooked Ingress Point**

**Scenario: The Forgotten Management Interface**

{% code title="Initial Security Audit" overflow="wrap" %}
```
Documented Ingress Points:
✅ SSH (TCP/22) - Secured with key auth, restricted source IPs
✅ HTTPS (TCP/443) - TLS 1.3, certificate auth
✅ Application API (TCP/8080) - OAuth2, rate limited

Security Assessment: "System appears properly hardened"
```
{% endcode %}

**What Was Missed**:

{% code title="Complete Discovery Revealed" overflow="wrap" %}
```bash
# Comprehensive port scan revealed
nmap -sT -p- server.example.com

PORT     STATE SERVICE
22/tcp   open  ssh
443/tcp  open  https
623/tcp  open  asf-rmcp    # ⚠️ IPMI - NOT DOCUMENTED
8080/tcp open  http-proxy

# IPMI Investigation
ipmitool lan print
IP Address Source       : DHCP
IP Address              : 192.168.1.50  # Exposed to internal network
Subnet Mask             : 255.255.255.0
Default Gateway IP      : 192.168.1.1
Auth Type Support       : MD5           # ⚠️ Weak authentication
```
{% endcode %}

**The Breach**:

* Attacker discovered IPMI interface through network scan
* IPMI using default credentials (never changed)
* Attacker gained full hardware-level access
* Compromised hypervisor, then all VMs
* **Total compromise** from one undocumented ingress point

**Lessons Learned**:

1. ❌ **Incomplete inventory** led to unmanaged risk
2. ❌ **Out-of-band management** often overlooked in security assessments
3. ❌ **Default credentials** remained because service was unknown
4. ✅ **Comprehensive discovery** would have identified the vulnerability
5. ✅ **Regular audits** prevent inventory drift

{% hint style="danger" %}
**Critical Takeaway**: This breach occurred despite having "proper security" on all known services. **Unknown ingress points are the highest-risk vulnerabilities.**
{% endhint %}
{% endtab %}
{% endtabs %}

***

### <mark style="color:$danger;">2️⃣-1️⃣ Why is it so important to understand the behaviors of processes that are connecting on those ingress points?</mark>

{% hint style="success" %}
**Key Answer**

Understanding process behaviors is necessary to comprehend what's happening on the server and determine how the system interacts with the network. It's what will allow us to implement technical preventative controls against potential compromises.

> `sysctl` _**is a tool used to modigy the kernel parameters at runtime, which will dictate how the system behaves in real-time.**_

* So, by filtering STIGs for `sysctl` we can identify necessary configurations for hardening. These configurations directy change the way that the kernel behaves with the networking stack in the system.

> `ssh` _**is a program for logging into a remote machine and for executing commands on a remote machine.**_

* STIGs specific to `ssh` deal with how the protocol and the daemon interact with the system, including how authentication happens via PAM (Pluggable Authentication Modules)
{% endhint %}

### Process Behavior Analysis

{% tabs %}
{% tab title="🔍 What Is Process Behavior?" %}
**Defining Process Network Behavior**

**Process Behavior Components**:

**Network Interaction Patterns**:

* Which ports does the process bind to?
* What protocols does it use?
* Does it initiate outbound connections?
* What authentication mechanisms does it implement?
* How does it handle incoming connections?

**System Interaction Patterns**:

* What files does the process access?
* Which system calls does it make?
* What privileges does it require?
* How does it interact with the kernel?

{% code title="Analyzing SSH Process Behavior" overflow="wrap" %}
```bash
# What ports does SSH bind to?
ss -tlpn | grep sshd
LISTEN 0 128 0.0.0.0:22 0.0.0.0:* users:(("sshd",pid=1234))

# What files does SSH access?
lsof -p $(pgrep sshd) | grep -E "(REG|DIR)"
/etc/ssh/sshd_config
/var/log/auth.log
/home/user/.ssh/authorized_keys

# What system calls does SSH make?
strace -p $(pgrep sshd) -e trace=network 2>&1 | head -20
bind(3, {sa_family=AF_INET, sin_port=htons(22)})
listen(3, 128)
accept4(3, {sa_family=AF_INET})

# What capabilities does SSH need?
getcap /usr/sbin/sshd
/usr/sbin/sshd = cap_net_bind_service+ep
```
{% endcode %}

{% hint style="info" %}
**Why This Matters**: Understanding behavior allows you to **configure appropriate restrictions** that permit legitimate function while blocking malicious activity.
{% endhint %}
{% endtab %}

{% tab title="🛠️ sysctl: Kernel Behavior Control" %}
**sysctl - Modifying Kernel Parameters at Runtime**

**What is sysctl?**

> **sysctl** is a tool used to modify kernel parameters at runtime, which dictates how the system behaves in real-time.

**Network Stack Behavior Control**:

The Linux kernel's networking stack has hundreds of tunable parameters that control:

* How packets are processed
* How connections are established
* How the system responds to network anomalies
* What traffic is accepted or rejected

{% code title="Example sysctl Network Hardening" overflow="wrap" lineNumbers="true" %}
```bash
# View current network parameters
sysctl -a | grep net.ipv4

# Disable IP source routing (prevents routing manipulation)
sysctl -w net.ipv4.conf.all.accept_source_route=0
sysctl -w net.ipv4.conf.default.accept_source_route=0

# Ignore ICMP redirects (prevents routing table manipulation)
sysctl -w net.ipv4.conf.all.accept_redirects=0
sysctl -w net.ipv4.conf.all.secure_redirects=0

# Enable SYN cookies (DoS protection)
sysctl -w net.ipv4.tcp_syncookies=1

# Ignore broadcast pings (reduces attack surface)
sysctl -w net.ipv4.icmp_echo_ignore_broadcasts=1

# Enable reverse path filtering (prevents IP spoofing)
sysctl -w net.ipv4.conf.all.rp_filter=1

# Log martian packets (suspicious packet detection)
sysctl -w net.ipv4.conf.all.log_martians=1

# Persist changes
cat >> /etc/sysctl.d/99-network-hardening.conf << EOF
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.log_martians = 1
EOF

# Apply configuration
sysctl -p /etc/sysctl.d/99-network-hardening.conf
```
{% endcode %}

**STIG Connection**:

By filtering STIGs for `sysctl`, we identify necessary configurations for hardening. These configurations **directly change the way the kernel behaves with the networking stack**.

<details>

<summary>📋 Common sysctl STIG Requirements</summary>

**IPv4 Network Security**:

* `net.ipv4.conf.all.accept_source_route = 0` (V-230548)
* `net.ipv4.conf.all.accept_redirects = 0` (V-230549)
* `net.ipv4.conf.all.send_redirects = 0` (V-230551)
* `net.ipv4.conf.all.forwarding = 0` (V-230553, unless router)
* `net.ipv4.conf.all.rp_filter = 1` (V-230555)
* `net.ipv4.icmp_echo_ignore_broadcasts = 1` (V-230557)
* `net.ipv4.tcp_syncookies = 1` (V-230559)

**IPv6 Network Security**:

* `net.ipv6.conf.all.accept_source_route = 0` (V-230561)
* `net.ipv6.conf.all.accept_redirects = 0` (V-230563)
* `net.ipv6.conf.all.forwarding = 0` (V-230565, unless router)

**Kernel Security**:

* `kernel.kptr_restrict = 1` (V-230567, kernel pointer protection)
* `kernel.yama.ptrace_scope = 1` (V-230569, process debugging restriction)
* `kernel.randomize_va_space = 2` (V-230571, ASLR)

</details>

{% hint style="success" %}
**Technical Preventative Control**: sysctl configurations operate at the **kernel level**, preventing malicious behavior before it can affect user-space processes.
{% endhint %}
{% endtab %}

{% tab title="🔐 SSH: Protocol and Daemon Behavior" %}
**SSH - Remote Access Process Behavior**

**What is SSH?**

> **SSH** is a program for logging into a remote machine and for executing commands on a remote machine.

**SSH Process Behavior Components**:

**Network Behavior**:

* Listens on TCP port 22 (configurable)
* Implements encrypted communication protocol
* Handles authentication negotiation
* Manages connection multiplexing

**Authentication Behavior**:

* Interacts with PAM (Pluggable Authentication Modules)
* Validates user credentials
* Checks authorized\_keys files
* Enforces access controls

**Security Behavior**:

* Encryption key exchange
* Session key generation
* Protocol version negotiation
* Host key verification

{% code title="SSH Daemon Configuration Analysis" overflow="wrap" lineNumbers="true" %}
```bash
# View SSH daemon configuration
cat /etc/ssh/sshd_config

# Key behavioral controls
Port 22                              # What port to listen on
ListenAddress 0.0.0.0               # What interfaces to bind
Protocol 2                           # SSH protocol version (2 only)
PermitRootLogin no                   # Root login behavior
PasswordAuthentication no            # Password auth behavior
PubkeyAuthentication yes             # Key-based auth behavior
AuthenticationMethods publickey      # Required auth methods
UsePAM yes                          # PAM integration behavior
X11Forwarding no                     # X11 forwarding behavior
AllowTcpForwarding no               # Port forwarding behavior
PermitTunnel no                     # VPN tunnel behavior
MaxAuthTries 3                      # Brute force protection
MaxSessions 10                      # Resource limit behavior
ClientAliveInterval 300             # Idle timeout behavior
ClientAliveCountMax 0               # Connection keep-alive

# Test configuration syntax
sshd -t

# View effective configuration (including defaults)
sshd -T
```
{% endcode %}

**STIG Connection**:

STIGs specific to `ssh` deal with how the protocol and daemon interact with the system, including:

<details>

<summary>🔒 SSH STIG Requirements</summary>

**Protocol and Encryption**:

* `Protocol 2` only (V-230287) - SSH1 is insecure
* Strong ciphers only (V-230288) - Remove weak encryption
* Strong MACs only (V-230289) - Cryptographic integrity
* Strong key exchange algorithms (V-230290)

**Authentication Security**:

* `PermitRootLogin no` (V-230291) - Prevent direct root access
* `PermitEmptyPasswords no` (V-230292) - Require authentication
* `PasswordAuthentication no` (V-230293) - Key-based only
* `PubkeyAuthentication yes` (V-230294) - Enable strong auth
* `UsePAM yes` (V-230295) - System auth integration
* `MaxAuthTries 3` (V-230296) - Brute force protection
* `HostbasedAuthentication no` (V-230297) - Disable weak auth

**Access Controls**:

* `PermitUserEnvironment no` (V-230298) - Prevent env manipulation
* `Banner /etc/issue` (V-230299) - Legal notice
* `PrintLastLog yes` (V-230300) - User awareness
* `AllowTcpForwarding no` (V-230301) - Prevent tunneling (if not needed)
* `X11Forwarding no` (V-230302) - Disable GUI forwarding
* `PermitTunnel no` (V-230303) - Prevent VPN tunneling

**Session Security**:

* `ClientAliveInterval 600` (V-230304) - Idle timeout
* `ClientAliveCountMax 0` (V-230305) - Terminate idle
* `LoginGraceTime 60` (V-230306) - Auth timeout
* `Compression delayed` (V-230307) - Prevent compression attacks

**Logging and Monitoring**:

* `LogLevel VERBOSE` (V-230308) - Detailed logging
* `SyslogFacility AUTHPRIV` (V-230309) - Proper log routing

</details>

{% hint style="success" %}
**PAM Integration**: SSH behavior is extended through **Pluggable Authentication Modules (PAM)**, enabling system-wide authentication policies, account management, and session controls.
{% endhint %}

{% code title="PAM Configuration for SSH" overflow="wrap" %}
```bash
# View SSH PAM configuration
cat /etc/pam.d/sshd

# Typical PAM stack for SSH
auth       required     pam_sepermit.so          # SELinux checks
auth       substack     password-auth             # Password validation
auth       include      postlogin                 # Post-auth processing
account    required     pam_nologin.so           # Check /etc/nologin
account    include      password-auth             # Account verification
password   include      password-auth             # Password changes
session    required     pam_selinux.so close     # SELinux context closing
session    required     pam_loginuid.so          # Set login UID
session    required     pam_selinux.so open      # SELinux context opening
session    required     pam_namespace.so         # Polyinstantiation
session    optional     pam_keyinit.so force revoke  # Keyring setup
session    include      password-auth             # Session setup
session    include      postlogin                 # Post-login processing
```
{% endcode %}
{% endtab %}

{% tab title="🎯 Behavioral Analysis Benefits" %}
**Why Process Behavior Understanding Matters**

**1. Implement Technical Preventative Controls**:

Understanding behavior enables **proactive security measures** rather than reactive detection:

{% code title="Behavior-Based Security Controls" overflow="wrap" %}
```bash
# Knowing SSH should only bind to management network
# allows us to restrict its listening behavior
echo "ListenAddress 10.10.10.5" >> /etc/ssh/sshd_config

# Understanding Apache's typical outbound connections
# allows us to restrict unusual behavior
iptables -A OUTPUT -m owner --uid-owner apache \
         -p tcp --dport 443 -m state --state NEW \
         -j LOG --log-prefix "Apache HTTPS out: "
iptables -A OUTPUT -m owner --uid-owner apache \
         -p tcp --dport 443 -m state --state NEW \
         -j DROP  # Block unless explicitly allowed

# Knowing database should only accept local connections
# allows us to configure accordingly
echo "bind-address = 127.0.0.1" >> /etc/my.cnf
```
{% endcode %}

**2. Detect Anomalous Activity**:

Baseline behavior allows **immediate detection** of compromises:

<details>

<summary>🚨 Behavioral Anomaly Detection</summary>

**Normal SSH Behavior**:

```
✅ Listens on TCP/22
✅ Accepts connections from known IP ranges
✅ Authenticates using public keys
✅ Creates session logs in /var/log/secure
✅ No outbound connections initiated
```

**Anomalous SSH Behavior (Potential Compromise)**:

```
❌ Listening on additional ports
❌ Connections from unexpected geographic locations
❌ Authentication from new keys without change management
❌ Outbound connections to external IPs
❌ Excessive failed authentication attempts
❌ Binary checksum doesn't match package manager
```

**Detection Methods**:

```bash
# Behavioral monitoring with auditd
auditctl -a exit,always -F arch=b64 -S bind -F exe=/usr/sbin/sshd
auditctl -a exit,always -F arch=b64 -S connect -F exe=/usr/sbin/sshd

# File integrity monitoring
aide --check | grep sshd

# Binary verification
rpm -V openssh-server
```

</details>

**3. Proper Resource Allocation**:

Understanding resource requirements enables **appropriate system tuning**:

{% code title="Resource-Based Configuration" overflow="wrap" %}
```bash
# SSH typically needs minimal resources
# Limit MaxStartups to prevent resource exhaustion
echo "MaxStartups 10:30:60" >> /etc/ssh/sshd_config

# Web server needs more connection handling
# Adjust kernel parameters accordingly
sysctl -w net.core.somaxconn=4096
sysctl -w net.ipv4.tcp_max_syn_backlog=8192

# Database requires large file descriptor limits
echo "mysql soft nofile 65535" >> /etc/security/limits.conf
echo "mysql hard nofile 65535" >> /etc/security/limits.conf
```
{% endcode %}

**4. Least Privilege Implementation**:

Behavioral understanding enables **precise privilege restriction**:

{% code title="Capability-Based Restrictions" overflow="wrap" %}
```bash
# SSH needs to bind to port 22 (privileged port < 1024)
# Grant only CAP_NET_BIND_SERVICE, not full root
setcap cap_net_bind_service=+ep /usr/sbin/sshd

# Web server needs to bind port 80/443
setcap cap_net_bind_service=+ep /usr/sbin/httpd

# Verify capabilities
getcap /usr/sbin/sshd
getcap /usr/sbin/httpd
```
{% endcode %}

{% hint style="success" %}
**Defense-in-Depth Principle**: Behavioral understanding enables **layered security controls** at multiple levels—network, kernel, process, and application—creating comprehensive protection.
{% endhint %}
{% endtab %}
{% endtabs %}

***

#### 📊 Integration: Network Security as Systems Engineering

#### The Complete Picture

{% code title="Systems Engineering Security Framework" overflow="wrap" %}
```
Systems Engineering Network Security Approach
│
├─ 1. Identify Ingress Points
│  ├─ Network services (SSH, HTTP, etc.)
│  ├─ Management interfaces (IPMI, console)
│  ├─ Inter-service communication
│  └─ Document all entry points
│
├─ 2. Understand Process Behaviors
│  ├─ How services interact with network
│  ├─ What kernel behaviors are required
│  ├─ Authentication and authorization flows
│  └─ Resource requirements and limits
│
├─ 3. Apply STIG Configurations
│  ├─ sysctl: Kernel network stack hardening
│  ├─ SSH: Remote access security
│  ├─ Network: Firewall and routing
│  └─ Service-specific hardening
│
├─ 4. Implement Technical Controls
│  ├─ Preventative: Block attacks before they reach the system
│  ├─ Detective: Monitor for anomalous behavior
│  ├─ Corrective: Automated response to threats
│  └─ Compensating: Additional controls for residual risk
│
├─ 5. Monitor and Maintain
│  ├─ Continuous behavioral monitoring
│  ├─ Log analysis and alerting
│  ├─ Regular inventory updates
│  └─ Periodic STIG compliance audits
│
└─ Result: High-Availability, Secure Systems
   ├─ Minimal attack surface
   ├─ Strong network boundary defense
   ├─ Comprehensive visibility
   └─ Operational excellence
```
{% endcode %}

***

### 📝 Key Takeaways

{% hint style="success" %}
**Summary: Network Security Focus in Systems Engineering**

**Why Network Security is Priority**

**The Network is the Primary Attack Surface**:

* ✅ 90%+ of breaches involve network-based attack vectors
* ✅ Network is the critical boundary between trusted system and untrusted environment
* ✅ Prevention at network boundary is exponentially more effective than post-penetration response

**Hardening Philosophy**:

* ✅ "Lock down the server to only do what it should do"
* ✅ Default deny posture with explicit permits for required functions
* ✅ Minimal attack surface through service reduction and hardening

**Availability Through Security**:

* ✅ Network hardening prevents compromise-induced downtime
* ✅ DoS protection maintains service availability
* ✅ Reduced incident response needs = higher uptime

**Comprehensive Ingress Point Inventory**

**Why Complete Inventory is Critical**:

* ✅ **Effective hardening**: Can only secure what you know exists
* ✅ **Prevent unauthorized access**: Every point authenticated and monitored
* ✅ **Informed risk management**: Data-driven security decisions

**Inventory Methodology**:

1. Network-level discovery (`ss`, `lsof`, `nmap`)
2. Service-level discovery (`systemctl`, `getcap`)
3. Process-level discovery (process network connections)
4. Configuration review (listen directives, bindings)
5. Documentation and tracking (living register)

**Real-World Impact**:

* ⚠️ Most compromises exploit overlooked connection points
* ⚠️ Unknown ingress points = unmanaged risk
* ⚠️ Regular audits prevent inventory drift

**Process Behavior Understanding**

**Why Behavioral Analysis Matters**:

**Technical Preventative Controls**:

* ✅ **sysctl**: Kernel-level network stack hardening (STIG-driven)
* ✅ **SSH configuration**: Protocol and daemon behavior control
* ✅ **PAM integration**: System-wide authentication policies

**Security Benefits**:

1. **Proactive prevention**: Stop attacks before they succeed
2. **Anomaly detection**: Baseline enables immediate compromise detection
3. **Least privilege**: Precise capability and resource restrictions
4. **Defense-in-depth**: Layered controls at multiple levels

**STIG Application**:

* 📋 **sysctl STIGs (33)**: Direct kernel network behavior modification
* 📋 **SSH STIGs (39)**: Protocol, authentication, and session security
* 📋 **Network STIGs (58)**: Firewall, routing, and protocol hardening

**Systems Engineering Excellence**

**Complete Security Approach**:

1. Identify all ingress points (network discovery)
2. Understand process behaviors (behavioral analysis)
3. Apply STIG configurations (hardening standards)
4. Implement technical controls (prevention, detection, correction)
5. Monitor and maintain (continuous security)

**Result**: **High-availability, secure, operationally excellent systems** with minimal attack surface and comprehensive defense.
{% endhint %}

***

### 🔗 Additional Resources

**RHEL 9 STIGs and Hardening**

* [DISA STIG Viewer - RHEL 9 ](https://public.cyber.mil/stigs/downloads/)
* [Red Hat Enterprise Linux 9 Security Guide ](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/security_hardening/index)

**Network Security and Kernel Hardening**

* [Linux Kernel Networking Documentation ](https://www.kernel.org/doc/Documentation/networking/)
* [sysctl and Kernel Parameters Guide ](https://www.kernel.org/doc/Documentation/sysctl/)
* [NIST SP 800-123: Guide to General Server Security ](https://csrc.nist.gov/publications/detail/sp/800-123/final)

**SSH Security Best Practices**

* [OpenSSH Security Documentation ](https://www.openssh.com/security.html)
* [SSH Hardening Guides - Mozilla ](https://infosec.mozilla.org/guidelines/openssh)
* [CIS Benchmark: SSH Configuration ](https://www.cisecurity.org/benchmark/red_hat_linux)

**Process and Behavioral Analysis**

* [Linux Security Modules (LSM) ](https://www.kernel.org/doc/html/latest/admin-guide/LSM/index.html)
* [Auditd for System Monitoring ](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/security_hardening/auditing-the-system_security-hardening)
