---
cover: ../../.gitbook/assets/Screenshot from 2025-11-17 21-35-47.png
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

# 📢 Security Unit 3 Discussion Post 2

> _**Read about Active Directory (or LDAP) configurations of Linux via SSSD:**_ [_**https://docs.rockylinux.org/guides/security/authentication/active\_directory\_authentication/**_](https://docs.rockylinux.org/guides/security/authentication/active_directory_authentication/)

***

## 📚 Research References

1. https://docs.rockylinux.org/guides/security/authentication/active\_directory\_authentication/
2. https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/9/html/configuring\_authentication\_and\_authorization\_in\_rhel/index
3. https://sssd.io/docs/introduction.html
4. https://web.mit.edu/kerberos/krb5-latest/doc/
5. https://pages.nist.gov/800-63-3/
6. https://www.stigviewer.com/stig/red\_hat\_enterprise\_linux\_9/

***

## 1️⃣ Why Not Use Local Authentication?

{% hint style="danger" %}
**Critical Enterprise Reality**

Nothing is wrong with local authentication, until you have more than one system. In enterprise environments that usually have more than one server, identity must be centrally managed. Relying on local authentication (`/etc/passwd`) immediately creates an unmanageable environment full of security risks and administrative overhead.
{% endhint %}

### The Centralized Identity Solution

{% hint style="success" %}
**Modern Enterprise Architecture**

This forces the adoption of centralized solutions like Active Directory (AD) or Identity Management (IdM), typically mediated by the System Security Services Daemon (SSSD).

Technology Stack:

* DNS — Discovery
* Kerberos — Authentication
* LDAP — Directory services
* SSSD — Universal translator and caching layer

This tech stack enables secure, scalable, and manageable Linux infrastructure. You must plan for scale from the beginning.
{% endhint %}

### Why Local Authentication Is Unsuitable

{% tabs %}
{% tab title="⏱️ Administrative Overhead" %}
**Administrative Overhead and Efficiency**

Core Problem: Using local authentication, the administrative workload increases linearly with every new server you deploy.

Decentralized Management Challenges:

* Manage users separately on every single machine
* Set authentication policies independently on each server
* Maintain passwords individually across the infrastructure
* Manually synchronize changes across the entire fleet

Real-World Impact:

<details>

<summary>📊 Administrative Time Comparison</summary>

Scenario: Onboarding a New Employee

| Task                     |                  Local Authentication | Centralized (AD/IdM) |
| ------------------------ | ------------------------------------: | -------------------: |
| Create user account      | 10 min × 50 servers = **500 minutes** |   5 minutes **once** |
| Assign group memberships |  5 min × 50 servers = **250 minutes** |   2 minutes **once** |
| Configure sudo access    | 10 min × 50 servers = **500 minutes** |   5 minutes **once** |
| **Total Time**           |      **\~1,250 minutes (20.8 hours)** |     **\~12 minutes** |

Scenario: Employee Departure

| Task                     |                 Local Authentication |        Centralized (AD/IdM) |
| ------------------------ | -----------------------------------: | --------------------------: |
| Delete user account      | 5 min × 50 servers = **250 minutes** |          2 minutes **once** |
| Verify deletion          | 2 min × 50 servers = **100 minutes** | Audit log review: 5 minutes |
| Revoke certificates/keys | 5 min × 50 servers = **250 minutes** |        Automatic revocation |
| **Total Time**           |         **\~600 minutes (10 hours)** |             **\~7 minutes** |

</details>

Wasted Time - Common Scenarios:

{% stepper %}
{% step %}
### New Employee Onboarding

Local Auth: Manually create user account on every Linux server they need access to.

{% code title="Manual Account Creation (Repeated 50+ times)" %}
```bash
# Server 1
useradd -m -s /bin/bash alice
passwd alice
usermod -aG wheel,developers alice

# Server 2
useradd -m -s /bin/bash alice
passwd alice
usermod -aG wheel,developers alice

# Server 3... Server 50...
```
{% endcode %}
{% endstep %}

{% step %}
### Employee Departure

Local Auth: Manually delete the account everywhere.

{% code title="Manual Account Deletion (Repeated 50+ times)" %}
```bash
userdel -r alice  # Server 1
userdel -r alice  # Server 2
# What about that undocumented development server?
```
{% endcode %}
{% endstep %}

{% step %}
### Password Change

Local Auth: User might have to change password dozens of times across different systems.

User Experience example:

```
User: "I changed my password on the web server but it still doesn't work on the database server"
Admin: "You need to change it on each server separately"
User: "How many servers do I have access to?"
Admin: "...good question. Let me check."
```

Result: Password fatigue → weak passwords → security compromise
{% endstep %}
{% endstepper %}

Centralized Control with IdM/AD:

Tools like `realmd` and SSSD allow your Linux system to join a domain (like AD), turning your servers into clients that pull identity information from a single, central repository.

{% code title="Join Linux to AD Domain (One-Time Setup)" %}
```bash
# Install required packages
dnf install realmd sssd oddjob oddjob-mkhomedir adcli samba-common-tools

# Discover domain
realm discover EXAMPLE.COM

# Join domain (one command per server, automated via provisioning)
realm join --user=admin EXAMPLE.COM
```
{% endcode %}

{% hint style="success" %}
Result: This significantly reduces administrative overhead from hours/days to minutes for identity management tasks.
{% endhint %}
{% endtab %}

{% tab title="📋 Policy Consistency" %}
**Policy and Authorization Consistency**

Core Problem: With compliance and security best practices (principle of least privilege), you cannot rely on local files to consistently enforce access rules across hundreds of machines.

Inconsistent Policies:

<details>

<summary>⚠️ Configuration Drift Example</summary>

Scenario: 50 Linux servers with local authentication

{% code title="Inconsistent sudoers Files" %}
```bash
# Server 1: /etc/sudoers
alice ALL=(ALL) ALL

# Server 2: /etc/sudoers  
alice ALL=(ALL) NOPASSWD: ALL  # ⚠️ No password required!

# Server 3: /etc/sudoers
bob ALL=(ALL) ALL  # ⚠️ Wrong user (copy-paste error)

# Server 4: /etc/sudoers
# alice not present  # ⚠️ Forgot to add user
```
{% endcode %}

Problems:

* Different privilege levels across servers
* Copy-paste errors create security holes
* No central audit of who has what access
* Difficult to verify compliance
* Configuration drift inevitable over time

</details>

Centralized Policy Management:

SSSD/IdM/AD allow for uniform policy enforcement.

{% stepper %}
{% step %}
### Access Control via SSSD

SSSD allows you to enforce access controls by whitelisting specific AD groups (e.g., `simple_allow_groups`) that are permitted to log in, denying access to everyone else at the PAM layer.

{% code title="/etc/sssd/sssd.conf - Group-Based Access Control" %}
```ini
[domain/EXAMPLE.COM]
id_provider = ad
auth_provider = ad
access_provider = ad

# Only allow specific AD groups
ad_access_filter = (memberOf=CN=Linux-Admins,OU=Groups,DC=example,DC=com)

# Alternative: simple access provider
[domain/EXAMPLE.COM]
access_provider = simple
simple_allow_groups = linux-admins, developers, sre-team
```
{% endcode %}
{% endstep %}

{% step %}
### Privilege Escalation Management

IdM can centrally manage `sudo` rules.

{% code title="Centralized Sudo Rules (IdM)" %}
```bash
ipa sudorule-add webserver-restart
ipa sudorule-add-user webserver-restart --groups=webadmins
ipa sudorule-add-host webserver-restart --hostgroups=webservers
ipa sudorule-add-allow-command webserver-restart --sudocmds='/usr/bin/systemctl restart httpd'
```
{% endcode %}

Benefits:

* Single source of truth for sudo policies
* Audit trail of policy changes in IdM
* Automatic propagation to all systems
* Host-based and user-based restrictions
* Command-level granularity
{% endstep %}

{% step %}
### SELinux/Mandatory Access Control

Central systems manage SELinux user mappings.

{% code title="Centralized SELinux User Mapping" %}
```bash
# Define SELinux user mappings in IdM
# AD user "alice" maps to SELinux user "staff_u" on all systems
# Avoid manual per-host semanage commands
```
{% endcode %}
{% endstep %}
{% endstepper %}

{% hint style="success" %}
Result: Tight, centralized access control with consistent policy enforcement across the entire infrastructure.
{% endhint %}

Policy Compliance Verification:

| Aspect      |              Local Authentication |          Centralized (AD/IdM) |
| ----------- | --------------------------------: | ----------------------------: |
| Consistency |      Manual verification required | Automatic, enforced centrally |
| Audit Trail |     Scattered logs across systems |        Centralized audit logs |
| Compliance  |                Difficult to prove |        Single policy database |
| Updates     | Propagate manually to all systems |         Automatic propagation |
{% endtab %}

{% tab title="🔐 Security & Audit" %}
**Enhanced Security and Auditability**

Core Problem: Local authentication introduces several critical security risks that centralized identity systems mitigate.

Password Sprawl and Risk:

* Users may write passwords down, reuse weak credentials, or share passwords.

{% hint style="danger" %}
Security Reality: Password complexity without centralization = users writing passwords down
{% endhint %}

Single Sign-On (SSO):

Centralized Kerberos integration provides SSO capabilities.

{% code title="Kerberos SSO Workflow" %}
```
1. User logs in to workstation
2. Authenticate to Key Distribution Center (KDC)
3. Receive Ticket Granting Ticket (TGT)
4. Access multiple services without re-authentication (SSH, NFS, LDAP, web apps, DBs)
```
{% endcode %}

Benefits of SSO:

* Reduced attack surface
* Better user experience (no password fatigue)
* Centralized revocation
* Centralized audit trail
* Time-limited access via tickets

<details>

<summary>🎫 Kerberos Ticket Lifecycle</summary>

{% code title="Kerberos Authentication Flow" %}
```bash
# Initial authentication
$ kinit alice@EXAMPLE.COM
Password for alice@EXAMPLE.COM: ****

# View current tickets
$ klist

# SSH to server (no password prompt - uses Kerberos ticket)
$ ssh server1.example.com
```
{% endcode %}

Security Features:

* Tickets time-limited (default: 10 hours)
* Can be renewed without password
* Centrally revoked if needed
* Encrypted communication
* Mutual authentication

</details>

Auditing:

Centralized systems provide a single source of truth for login/authentication events.

{% code title="Centralized vs. Distributed Logging" %}
```bash
# LOCAL AUTHENTICATION:
for server in server1 server2 ...; do
  ssh $server "grep alice /var/log/secure"
done

# CENTRALIZED AUTHENTICATION:
ipa user-find alice --all
# Or query SIEM ingesting domain controller logs
```
{% endcode %}

Compliance Benefit: Centralized audit logs are crucial for monitoring, incident response, and regulatory compliance (SOX, PCI-DSS, HIPAA).

<details>

<summary>🚨 Breach Scenario Comparison</summary>

Scenario: Compromised credentials detected

Local Authentication Response (manual, slow):

1. Identify systems user has access to
2. SSH to each, lock account, kill sessions, remove keys
3. Review logs per-host Estimated time: hours

Centralized Authentication Response (fast):

1. Disable user in AD/IdM (one command)
2. Active sessions invalidated or revocation enforced
3. Query central logs for activity Estimated time: minutes

</details>
{% endtab %}

{% tab title="📈 Scalability & Reliability" %}
**Scalability and Reliability in Production**

Core Problem: Scaling infrastructure rapidly is impossible when relying on local management.

{% hint style="info" %}
Scaling Guidelines:

* < 30-50 systems: Direct AD integration might be considered
* 50-500 systems: IdM as intermediary recommended
* 500+ systems: IdM essential
* Rapid growth anticipated: Use IdM from the start
{% endhint %}

<details>

<summary>📊 Infrastructure Growth Reality</summary>

Typical Growth Pattern:

|   Year | Server Count | Admin Team Size | Management Approach   |
| -----: | -----------: | --------------: | --------------------- |
| Year 1 |           10 |               1 | Local auth "works"    |
| Year 2 |           50 |               2 | Local auth struggling |
| Year 3 |          200 |               3 | Local auth crisis     |
| Year 4 |          500 |               5 | Must centralize       |

</details>

Identity Model Mismatch:

Linux uses UIDs/GIDs while AD uses SIDs. Managing these manually leads to file ownership chaos.

{% code title="Identity Model Mismatch Problem" %}
```
Server 1: alice = UID 1001
Server 2: alice = UID 1005  ⚠️ DIFFERENT!
Result: File ownership chaos across systems
```
{% endcode %}

SSSD solves this by:

1. POSIX ID Mapping (automatic mapping of SID → consistent UID/GID)

{% code title="Automatic ID Mapping" %}
```ini
[domain/EXAMPLE.COM]
id_provider = ad
ldap_id_mapping = true
```
{% endcode %}

2. Explicit POSIX Attributes (AD defines uidNumber/gidNumber)

{% code title="Explicit POSIX Attributes in AD" %}
```ini
[domain/EXAMPLE.COM]
id_provider = ad
ldap_id_mapping = false
# AD user object: uidNumber: 5001, gidNumber: 5000
```
{% endcode %}

{% hint style="success" %}
Critical: Local systems cannot perform consistent mapping across hundreds of servers.
{% endhint %}

Offline Operation (SSSD Cache):

SSSD can authenticate using cached credentials when AD/IdM is unreachable.

{% code title="SSSD Caching Behavior" %}
```
Normal: User Login → SSSD → AD/IdM → Authenticate → Cache Result → Allow Access

Network Outage: User Login → SSSD → AD/IdM (unreachable) → Check Cache → Allow Access
```
{% endcode %}

Example cache settings:

{% code title="/etc/sssd/sssd.conf - Cache Configuration" %}
```ini
[domain/EXAMPLE.COM]
id_provider = ad
auth_provider = ad
cache_credentials = true
offline_credentials_expiration = 0  # Cache doesn't expire
entry_cache_timeout = 5400  # 90 minutes
```
{% endcode %}

Cache Consideration: Disabled accounts may remain accessible until cache expires. For high-security environments, configure shorter cache timeouts.

<details>

<summary>🏢 Enterprise Scale Example</summary>

Scenario: Global corporation with 10,000 Linux servers.

With Local Authentication: Onboarding/offboarding requires thousands of manual changes — unmanageable.

With Centralized Identity: Create/disable in AD, affects everything — manageable.

Administrative Time Savings: Local: 10,000 hours/year vs Centralized: 100 hours/year

</details>

{% hint style="success" %}
Scalability Principle: Centralized identity management scales logarithmically while local authentication scales linearly (or worse).
{% endhint %}
{% endtab %}
{% endtabs %}

***

## 2️⃣ SSSD STIGs: What Are They and What Do They Do?

{% hint style="info" %}
STIG Overview

There are 5 SSSD STIGs for RHEL 9. These STIGs implement multi-factor authentication and PKI-based security controls.
{% endhint %}

### SSSD STIG Requirements

{% tabs %}
{% tab title="🎫 V-258122 - Smart Card Auth" %}
**V-258122: Enable Certificate-Based Smart Card Authentication**

{% hint style="danger" %}
Requirement: RHEL 9 must enable certificate-based smart card authentication.
{% endhint %}

Rationale: Multifactor authentication (e.g., DOD CAC) mitigates access risks for privileged accounts.

Control Correlation Identifiers (CCIs):

<details>

<summary>📋 CCI Mappings</summary>

* CCI-000765: Implement multifactor authentication for network access to privileged accounts.
* CCI-004046: Implement multi-factor authentication such that one factor is provided by a device separate from the system.
* CCI-004047: Ensure device meets organization-defined strength.
* CCI-001948: Implement multifactor authentication for remote access to privileged accounts.

</details>

What This STIG Does:

{% stepper %}
{% step %}
#### Enables PKI-Based Authentication

Configures the system to accept X.509 certificates from smart cards.

{% code title="SSSD Smart Card Configuration" %}
```ini
[pam]
pam_cert_auth = True
pam_cert_db_path = /etc/sssd/pki/sssd_auth_ca_db.pem

[domain/EXAMPLE.COM]
id_provider = ad
auth_provider = ad
pam_cert_auth = True
certificate_verification = ocsp_dgst=sha256
```
{% endcode %}
{% endstep %}

{% step %}
#### Configures PAM for Certificate Authentication

Example PAM snippet to try certificate auth via SSSD.

{% code title="/etc/pam.d/system-auth" %}
```bash
auth        [success=done ignore=ignore default=die] pam_sss.so try_cert_auth
auth        required      pam_deny.so
```
{% endcode %}
{% endstep %}

{% step %}
#### Enforces Multi-Factor Authentication

Smart card provides:

* Something you have: smart card
* Something you know: PIN
{% endstep %}
{% endstepper %}

System Impact:

* Security: Strong authentication
* Compliance: Meets DoD/federal requirements
* Infrastructure: Requires smart card readers and PKI
* User Experience: Users need issued smart cards

{% hint style="success" %}
Security Benefit: Even if an attacker obtains a password, they cannot authenticate without the physical smart card.
{% endhint %}
{% endtab %}

{% tab title="📜 V-258123 - Certificate Status" %}
**V-258123: Implement Certificate Status Checking**

{% hint style="danger" %}
Requirement: RHEL 9 must implement certificate status checking for multifactor authentication.
{% endhint %}

Rationale: Ensure authenticity of smart card certificates via OCSP/CRL.

CCIs:

<details>

<summary>📋 CCI Mappings</summary>

* CCI-004046: Multi-factor authentication where one factor is a separate device.
* CCI-001954: Electronically verifies PIV-compliant credentials.
* CCI-001948: Multifactor for remote privileged access.

</details>

What This STIG Does:

{% stepper %}
{% step %}
#### Enables OCSP Certificate Validation

SSSD performs OCSP checks to confirm certificates are not revoked.

{% code title="SSSD OCSP Configuration" %}
```ini
[pam]
pam_cert_auth = True

[sssd]
certificate_verification = ocsp_dgst=sha256
```
{% endcode %}
{% endstep %}

{% step %}
#### Uses SHA-256 Digest Function

Ensures cryptographic strength in validation.

{% code title="Cryptographic Strength" %}
```ini
certificate_verification = ocsp_dgst=sha256
```
{% endcode %}
{% endstep %}

{% step %}
#### Validates Certificate Chain

Verifies chain of trust to trusted root.
{% endstep %}
{% endstepper %}

System Impact:

* Security: Prevents use of revoked certificates
* Network Dependency: Requires connectivity to OCSP responder
* Performance: Small added latency to authentication

{% hint style="warning" %}
Network Consideration: If OCSP responder is unreachable, configure appropriate fallback (fail open/closed) per risk appetite.
{% endhint %}
{% endtab %}

{% tab title="🔗 V-258131 - Certification Path" %}
**V-258131: Validate Certificates with Certification Path**

{% hint style="danger" %}
Requirement: Validate certificates by constructing a certification path to a trusted anchor, including status info.
{% endhint %}

Rationale: Path validation is necessary for informed trust decisions.

CCIs:

<details>

<summary>📋 CCI Mappings</summary>

* CCI-000185: Validate certification path including status checks.
* CCI-004068: Implement local cache of revocation data.
* CCI-001991: Local cache to support path discovery when network is unavailable.

</details>

What This STIG Does:

{% stepper %}
{% step %}
#### Constructs Complete Certification Path

Validates the entire chain (end-entity → intermediate → root).

{% code title="Certification Path Validation" %}
```
User Cert -> Intermediate CA -> Root CA (trust anchor)
Check: signatures, validity, revocation
```
{% endcode %}
{% endstep %}

{% step %}
#### Includes Status Information

Checks OCSP/CRL for each certificate in the chain.
{% endstep %}

{% step %}
#### Implements Local Revocation Cache

Maintains cached CRLs/OCSP responses for offline validation.

{% code title="/etc/sssd/sssd.conf - Revocation Cache" %}
```ini
[sssd]
certificate_verification = ocsp_dgst=sha256

[nss]
nssdb = /etc/pki/nssdb
# CRL cache location and updates handled by system tooling
```
{% endcode %}
{% endstep %}

{% step %}
#### Validates Trust Anchors

Ensure root CAs are from approved trust stores (e.g., /etc/pki/ca-trust/).

{% code title="Trust Anchor Management" %}
```bash
trust list
trust anchor /path/to/root-ca.crt
update-ca-trust
```
{% endcode %}
{% endstep %}
{% endstepper %}

System Impact:

* Security: Prevents trusting unauthorized certificates
* Management: Must maintain trust anchors and revocation caches

{% hint style="success" %}
Security Guarantee: Path validation ensures that every link in the trust chain is verified before allowing authentication.
{% endhint %}
{% endtab %}

{% tab title="🔐 V-258132 - Identity Mapping" %}
**V-258132: Map Authenticated Identity to User Account**

{% hint style="danger" %}
Requirement: Map the authenticated identity (certificate) to the user or group account for PKI-based authentication.
{% endhint %}

Rationale: Without mapping, forensic attribution is not possible.

CCI:

* CCI-000187: Map authenticated identity to account.

What This STIG Does:

{% stepper %}
{% step %}
#### Extracts Identity from Certificate

SSSD extracts fields such as DN, SAN (email), or UPN from X.509 certificates.

{% code title="Certificate Identity Fields" %}
```
Subject DN: CN=Alice Smith, OU=Engineering, O=Example Corp, C=US
SAN: email: alice@example.com, UPN: alice@EXAMPLE.COM
Serial Number: 1A:2B:3C:4D
```
{% endcode %}
{% endstep %}

{% step %}
#### Maps to Local/Domain Account

SSSD mapping rules allow mapping certificate identity to a system account.

{% code title="/etc/sssd/sssd.conf - Identity Mapping" %}
```ini
[certmap/EXAMPLE.COM/examplecerts]
matchrule = <SAN>.*@EXAMPLE.COM
maprule = (userPrincipalName={subject_principal})
```
{% endcode %}
{% endstep %}

{% step %}
#### Enables Audit Trail

Logs show certificate serial, UPN, mapped username, source IP, and timestamp.

{% code title="/var/log/secure - Certificate Authentication Log" %}
```
Nov 17 10:15:23 server1 sshd[12345]: Accepted certificate ID alice@EXAMPLE.COM (serial 1A:2B:3C:4D) for alice from 192.168.1.100 port 54321
```
{% endcode %}
{% endstep %}
{% endstepper %}

System Impact:

* Forensics: Clear attribution from certificate to system actions
* Accountability: Non-repudiation of authentication events

{% hint style="success" %}
Forensic Value: Identity mapping provides complete chain of attribution from physical smart card to system actions.
{% endhint %}
{% endtab %}

{% tab title="⏱️ V-258133 - Cache Expiration" %}
**V-258133: Prohibit Cached Authenticators After One Day**

{% hint style="danger" %}
Requirement: RHEL 9 must prohibit use of cached authenticators after one day.
{% endhint %}

Rationale: Prevent authentication with stale credentials.

CCI:

* CCI-002007: Prohibit cached authenticators after defined time period.

What This STIG Does:

{% stepper %}
{% step %}
#### Configures Cache Timeout

Set offline credential expiration to 1 day.

{% code title="/etc/sssd/sssd.conf - Cache Timeout" %}
```ini
[pam]
offline_credentials_expiration = 1  # Days

[domain/EXAMPLE.COM]
entry_cache_timeout = 86400
cached_auth_timeout = 86400
```
{% endcode %}
{% endstep %}

{% step %}
#### Enforces Re-Authentication

After 24 hours, SSSD requires live AD/IdM authentication.

{% code title="Cache Expiration Workflow" %}
```
Day 1: Login → cached
Day 2 (25h later): Cache expired → must authenticate to AD/IdM
```
{% endcode %}
{% endstep %}

{% step %}
#### Prevents Stale Credentials

Ensures disabled accounts, password changes, and group changes are enforced within 24 hours.
{% endstep %}
{% endstepper %}

System Impact:

* Security: Limits window for compromised accounts
* Availability: Users cannot authenticate offline after 24 hours during outages
* Operational trade-offs must be considered

Recommended STIG-compliant example:

{% code title="/etc/sssd/sssd.conf - STIG-Compliant Caching" %}
```ini
[domain/EXAMPLE.COM]
id_provider = ad
auth_provider = ad
cache_credentials = true
offline_credentials_expiration = 1
offline_failed_login_attempts = 3
offline_failed_login_delay = 5
entry_cache_timeout = 86400

[pam]
cached_auth_timeout = 86400
```
{% endcode %}

Verification commands:

{% code title="Test Cache Expiration" %}
```bash
sssctl cache-expire -E
sssctl user-show alice
getent passwd alice
sss_cache -E
```
{% endcode %}

{% hint style="warning" %}
Operational Consideration: For sites with frequent network outages, 1-day cache expiration may cause issues. Document risk acceptance if longer timeout is required.
{% endhint %}
{% endtab %}
{% endtabs %}

***

## 📊 SSSD STIG Summary Table

| STIG ID  | Title                         | Primary Purpose                        | Key Configuration                             |
| -------- | ----------------------------- | -------------------------------------- | --------------------------------------------- |
| V-258122 | Enable Smart Card Auth        | Implement multi-factor authentication  | pam\_cert\_auth = True                        |
| V-258123 | Certificate Status Checking   | Validate certificate revocation status | certificate\_verification = ocsp\_dgst=sha256 |
| V-258131 | Certification Path Validation | Verify complete certificate chain      | Path construction + local revocation cache    |
| V-258132 | Identity Mapping              | Map certificate to system account      | Certificate mapping rules in sssd.conf        |
| V-258133 | Cache Expiration              | Limit cached credential validity       | offline\_credentials\_expiration = 1          |

***

## 📝 Key Takeaways

{% hint style="success" %}
Summary: Centralized Identity and SSSD STIGs

Why Centralized Identity is Essential:

* Administrative Overhead: Local auth scales poorly
* Inconsistent Policies: Configuration drift
* Security Risks: Password sprawl, orphaned accounts, poor auditability
* No Scalability: Impossible to manage 100+ systems manually

Centralized Identity Benefits:

* Efficiency: Logarithmic scaling, single point of management
* Consistency: Uniform policy enforcement via AD/IdM
* Security: SSO via Kerberos, centralized audit, rapid incident response
* Reliability: SSSD caching enables offline operation
* Scale: Manages thousands of systems with current admin teams

Technology Stack:

* DNS, Kerberos, LDAP, SSSD

SSSD STIG Requirements (high level):

* V-258122: Smart card authentication (PKI, 2FA)
* V-258123: Certificate status checking (OCSP, SHA-256)
* V-258131: Certification path validation (chain + revocation cache)
* V-258132: Identity mapping (certificate → account)
* V-258133: Cache expiration (24 hours)

Critical Success Factors:

1. Infrastructure Ready: DNS, Kerberos, LDAP/AD configured
2. PKI Infrastructure: For smart card authentication
3. SSSD Properly Configured: Implement STIGs
4. Testing: Online & offline authentication
5. Monitoring: SSSD logs, cert expirations, cache status
6. Documentation: Mapping rules, trust anchors, cache policies
{% endhint %}

***

## 🔗 Additional Resources

SSSD and Centralized Identity

* https://docs.rockylinux.org/guides/security/authentication/active\_directory\_authentication/
* https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/9/html/configuring\_authentication\_and\_authorization\_in\_rhel/index
* https://sssd.io/docs/introduction.html

PKI and Smart Card Authentication

* https://csrc.nist.gov/publications/detail/sp/800-73/4/final
* https://public.cyber.mil/pki-pke/
* https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/9/html/security\_hardening/configuring-smart-card-authentication\_security-hardening

Kerberos and SSO

* https://web.mit.edu/kerberos/
* https://web.mit.edu/kerberos/krb5-latest/doc/admin/index.html
* https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/9/html/configuring\_authentication\_and\_authorization\_in\_rhel/using-kerberos\_configuring-authentication-and-authorization-in-rhel

STIG Compliance

* https://www.stigviewer.com/stig/red\_hat\_enterprise\_linux\_9/
* https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final
* https://pages.nist.gov/800-63-3/sp800-63b.html
