---
cover: ../../.gitbook/assets/Screenshot from 2025-11-17 21-26-47.png
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

# 📢 Security Unit 3 Discussion Post 1

> _**There are \~16 STIGs that involve PAM for RHEL 9. Read the guide from Rocky Linux:**_ [_**https://docs.rockylinux.org/guides/security/pam/**_](https://docs.rockylinux.org/guides/security/pam/)

***

## 📚 Research References

1. [https://docs.rockylinux.org/guides/security/pam/](https://docs.rockylinux.org/guides/security/pam/)
2. [https://www.redhat.com/en/blog/pam-%E2%80%93-pluggable-authentication-modules-linux-and-how-edit-defaults](https://www.redhat.com/en/blog/pam-%E2%80%93-pluggable-authentication-modules-linux-and-how-edit-defaults)
3. [http://www.linux-pam.org/Linux-PAM-html/Linux-PAM\_SAG.html](http://www.linux-pam.org/Linux-PAM-html/Linux-PAM_SAG.html)
4. [https://pages.nist.gov/800-63-3/sp800-63b.html](https://pages.nist.gov/800-63-3/sp800-63b.html)
5. [https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/9/html/security\_hardening/configuring-authentication\_security-hardening](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/security_hardening/configuring-authentication_security-hardening)
6. [https://documentation.suse.com/sles/15-SP2/html/SLES-all/cha-pam.html](https://documentation.suse.com/sles/15-SP2/html/SLES-all/cha-pam.html)

***

## 🔐 PAM Overview

{% hint style="info" %}
Pluggable Authentication Modules (PAM)

PAM operates as a system of libraries that dynamically handle the authentication tasks of applications (services) on the system, providing a stable, general interface that privilege-granting programs like `login` or `su` defer to.
{% endhint %}

### Core PAM Architecture

Separation of Concerns: The core functionality of PAM is separating the tasks of authentication into four independent management groups, often referred to as interfaces or types of calls:

* auth — Authentication Management
* account — Account Management
* password — Password Management
* session — Session Management

Configuration Format:

{% code title="PAM Directive Syntax" overflow="wrap" %}
```
mechanism [control] path-to-module [argument]
```
{% endcode %}

These groups define the service requested by the calling application, and they correspond to the first field, or Type, in a PAM configuration file directive.

{% hint style="success" %}
Module Flexibility: An individual module, such as `pam_unix.so`, can provide functionality for any or all four of these interfaces.
{% endhint %}

***

## 1️⃣ What are the Mechanisms and How Do They Affect PAM Functionality?

{% hint style="info" %}
Review `/etc/pam.d/sshd` on a Linux system to understand how these functionalities work in practice.
{% endhint %}

### The Four PAM Mechanisms

{% tabs %}
{% tab title="🔑 auth - Authentication" %}
**auth - Authentication Management**

Responsibility: Authenticating a user and setting up user credentials. This phase establishes the rights of the account.

Core Functions:

{% stepper %}
{% step %}
#### Verification

This involves a challenge-response process, prompting a user for a password and verifying its validity against stored data (local files `/etc/passwd` and `/etc/shadow`, or remote sources like LDAP or Kerberos).

{% code title="Authentication Flow" overflow="wrap" %}
```
User Login Attempt
    ↓
Application Query: "Is this user who they say they are?"
    ↓
libpam Authentication Call
    ↓
Challenge-Response (Password Prompt)
    ↓
Verification Against Stored Data
    ↓
Success or Failure
```
{% endcode %}
{% endstep %}

{% step %}
#### Credential Setup

`auth` modules can also set initial user credentials, such as group memberships. They establish account settings like UID, GID, groups, and resource limits.

Credentials Established:

* User ID (UID)
* Group ID (GID)
* Supplementary group memberships
* Resource limits (from `/etc/security/limits.conf`)
* Initial environment variables
{% endstep %}

{% step %}
#### Application Query

When a program (`login` or `sshd`) initiates authentication, it makes a `libpam` authentication call, asking: "Is this user who they say they are?"

{% code title="Example from /etc/pam.d/sshd" overflow="wrap" %}
```bash
# Authentication with local password files
auth       required     pam_unix.so nullok

# The 'nullok' argument allows blank passwords (not recommended for production)
```
{% endcode %}
{% endstep %}
{% endstepper %}

Example Line:

{% code title="/etc/pam.d/sshd - auth mechanism" overflow="wrap" %}
```bash
auth required pam_unix.so nullok
```
{% endcode %}

{% hint style="warning" %}
Security Note: The `nullok` argument allows blank passwords, which should be avoided in production environments.
{% endhint %}
{% endtab %}

{% tab title="👤 account - Account Management" %}
**account - Account Management**

Responsibility: Provides account verification services to ensure that access is permitted beyond the initial password check.

Core Functions:

<details>

<summary>🔍 Availability Check</summary>

This management group checks whether the requested account is available or permitted access for reasons other than credential validation.

Checks Performed:

* Is the account expired?
* Is the account locked?
* Is the user permitted to log in at this time of day?
* Has the password expired?
* Is the user in the required group for this service?

</details>

<details>

<summary>📋 Policy Enforcement</summary>

This is where constraints like time-based access control are checked. It verifies the state of the account.

Policy Examples:

* Time-of-day restrictions (`pam_time.so`)
* Account expiration checks
* Password aging requirements
* System load restrictions (`pam_limits.so`)
* Access control lists

</details>

<details>

<summary>⚙️ Module Action</summary>

The `pam_unix.so` module, when invoked with the `account` type, checks for:

* Expired accounts
* Whether the user needs to change their password within a grace period
* Account lock status
* Password expiration status

</details>

Example Usage:

{% code title="Time-Based Access Control" overflow="wrap" %}
```bash
# In /etc/pam.d/sshd
account required pam_time.so

# Configuration in /etc/security/time.conf

# Syntax: services;ttys;users;times

# Allow alice to login only during business hours on weekdays
sshd;*;alice;Wk0800-1700

# Deny bob from login on weekends
sshd;*;bob;!Wk
```
{% endcode %}

{% hint style="info" %}
Account vs Auth: While `auth` verifies "who you are," `account` verifies "are you allowed to access this service right now?"
{% endhint %}
{% endtab %}

{% tab title="🔐 password - Password Management" %}
**password - Password Management**

Responsibility: Handles the modification or updating of the authentication token (the password) associated with an account.

Core Functions:

{% stepper %}
{% step %}
#### Update Responsibility

This group is responsible for changing the authentication token. This task is strongly coupled with services provided by the `auth` group.

Update Process:

1. Verify current password (auth mechanism)
2. Prompt for new password
3. Verify new password meets quality requirements
4. Confirm new password (re-entry)
5. Write new password to storage
{% endstep %}

{% step %}
#### Strength Verification

It often involves modules like `pam_pwquality.so` (or the older `pam_cracklib.so`) that test the strength of a newly created password to ensure it is not easily cracked or built from the old one.

Quality Checks:

* Minimum length requirements
* Character diversity (uppercase, lowercase, digits, symbols)
* Dictionary word detection
* Palindrome detection
* Similarity to old password
* Common pattern detection

{% code title="Password Quality Configuration" overflow="wrap" %}
```bash
# In /etc/pam.d/system-auth
password requisite pam_pwquality.so retry=3 minlen=14 dcredit=-1 ucredit=-1 ocredit=-1 lcredit=-1

# Arguments explained:
# retry=3       - Allow 3 attempts to enter a valid password
# minlen=14     - Minimum password length of 14 characters
# dcredit=-1    - Require at least 1 digit
# ucredit=-1    - Require at least 1 uppercase letter
# ocredit=-1    - Require at least 1 special character
# lcredit=-1    - Require at least 1 lowercase letter
```
{% endcode %}
{% endstep %}

{% step %}
#### Persistence

The password call is used by the application (like `passwd`) to get the verified new password written to the correct location, such as the local `/etc/shadow` file by the `pam_unix` module.

Storage Locations:

* Local: `/etc/shadow` (via `pam_unix.so`)
* LDAP: Remote directory (via `pam_ldap.so`)
* Kerberos: KDC (via `pam_krb5.so`)
* SSSD: Centralized identity (via `pam_sss.so`)
{% endstep %}
{% endstepper %}

Complete Example:

{% code title="/etc/pam.d/passwd" overflow="wrap" lineNumbers="true" %}
```bash
# Check password quality
password requisite pam_pwquality.so retry=3

# Update password in local files
password sufficient pam_unix.so sha512 shadow use_authtok

# Update password in LDAP (if configured)
password sufficient pam_ldap.so use_authtok
```
{% endcode %}

{% hint style="success" %}
Scenario: If a user's password has expired, the `password` component of `pam_pwquality.so` prompts for a new password and verifies it meets quality requirements. The `password` mechanism then writes the change using `pam_unix.so`.
{% endhint %}
{% endtab %}

{% tab title="🖥️ session - Session Management" %}
**session - Session Management**

Responsibility: Covers tasks related to session setup before a service is granted, and session termination after the service is withdrawn. It acts as an important opening and closing hook for modules to affect the services available to a user.

Core Functions:

{% stepper %}
{% step %}
#### Setup

Session calls perform tasks associated with session setup.

Common Setup Tasks:

* Mounting a user's home directory
* Logging the user name and service type at the start of the session
* Enabling SELinux user contexts
* Performing X11 authentication
* Setting up keyring credentials
* Initializing audit session
* Configuring resource limits

{% code title="Session Setup Example" overflow="wrap" %}
```bash
# In /etc/pam.d/sshd
session required pam_selinux.so close
session required pam_loginuid.so
session required pam_selinux.so open env_params
session optional pam_keyinit.so force revoke
session required pam_limits.so
session required pam_unix.so
```
{% endcode %}
{% endstep %}

{% step %}
#### Audit Trails

The `pam_unix.so` module, under the `session` interface, writes a login timestamp to the `/var/log/wtmp` file. This log information is often stored in `/var/log/secure`.

Logged Information:

* Login timestamp
* Username
* Service type (ssh, login, su, sudo)
* Source IP or TTY
* Session ID
* Logout timestamp (on termination)

{% code title="Viewing Session Logs" overflow="wrap" %}
```bash
# View wtmp logs (successful logins)
last

# View btmp logs (failed logins)  
lastb

# View secure log (authentication events)
tail -f /var/log/secure

# Example log entry
Nov 17 10:32:15 server sshd[12345]: pam_unix(sshd:session): session opened for user alice by (uid=0)
```
{% endcode %}
{% endstep %}

{% step %}
#### Termination

When a user logs out and the session is closed, another session call can be made, such as to write the logout timestamp to the `/var/log/wtmp` file.

Cleanup Tasks:

* Unmounting home directories
* Closing SELinux contexts
* Revoking keyring credentials
* Writing logout timestamp
* Cleaning temporary files
* Terminating user processes (if configured)

{% code title="Session Termination" overflow="wrap" %}
```bash
# Log entry on session close
Nov 17 14:22:37 server sshd[12345]: pam_unix(sshd:session): session closed for user alice
```
{% endcode %}
{% endstep %}
{% endstepper %}

Complete Session Configuration:

{% code title="/etc/pam.d/sshd - session mechanism" overflow="wrap" lineNumbers="true" %}
```bash
session    required     pam_selinux.so close
session    required     pam_loginuid.so
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    optional     pam_motd.so
session    include      password-auth
session    include      postlogin
```
{% endcode %}

{% hint style="info" %}
Example Line: `session required pam_unix.so` manages the session and handles the logging of the session start and end.
{% endhint %}
{% endtab %}
{% endtabs %}

### Real-World Example: /etc/pam.d/sshd

{% code title="Complete /etc/pam.d/sshd Analysis" overflow="wrap" lineNumbers="true" %}
```bash
#%PAM-1.0

# Authentication Phase
auth       substack     password-auth
auth       include      postlogin

# Loads external configuration from password-auth and postlogin

# Account Verification Phase  
account    required     pam_sepermit.so
account    required     pam_nologin.so
account    include      password-auth

# Checks account status, expiration, system maintenance mode

# Password Update Phase (for password changes via SSH)
password   include      password-auth

# Delegates to system password policy

# Session Management Phase
session    required     pam_selinux.so close
session    required     pam_loginuid.so

# Session opened for user USER by (uid=0)
session    required     pam_selinux.so open env_params
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    include      password-auth
session    include      postlogin

# -session   optional     pam_ck_connector.so

# Sets up environment, logging, audit, SELinux context
```
{% endcode %}

<details>

<summary>📋 Line-by-Line Breakdown</summary>

Authentication (`auth`):

* `auth substack password-auth` - Loads `/etc/pam.d/password-auth` for actual authentication logic
* `auth include postlogin` - Post-login actions (display messages, etc.)

Account (`account`):

* `account required pam_sepermit.so` - SELinux user separation checks
* `account required pam_nologin.so` - Check if `/etc/nologin` exists (maintenance mode)
* `account include password-auth` - Account verification from system configuration

Password (`password`):

* `password include password-auth` - Password change policies from system configuration

Session (`session`):

* `session required pam_selinux.so close` - Close current SELinux context
* `session required pam_loginuid.so` - Set login UID for audit
* `session required pam_selinux.so open env_params` - Open new SELinux context
* `session required pam_namespace.so` - Setup polyinstantiated directories
* `session optional pam_keyinit.so force revoke` - Initialize keyring
* `session include password-auth` - Session setup from system configuration
* `session include postlogin` - Post-login session tasks

</details>

***

## 2️⃣ What are the Common PAM Modules?

{% hint style="info" %}
Review `/etc/pam.d/sshd` on a Linux system to see how these modules work in practice.
{% endhint %}

### Essential PAM Modules

{% tabs %}
{% tab title="🔧 pam_unix.so" %}
**pam\_unix.so - Global Authentication Policy**

Functionality: The workhorse of PAM, providing all four PAM module interface types.

| Mechanism | Function                                                                                              |
| --------- | ----------------------------------------------------------------------------------------------------- |
| auth      | Prompts for password, checks against `/etc/passwd` and `/etc/shadow`; `nullok` allows blank passwords |
| account   | Performs account verification: expired accounts, grace periods, password changes needed               |
| password  | Manages password changes, can create shadow passwords with `shadow` argument                          |
| session   | Manages session logging to `/var/log/secure`, writes session start/end                                |

{% code title="Complete pam_unix.so Usage" overflow="wrap" lineNumbers="true" %}
```bash
# Authentication
auth       sufficient   pam_unix.so nullok try_first_pass

# Account verification
account    required     pam_unix.so

# Password updates
password   sufficient   pam_unix.so sha512 shadow use_authtok

# Session management
session    required     pam_unix.so
```
{% endcode %}

Common Arguments:

<details>

<summary>📖 pam_unix.so Arguments</summary>

Authentication Arguments:

* `nullok` - Allow blank passwords (not recommended)
* `try_first_pass` - Try password from previous auth module
* `use_first_pass` - Require password from previous auth module

Password Arguments:

* `sha512` - Use SHA-512 hashing (strongest available)
* `md5` - Use MD5 hashing (legacy, weak)
* `shadow` - Use shadow passwords
* `use_authtok` - Use password from previous password module
* `remember=N` - Remember last N passwords to prevent reuse

Example with Security Hardening:

```bash
password sufficient pam_unix.so sha512 shadow remember=5 use_authtok
# SHA-512 hashing + shadow passwords + remember last 5 passwords
```

</details>

{% hint style="success" %}
Universal Module: `pam_unix.so` is the foundation of local authentication on Linux systems, providing complete PAM functionality.
{% endhint %}
{% endtab %}

{% tab title="⏰ pam_time.so" %}
**pam\_time.so - Time-Based Access Control**

Functionality: Essential for implementing time-based access control.

* Purpose: Limit access times to services managed by PAM
* Mechanism: Implemented using the `account` mechanism
* Configuration: Defined in `/etc/security/time.conf`

Configuration Syntax:

{% code title="/etc/security/time.conf" overflow="wrap" %}
```
services;ttys;users;times

# Fields:
# services - PAM service names (login, sshd, su, etc.) or ALL
# ttys     - Terminals (* for all, specific ttys like tty1)
# users    - Usernames or groups (!group syntax)
# times    - Time specifications (see examples below)
```
{% endcode %}

Time Specification Format examples:

* Wk0800-1700 — Weekdays 8 AM - 5 PM
* !Wk — NOT weekdays (weekends only)
* Al0900-1800 — All days 9 AM - 6 PM

Real-World Examples:

{% code title="Time-Based Access Control Examples" overflow="wrap" lineNumbers="true" %}
```bash
# Allow alice to SSH only during business hours (weekdays 8AM-5PM)
sshd;*;alice;Wk0800-1700

# Deny bob from SSH on weekends
sshd;*;bob;!Wk

# Allow contractors group only during core hours
sshd;*;@contractors;Al0900-1700

# Allow remote workers any time except maintenance window (2-4 AM)
sshd;*;@remote;!Al0200-0400

# Restrict console login to local terminals during office hours
login;tty1|tty2|tty3;@employees;Wk0800-1800

# Emergency admin access - 24/7
*;*;admin;Al0000-2400
```
{% endcode %}

PAM Configuration:

{% code title="/etc/pam.d/sshd" overflow="wrap" %}
```bash
# Add to account section
account    required     pam_time.so
```
{% endcode %}

{% hint style="warning" %}
Testing Critical: Always test time restrictions carefully! Improperly configured time constraints can lock out all users, including administrators.
{% endhint %}

<details>

<summary>🔧 Troubleshooting pam_time</summary>

Common Issues:

1.  Syntax Errors in time.conf\
    Wrong:

    ```bash
    sshd * alice Wk0800-1700
    ```

    Correct:

    ```bash
    sshd;*;alice;Wk0800-1700
    ```
2.  Time Format Mistakes\
    Wrong:

    ```bash
    sshd;*;alice;Wk8-17
    ```

    Correct:

    ```bash
    sshd;*;alice;Wk0800-1700
    ```
3.  Debug Logging\
    Check `/var/log/secure` for pam\_time messages:

    ```bash
    grep "pam_time" /var/log/secure
    ```

</details>
{% endtab %}

{% tab title="🔐 pam_tally2.so / pam_faillock.so" %}
**pam\_tally2.so / pam\_faillock.so - Brute-Force Protection**

Functionality: Provides brute-force protection by tracking failed login attempts.

{% hint style="info" %}
Version Note: RHEL 9 uses `pam_faillock.so` (newer). `pam_tally2.so` is deprecated but covered for legacy systems.
{% endhint %}

Modern Approach: pam\_faillock.so

{% code title="/etc/pam.d/system-auth (RHEL 9)" overflow="wrap" lineNumbers="true" %}
```bash
# Authentication phase - check if account is locked
auth        required      pam_faillock.so preauth silent deny=3 unlock_time=600

# Perform actual authentication
auth        sufficient    pam_unix.so try_first_pass

# Authentication phase - record failed attempt
auth        [default=die] pam_faillock.so authfail deny=3 unlock_time=600

# Account phase - enforce lock status
account     required      pam_faillock.so

# Arguments explained:
# preauth       - Check lock status before authentication
# authfail      - Record failure after authentication attempt
# deny=3        - Lock after 3 failed attempts
# unlock_time=600 - Unlock after 600 seconds (10 minutes)
# silent        - Don't log to syslog
```
{% endcode %}

Configuration File:

{% code title="/etc/security/faillock.conf" overflow="wrap" lineNumbers="true" %}
```bash
# Deny access after N failed attempts
deny = 3

# Account unlock time (seconds)
unlock_time = 600

# Root account exempt from locking (dangerous!)
# even_deny_root

# Lock root account too (recommended)
root_unlock_time = 1800

# Audit failed attempts
audit

# Silent mode
silent

# Admin group users exempt
# admin_group = wheel
```
{% endcode %}

Management Commands:

{% code title="faillock Management" overflow="wrap" %}
```bash
# View failed login attempts
faillock

# View specific user
faillock --user alice

# Reset failed attempts for user
faillock --user alice --reset

# Reset all users
faillock --reset
```
{% endcode %}

Legacy: pam\_tally2.so

<details>

<summary>📜 Legacy pam_tally2.so Configuration</summary>

{% code title="/etc/pam.d/system-auth (Legacy)" overflow="wrap" lineNumbers="true" %}
```bash
# Auth mechanism - accepts or denies and resets counter
auth        required      pam_tally2.so deny=3 unlock_time=600 onerr=fail

# Account mechanism - increments counter
account     required      pam_tally2.so
```
{% endcode %}

Management Commands:

```bash
# View failed attempts
pam_tally2

# View specific user
pam_tally2 --user alice

# Reset user counter
pam_tally2 --user alice --reset

# Reset all
pam_tally2 --reset
```

Arguments:

* `deny=n` - Lock after N failures
* `unlock_time=n` - Unlock after N seconds
* `onerr=fail` - Fail if error accessing counter file
* `file=/var/log/tallylog` - Counter file location

</details>

{% hint style="danger" %}
Security vs. Availability Trade-off:

* Too few attempts = legitimate users locked out frequently
* Too many attempts = attackers have more opportunities\
  Recommended: 3-5 failed attempts, 10-30 minute lockout
{% endhint %}
{% endtab %}

{% tab title="💪 pam_pwquality.so" %}
**pam\_pwquality.so / pam\_cracklib.so - Password Quality**

Functionality: Mandatory for enforcing robust password policies.

* Purpose: Test the strength and quality of new passwords
* Mechanism: Only affects the `password` mechanism
* Modern: `pam_pwquality.so` (RHEL 7+)
* Legacy: `pam_cracklib.so` (deprecated)

{% hint style="info" %}
Default Checks: By default, `pam_cracklib.so` checks if the password is from a dictionary, is a palindrome of the old password, or only differs from the old password by case.
{% endhint %}

Configuration:

{% code title="/etc/pam.d/system-auth" overflow="wrap" %}
```bash
password    requisite     pam_pwquality.so retry=3
```
{% endcode %}

{% code title="/etc/security/pwquality.conf" overflow="wrap" lineNumbers="true" %}
```bash
# Minimum password length
minlen = 14

# Minimum number of digits
dcredit = -1

# Minimum number of uppercase letters
ucredit = -1

# Minimum number of lowercase letters
lcredit = -1

# Minimum number of special characters
ocredit = -1

# Maximum number of consecutive characters of same class
maxrepeat = 3

# Maximum number of sequential characters
maxsequence = 3

# Minimum different characters from old password
difok = 5

# Check against dictionary
dictcheck = 1

# Reject username in password
usercheck = 1

# Enforce for root too
enforce_for_root
```
{% endcode %}

Inline Configuration:

{% code title="Inline Configuration" overflow="wrap" %}
```bash
password requisite pam_pwquality.so \
    retry=3 \
    minlen=14 \
    dcredit=-1 \
    ucredit=-1 \
    lcredit=-1 \
    ocredit=-1 \
    difok=5 \
    maxrepeat=3 \
    usercheck=1 \
    enforce_for_root
```
{% endcode %}

Understanding Credits:

<details>

<summary>🔢 Credit System Explained</summary>

Positive Values (reward for diversity):

* `dcredit=1` - Each digit adds 1 to calculated length
* `ucredit=2` - Each uppercase adds 2 to calculated length

Negative Values (requirement):

* `dcredit=-1` - Require at least 1 digit
* `ucredit=-2` - Require at least 2 uppercase letters

Example:

```bash
# Password: Pass123!
# minlen=10, dcredit=-1, ucredit=-1, lcredit=-1, ocredit=-1
# Length: 8 characters
# Has: 1 uppercase (P), 3 lowercase (ass), 3 digits (123), 1 special (!)
# Meets: All credit requirements
# Result: PASS
```

</details>

STIG-Compliant Configuration:

{% code title="STIG Password Requirements" overflow="wrap" lineNumbers="true" %}
```bash
# RHEL 9 STIG Requirements
password requisite pam_pwquality.so \
    retry=3 \
    minlen=15 \
    dcredit=-1 \
    ucredit=-1 \
    lcredit=-1 \
    ocredit=-1 \
    difok=8 \
    maxrepeat=3 \
    usercheck=1 \
    enforce_for_root \
    dictcheck=1

# Also require password history
password sufficient pam_unix.so sha512 shadow use_authtok remember=5
```
{% endcode %}

{% hint style="success" %}
Balance: Strong passwords improve security, but overly complex requirements can lead to users writing passwords down or using predictable patterns.
{% endhint %}
{% endtab %}

{% tab title="🚫 Restrictive Modules" %}
**Restrictive PAM Modules**

These modules restrict access based on specific criteria.

{% stepper %}
{% step %}
#### pam\_nologin.so - Maintenance Mode

Functionality: If the `/etc/nologin` file exists and is readable, this module disables all accounts except the root user from connecting.

* Mechanism: Commonly implemented using the `auth` or `account` mechanism
* Use Case: System maintenance windows

{% code title="Usage" overflow="wrap" %}
```bash
# In /etc/pam.d/sshd
account    required     pam_nologin.so

# Create nologin file to block users
echo "System maintenance in progress. Please try again later." > /etc/nologin

# Remove to allow access
rm /etc/nologin
```
{% endcode %}

Custom Message:

```bash
cat > /etc/nologin << EOF
====================================
SYSTEM MAINTENANCE IN PROGRESS
====================================

Scheduled maintenance window: 02:00 - 04:00 EST

The system will be unavailable during this time.
Emergency access: contact sysadmin@example.com

Thank you for your patience.
====================================
EOF
```

Users will see this message when attempting to log in.
{% endstep %}

{% step %}
#### pam\_securetty.so - Root TTY Restriction

Functionality: Critical for restricting direct root access. Ensures that root login is allowed only from a terminal listed in the `/etc/securetty` file.

* Mechanism: Used in the `auth` mechanism
* Security: If terminal is not listed, the root login attempt will fail

{% code title="/etc/securetty Example" overflow="wrap" lineNumbers="true" %}
```bash
# Allow root login only from these terminals
console
tty1
tty2
tty3
tty4
tty5
tty6

# Empty file = no root logins allowed
# (recommended for SSH - use sudo instead)
```
{% endcode %}

{% code title="/etc/pam.d/login" overflow="wrap" %}
```bash
auth       required     pam_securetty.so
```
{% endcode %}

{% hint style="danger" %}
Best Practice: For SSH access, leave `/etc/securetty` empty and use `PermitRootLogin no` in `/etc/ssh/sshd_config`. Root should access systems via `sudo` after authenticating as a regular user.
{% endhint %}
{% endstep %}

{% step %}
#### pam\_wheel.so - Privilege Escalation Limiting

Functionality: Limits access to the `su` command to members of the `wheel` group (or specified group).

* Mechanism: Used in `auth` mechanism
* Security: Forces use of `sudo` instead of `su` if wheel group is empty

{% code title="/etc/pam.d/su" overflow="wrap" lineNumbers="true" %}
```bash
# Restrict su to wheel group members
auth       required     pam_wheel.so use_uid

# Alternative: Use different group
auth       required     pam_wheel.so use_uid group=admin

# Arguments:
# use_uid    - Check real UID, not effective UID
# group=name - Use alternate group instead of wheel
# trust      - Don't prompt wheel group members for password (dangerous!)
```
{% endcode %}

Configuration:

```bash
# Add user to wheel group
usermod -aG wheel alice

# Verify membership
groups alice
alice : alice wheel

# Test su access
su - root
# Will succeed for alice, fail for users not in wheel
```

{% hint style="warning" %}
Empty Wheel Group: If the specified group is empty, the `su` command may become unavailable on the system, forcing the use of `sudo` (which is generally preferred for audit trails).
{% endhint %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="🏢 Enterprise Modules" %}
**Enterprise Identity Management Modules**

{% stepper %}
{% step %}
#### pam\_sss.so - SSSD Integration

Functionality: Used in Red Hat environments for centralized identity management. Facilitates authentication against domains managed by SSSD (IdM, Active Directory, LDAP).

Configuration: When configured, it can restrict PAM service's authentication to specified domains using the `domains` option in the PAM configuration file.

{% hint style="info" %}
Scope: This restriction applies only to authentication actions, not to user lookups.
{% endhint %}

{% code title="/etc/pam.d/system-auth with SSSD" overflow="wrap" lineNumbers="true" %}
```bash
auth        sufficient    pam_sss.so forward_pass
auth        required      pam_unix.so try_first_pass

account     [default=bad success=ok user_unknown=ignore] pam_sss.so
account     required      pam_unix.so

password    sufficient    pam_sss.so use_authtok
password    sufficient    pam_unix.so sha512 shadow use_authtok

session     optional      pam_keyinit.so revoke
session     required      pam_limits.so
session     optional      pam_sss.so
session     required      pam_unix.so
```
{% endcode %}

SSSD Configuration:

{% code title="/etc/sssd/sssd.conf" overflow="wrap" lineNumbers="true" %}
```ini
[sssd]
domains = example.com
services = nss, pam, ssh, sudo
config_file_version = 2

[domain/example.com]
id_provider = ldap
auth_provider = ldap
ldap_uri = ldap://ldap.example.com
ldap_search_base = dc=example,dc=com
cache_credentials = true
```
{% endcode %}

Domain Restriction:

{% code title="Restrict to Specific Domain" overflow="wrap" %}
```bash
# Only authenticate against corporate domain
auth sufficient pam_sss.so domains=example.com
```
{% endcode %}
{% endstep %}

{% step %}
#### pam\_ldap.so - Direct LDAP Authentication

Functionality: Direct LDAP integration without SSSD.

{% code title="/etc/pam.d/system-auth with LDAP" overflow="wrap" lineNumbers="true" %}
```bash
auth        sufficient    pam_ldap.so use_first_pass
auth        required      pam_unix.so try_first_pass

account     [default=bad success=ok user_unknown=ignore] pam_ldap.so
account     required      pam_unix.so

password    sufficient    pam_ldap.so use_authtok
password    sufficient    pam_unix.so sha512 shadow use_authtok
```
{% endcode %}
{% endstep %}

{% step %}
#### pam\_krb5.so - Kerberos Authentication

Functionality: Kerberos single sign-on integration.

{% code title="/etc/pam.d/system-auth with Kerberos" overflow="wrap" lineNumbers="true" %}
```bash
auth        sufficient    pam_krb5.so use_first_pass
auth        required      pam_unix.so try_first_pass

account     sufficient    pam_krb5.so
account     required      pam_unix.so

password    sufficient    pam_krb5.so use_authtok
password    sufficient    pam_unix.so sha512 shadow use_authtok

session     optional      pam_krb5.so
session     required      pam_unix.so
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="🎖️ Honorable Mentions" %}
**Other Important PAM Modules**

<details>

<summary>✅ pam_rootok.so - Root Bypass</summary>

* Mechanism: `auth`
* Function: Checks if the current user attempting access has a UID of 0 (root).

{% code title="Usage" overflow="wrap" %}
```bash
# In /etc/pam.d/su
auth       sufficient   pam_rootok.so
# Allows root to 'su' to any account without password
```
{% endcode %}

Use Case: Allows root to switch to any user without providing a password.

</details>

<details>

<summary>✅ pam_permit.so - Unconditional Allow</summary>

* Mechanism: `auth` and `session`
* Function: Explicitly allows the action without further checks.

{% code title="Usage" overflow="wrap" %}
```bash
# Used in utilities that should never fail authentication
auth       required     pam_permit.so
```
{% endcode %}

{% hint style="danger" %}
Security Warning: Use with extreme caution! This module allows access without any authentication.
{% endhint %}

</details>

<details>

<summary>✅ pam_timestamp.so - Credential Caching</summary>

* Mechanism: `auth` and `session`
* Function: Administrative credential caching for graphical tools.

Behavior:

* Upon successful authentication, creates a timestamp file (default: `/var/run/sudo/`)
* If file exists and is recent (default: 300 seconds), subsequent auth requests succeed without re-prompting

{% code title="Configuration" overflow="wrap" %}
```bash
# In /etc/pam.d/polkit-1
auth       sufficient   pam_timestamp.so verbose
auth       required     pam_unix.so

# Adjust timeout in /etc/security/pam_timestamp.conf
# timestamp_timeout=300  # 5 minutes (default)
```
{% endcode %}

Use Case: Graphical administrative programs can perform multiple privileged operations without repeatedly prompting for password.

</details>

<details>

<summary>✅ pam_access.so - Access Control Lists</summary>

* Mechanism: `account`
* Configuration: `/etc/security/access.conf`

{% code title="Access Control Examples" overflow="wrap" lineNumbers="true" %}
```bash
# Format: permission:users:origins
# permission: + (allow) or - (deny)
# users: username, @group, ALL
# origins: tty, hostname, domain, IP, ALL

# Deny all except wheel group on all origins
-:ALL EXCEPT wheel:ALL

# Allow admin from specific network
+:admin:192.168.1.0/24

# Deny user bob from all origins
-:bob:ALL

# Allow users in devs group from specific hosts
+:@devs:dev1.example.com dev2.example.com
```
{% endcode %}

</details>

<details>

<summary>✅ pam_limits.so - Resource Limits</summary>

* Mechanism: `session`
* Configuration: `/etc/security/limits.conf`

{% code title="Resource Limits Example" overflow="wrap" lineNumbers="true" %}
```bash
# Format: domain type item value
# domain: username, @group, *
# type: soft, hard, -
# item: nofile, nproc, core, etc.

# Limit max open files
*           soft    nofile      4096
*           hard    nofile      65536

# Limit max processes
@users      soft    nproc       100
@users      hard    nproc       200

# Core dump limits
*           soft    core        0
*           hard    core        unlimited

# Database user limits
mysql       soft    nofile      65535
mysql       hard    nofile      65535
```
{% endcode %}

</details>
{% endtab %}
{% endtabs %}

***

## 3️⃣ Real-World Application Blog Post

{% hint style="success" %}
Featured Article

PAM – Pluggable Authentication Modules for Linux and how to edit the defaults

Author: Christina Kyriakidou (Red Hat)

Link: https://www.redhat.com/en/blog/pam-%E2%80%93-pluggable-authentication-modules-linux-and-how-edit-defaults
{% endhint %}

### Article Synopsis

This comprehensive article from Red Hat provides practical guidance on safely editing PAM's complex configuration files. The author details:

Key Topics Covered:

{% tabs %}
{% tab title="📋 Configuration Fundamentals" %}
**Understanding PAM Configuration**

Configuration File Structure:

* Location of PAM files in `/etc/pam.d/`
* Relationship between service files and system-auth
* Include/substack directives

Configuration Syntax:

* Four mechanisms (auth, account, password, session)
* Control flags (required, requisite, sufficient, optional)
* Module paths and arguments

File Relationships:

* Service-specific files reference system-wide configurations
* Cascading configurations through includes
* Override precedence
{% endtab %}

{% tab title="🎛️ Control Keywords" %}
**PAM Control Flags Explained**

The article provides clear explanations of PAM control modes:

| Control Flag | Behavior                                     | Use Case                 |
| ------------ | -------------------------------------------- | ------------------------ |
| required     | Must succeed; continue checking              | Essential requirements   |
| requisite    | Must succeed; stop if fails                  | Critical early checks    |
| sufficient   | Success stops checks (if no required failed) | Alternative auth methods |
| optional     | Failure ignored unless it's the only module  | Non-essential features   |

Advanced Control Syntax example:

```bash
# Simple control
auth required pam_unix.so

# Advanced control with value-action pairs
auth [success=ok new_authtok_reqd=ok ignore=ignore default=bad] pam_unix.so
```
{% endtab %}

{% tab title="⚠️ Safe Editing Practices" %}
**How to Safely Edit PAM Configuration**

Critical Safety Guidelines:

* Always keep a root shell open during PAM modifications
* Test changes immediately in a separate session
* Have rescue media available (live CD, single-user mode access)
* Make backups before any changes
* Use version control for tracking changes

Step-by-Step Safe Editing:

{% stepper %}
{% step %}
#### Backup Current Configuration

{% code overflow="wrap" %}
```bash
# Backup entire PAM configuration
tar -czf /root/pam-backup-$(date +%Y%m%d).tar.gz /etc/pam.d/

# Backup specific file
cp /etc/pam.d/sshd /etc/pam.d/sshd.backup
```
{% endcode %}
{% endstep %}

{% step %}
#### Keep Root Shell Open

```bash
sudo -i
# Keep this terminal open!
```
{% endstep %}

{% step %}
#### Make Changes in Editor

```bash
vi /etc/pam.d/sshd
# Make your modifications
```
{% endstep %}

{% step %}
#### Test in New Session

Open new terminal and test:

```bash
ssh user@localhost
# Verify authentication works
```

If it fails, you still have the root shell to fix it!
{% endstep %}

{% step %}
#### Verify and Finalize

If successful:

```bash
# Document the change
echo "$(date): Modified /etc/pam.d/sshd - added pam_time.so" >> /root/pam-changes.log
```

If failed:

```bash
# Restore from backup in root shell
cp /etc/pam.d/sshd.backup /etc/pam.d/sshd
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="🎓 Learning Resources" %}
**Educational Value**

The article excels at:

* Practical Examples: Real-world configuration scenarios, step-by-step walkthroughs, common pitfalls and solutions
* Visual Aids: Configuration file diagrams, authentication flow illustrations, troubleshooting decision trees
* Troubleshooting Guidance: Common error messages and their meanings, debugging techniques (`/var/log/secure` analysis), recovery procedures for locked-out scenarios
{% endtab %}
{% endtabs %}

### Why This Article Matters

{% hint style="success" %}
Key Takeaway

PAM configuration is powerful but dangerous. One misconfiguration can lock you out of your system entirely. This article provides the safety procedures and understanding necessary to modify PAM confidently and correctly.
{% endhint %}

Highlights:

* Clear explanations of complex PAM concepts
* Safety-first approach to configuration changes
* Real-world examples from enterprise environments
* Troubleshooting guidance for common issues
* Best practices for production systems

Target Audience:

* System administrators new to PAM
* Experienced admins needing a refresher
* Security engineers implementing authentication policies
* Anyone responsible for Linux system security

***

## 📝 Key Takeaways

{% hint style="success" %}
Summary: PAM Mechanisms and Modules
{% endhint %}

### Four PAM Mechanisms

auth - Authentication Management

* Verifies user identity (password checks)
* Sets up user credentials (UID, GID, groups)
* Establishes account rights\
  Example: `auth required pam_unix.so nullok`

account - Account Management

* Verifies account availability beyond password
* Enforces policies (time restrictions, expiration)
* Checks account state (expired, locked, grace period)\
  Example: `account required pam_time.so`

password - Password Management

* Handles password updates and changes
* Enforces password quality requirements
* Writes new passwords to storage\
  Example: `password requisite pam_pwquality.so retry=3`

session - Session Management

* Session setup (mounting home, SELinux context, logging)
* Session teardown (cleanup, logout logging)
* Audit trails (wtmp, secure log)\
  Example: `session required pam_unix.so`

### Essential PAM Modules

Core Functionality:

* `pam_unix.so`: Universal authentication (all 4 mechanisms)
* `pam_pwquality.so`: Password quality enforcement
* `pam_faillock.so`: Brute-force protection
* `pam_time.so`: Time-based access control

Security Restrictions:

* `pam_nologin.so`: Maintenance mode (block non-root users)
* `pam_securetty.so`: Restrict root to specific terminals
* `pam_wheel.so`: Limit su to wheel group members

Enterprise Integration:

* `pam_sss.so`: SSSD centralized identity (IdM, AD, LDAP)
* `pam_ldap.so`: Direct LDAP authentication
* `pam_krb5.so`: Kerberos single sign-on

Additional Utilities:

* `pam_rootok.so`, `pam_permit.so`, `pam_timestamp.so`, `pam_access.so`, `pam_limits.so`

### Real-World Application Insights

Safe PAM Editing Practices:

{% stepper %}
{% step %}
Always keep a root shell open during modifications
{% endstep %}

{% step %}
Backup configuration before changes
{% endstep %}

{% step %}
Test immediately in a separate session
{% endstep %}

{% step %}
Have rescue media available
{% endstep %}

{% step %}
Document all changes
{% endstep %}
{% endstepper %}

Configuration Understanding:

* Service files reference system-wide configurations
* Control flags determine module behavior (required, sufficient, etc.)
* Modules can provide multiple mechanism types
* Order matters - modules are evaluated sequentially

Common Use Cases:

* Implementing time-based access restrictions
* Enforcing password complexity requirements
* Protecting against brute-force attacks
* Integrating with enterprise identity management
* Restricting administrative access

### Critical Security Considerations

Risk Management:

* Misconfiguration = lockout: One wrong line can prevent all authentication
* Test thoroughly: Changes affect all authentication on the system
* Balance security vs. usability: Overly restrictive policies cause workarounds
* Monitor logs: `/var/log/secure` contains authentication events

Best Practices:

* Use `required` for essential security modules
* Use `sufficient` for alternative authentication methods
* Place `pam_faillock.so` early in auth mechanism
* Apply password quality STIG requirements
* Enable audit logging for all authentication events
* Regularly review and update PAM configurations

RHEL 9 STIGs and PAM: \~16 PAM-related STIGs cover:

* Password complexity and history requirements
* Failed login attempt lockouts
* Session timeout configurations
* Root access restrictions
* Audit logging requirements
* Authentication method security

Compliance Pattern:

1. Configure password quality (`pam_pwquality.so`)
2. Enable account lockout (`pam_faillock.so`)
3. Restrict root access (`pam_securetty.so`, `pam_wheel.so`)
4. Implement session security (timeouts, logging)
5. Document and test all configurations

***

## 🔗 Additional Resources

Official PAM Documentation

* http://www.linux-pam.org/Linux-PAM-html/Linux-PAM\_SAG.html
* https://docs.rockylinux.org/guides/security/pam/
* https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/9/html/security\_hardening/configuring-authentication\_security-hardening

Red Hat Blog Articles

* https://www.redhat.com/en/blog/pam-%E2%80%93-pluggable-authentication-modules-linux-and-how-edit-defaults
* https://www.redhat.com/sysadmin/pluggable-authentication-modules-pam

Security Standards and STIGs

* https://public.cyber.mil/stigs/downloads/
* https://pages.nist.gov/800-63-3/sp800-63b.html
* https://www.cisecurity.org/benchmark/red\_hat\_linux

Advanced PAM Topics

* https://developers.yubico.com/pam-u2f/
* https://sssd.io/docs/introduction.html
* https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/9/html/using\_selinux/index

***
