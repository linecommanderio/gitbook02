---
cover: ../../.gitbook/assets/Screenshot from 2025-11-17 21-43-33.png
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

# 📢 Security Unit 4 Discussion Post 1

> _**Review some of the blogs on air-gapped systems and bastion hosts from AWS, Red Hat, and other security resources.**_

***

## 📚 Research References

1. https://www.ibm.com/think/topics/air-gap
2. https://www.fortinet.com/resources/cyberglossary/what-is-air-gap
3. https://www.redhat.com/en/blog/air-gapped-networks
4. https://clairservices.com/patching-rhel-in-an-air-gapped-environment/
5. https://www.sentinelone.com/cybersecurity-101/cybersecurity/what-is-an-air-gap/
6. https://n2ws.com/blog/aws-cloud/aws-backup-vault
7. https://aws.amazon.com/blogs/storage/building-cyber-resiliency-with-aws-backup-logically-air-gapped-vault/
8. https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-123.pdf
9. https://hoop.dev/blog/jump-server-vs-bastion-host-essential-security-tools-for-technology-managers/
10. https://www.redhat.com/en/blog/ssh-proxy-bastion-proxyjump

***

## 1️⃣ What is the Theme of Air-Gapped Systems?

{% hint style="info" %}
Air-gapped systems are computer systems or networks that are physically or logically isolated from external networks, including the internet and other unsecured networks, to prevent unauthorized access and cyber threats.
{% endhint %}

### Core Concept: Complete Network Isolation

Isolation mechanisms include:

* Disconnecting network interfaces (no Ethernet cables)
* Disabling wireless communication (Wi‑Fi, Bluetooth, cellular)
* Restricting physical access to authorized personnel only
* Ensuring data cannot be transferred except through strictly controlled, manual means such as USB drives or external media

{% tabs %}
{% tab title="🔐 Key Principles" %}
**Key Principles of Air-Gapped Systems**

{% stepper %}
{% step %}
### Physical Isolation

The system is disconnected from all external networks, with no network cables, Wi-Fi, or Bluetooth connections.

Implementation requirements:

* No physical network connections to external networks
* Wireless interfaces disabled in BIOS/UEFI
* Network hardware physically removed when possible
* Faraday cage or TEMPEST shielding for classified environments
* RF shielding to prevent electromagnetic emanations

{% code title="Disable Network Interfaces (Linux)" overflow="wrap" %}
```bash
# Disable network interfaces at boot
systemctl disable NetworkManager
systemctl stop NetworkManager

# Physically disable network interfaces
ip link set eth0 down
ip link set wlan0 down

# Blacklist network drivers (permanent)
cat >> /etc/modprobe.d/blacklist-network.conf << EOF
blacklist iwlwifi
blacklist e1000
blacklist e1000e
EOF

# Verify no network interfaces are active
ip link show
```
{% endcode %}
{% endstep %}

{% step %}
### Restricted Connectivity

Only authorized individuals can access the system, and data transfer is limited to secure, manual methods.

Access control measures:

* Physical access controls (badge readers, biometrics)
* Multi-person authorization for physical entry
* Locked server rooms with 24/7 monitoring
* Video surveillance of all access points
* Strict logging of all physical access events

Data transfer controls:

* USB drives scanned for malware before use
* Write-once media (CD-R, DVD-R) preferred for incoming data
* Separate dedicated systems for scanning external media
* Chain-of-custody documentation for all media transfers
* Cryptographic checksums to verify data integrity

{% code title="USB Device Control with USBGuard" overflow="wrap" lineNumbers="true" %}
```bash
# Install USBGuard
dnf install usbguard

# Generate initial policy (with no USB devices connected)
usbguard generate-policy > /etc/usbguard/rules.conf

# Only allow specific authorized USB devices
cat > /etc/usbguard/rules.conf << EOF
# Allow specific USB device by serial number
allow id 1234:5678 serial "ABC123" name "Approved USB Drive"

# Block all other USB devices
block
EOF

# Start and enable USBGuard
systemctl enable --now usbguard
```
{% endcode %}
{% endstep %}

{% step %}
### Unidirectional Data Flow

In some cases, data can only flow in one direction (e.g., from a trusted network to the air-gapped system) to prevent data exfiltration.

Data diode implementation: A hardware data diode physically enforces one-way data flow using fiber optic cables with transmit-only on one end and receive-only on the other.

