---
cover: ../../.gitbook/assets/lab-ezgif.png
coverY: -223.2639998237675
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

# 🛠️ Session 3 Labs

This hands-on lab sequence makes us analyze PAM security controls, deploy an OpenLDAP directory service, and integrate our system using SSSD.



<details>

<summary>🏗️ What We'll Build</summary>

**Complete Identity and Access Management Stack**:

* [ ] Security-hardened PAM authentication configuration meeting DISA STIGs
* [ ] Production-grade OpenLDAP directory service with TLS encryption
* [ ] SSSD integration enabling centralized authentication
* [ ] Automated home directory provisioning for directory users



</details>

### ⏳ Lab Progression

You'll work through five progressive challenges, each building on the previous:

{% stepper %}
{% step %}
#### 🔍 **PAM Reconnaissance**  → Understand actual authentication architecture
{% endstep %}

{% step %}
#### 📋 **STIG Assessment** → Analyze authentication security controls
{% endstep %}

{% step %}
#### 🛡️ **PAM Hardening** → Implement security remediation systematically
{% endstep %}

{% step %}
#### 🏗️ **LDAP Deployment** → Build centralized directory infrastructure
{% endstep %}

{% step %}
#### 🔗 **SSSD Integration** → Connect systems to enterprise authentication
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Real-World Workflow**

In corporate environments, we won't build LDAP servers—IAM teams run Active Directory or managed directory services.

But you'll absolutely configure SSSD to integrate your Linux systems with their infrastructure. This lab teaches you both sides so you understand the complete authentication flow.
{% endhint %}

By the end, we'll understand:&#x20;

* The **authentication security controls** (PAM and password complexity) and the **centralized identity infrastructure** (LDAP directory services with SSSD integration).&#x20;
* And have a complete identity management solution that demonstrates both security compliance and enterprise integration capabilities.

***

### 🛠️ Labs to Build Here

{% stepper %}
{% step %}
#### 🔥 Pre-Lab Warm-Up: PAM Configuration Reconnaissance

Quick exploration of authentication system files.

Run through these commands to understand your system's authentication infrastructure:

