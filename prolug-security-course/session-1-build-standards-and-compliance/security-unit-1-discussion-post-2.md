---
cover: ../../.gitbook/assets/Screenshot from 2025-11-16 23-44-46.png
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

# 📢 Security Unit 1 Discussion Post 2

> _**Find a STIG or compliance requirement that you do not agree is necessary for a server or service build.**_

### Research References

1. [https://public.cyber.mil/stigs/](https://public.cyber.mil/stigs/)
2. [https://www.stigviewer.com/](https://www.stigviewer.com/)
3. [https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
4. [https://mariadb.com/kb/en/audit-plugin/](https://mariadb.com/kb/en/audit-plugin/)
5. [https://www.nist.gov/itl/applied-cybersecurity/nice/resources/database-security-configuration-guide](https://www.nist.gov/itl/applied-cybersecurity/nice/resources/database-security-configuration-guide)

***

### Selected STIG Requirement

### V-253678: MariaDB Audit Log Overwrite Configuration

{% hint style="info" %}
STIG Requirement Summary

MariaDB must be configurable to overwrite audit log records, oldest first (First-In-First-Out - FIFO), in the event of unavailability of space for more audit log records.
{% endhint %}

Real-World Analogy:

Think of this like a security camera's memory card that's almost full. The STIG states that when it reaches maximum capacity, it must overwrite data from the first file forward—automatically replacing the oldest footage to keep recording new events.

<details>

<summary>Full STIG Requirement Details</summary>

**STIG ID**: V-253678

**Severity**: CAT II (Medium)

**Rule Title**: MariaDB must be configurable to overwrite audit log records, oldest first (First-In-First-Out - FIFO), in the event of unavailability of space for more audit log records.

**Discussion**: It is critical that when MariaDB is at risk of failing to process audit logs as required, it take action to mitigate the failure. Audit processing failures include software/hardware errors, failures in the audit capturing mechanisms, and audit storage capacity being reached or exceeded. Responses to audit failure depend upon the nature of the failure mode.

**Check Content**: Review the MariaDB configuration to determine if audit logs are configured to overwrite the oldest records when the log reaches capacity.

**Fix Text**: Configure MariaDB audit plugin to overwrite oldest audit log records when storage capacity is reached, ensuring FIFO operation.

</details>

***

### <mark style="color:$danger;">1️⃣ What is the STIG Requirement Trying to Do?</mark>

{% hint style="success" %}
Primary Objective

The requirement aims to ensure continuous audit logging capability by preventing audit system failure when storage space is exhausted.
{% endhint %}

#### Core Security Goals

{% tabs %}
{% tab title="Continuity" %}
**Maintain Audit Continuity**

Goal: Ensure audit logging never stops due to storage limitations.

Rationale:

* Security monitoring must be continuous
* Gaps in audit logs create blind spots for incident detection
* Attackers may time malicious activities during logging outages
* Compliance requirements mandate uninterrupted audit trails

STIG Philosophy: "Better to overwrite old logs and maintain continuous monitoring than to stop logging entirely and lose visibility into current activities."

{% hint style="warning" %}
Trade-off: Sacrifices historical data preservation for operational continuity.
{% endhint %}
{% endtab %}

{% tab title="Compliance" %}
**Meet Regulatory Requirements**

Applicable Standards:

| Standard      | Requirement                                                                                               |
| ------------- | --------------------------------------------------------------------------------------------------------- |
| PCI DSS 3.2.1 | Requirement 10.7: Retain audit trail history for at least one year                                        |
| HIPAA         | § 164.312(b): Implement hardware, software, and/or procedural mechanisms that record and examine activity |
| SOX           | Section 404: Maintain records of system activity for financial reporting systems                          |
| FISMA         | NIST 800-53 AU-4: Audit Storage Capacity                                                                  |

Compliance Perspective:

* Many regulations require continuous audit capability
* System unavailability due to full logs may violate compliance
* Automatic FIFO overwrite prevents audit system shutdown

{% hint style="info" %}
The STIG assumes that some audit data is better than no audit data when storage constraints arise.
{% endhint %}
{% endtab %}

{% tab title="Operational Resilience" %}
**Prevent System Degradation**

Operational Concerns:

Without Automatic Overwrite:

* ❌ Audit system halts when storage is full
* ❌ May cause database performance degradation
* ❌ Could trigger application failures if audit writes fail
* ❌ Requires manual intervention to restore logging

With Automatic Overwrite:

* ✅ Continuous operation without intervention
* ✅ No impact on application availability
* ✅ Self-managing audit storage
* ✅ Predictable behavior under storage pressure

{% code title="Failure Scenario Without FIFO" overflow="wrap" %}
```bash
# Audit log fills completely
[ERROR] MariaDB audit plugin: Cannot write to audit log - disk full
[ERROR] Database performance degradation detected
[WARNING] Application experiencing timeout errors
[CRITICAL] Manual intervention required to clear audit logs
```
{% endcode %}
{% endtab %}
{% endtabs %}

***

### <mark style="color:$danger;">2️⃣ What Category and Type of Control Is It?</mark>

{% hint style="info" %}
Control Classification

Category: Technical Control\
Type: Corrective Control
{% endhint %}

#### Control Analysis

{% tabs %}
{% tab title="Technical Control" %}
**Why Technical Control?**

Definition: Technical controls are security measures implemented through technology—automated mechanisms that enforce security requirements without requiring human intervention.

V-253678 as Technical Control:

* ✅ Automated: System automatically overwrites oldest logs
* ✅ Configuration-Based: Implemented via database configuration parameters
* ✅ No Human Intervention: Functions without administrator action
* ✅ Programmatic Enforcement: MariaDB audit plugin handles the logic

{% code title="Technical Implementation Example" overflow="wrap" lineNumbers="true" %}
```ini
# MariaDB audit plugin configuration
[mysqld]
plugin-load-add=server_audit.so
server_audit_logging=ON
server_audit_file_path=/var/log/mysql/audit.log
server_audit_file_rotate_size=100000000  # 100MB rotation
server_audit_file_rotations=10           # Keep 10 rotated files
server_audit_incl_users=                 # Audit all users

# When rotations limit is reached, oldest file is automatically overwritten

# This implements FIFO behavior at the technical control level
```
{% endcode %}

{% hint style="success" %}
Technical Control Advantage: Operates reliably without depending on human memory or action.
{% endhint %}
{% endtab %}

{% tab title="Corrective Control" %}
**Why Corrective Control?**

Control Type Definitions:

| Control Type | Purpose                               | Timing        |
| ------------ | ------------------------------------- | ------------- |
| Preventive   | Stop incidents before they occur      | Before event  |
| Detective    | Identify incidents when they occur    | During event  |
| Corrective   | Mitigate or remediate after detection | After event   |
| Recovery     | Restore systems to normal operations  | Post-incident |

V-253678 as Corrective Control:

Problem Condition: Audit log storage space exhaustion

Corrective Action: Automatically overwrite oldest logs to free space

Result: Restores ability to continue audit logging

{% code title="Corrective Control Flow" overflow="wrap" %}
```
Normal Operation
    ↓
Storage Capacity Reached (Problem Detected)
    ↓
Automatic FIFO Overwrite Triggered (Corrective Action)
    ↓
Oldest Log Deleted → Space Freed → New Log Written
    ↓
Audit Logging Continues (Problem Corrected)
```
{% endcode %}

{% hint style="warning" %}
Not Preventive: This control doesn't prevent the storage from filling—it responds after it fills.

Not Detective: This control doesn't detect security incidents—it maintains the logging system that enables detection.
{% endhint %}
{% endtab %}

{% tab title="Alternative Classifications" %}
**Could This Be Other Control Types?**

Preventive Control Approach (Alternative):

Instead of overwriting when full, prevent the condition through:

* Proactive storage monitoring
* Automated storage expansion
* Alert-driven capacity management

Detective Control Component:

While primarily corrective, V-253678 has a detective aspect:

* Storage threshold monitoring (detects capacity issues)
* Log rotation triggers (detects when action needed)

Compensating Control:

In some interpretations, FIFO overwrite compensates for:

* Insufficient storage provisioning
* Lack of log forwarding to SIEM
* Missing automated archive processes

{% hint style="info" %}
Classification Reality: Security controls often have characteristics of multiple types. V-253678 is primarily corrective but includes detective elements (monitoring capacity) and may serve as a compensating control for inadequate storage planning.
{% endhint %}
{% endtab %}
{% endtabs %}

***

### <mark style="color:$danger;">3️⃣ Defense: Why This Requirement May Not Be Necessary</mark>

{% hint style="danger" %}
Core Argument

Instead of automatically overwriting audit logs (potentially destroying critical forensic evidence), implement proactive capacity management with alerting to prevent storage exhaustion before it occurs.
{% endhint %}

#### Argument Framework

{% tabs %}
{% tab title="Legal & Forensic Impact" %}
**Legal and Forensic Concerns**

Evidence Preservation Requirements:

Many regulatory frameworks and legal proceedings require complete, unaltered audit trails for specific retention periods:

| Framework | Retention Requirement                          | Overwrite Conflict                                  |
| --------- | ---------------------------------------------- | --------------------------------------------------- |
| PCI DSS   | 1 year minimum, 3 months immediately available | ⚠️ FIFO may delete evidence within retention period |
| HIPAA     | 6 years minimum                                | ⚠️ Critical PHI access logs could be overwritten    |
| SOX       | 7 years for financial records                  | ⚠️ Financial transaction audit trails at risk       |
| GDPR      | Varies by data processor requirements          | ⚠️ May delete evidence of unauthorized access       |

{% hint style="danger" %}
Forensic Investigation Risk

Overwriting audit logs can permanently destroy evidence needed for:

* Breach investigations (determining scope and impact)
* Compliance audits (demonstrating security controls)
* Legal proceedings (evidence in litigation)
* Incident root cause analysis (understanding attack vectors)
{% endhint %}

Real-World Scenario:

{% code title="Incident Investigation Timeline" overflow="wrap" %}
```
Day 1: Attacker gains initial access
    ↓
Day 15: Attacker establishes persistence
    ↓
Day 45: Attacker exfiltrates sensitive data
    ↓
Day 60: Audit logs reach capacity, FIFO overwrite begins
    ↓
Day 75: Breach discovered, investigation initiated
    ↓
Result: Initial access logs (Days 1-15) OVERWRITTEN
        Critical forensic evidence PERMANENTLY LOST
```
{% endcode %}

{% hint style="warning" %}
Criminal Prosecution Impact: Destroyed audit logs may prevent successful prosecution of attackers or result in regulatory penalties for evidence spoliation.
{% endhint %}
{% endtab %}

{% tab title="Proactive Alternative" %}
**Proactive Capacity Management Approach**

Alternative Solution: Implement graduated alerting with automated remediation before reaching capacity.

Capacity Threshold Strategy:

{% stepper %}
{% step %}
#### Warning Alert: 75% Capacity

Action: Notify administrators

{% code title="Alert Configuration" overflow="wrap" %}
```bash
# Monitoring configuration
if [ $(df -h /var/log/mysql | awk 'NR==2 {print $5}' | sed 's/%//') -ge 75 ]; then
    send_alert "WARNING: Audit log storage 75% full"
fi
```
{% endcode %}

Response Time: 7 days to address
{% endstep %}

{% step %}
#### Critical Alert: 85% Capacity

Action: Escalate to senior administrators + automated remediation options

{% code title="Remediation Options" overflow="wrap" %}
```bash
# Automated remediation choices
1. Compress older audit logs (gzip)
2. Archive to secondary storage (S3, tape)
3. Forward to SIEM (real-time streaming)
4. Expand storage (LVM, cloud volume)
```
{% endcode %}

Response Time: 48 hours to address
{% endstep %}

{% step %}
#### Emergency Alert: 90% Capacity

Action: Multiple escalations + automated emergency actions

{% code title="Emergency Actions" overflow="wrap" %}
```bash
# Emergency response
1. Immediately compress all compressible logs
2. Auto-archive oldest logs to emergency storage
3. Enable log forwarding to SIEM (if not already active)
4. Page on-call administrator
5. Trigger automated storage expansion (if configured)
```
{% endcode %}

Response Time: 12 hours maximum
{% endstep %}

{% step %}
#### Last Resort: 95% Capacity

Action: Automated protective measures before FIFO

{% code title="Protective Measures" overflow="wrap" %}
```bash
# Final safeguards before overwrite
1. Create emergency backup of all audit logs
2. Enable maximum compression
3. Reduce logging verbosity (emergency mode)
4. Execute forced archive to remote storage
5. Only then: Enable FIFO if all else fails
```
{% endcode %}

Result: FIFO overwrite becomes last resort, not default behavior
{% endstep %}
{% endstepper %}

{% hint style="success" %}
Proactive Advantage: This approach preserves all audit data while maintaining continuous logging capability—achieving both goals without compromise.
{% endhint %}
{% endtab %}

{% tab title="Cost-Benefit Analysis" %}
**Economic and Risk Analysis**

STIG Assumption: Storage is expensive, continuous logging is paramount.

Modern Reality: Storage costs have decreased dramatically while data value has increased.

Cost Comparison (2024):

| Resource           | Cost                 | Value of Lost Audit Data                  |
| ------------------ | -------------------- | ----------------------------------------- |
| 1TB SSD Storage    | \~$100               | Potentially millions in breach response   |
| Cloud Storage (S3) | \~$23/TB/month       | Investigation costs: $50K - $5M+          |
| Automated Archival | \~$500 initial setup | Regulatory fines: $100K - $50M+           |
| SIEM Integration   | \~$1K - $10K/month   | Forensic reconstruction: Often impossible |

{% hint style="info" %}
Risk Calculation

Cost of Prevention (storage + monitoring): $1,000 - $5,000 annually

Cost of Lost Evidence (single breach): $50,000 - $5,000,000+

ROI of Proactive Approach: 10x - 5,000x
{% endhint %}

Audit Data Value Examples:

<details>

<summary>Real-World Breach Investigation Costs</summary>

Scenario 1: Healthcare Data Breach

* Lost audit logs prevented determining scope
* Unable to notify affected patients accurately
* HIPAA fine: $4.3 million
* Legal settlements: $12 million
* Audit log value: $16.3 million

Scenario 2: Financial Services Breach

* Overwritten logs eliminated evidence of initial access
* Could not demonstrate due diligence to regulators
* SOX compliance violation
* Stock price impact: 23% drop
* Audit log value: $180 million market cap loss

Scenario 3: Government Contractor

* Lost audit trail prevented proving unauthorized access scope
* Contract termination
* Criminal investigation hampered
* Audit log value: $40 million contract + reputation

</details>

{% hint style="success" %}
Modern Best Practice: The cost of comprehensive audit log retention is negligible compared to the value of complete forensic evidence.
{% endhint %}
{% endtab %}

{% tab title="Enterprise Implementation" %}
**Enterprise-Grade Solution**

Recommended Architecture:

{% code title="Production Audit Infrastructure" overflow="wrap" %}
```
MariaDB Server
    ↓
    ├─► Local Audit Logs
    │   ├─ High-performance SSD
    │   ├─ 30-day retention minimum
    │   └─ Real-time monitoring
    │
    ├─► Real-Time SIEM Forwarding
    │   ├─ Splunk / ELK / Sentinel
    │   ├─ Immediate analysis
    │   ├─ Alerting and detection
    │   └─ Hot data (90 days)
    │
    ├─► Automated Archive System
    │   ├─ Compress + encrypt
    │   ├─ Object storage (S3 / Azure Blob)
    │   ├─ Warm data (1 year)
    │   └─ Compliance retention
    │
    └─► Long-Term Archive
        ├─ Tape / Glacier / Cold storage
        ├─ 7+ year retention
        ├─ Regulatory compliance
        └─ Legal hold capable

Capacity Monitoring & Alerting
    ├─ 75% → Warning
    ├─ 85% → Critical
    ├─ 90% → Emergency
    └─ 95% → Automated remediation
```
{% endcode %}

Implementation Benefits:

* ✅ Complete Evidence Preservation: All audit data retained per compliance requirements
* ✅ Performance Optimization: Hot data on fast storage, cold data archived
* ✅ Cost Efficiency: Tiered storage matches data access patterns
* ✅ Operational Excellence: Automated, self-managing system
* ✅ Compliance Assurance: Demonstrable adherence to retention policies
* ✅ Forensic Readiness: Complete audit trail available for investigations

{% hint style="success" %}
Enterprise Standard: This architecture is the industry standard for production database audit logging, far superior to FIFO overwrite.
{% endhint %}
{% endtab %}

{% tab title="Alternative Controls" %}
**More Effective Alternative Controls**

Instead of V-253678 (FIFO overwrite), implement these superior controls:

1. Preventive Controls (Stop the problem before it occurs):

{% code title="Preventive Control Implementation" overflow="wrap" %}
```bash
# Automated storage expansion (cloud environments)
if [ $(df -h /var/log/mysql | awk 'NR==2 {print $5}' | sed 's/%//') -ge 80 ]; then
    aws ec2 modify-volume --volume-id vol-xxxxx --size $((CURRENT_SIZE + 100))
fi

# LVM automatic expansion
lvextend -L +50G /dev/vg_logs/lv_audit
resize2fs /dev/vg_logs/lv_audit
```
{% endcode %}

2. Detective Controls (Identify issues early):

{% code title="Detective Control Monitoring" overflow="wrap" %}
```yaml
# Prometheus monitoring with graduated alerts
alert: AuditLogStorageWarning
expr: node_filesystem_avail_bytes{mountpoint="/var/log/mysql"} / 
      node_filesystem_size_bytes{mountpoint="/var/log/mysql"} < 0.25
for: 5m
labels:
  severity: warning
annotations:
  summary: "Audit log storage 75% full"

alert: AuditLogStorageCritical
expr: node_filesystem_avail_bytes{mountpoint="/var/log/mysql"} / 
      node_filesystem_size_bytes{mountpoint="/var/log/mysql"} < 0.10
for: 5m
labels:
  severity: critical
annotations:
  summary: "URGENT: Audit log storage 90% full"
```
{% endcode %}

3. Corrective Controls (Better than FIFO):

{% code title="Intelligent Corrective Actions" overflow="wrap" %}
```bash
#!/bin/bash
compress_and_archive_logs() {
    # Compress logs older than 30 days
    find /var/log/mysql/audit*.log -mtime +30 -exec gzip {} \;
    
    # Archive to S3 with versioning
    aws s3 sync /var/log/mysql/ s3://audit-archive/ \
        --exclude "*" \
        --include "*.gz" \
        --storage-class GLACIER
    
    # Remove local copies after successful archive
    find /var/log/mysql/*.gz -mtime +7 -exec rm {} \;
}

# Run hourly
echo "0 * * * * /usr/local/bin/compress_and_archive_logs.sh" | crontab -
```
{% endcode %}

Control Effectiveness Comparison:

| Control Approach   | Evidence Preservation | Continuous Logging | Compliance      | Cost          |
| ------------------ | --------------------- | ------------------ | --------------- | ------------- |
| V-253678 (FIFO)    | ❌ Data Loss           | ✅ Maintained       | ⚠️ Questionable | 💰 Low        |
| Proactive Alerting | ✅ Complete            | ✅ Maintained       | ✅ Full          | 💰💰 Medium   |
| SIEM Integration   | ✅ Complete            | ✅ Maintained       | ✅ Full          | 💰💰💰 Higher |
| Tiered Archive     | ✅ Complete            | ✅ Maintained       | ✅ Full          | 💰💰 Medium   |

{% hint style="success" %}
Recommendation: Implement a combination of proactive alerting, automated archival, and SIEM integration for superior security and compliance posture.
{% endhint %}
{% endtab %}
{% endtabs %}

***

### Additional Considerations

#### When FIFO Overwrite Might Be Acceptable

{% hint style="warning" %}
Limited Scenarios

FIFO overwrite may be appropriate in very specific circumstances:
{% endhint %}

<details>

<summary>Acceptable Use Cases for V-253678</summary>

1. Development/Testing Environments

* Non-production systems with no compliance requirements
* Rapid iteration cycles where historical logs have no value
* Resource-constrained lab environments

2. Low-Value Audit Data

* Non-critical application logging
* Systems not subject to compliance requirements
* Redundant logging (when data is already captured elsewhere)

3. Resource-Constrained Edge Devices

* IoT devices with extremely limited storage
* Embedded systems without network connectivity
* Temporary logging before data transmission

4. With Strong Compensating Controls

* Real-time SIEM forwarding (logs preserved elsewhere)
* Immediate archive before overwrite
* Very short retention requirements (< 7 days)

Conditions Required:

* ✅ No legal retention requirements
* ✅ No compliance mandates
* ✅ Low security impact if evidence is lost
* ✅ Alternative evidence sources available
* ✅ Documented risk acceptance by management

</details>

#### Risk Acceptance Framework

{% hint style="info" %}
Risk Acceptance Decision

If implementing V-253678 despite the arguments against it, follow formal risk acceptance procedures:
{% endhint %}

{% code title="Risk Acceptance Documentation" overflow="wrap" %}
```
Risk: Potential loss of audit evidence due to FIFO overwrite
Likelihood: High (inevitable with storage exhaustion)
Impact: High to Critical (depends on timing and investigation needs)

Mitigation Measures:
1. Implement capacity monitoring with alerts
2. Document retention periods achieved before overwrite
3. Establish evidence preservation procedures
4. Plan for reduced forensic capability

Risk Owner: [CISO / Security Manager]
Risk Acceptance Date: [Date]
Review Frequency: Quarterly
Compensating Controls: [List specific compensating controls]
```
{% endcode %}

***

### Key Takeaways

{% hint style="success" %}
Summary: V-253678 Critical Analysis
{% endhint %}

STIG Requirement Intent

* Goal: Maintain continuous audit logging capability
* Method: Automatic FIFO overwrite when storage is full
* Control Type: Technical (automated) - Corrective (responds to storage exhaustion)
* Rationale: Continuous logging prioritized over complete data retention

Core Arguments Against FIFO Overwrite

1. Evidence Preservation vs. Continuity Trade-off

* ❌ Permanently destroys forensic evidence
* ❌ May violate legal retention requirements
* ❌ Hampers breach investigation capabilities
* ❌ Creates compliance risk

2. Proactive Alternative Superior

* ✅ Graduated alerting prevents storage exhaustion
* ✅ Preserves all audit data
* ✅ Maintains continuous logging
* ✅ Meets both goals without compromise

3. Cost-Benefit Analysis

* ✅ Storage costs are minimal (< $500/year)
* ✅ Lost evidence costs are massive ($50K - $5M+)
* ✅ ROI strongly favors prevention over overwrite

4. Enterprise Best Practices

* ✅ Tiered storage architecture (hot → warm → cold)
* ✅ Real-time SIEM forwarding
* ✅ Automated archival and compression
* ✅ Capacity monitoring with automated remediation

Recommended Approach (high level)

* Implement graduated capacity alerting (75% / 85% / 90% / 95% thresholds)
* Deploy automated archival system (compress, encrypt, transfer to long-term storage)
* Integrate with SIEM (real-time log forwarding for analysis)
* Enable automated storage expansion (cloud auto-scaling or LVM growth)
* Document risk acceptance (if FIFO is still required for specific scenarios)
* Maintain compensating controls (alternative evidence sources, rapid response procedures)

When V-253678 May Be Acceptable

* ✅ Non-production environments
* ✅ Low-value audit data
* ✅ Strong compensating controls in place (SIEM forwarding)
* ✅ Formal risk acceptance documented
* ✅ No legal retention requirements

Enterprise Security Standard Modern best practice: Proactive capacity management with complete audit data preservation is the industry standard for production database systems. FIFO overwrite should be a last resort, not a default configuration.

***

### Additional Resources

STIG Documentation

* [https://public.cyber.mil/stigs/](https://public.cyber.mil/stigs/)
* [https://www.stigviewer.com/](https://www.stigviewer.com/)

Compliance Standards

* [https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
* [https://www.pcisecuritystandards.org/](https://www.pcisecuritystandards.org/)

Database Security

* [https://mariadb.com/kb/en/audit-plugin/](https://mariadb.com/kb/en/audit-plugin/)
* [https://www.nist.gov/itl/applied-cybersecurity/nice/resources/database-security-configuration-guide](https://www.nist.gov/itl/applied-cybersecurity/nice/resources/database-security-configuration-guide)

Audit and Logging Best Practices

* [https://cheatsheetseries.owasp.org/cheatsheets/Logging\_Cheat\_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
* [https://www.cisecurity.org/controls/v8/](https://www.cisecurity.org/controls/v8/)