{% code title="Unidirectional Data Flow Concept" overflow="wrap" %}
```
External Network (Lower Classification)
         ↓
    [Data Diode Hardware]
    • Transmit only (TX)
    • Physically cannot receive
         ↓
Air-Gapped Network (Higher Classification)
    • Receive only (RX)
    • Physically cannot transmit
         ↓
    Data flows IN only
    No data can flow OUT
```
{% endcode %}

Use cases:

* SCADA systems receiving commands but not sending operational data out
* Classified networks receiving updates from lower classification networks
* Industrial control systems receiving configuration but not exposing telemetry
* Financial systems receiving market data without exposing trading strategies
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
Security Reality: While air-gapped systems are highly secure against remote attacks, they are not immune to insider threats, physical tampering, or sophisticated attacks that exploit human or procedural weaknesses. Strict protocols for data transfer and access control are essential to maintain their integrity.
{% endhint %}
{% endtab %}

{% tab title="☁️ AWS Logical Air-Gap" %}
**AWS Perspective: Logical Air-Gapping**

AWS refers to air-gapped systems primarily in the context of "logically air-gapped" cloud infrastructure, particularly for backup and ransomware protection.

Physical vs. Logical Air-Gapping (summary):

* Physical Air-Gap: complete physical disconnection
* Logical Air-Gap (AWS): strong logical controls and encryption, network exists but is heavily restricted, software-enforced immutability (Vault Lock), separate accounts and IAM controls

**AWS Logical Isolation Methods**

{% stepper %}
{% step %}
### Separate AWS Accounts

Backup vaults deployed in completely separate AWS accounts from production resources.

{% code title="Multi-Account Architecture" overflow="wrap" %}
```
Production Account (123456789012)
├─ Application infrastructure
├─ Databases
└─ Workloads

Backup Account (987654321098)
├─ AWS Backup vaults
├─ Immutable storage
└─ Restricted cross-account access
    • No production admin access
    • Service-owned encryption keys
    • Separate IAM policies
```
{% endcode %}
{% endstep %}

{% step %}
### Restricted Cross-Account Permissions

Minimal permissions granted from production to backup accounts using least privilege principles.

{% code title="Cross-Account IAM Policy" overflow="wrap" lineNumbers="true" %}
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "backup.amazonaws.com"
      },
      "Action": "backup:CopyIntoBackupVault",
      "Resource": "arn:aws:backup:us-east-1:987654321098:backup-vault:LogicalAirGapVault",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "123456789012"
        }
      }
    }
  ]
}
```
{% endcode %}

Key restrictions:

* Production admins cannot access backup account
* No delete permissions on backup data
* Recovery requires separate approval workflow
* Service-controlled encryption keys (not accessible to admins)
{% endstep %}

{% step %}
### Vault Lock Functionality

AWS Backup Vault Lock provides Write-Once-Read-Many (WORM) protection.

{% code title="Vault Lock Configuration" overflow="wrap" lineNumbers="true" %}
```bash
# Create logically air-gapped backup vault
aws backup create-backup-vault \
  --backup-vault-name LogicalAirGapVault \
  --region us-east-1

# Enable Vault Lock with minimum retention
aws backup put-backup-vault-lock-configuration \
  --backup-vault-name LogicalAirGapVault \
  --min-retention-days 30 \
  --max-retention-days 365

# Backups now cannot be deleted for minimum retention period
# Even by AWS account root user
```
{% endcode %}

Vault Lock features:

* Backups immutable for configured retention period
* Protection against ransomware deletion attempts
* Regulatory compliance (SEC, FINRA, HIPAA)
* Prevents accidental or malicious deletion
{% endstep %}

{% step %}
### Service-Owned Encryption Keys

Backup data encrypted with AWS-managed keys not accessible to account administrators.

{% code title="Encryption Architecture" overflow="wrap" %}
```
Production Data
    ↓
Backup Process
    ↓
Encrypted with AWS Backup Service Key
    • Not accessible to account admins
    • Not exposed in AWS KMS
    • Service-controlled decryption
    ↓
Stored in Logically Air-Gapped Vault
    ↓
Recovery requires:
    • Separate approval workflow
    • Break-glass procedure
    • Multi-party authorization
```
{% endcode %}
{% endstep %}

{% step %}
### Multi-Party Approval Workflows

High-assurance scenarios implement multi-person authorization for recovery operations.

{% code title="Recovery Workflow" overflow="wrap" %}
```
Recovery Request Initiated
    ↓