{% code title="pam-recon.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Explore PAM configuration directory
ls -l /etc/pam.d/

# Question: What are the permissions and names of files? Can everyone read them?

# Examine SSH authentication configuration
cat /etc/pam.d/sshd

# Questions:

# - What information do you see in this file?

# - Does any of it look familiar to you?
```
{% endcode %}

<details>

<summary>🎯 What You're Looking For</summary>

**PAM Architecture Indicators**:

* **File permissions**: Are PAM configs world-readable? Why might this be acceptable?
* **Module stacking**: How PAM modules chain together for authentication
* **Control flags**: `required`, `requisite`, `sufficient`, `optional`—how they affect authentication flow
* **Module arguments**: Parameters that control authentication behavior

These baseline checks reveal how Linux handles authentication before integrating with enterprise directory services.

</details>

{% hint style="success" %}
✅ Outcome: Understanding of Linux authentication architecture and PAM module structure
{% endhint %}
{% endstep %}

{% step %}
#### 🔧 STIG Viewer Setup

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
**Tool Context**

You've already used STIG Viewer in Session 2—now you're applying it to authentication and access control STIGs, a critical security domain in enterprise environments.
{% endhint %}

{% hint style="success" %}
✅ Outcome: STIG Viewer environment ready for authentication security assessment
{% endhint %}
{% endstep %}

{% step %}
#### 🔐 PAM Configuration STIG Remediation

Hands-on authentication security hardening.

Connect to your lab server and begin systematic STIG assessment focusing on authentication controls.

**Filter by PAM**

{% code overflow="wrap" %}
```bash
# Filter STIG Viewer by "pam" keyword

# Question: How many STIGs do you see? (Why is it really only 16?)
```
{% endcode %}

**Examine and Remediate V-257986: SSH PAM Authentication**

{% tabs %}
{% tab title="🔍 Analysis" %}
**What is the problem?**

**What is the fix?**

**What type of control is being implemented?**

**Current Status Check**:

{% code overflow="wrap" %}
```bash
grep -i pam /etc/ssh/sshd_config
```
{% endcode %}

**Can you remediate this finding?**
{% endtab %}

{% tab title="🔧 Remediation" %}
Based on STIG requirements, configure SSH to use PAM authentication properly.

Review `/etc/ssh/sshd_config` and `/etc/pam.d/sshd` to understand the authentication flow.

After remediation:

{% code overflow="wrap" %}
```bash
# Restart SSH service
systemctl restart sshd

# Verify configuration
grep -i pam /etc/ssh/sshd_config
```
{% endcode %}
{% endtab %}
{% endtabs %}

**Examine and Remediate V-258055**

{% hint style="warning" %}
**Assignment**

Follow the same analysis pattern:

{% stepper %}
{% step %}
#### What is the problem?
{% endstep %}

{% step %}
#### What is the fix?
{% endstep %}

{% step %}
#### What type of control is being implemented?

**Critical thinking**: Are there any major implications to think about with this change on your system? Why or why not?
{% endstep %}

{% step %}
#### Is it set properly on your system?
{% endstep %}

{% step %}
#### How would you remediate this?
{% endstep %}
{% endstepper %}
{% endhint %}

**Examine and Remediate V-258098**

{% hint style="warning" %}
**Assignment**

Analyze this STIG using the systematic approach:

* What is the problem?
* What is the fix?
* What type of control is being implemented?
* Is it set properly on your system?
{% endhint %}

**Filter by Password Complexity**

{% code overflow="wrap" %}
```bash
# Filter STIG Viewer by "password complexity"

# Analysis questions:

# - How many password complexity STIGs are there?

# - What are the password complexity rules?

# - Are there any you haven't seen before?
```
{% endcode %}

<details>

<summary>🔑 Password Complexity Controls</summary>

Password complexity STIGs typically enforce:

* **Minimum length requirements**
* **Character diversity** (uppercase, lowercase, numbers, special characters)
* **Password history** (preventing reuse)
* **Maximum age and expiration**
* **Dictionary checks** (preventing common passwords)

These controls work together to prevent weak authentication credentials.

</details>

**Filter by SSSD**

{% code overflow="wrap" %}
```bash
# Filter STIG Viewer by "sssd"

# Analysis questions:

# - How many SSSD STIGs do you see?

# - What do these STIGs appear to be trying to do?

# - What types of controls are they?
```
{% endcode %}

<details>

<summary>🔐 SSSD Security Context</summary>

SSSD (System Security Services Daemon) STIGs typically address:

* **Secure LDAP connections** (TLS/SSL enforcement)
* **Credential caching security**
* **Authentication provider configuration**
* **Certificate validation**

SSSD is the bridge between your Linux system and enterprise directory services like LDAP or Active Directory.

</details>

{% hint style="success" %}
✅ Outcome: Systematic understanding of authentication security controls and their implementation
{% endhint %}
{% endstep %}

{% step %}
#### 🏗️ OpenLDAP Server Deployment

Build a production-grade LDAP directory service.

{% hint style="info" %}
**Real-World Context**

You'll likely not build an LDAP server in a real corporate environment since most use Active Directory.&#x20;

We're doing this for deep understanding of directory service architecture and the ability to integrate Linux systems with any directory backend.

**Simplified Setup**: There's a file at `/lab_work/identity_and_access_management.tar.gz` with pre-built LDIF files to reduce typing.
{% endhint %}

**Installation and Initial Configuration**

{% code title="ldap-install.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Stop warewulf client (if running)
systemctl stop wwclient

# Configure /etc/hosts for LDAP resolution

# Edit /etc/hosts and add (use your server IP):

# 192.168.200.151 hammer1 hammer1-default ldap.prolug.lan ldap

# Enable additional repository
dnf config-manager --set-enabled plus
dnf repolist

# Install OpenLDAP packages
dnf -y install openldap-servers openldap-clients openldap

# Start LDAP service
systemctl start slapd
ss -ntulp | grep 389

# Configure firewall
firewall-cmd --add-service={ldap,ldaps} --permanent
firewall-cmd --reload
firewall-cmd --list-all
```
{% endcode %}

**Generate Administrative Password**

{% code title="generate-ldap-password.sh" overflow="wrap" %}
```bash
# Generate hashed password (use: testpassword)
slappasswd

# Output example:

# {SSHA}wpRvODvIC/EPYf2GqHUlQMDdsFIW5yig
```
{% endcode %}

**Configure Root Password**

{% code title="changerootpass.ldif" %}
```ldif
dn: olcDatabase={0}config,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: {SSHA}vKobSZO1HDGxp2OElzli/xfAzY4jSDMZ
```
{% endcode %}

{% code title="apply-root-password.sh" overflow="wrap" %}
```bash
ldapadd -Y EXTERNAL -H ldapi:/// -f changerootpass.ldif
```
{% endcode %}

**Import LDAP Schemas**

{% code title="import-schemas.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Import required schemas for user/group management
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
```
{% endcode %}

**Configure Domain (USE YOUR GENERATED PASSWORD)**

{% code title="setdomain.ldif" %}
```ldif
dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by 
 dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
 read by dn.base="cn=Manager,dc=prolug,dc=lan" read by * none

dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=prolug,dc=lan

dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=Manager,dc=prolug,dc=lan

dn: olcDatabase={2}mdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}Uf13AbVHOcs/aDWJOvDxxfBSl3omExG2

dn: olcDatabase={2}mdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by
 dn="cn=Manager,dc=prolug,dc=lan" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=Manager,dc=prolug,dc=lan" write by * read
```
{% endcode %}

{% code title="apply-domain.sh" overflow="wrap" %}
```bash
ldapmodify -Y EXTERNAL -H ldapi:/// -f setdomain.ldif
```
{% endcode %}

**Verify Domain Configuration**

{% code title="verify-domain.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Search for naming contexts
ldapsearch -H ldap:// -x -s base -b "" -LLL "namingContexts"

# Expected output:

# dn:

# namingContexts: dc=prolug,dc=lan
```
{% endcode %}

**Create Organizational Structure**

{% code title="addou.ldif" %}
```ldif
dn: dc=prolug,dc=lan
objectClass: top
objectClass: dcObject
objectclass: organization
o: My prolug Organisation
dc: prolug

dn: cn=Manager,dc=prolug,dc=lan
objectClass: organizationalRole
cn: Manager
description: OpenLDAP Manager

dn: ou=People,dc=prolug,dc=lan
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=prolug,dc=lan
objectClass: organizationalUnit
ou: Group
```
{% endcode %}

{% code title="add-org-structure.sh" overflow="wrap" %}
```bash
ldapadd -x -D cn=Manager,dc=prolug,dc=lan -W -f addou.ldif

# Verify organizational units
ldapsearch -x -b "dc=prolug,dc=lan" ou
```
{% endcode %}

**Add Test User**

{% code title="generate-user-password.sh" overflow="wrap" %}
```bash
# Generate user password (use: testuser1234)
slappasswd
```
{% endcode %}

{% code title="adduser.ldif" %}
```ldif
dn: uid=testuser,ou=People,dc=prolug,dc=lan
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: testuser
sn: temp
userPassword: {SSHA}dk/Lks9078gfZQJ31ABvPpvKv3sHhr29
loginShell: /bin/bash
uidNumber: 15000
gidNumber: 15000
homeDirectory: /home/testuser
shadowLastChange: 0
shadowMax: 0
shadowWarning: 0

dn: cn=testuser,ou=Group,dc=prolug,dc=lan
objectClass: posixGroup
cn: testuser
gidNumber: 15000
memberUid: testuser
```
{% endcode %}

{% code title="add-user.sh" overflow="wrap" %}
```bash
ldapadd -x -D cn=Manager,dc=prolug,dc=lan -W -f adduser.ldif

# Verify user creation
ldapsearch -x -b "ou=People,dc=prolug,dc=lan"
```
{% endcode %}

**Secure LDAP with TLS**

{% code title="generate-tls-certs.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Generate self-signed certificate (accept all defaults)
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/pki/tls/ldapserver.key \
  -out /etc/pki/tls/ldapserver.crt

# Set ownership
chown ldap:ldap /etc/pki/tls/{ldapserver.crt,ldapserver.key}

# Verify permissions
ls -l /etc/pki/tls/ldap*
```
{% endcode %}

{% code title="tls.ldif" %}
```ldif
dn: cn=config
changetype: modify
add: olcTLSCACertificateFile
olcTLSCACertificateFile: /etc/pki/tls/ldapserver.crt
-
add: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/pki/tls/ldapserver.key
-
add: olcTLSCertificateFile
olcTLSCertificateFile: /etc/pki/tls/ldapserver.crt
```
{% endcode %}

{% code title="apply-tls.sh" overflow="wrap" %}
```bash
ldapadd -Y EXTERNAL -H ldapi:/// -f tls.ldif
```
{% endcode %}

**Configure LDAP Client for TLS**

{% code title="/etc/openldap/ldap.conf" %}
```ini
#
# LDAP Defaults

#
# See ldap.conf(5) for details

# This file should be world readable but not world writable.

TLS_CACERT /etc/pki/tls/ldapserver.crt
TLS_REQCERT never

# System-wide Crypto Policies
SASL_NOCANON on
```
{% endcode %}

{% code title="restart-ldap.sh" %}
```bash
systemctl restart slapd
```
{% endcode %}

{% hint style="success" %}
✅ Outcome: Fully operational OpenLDAP directory service with TLS encryption, organizational structure, and test user account
{% endhint %}
{% endstep %}

{% step %}
#### 🔗 SSSD Configuration and LDAP Integration

Connect your Linux system to the LDAP directory for centralized authentication.

{% hint style="info" %}
**Enterprise Relevance**

SSSD (System Security Services Daemon) is how Linux systems integrate with enterprise identity management. In corporate environments, this would connect to Active Directory—the workflow is nearly identical.
{% endhint %}

**Install SSSD Components**

{% code title="install-sssd.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Install required packages
dnf install openldap-clients sssd sssd-ldap oddjob-mkhomedir authselect

# Configure authentication stack
authselect select sssd with-mkhomedir --force

# Enable home directory creation service
systemctl enable --now oddjobd.service
systemctl status oddjobd.service
```
{% endcode %}

**Configure LDAP Client Settings**

{% code title="/etc/openldap/ldap.conf" %}
```ini
# Uncomment and configure these lines:
BASE    dc=prolug,dc=lan
URI     ldap://ldap.prolug.lan/
```
{% endcode %}

**Configure SSSD**

{% code title="/etc/sssd/sssd.conf" %}
```ini
[domain/default]
id_provider = ldap
autofs_provider = ldap
auth_provider = ldap
chpass_provider = ldap
ldap_uri = ldap://ldap.prolug.lan/
ldap_search_base = dc=prolug,dc=lan
#ldap_id_use_start_tls = True
#ldap_tls_cacertdir = /etc/openldap/certs
cache_credentials = True
#ldap_tls_reqcert = allow

[sssd]
services = nss, pam, autofs
domains = default

[nss]
homedir_substring = /home
```
{% endcode %}

**Secure and Start SSSD**

{% code title="configure-sssd.sh" overflow="wrap" lineNumbers="true" %}
```bash
# Set proper permissions (CRITICAL - SSSD requires 0600)
chmod 0600 /etc/sssd/sssd.conf

# Start SSSD service
systemctl start sssd
systemctl status sssd
```
{% endcode %}

**Validate Integration**

{% code title="validate-ldap-user.sh" overflow="wrap" %}
```bash
# Verify LDAP user is visible to the system
id testuser

# Expected output:

# uid=15000(testuser) gid=15000 groups=15000

# Test authentication
su - testuser

# Password: testuser1234

# Verify home directory creation
ls -la /home/testuser
```
{% endcode %}

<details>

<summary>🔍 Understanding SSSD Integration</summary>

When you run `id testuser`, you're seeing:

* **NSS (Name Service Switch)**: Resolves user/group information from LDAP
* **PAM (Pluggable Authentication Modules)**: Handles authentication against LDAP
* **oddjobd**: Automatically creates home directories on first login
* **SSSD caching**: Stores credentials locally for offline authentication

This is the same architecture used for Active Directory domain joins—SSSD just connects to a different backend.

</details>

{% hint style="success" %}
✅ Outcome:

* Fully integrated LDAP authentication
* SSSD caching for offline access
* Automatic home directory creation
* Enterprise IAM workflow understanding
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
🎓 **Professional Development**

These labs mirror actual enterprise IAM engineering workflows—from authentication security analysis through directory infrastructure deployment to system integration.&#x20;

> _You'll understand PAM authentication, build OpenLDAP directory services, and configure SSSD integration using the same tools and processes required in corporate environments._

**Career Impact**: Add these skills to your project section on your resume:

* **"Enterprise LDAP deployment"**
* **"SSSD/Active Directory integration"**
* **"PAM security hardening"**
{% endhint %}

***