First Approver (Security Team)
    ↓
Second Approver (Compliance Officer)
    ↓
Time Delay (4-24 hours)
    ↓
Final Approval (CISO or Designated Authority)
    ↓
Recovery Execution
    ↓
Audit Trail Logged
```
{% endcode %}
{% endstep %}
{% endstepper %}

<details>

<summary>🏦 Banking and Financial Services Example (Architecture)</summary>

AWS guidance for highly regulated sectors suggests segregating vaults and network segments from production resources:

* Separate AWS accounts (production, staging, backup, security)
* No direct internet connectivity for backup VPCs
* VPC endpoints only (S3, Backup)
* Security groups, NACLs, and SCPs to prevent unauthorized API calls
* Centralized CloudTrail logging to separate logging account

{% code title="Network Isolation Architecture" overflow="wrap" %}
```
Production VPC (10.0.0.0/16)
├─ Public Subnets (10.0.1.0/24)
│  └─ NAT Gateways, Load Balancers
├─ Private Subnets (10.0.10.0/24)
│  └─ Application Servers, Databases
└─ NO connection to Backup VPC

Backup VPC (172.16.0.0/16) - Separate Account
├─ Private Subnets Only (172.16.10.0/24)
│  ├─ AWS Backup vault resources
│  └─ VPC Endpoints (S3, Backup, KMS)
├─ NO Internet Gateway
├─ NO NAT Gateway
└─ NO VPC Peering to Production
```
{% endcode %}

</details>

Key AWS concepts:

* AWS Backup Vaults (immutable storage with Vault Lock)
* AWS Organizations (SCPs for governance)
* IAM Roles (minimal cross-account permissions)
* AWS KMS (service-owned encryption keys)
* VPC Endpoints (no internet exposure for backup traffic)

{% hint style="success" %}
Logical Air-Gap Advantage: Provides near-air-gap security while maintaining automation and compliance controls that meet regulatory needs without physical separation.
{% endhint %}
{% endtab %}

{% tab title="🎯 Security Considerations" %}
**Security Limitations and Threats**

{% hint style="danger" %}
Threat Reality

While air-gapped systems are highly secure against remote attacks, they face unique vulnerabilities that require dedicated security measures.
{% endhint %}

**Insider Threats**

| Threat Type       | Attack Vector                                   | Mitigation                               |
| ----------------- | ----------------------------------------------- | ---------------------------------------- |
| Malicious Insider | Authorized user exfiltrates data via USB        | Two-person integrity, USB device control |
| Coerced Insider   | External actor forces insider to extract data   | Psychological screening, monitoring      |
| Negligent Insider | Accidental data exposure through poor practices | Training, procedures, technical controls |

<details>

<summary>🚨 Notable Insider Threat Cases</summary>

* Edward Snowden (2013): used removable media to exfiltrate data from air-gapped networks.
* Chelsea Manning (2010): extracted classified documents from SIPRNET using writable media.

</details>

Physical tampering threats:

* Hardware keyloggers
* Modified BIOS/UEFI firmware
* Supply chain attacks
* "Evil maid" attacks
* Covert channels via modified hardware (e.g., USB cables with wireless transmitters)

<details>

<summary>💀 Advanced Air-Gap Breach Methods (research)</summary>

Examples include:

* Stuxnet (USB delivery into air-gapped ICS)
* AirHopper (FM signals via video cables)
* Fansmitter (acoustic via fan noise)
* USBee (EM emissions via USB)
* MOSQUITO (ultrasonic speakers/microphones)
* PowerHammer (power-line exfiltration)
* MAGNETO & ODINI (magnetic field exfiltration)

</details>

{% hint style="warning" %}
Defense Strategy: Multiple layers of physical security, procedural controls, and monitoring are required to maintain air-gap integrity against sophisticated adversaries.
{% endhint %}

**Essential Security Protocols**

{% stepper %}
{% step %}
#### Data Transfer Procedures (Strict Manual Media Controls)

Converted the numbered checklist into a stepper for clarity and procedures:

1. All incoming media scanned on dedicated scanning station (itself air-gapped from both networks)
2. Media scanned with multiple antivirus engines
3. File integrity verification (cryptographic checksums)
4. Media contents transferred via read-only process
5. Chain-of-custody documentation for all transfers
6. Media destroyed or securely stored after transfer
{% endstep %}
{% endstepper %}

{% stepper %}
{% step %}
#### Access Control Enforcement

Multi-layer access controls:

* Physical access requires multiple authorization levels
* Two-person integrity for sensitive operations
* Continuous video surveillance with long-term retention
* All physical access logged and reviewed
* Background checks and security clearances required
* Regular polygraph testing for high-security environments
{% endstep %}

{% step %}
#### Monitoring and Auditing

Comprehensive security monitoring:

* Host-based intrusion detection systems (HIDS)
* File integrity monitoring (AIDE, Tripwire)
* USB device logging and auditing
* Electromagnetic emanation monitoring (TEMPEST)
* Behavioral analytics for insider threat detection
* Regular security audits and penetration testing
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

***

## 2️⃣ What is the Purpose of Air-Gapped Systems?

{% hint style="success" %}
Primary Purpose

Air-gapped systems are commonly used to protect highly sensitive data and critical infrastructure, such as military secrets, financial records, and systems controlling utilities or nuclear power. They serve as a last line of defense against sophisticated cyberattacks, malware, and remote exploitation, significantly reducing the attack surface.
{% endhint %}

### Common Use Cases

{% tabs %}
{% tab title="🏛️ Government & Military" %}
**Government and Military Applications**

Classified information systems:

* National security secrets, intelligence data
* Military command and control (nuclear weapon systems, ops centers)
* Cryptographic key management
* Secure communications (diplomatic, classified email)

Example: SIPRNET (Secret Internet Protocol Router Network)

* Air-gapped from public internet (NIPRNET)
* Handles SECRET-level classified information

Security classification levels often map to different isolation levels (UNCLASSIFIED → no air-gap, SECRET/TOP SECRET → physical air-gap/enhanced isolation).
{% endtab %}

{% tab title="🏭 Critical Infrastructure" %}
**Critical Infrastructure Protection**

Industrial Control Systems (ICS) / SCADA protection for:

* Energy (nuclear, grid management)
* Water/wastewater treatment
* Transportation (air traffic control, rail signaling)
* Manufacturing (assembly lines, pharma production)

{% stepper %}
{% step %}
#### Prevent Remote Cyber Attacks

Example: Stuxnet highlighted that physical isolation alone is not sufficient; defense-in-depth is required.
{% endstep %}

{% step %}
#### Ensure Safety and Reliability

Control systems must remain predictable and resilient; air-gapping reduces external interference.
{% endstep %}

{% step %}
#### Regulatory Compliance

Standards such as NERC CIP, NIST CSF, IEC 62443, CFATS often require or recommend isolation controls.
{% endstep %}
{% endstepper %}

<details>

<summary>⚠️ Real-World Infrastructure Attack Examples</summary>

* Ukraine Power Grid attacks (2015/2016) — lateral movement into ICS.
* Colonial Pipeline (2021) — IT compromised; OT air-gap helped limit impact.
* Triton/Trisis (2017) — targeted safety systems, likely via engineering workstation bridging IT/OT.

</details>

{% hint style="danger" %}
Critical Infrastructure Risk: Even momentary connection to external networks (for updates, remote support) can compromise air-gap integrity. Strict procedural controls are essential.
{% endhint %}
{% endtab %}

{% tab title="💰 Financial Services" %}
**Financial Services and Banking**

Air-gapped systems protect:

* Payment processing (SWIFT, Fedwire)
* Trading infrastructure and proprietary algorithms
* Cryptographic key management
* Archival and regulatory reporting systems

Real-world lesson: Bangladesh Bank Heist (2016) — shows need for maximal isolation of critical payment systems.

Compliance frameworks relevant: PCI DSS, FFIEC guidance, GLBA, SOX, SWIFT CSP.
{% endtab %}

{% tab title="🔬 Research & Healthcare" %}
**Research and Healthcare**

Protects:

* Pharmaceutical and medical research (drug formulas, clinical trial data)
* Patient data (PHI under HIPAA)
* Defense and aerospace R\&D

Air-gapped backups and isolated analysis systems reduce risk of ransomware and data exfiltration. Example: WannaCry (2017) — air-gapped backups could have mitigated disruption.
{% endtab %}

{% tab title="🗳️ Election Systems" %}
**Election Infrastructure**

Air-gapped voting systems protect:

* Voting machine tabulation systems
* Voter registration master copies
* Tabulation and audit systems

{% stepper %}
{% step %}
#### Prevent Foreign Interference

No network connectivity prevents remote manipulation of results.
{% endstep %}

{% step %}
#### Ensure Voter Confidence

Air-gap provides demonstrable security and supports paper/auditable trails.
{% endstep %}

{% step %}
#### Regulatory Compliance

Standards: EAC Testing/Certification, NIST guidance, CISA recommendations.
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

### Attack Surface Reduction

Air-gapping eliminates entire categories of cyber threats by removing the network attack surface. Remaining risks require physical and procedural controls.

{% hint style="success" %}
Security Principle: Air-gapping eliminates entire categories of cyber threats by removing the network attack surface. Remaining risks require physical and procedural controls.
{% endhint %}

***

## 3️⃣ Common Themes: Bastion Hosts and Jump Servers

{% hint style="info" %}
Bastion systems (bastion hosts, jump boxes, jump servers) are hardened computers designed to withstand attacks and provide secure, controlled access to private networks from external or untrusted networks.
{% endhint %}

### Core Concept: Fortified Gateway

Bastion hosts are fortified gateways positioned at network perimeters (DMZs or public subnets) and act as the single point of entry for remote administrative access.

{% tabs %}
{% tab title="🏰 Architecture & Positioning" %}
**Strategic Network Positioning**

Bastion hosts maintain dual connectivity:

* External interface (public/untrusted)
* Internal interface (private/trusted)

They minimize direct exposure of internal systems by consolidating access and enforcing controls.

{% code title="Bastion Host Network Architecture" overflow="wrap" %}
```
Internet (Untrusted)
    ↓
Firewall (Border Router)
    ↓
DMZ / Public Subnet
    ↓
[Bastion Host] ← Single Point of Entry
├─ Public IP: 203.0.113.50 (external interface)
├─ Private IP: 10.0.1.100 (internal interface)
└─ Hardened, minimal services
    ↓
Internal Firewall
    ↓
Private Network (10.0.10.0/24)
├─ Application Servers
├─ Databases
├─ Internal Services
└─ Management Systems

Remote Administrator
    ↓
SSH to Bastion Host (203.0.113.50)
    ↓
SSH from Bastion to Internal Server (10.0.10.15)
```
{% endcode %}

Common bastion architecture types (single-homed, dual-homed, screened host, screened subnet/DMZ, virtual/cloud) provide varying depth of defense.
{% endtab %}

{% tab title="🔒 Hardening & Security" %}
**Bastion Host Hardening**

Principles:

* Minimal services — remove everything not required
* Harden SSH configuration
* Strict firewall rules
* OS hardening (CIS/STIG, SELinux, kernel sysctl)
* Session recording and strict logging

Examples include disabling unnecessary services, securing /etc/ssh/sshd\_config, applying kernel hardening, and using AIDE/oscap for integrity checks.

{% hint style="success" %}
Hardening Result: Minimal attack surface, defense-in-depth security controls, and compliance with security standards.
{% endhint %}
{% endtab %}

{% tab title="🎫 Access Control" %}
**Access Control and Authentication**

Multiple layers:

* IP whitelisting (security groups, firewalld, hosts.allow)
* SSH key-based authentication and key management best practices
* Multi-Factor Authentication (TOTP, hardware keys, Duo/Okta)
* Principle of Least Privilege (sudoers per-role)
* Session management and recording (script, tlog)

Session recording supports audit, forensics, and compliance.
{% endtab %}

{% tab title="📊 Monitoring & Maintenance" %}
**Monitoring, Logging, and Continuous Maintenance**

Key areas:

* Detailed activity logging (SSH verbose, auditd rules)
* Centralized log management (rsyslog to SIEM: Splunk, Elastic, CloudWatch)
* Real-time monitoring and alerting (fail2ban, SIEM rules)
* Host-based intrusion detection (AIDE, OSSEC)
* Scheduled maintenance and patching (daily/weekly/monthly/quarterly/annual tasks)
* Automated update scripts to apply security updates and report reboots required

Maintenance checklist and automation help enforce continuous security posture.
{% endtab %}

{% tab title="☁️ AWS Best Practices" %}
**AWS Bastion Host Best Practices**

Recommendations:

* Deploy in public subnets with auto-scaling for availability
* Strict security group rules (SSH/RDP only from specific IPs)
* Consider AWS Systems Manager Session Manager as a modern alternative (no inbound ports, IAM-based access, session logging)
* Use encryption and centralized logging (CloudWatch, S3, CMKs)
* Multi-account architecture for separation of concerns
* Ephemeral bastions (spin-up on-demand) to reduce persistent attack surface
{% endtab %}

{% tab title="📖 NIST & Standards" %}
**NIST Guidance and Industry Standards**

NIST SP 800-123 defines bastion hosts as "special purpose computers on a network specifically designed and configured to withstand attacks." NIST guidance recommends minimal services, enhanced logging, network isolation, patching, and monitoring.

NIST SP 800-190 provides container/Kubernetes control-plane access guidance (use bastion servers to consolidate and secure SSH access to cluster components).

Industry standards mapping:

* CIS Benchmarks, PCI DSS, HIPAA, SOC 2, ISO 27001, FedRAMP — all support bastion-like controls for access, logging, and auditing.
{% endtab %}

{% tab title="🔄 Bastion vs Jump Server" %}
**Bastion Hosts vs. Jump Servers**

Differences (summary):

* Bastion Host: internet-facing perimeter, maximum hardening
* Jump Server: internal bridge between security zones, hardened but typically internal-facing

Many organizations use both in a layered architecture: Remote Admin → Bastion → Jump Server → Production.
{% endtab %}

{% tab title="🐧 Red Hat Perspective" %}
**Red Hat's Guidance**

Red Hat documents air-gapped deployments and bastion best practices for RHEL and OpenShift:

* Download packages/images on connected systems; transfer via secure media
* Set up local mirrors and registries in air-gapped environments
* Use Red Hat Satellite for content lifecycle and patch management
* Use bastion nodes as centralized access points for OpenShift/RHEL; prefer API/oc commands and reserve SSH for emergency/debugging
* Use ProxyJump (ssh -J) for automated multi-hop SSH workflows

Example steps include reposync/createrepo for offline package mirrors, container image mirroring for OpenShift, and labeling/tainting bastion nodes in OpenShift.
{% endtab %}
{% endtabs %}

***

## 📝 Key Takeaways

{% hint style="success" %}
Summary: Air-Gapped Systems and Bastion Hosts

* Air-Gap Core Concept: Complete network isolation to protect highly sensitive data and critical infrastructure.
* Key Air-Gap Principles: Physical isolation, restricted connectivity, unidirectional data flow (data diodes).
* Modern Variant: AWS logical air-gap (separate accounts, vault lock, service-owned keys, multi-party approval).
* Limitations: Not immune to insider threats, supply-chain, or covert channels — requires strict protocols.
* Primary Use Cases: Government/military, critical infrastructure (ICS/SCADA), financial services, research & healthcare, election systems.
* Bastion Hosts: Hardened single entry points (DMZ) with minimal services, strong access controls, logging, and monitoring.
* AWS Best Practices: Consider Session Manager, multi-account architectures, ephemeral bastions, centralized logging & CMKs.
* Standards: Align with NIST, CIS, PCI DSS, HIPAA, ISO 27001, FedRAMP, etc.
* Core Security Principles: Defense-in-depth, least privilege, continuous monitoring, regular updates, audit everything.
{% endhint %}

***

## 🔗 Additional Resources

Air-Gapped Systems

* https://www.ibm.com/think/topics/air-gap
* https://www.redhat.com/en/blog/air-gapped-networks
* https://aws.amazon.com/blogs/storage/building-cyber-resiliency-with-aws-backup-logically-air-gapped-vault/
* https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-123.pdf

Bastion Hosts and Jump Servers

* https://aws.amazon.com/quickstart/architecture/linux-bastion/
* https://www.redhat.com/en/blog/ssh-proxy-bastion-proxyjump
* https://hoop.dev/blog/jump-server-vs-bastion-host-essential-security-tools-for-technology-managers/
* https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html

Security Standards and Hardening

* https://www.cisecurity.org/cis-benchmarks/
* https://csrc.nist.gov/publications/detail/sp/800-190/final
* https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/9/html/security\_hardening/index

Advanced Security Topics

* https://en.wikipedia.org/wiki/Tempest\_(codename)
* https://cyber.bgu.ac.il/advanced-cyber/airgap
* https://csrc.nist.gov/publications/detail/sp/800-207/final

***
