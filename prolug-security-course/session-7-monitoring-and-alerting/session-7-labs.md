---
cover: ../../.gitbook/assets/lab-ezgif.png
coverY: -217.6296730034564
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

# 🛠️ Session 7 Labs

I'm building a complete enterprise monitoring and alerting infrastructure through progressively complex labs.

<details>

<summary>🏗️ What We'll Build</summary>

A **production-grade monitoring and alerting system** with intrusion detection and metric visualization:

* [ ] Fail2Ban intrusion detection with real-time log analysis
* [ ] Grafana dashboards for security event visualization
* [ ] Prometheus metrics collection and time-series storage
* [ ] Telegraph + InfluxDB telemetry pipeline
* [ ] Discord alert integration for security notifications
* [ ] Alert threshold tuning and notification routing

</details>

### ⏳ Lab Progression

{% stepper %}
{% step %}
**📊 Log Monitoring → Visualize Fail2Ban security events in real-time**
{% endstep %}

{% step %}
**📈 Metrics & Alerting → Implement telemetry collection with alert automation**
{% endstep %}

{% step %}
**🔔 Advanced Configuration → Explore alert routing and threshold tuning**
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Real-World Workflow**

In production environments, you'll **tune existing monitoring systems** more often than you'll build them from scratch.

> _But you'll **absolutely** configure alerting thresholds, create custom dashboards, and integrate notification channels._
{% endhint %}

This lab teaches you the complete observability pipeline so you understand how metrics flow from collection through visualization to actionable alerts.

By the end, we'll understand:

* The **log monitoring architecture** (Fail2Ban logs → Grafana visualization)
* The **metrics collection pipeline** (Telegraph → InfluxDB → Prometheus → Grafana)
* The **alerting infrastructure** (threshold configuration, notification routing, Discord integration)

And have a complete monitoring solution that demonstrates both defensive security operations and operational observability capabilities.

***

### 📋 Required Materials

{% tabs %}
{% tab title="🛠️ Tools" %}
**SSH Client**:

* PuTTY (Windows)
* OpenSSH (Linux/macOS)
* Terminal emulator with SSH support

**Lab Environment**:

* KillerCoda interactive labs (browser-based, no installation required)
* Discord webhook URL for alert testing
* Internet connectivity for webhook notifications

**Optional Tools**:

* STIG Viewer 2.18 (if extending security compliance analysis)
* Download: [https://public.cyber.mil/stigs/downloads/](https://public.cyber.mil/stigs/downloads/)
{% endtab %}

{% tab title="📚 Prerequisites" %}
<table data-full-width="true"><thead><tr><th width="375" valign="top">Required Knowledge</th><th valign="top">Required Skills from Previous Sessions</th></tr></thead><tbody><tr><td valign="top"><ul><li>SystemD service management fundamentals</li><li>Configuration file editing (<code>/etc/</code> hierarchy)</li><li>Log analysis concepts (Session 6)</li><li>Basic networking (HTTP, webhooks)</li><li>Command-line troubleshooting</li></ul></td><td valign="top"><ul><li>Service status verification (<code>systemctl status</code>)</li><li>Log viewing (<code>journalctl</code>, log files)</li><li>Text editing (<code>vim</code>/<code>nano</code>)</li><li>Network service validation (<code>ss</code>, <code>curl</code>)</li><li>Firewall awareness (<code>firewall-cmd</code>)</li></ul></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

***

### 🛠️ Labs to Build Here

{% stepper %}
{% step %}
#### **🔍 Lab 1: Monitoring Fail2Ban Jails with Log Analysis**

Implement real-time log monitoring to visualize intrusion detection events in Grafana dashboards.

{% hint style="info" %}
**What You're Building**

A complete log monitoring pipeline that:

* Captures Fail2Ban security events from system logs
* Parses authentication failures and IP bans
* Visualizes security events in real-time dashboards
* Enables investigation of banned IPs and security incidents

This lab establishes the **log-based monitoring foundation** for defensive security operations.
{% endhint %}

**📖 Lab Instructions**

{% stepper %}
{% step %}
**Access the Lab Environment**

Navigate to the KillerCoda lab:

🔗 [**Fail2Ban with Log Monitoring**](https://killercoda.com/het-tanis/course/Linux-Labs/109-fail2ban-with-log-monitoring)

{% hint style="success" %}
**Interactive Environment**

KillerCoda provides a fully-configured Linux environment in your browser—no local installation required.

The lab environment includes pre-installed tools and automated setup scripts for hands-on learning.
{% endhint %}
{% endstep %}

{% step %}
**Understand the Lab Architecture**

**Components You'll Work With**:

```
Fail2Ban (Intrusion Detection)
    ↓ (monitors logs)
/var/log/secure (SSH authentication logs)
    ↓ (parsed by)
Loki (Log aggregation)
    ↓ (queried by)
Grafana (Visualization)
    ↓ (displays)
Security Events Dashboard
```

**Key Concepts**:

* **Fail2Ban**: Monitors authentication logs and bans IPs after failed attempts
* **Loki**: Log aggregation system (lightweight alternative to Elasticsearch)
* **LogQL**: Query language for extracting log information
* **Grafana**: Dashboard platform for visualization

{% hint style="info" %}
**Integration Point**

This builds directly on Session 6 (Logging) concepts—you're now **visualizing** the logs you learned to parse and forward.
{% endhint %}
{% endstep %}

{% step %}
**Complete the Lab Exercises**

{% stepper %}
{% step %}
Configure Fail2Ban to monitor SSH authentication attempts
{% endstep %}

{% step %}
Trigger security events by attempting failed SSH logins
{% endstep %}

{% step %}
View Loki logs showing Fail2Ban NOTICE events
{% endstep %}

{% step %}
Create Grafana dashboard displaying banned IPs in real-time
{% endstep %}

{% step %}
Investigate incidents by examining log details for banned IPs
{% endstep %}

{% step %}
Unban test IPs using Fail2Ban commands
{% endstep %}
{% endstepper %}

**Critical Commands You'll Use**:

```bash
# View Fail2Ban status
fail2ban-client status sshd

# Check banned IPs
fail2ban-client get sshd banned

# Unban an IP address
fail2ban-client set sshd unbanip 192.0.2.100

# View authentication logs
tail -f /var/log/secure

# Query Grafana for Fail2Ban events
# (performed in Grafana UI using LogQL)
```
{% endstep %}

{% step %}
**Verify Your Implementation**

**Success Criteria**:

✅ **Fail2Ban is actively monitoring SSH logs**

```bash
systemctl status fail2ban

# Should show: active (running)

fail2ban-client status sshd

# Should show: Currently banned: X
```

✅ **Grafana dashboard displays Fail2Ban events**

* Navigate to Grafana web interface
* Locate Fail2Ban dashboard
* Verify NOTICE events appear when bans occur

✅ **You can investigate and manage banned IPs**

* Identify banned IP addresses in dashboard
* Use `fail2ban-client` commands to unban test IPs
* Observe dashboard updates in real-time

{% hint style="success" %}
**Operational Validation**

In production, you'd monitor this dashboard continuously—any spikes in ban activity indicate reconnaissance or brute-force attacks requiring investigation.
{% endhint %}
{% endstep %}

{% step %}
**Analyze Security Events**

**Investigation Questions**:

1. **Pattern Recognition**: What patterns do you see in authentication failures?
   * Time-based clusters?
   * Same IP attempting multiple accounts?
   * Dictionary attack signatures?
2. **Legitimate vs. Malicious**: How would you distinguish between:
   * User forgetting password (legitimate)
   * Automated brute-force attack (malicious)
   * Misconfigured automation tool (legitimate but needs fixing)
3. **Response Strategy**: For each IP ban, what would be your next steps?
   * Investigate further using `whois` lookup?
   * Check if IP is on threat intelligence lists?
   * Extend ban duration or make permanent?

**Real-World Context**:

> "In my home lab, I see constant bot scanning—dozens of IPs banned daily. But in corporate environments with legitimate users, every ban might represent a security incident requiring investigation."

{% hint style="warning" %}
**Professional Judgment**

Not every security alert is malicious—but you need **visibility** to make that determination. This lab provides the investigation tools.
{% endhint %}
{% endstep %}
{% endstepper %}

**🔬 Analysis Questions**

After completing the KillerCoda lab:

1. **Were you able to see the IP address that was banned and successfully unban it?**
   * What commands did you use?
   * How did the dashboard reflect the changes?
2. **Were you able to see all the NOTICE events in Grafana?**
   * What does a "NOTICE" event represent in Fail2Ban?
   * How would you filter for specific types of events?
3. **What other questions do you have about this lab?**
   * Where would you look to figure them out?
   * What additional functionality would you want in production?

<details>

<summary><strong>Understanding Fail2Ban NOTICE Events</strong></summary>

**NOTICE Event Types**:

* `[sshd] Ban` - IP address has been banned
* `[sshd] Unban` - IP address ban has been lifted
* `[sshd] Found` - Match found for ban criteria (failed attempt detected)

**Why These Matter**:

* **Audit trail**: Complete record of security actions taken
* **Trend analysis**: Identify attack patterns over time
* **Incident response**: Investigate who was banned and why
* **False positive detection**: Identify legitimate users accidentally banned

**Query Example in LogQL**:

```
{job="fail2ban"} |= "NOTICE"
```

This filters Fail2Ban logs for all NOTICE-level events, showing security actions in real-time.

</details>

{% hint style="success" %}
✅ **Outcome**:

* Understanding of log-based security monitoring architecture
* Experience with Fail2Ban intrusion detection configuration
* Proficiency with Grafana dashboard creation for security events
* Knowledge of LogQL query language for log analysis
* Foundation for building defensive security monitoring systems
{% endhint %}
{% endstep %}

{% step %}
#### **📊 Lab 2: Monitoring Fail2Ban with Telemetry and Alerting**

Extend monitoring to include metrics-based alerting with automated Discord notifications for security events.

{% hint style="info" %}
**What You're Expanding**

Building on log monitoring, you're now adding:

* **Metrics collection**: Quantitative data about Fail2Ban jails (banned count, jail status)
* **Time-series storage**: Telegraph → InfluxDB pipeline for metric persistence
* **Alert configuration**: Threshold-based alerting in Grafana
* **Notification integration**: Discord webhooks for real-time security alerts

This lab completes the **full observability pipeline**: logs + metrics + alerts.
{% endhint %}

**📖 Lab Instructions**

{% stepper %}
{% step %}
**Access the Lab Environment**

Navigate to the KillerCoda lab:

🔗 [**Fail2Ban with Metric Alerting**](https://killercoda.com/het-tanis/course/Linux-Labs/110-fail2ban-with-metric-alerting)

{% hint style="warning" %}
**Lab Sequence**

This lab builds on Lab 1 concepts—ensure you've completed the log monitoring lab first to understand the full progression.
{% endhint %}
{% endstep %}

{% step %}
**Understand the Extended Architecture**

**Complete Monitoring Pipeline**:

```
Fail2Ban (Intrusion Detection)
    ↓ (generates)
Metrics (banned count, jail status)
    ↓ (collected by)
Telegraph (Metrics collection agent)
    ↓ (stored in)
InfluxDB (Time-series database)
    ↓ (queried by)
Grafana (Visualization + Alerting)
    ↓ (triggers alerts via)
Discord Webhook (Notification channel)
```

**New Components**:

* **Telegraph**: Flexible metrics collection agent (supports many inputs)
* **InfluxDB**: Time-series database optimized for metrics storage
* **Grafana Alerting**: Threshold-based alert rules with notification routing
* **Discord Integration**: Real-time security notifications to team channels

**Key Difference from Lab 1**:

* Lab 1: **Log-based** (text parsing, event visualization)
* Lab 2: **Metrics-based** (quantitative data, threshold alerting)

Both are essential—logs for investigation, metrics for automated alerting.
{% endstep %}

{% step %}
**Complete the Lab Exercises**

{% stepper %}
{% step %}
Configure Telegraph to collect Fail2Ban metrics
{% endstep %}

{% step %}
Set up InfluxDB as metrics storage backend
{% endstep %}

{% step %}
Create Grafana metrics dashboard showing:

* Banned IP count over time
* Jail status (binary: active bans or not)
* Failed authentication attempt rates
{% endstep %}

{% step %}
Configure alert rules with thresholds for security events
{% endstep %}

{% step %}
Integrate Discord webhook for alert notifications
{% endstep %}

{% step %}
Test alerting pipeline by triggering Fail2Ban events
{% endstep %}

{% step %}
Verify real alert delivery to Discord channel
{% endstep %}
{% endstepper %}

**Critical Configuration Files**:

```bash
# Telegraph configuration for Fail2Ban metrics
/etc/telegraf/telegraf.conf

# InfluxDB database for metrics storage
influx -execute 'CREATE DATABASE monitoring'

# Grafana alert notification channel (Discord)
# Configured in Grafana UI: Alerting → Notification channels
```
{% endstep %}

{% step %}
**Configure Alert Thresholds**

**Alert Rule Configuration**:

```
Rule Name: SSH Brute Force Detected
Condition: fail2ban_banned > 0 (for 5 minutes)
Notification: Discord webhook
Message: "SSH Brute Force: {banned_count} IPs banned on {hostname}"
```

**Threshold Considerations**:

* **Duration**: 5-minute confirmation prevents alert on transient events
* **Threshold**: Any banned IP (>0) indicates security event
* **Timing**: When should this alert fire?
  * Immediately during business hours?
  * Only critical escalation after hours?
  * Depends on your operational requirements

{% hint style="warning" %}
**Alert Fatigue Management**

In production with constant bot scanning, alerting on _every_ ban creates noise. You might adjust to:

* Alert only on multiple bans in short time window (attack surge)
* Alert only on bans from unexpected geographic regions
* Alert only during business hours for routine bans

This is **alert tuning**—balancing visibility with actionable intelligence.
{% endhint %}
{% endstep %}

{% step %}
**Verify Your Implementation**

**Success Criteria**:

✅ **Telegraph is collecting Fail2Ban metrics**

```bash
systemctl status telegraf

# Should show: active (running)

# Verify metrics in InfluxDB
influx -database monitoring -execute 'SELECT * FROM fail2ban LIMIT 5'

# Should show: banned counts, timestamps, jail names
```

✅ **Grafana dashboard displays real-time metrics**

* Navigate to Grafana web interface
* Locate Fail2Ban metrics dashboard
* Verify panels show: banned count, jail status graph

✅ **Test alert triggers successfully**

* Manually set test alert condition in Grafana
* Verify Discord receives test notification
* Contains expected fields: hostname, banned count, timestamp

✅ **Real alert fires on actual security event**

* Trigger Fail2Ban by multiple failed SSH attempts
* Wait for alert evaluation period (5 minutes)
* Verify Discord receives real security alert
* Alert message contains actionable information

{% hint style="success" %}
**Production Validation**

In corporate environments, alerts would route to:

* PagerDuty for 24/7 on-call rotation
* Security Operations Center (SOC) ticket queue
* Slack/Teams for team awareness
* Email for audit trail

Discord is our lab example—same principles apply to enterprise notification systems.
{% endhint %}
{% endstep %}

{% step %}
**Troubleshooting Common Issues**

<details>

<summary><strong>Problem: Don't see Fail2Ban in Grafana dashboard</strong></summary>

**Troubleshooting Steps**:

Check Telegraph Status and Logs

```bash
# Is Telegraph running
systemctl status telegraf
# Check for collection errors
journalctl -u telegraf -n 50 --no-pager
```

Verify InfluxDB Connection

```bash
# Can Telegraph reach InfluxDB
curl -i http://localhost:8086/ping
# Should return: HTTP/1.1 204 No Content

# Query InfluxDB directly
influx -database monitoring -execute 'SHOW MEASUREMENTS'
# Should show: fail2ban (if metrics are flowing)
```

Check Telegraph Configuration

```bash
# Verify Fail2Ban input is enabled in config
grep -A 10 '\[inputs.fail2ban\]' /etc/telegraf/telegraf.conf
# Verify InfluxDB output is configured
grep -A 10 '\[outputs.influxdb\]' /etc/telegraf/telegraf.conf
```

Restart Telegraph After Config Changes

```bash
nano /etc/telegraf/telegraf.conf  # Make correction
systemctl restart telegraf           # Apply changes
systemctl status telegraf            # Verify success
```

Check Grafana Data Source

## In Grafana UI:

## 1. Configuration → Data Sources

## 2. Select InfluxDB

## 3. Click "Test" button

## 4. Should show: "Data source is working"

**Common Causes**:

* Telegraph not configured to collect Fail2Ban metrics
* InfluxDB not running or not accessible
* Grafana data source pointing to wrong database/URL
* Firewall blocking localhost connections (uncommon but possible)

</details>

<details>

<summary><strong>Problem: Test alert worked, but real alert didn't trigger</strong></summary>

**Troubleshooting Steps**:

Verify Alert Rule Configuration

## In Grafana UI:

## Alerting → Alert Rules

## Check your Fail2Ban alert rule

## Verify condition: fail2ban\_banned > 0

## Verify evaluation interval: Every 1 minute

## Verify for duration: 5 minutes

Check Alert Evaluation State

## In the alert rule panel:

## Look for alert state indicator:

## - Green (OK): Condition not met

## - Yellow (Pending): Condition met, waiting for duration

## - Red (Alerting): Alert active, notifications sent

## If stuck in "Pending", wait full duration period

Manually Trigger High Ban Count

```bash
# Force multiple Fail2Ban events rapidly
for i in {1..5}; do
    # Trigger failed SSH from external system
    # Or use fail2ban-client to simulate
    sleep 1
done

# Check if metrics show increasing banned count
influx -database monitoring -execute '
    SELECT "banned" FROM "fail2ban"
     WHERE time > now() - 5m'
```

Verify Discord Webhook URL

## In Grafana notification channel config:

## 1. Test the webhook URL with curl:

curl -X POST "YOUR\_DISCORD\_WEBHOOK\_URL"\
-H "Content-Type: application/json"\
-d '{"content":"Test from command line"}'

## Should see message appear in Discord

## If curl test fails, webhook URL is invalid

Check Alert History

## In Grafana UI:

## Alerting → Alert Rules → \[Your Rule] → State history

## Shows all alert state transitions

## Verify alert entered "Alerting" state

## Check notification attempt status

**Common Causes**:

* Alert rule condition never actually met (no real bans occurred)
* Duration requirement not satisfied (alert pending < 5 minutes)
* Discord webhook URL incorrect or expired
* Alert notification channel not assigned to rule
* Grafana unable to reach Discord webhook (network/firewall)

</details>

<details>

<summary><strong>Problem: Too many alerts (noise)</strong></summary>

**Alert Tuning Strategies**:

Increase Threshold

```
# Instead of: banned > 0 (any ban alerts)
# Use: banned > 5 (alert only on attack surge)
# Rationale: Filters out isolated bot attempts
```

Extend Duration

```
# Instead of: for 5 minutes
# Use: for 30 minutes
# Rationale: Ensures sustained attack, not transient spike
```

Add Time-Based Routing

## In Alertmanager (if using Prometheus)

## Or Grafana notification channel settings:

## Route to PagerDuty: During business hours only

## Route to Discord: Informational, no urgent action

## Route to email: Audit trail for all events

Implement Alert Aggregation

## Group multiple Fail2Ban events into single alert:

## "10 IPs banned in last hour" (aggregated)

## Instead of: 10 separate "IP banned" alerts (noisy)

Use Alert Severity Levels

## Configure multiple alert rules:

## CRITICAL: >20 bans in 10 minutes (active attack)

## WARNING: >5 bans in 30 minutes (elevated activity)

## INFO: 1-5 bans in hour (normal bot scanning)

**Real-World Example**:

> "In my home lab, bot scanning is constant—alerting on every ban would be useless noise. I only alert when I see unusual patterns: sudden burst of bans, bans from specific countries I don't expect, or bans during unusual time windows."

**Key Principle**: **Alert on what's actionable**, not just what's measurable.

</details>
{% endstep %}
{% endstepper %}

**🔬 Analysis Questions**

After completing the KillerCoda lab:

1. **Do you see fail2ban metrics in the Grafana Dashboard?**
   * If not, what troubleshooting steps did you take?
   * How did you verify Telegraph → InfluxDB → Grafana pipeline?
2. **Did you get your test alert and real alert to trigger into the Discord channel?**
   * What was different between test and real alert messages?
   * How long did it take from ban event to Discord notification?
3. **What other applications or uses for this could you think of?**
   * Beyond SSH brute force, what would you monitor?
   * File integrity changes?
   * Service failures?
   * Resource exhaustion?
4. **Do you have other places you could send alerts that would help you professionally?**
   * Slack for team collaboration?
   * PagerDuty for on-call rotation?
   * ServiceNow for ticket creation?
   * Email for compliance audit trail?

<details>

<summary><strong>Extending Monitoring to Other Security Events</strong></summary>

**Other Security Monitoring Use Cases**:

Sudo Privilege Escalation Monitoring

```bash
# Monitor /var/log/secure for sudo events
# Alert on:
# - sudo failures (unauthorized escalation attempts)
# - sudo to root from unexpected users
# - sudo outside business hours
```

File Integrity Monitoring

```bash
# Use Telegraph file plugin or AIDE
# Alert on:
# - Changes to /etc/passwd, /etc/shadow
# - Modifications to SSH keys (/root/.ssh/)
# - Unexpected changes to system binaries
```

Service Availability Monitoring

```bash
# Monitor critical services via Telegraph
# Alert on:
# - httpd/nginx stopped unexpectedly
# - database service failures
# - firewall service disabled
```

Resource Exhaustion Detection

```bash
# Monitor system resources
# Alert on:
# - Disk usage > 85%
# - Memory pressure (OOM killer active)
# - CPU sustained > 95% for 15 minutes
```

**Professional Application**:

These same monitoring patterns apply to:

* **Application performance**: Response time thresholds
* **Business metrics**: Transaction failures, error rates
* **Infrastructure health**: Network latency, packet loss
* **Compliance monitoring**: Access to sensitive data, policy violations

**Key Insight**: The infrastructure you've built (Telegraph → InfluxDB → Grafana → Alerts) is **the same architecture** used for all enterprise observability—just with different input plugins and metrics.

</details>

{% hint style="success" %}
✅ **Outcome**:

* Complete understanding of metrics-based alerting architecture
* Experience with Telegraph metrics collection configuration
* Proficiency with InfluxDB time-series data storage
* Advanced Grafana alerting with notification routing
* Discord webhook integration for security notifications
* Alert threshold tuning and troubleshooting expertise
* Foundation for enterprise-grade monitoring and alerting systems
{% endhint %}
{% endstep %}

{% step %}
#### **🎯 Optional Challenge: Digging Deeper**

Advanced challenges for engineers ready to extend their monitoring and alerting knowledge.

{% hint style="info" %}
**Professional Development**

These challenges aren't required for lab completion but represent real-world scenarios you'll encounter in production environments. They're opportunities to:

* Explore enterprise monitoring platforms (Prometheus, Alertmanager)
* Understand advanced alert routing and notification strategies
* Practice documentation reading and self-directed learning
* Prepare for professional tool configuration and optimization
{% endhint %}

**📋 Challenge Tasks**

<details>

<summary><strong>Challenge 1: Explore Alertmanager Receivers</strong></summary>

**Objective**: Review the Alertmanager documentation and understand notification channel diversity.

**Documentation**: [Prometheus Alertmanager Configuration](https://prometheus.io/docs/alerting/latest/configuration/)

**Research Questions**:

What Are All the Types of Receivers You See?Review the `receiver` configuration section:email: Traditional email notificationspagerduty: Enterprise on-call managementslack: Team collaboration alertsvictorops: Incident response platformopsgenie: Incident and on-call managementwebhook: Generic HTTP endpoint (like our Discord integration)pushover: Mobile push notificationsweChat: Popular in Asian marketstelegram: Instant messaging alertsdiscord: Gaming/community platform (our lab example)msteams: Microsoft Teams integrationWhich Receivers Do You Have Experience With?Professional Experience Audit:✅ Email - Universal, used everywhereSlack - Common in tech companiesPagerDuty - Standard for on-call rotationMicrosoft Teams - Common in enterpriseDiscord - Personal/community useWebhook - Generic integration (most flexible)How Would You Choose Receivers for Different Alert Types?Mapping Severity to Notification Channel:CRITICAL AlertsPagerDuty → Wakes up on-call engineerPhone call → Immediate escalationRequirement: Someone must acknowledge within 5 minutesWARNING AlertsSlack/Teams → Team awareness during business hoursEmail → Asynchronous investigationRequirement: Response within 1 hourINFO EventsDashboard only → No active notificationDaily digest email → SummaryRequirement: Awareness, no urgent actionTime-Based Routing ConsiderationsBusiness Hours (9 AM - 5 PM):WARNING → Slack (team is at work)CRITICAL → Slack + Page (during business hours, team responds)After Hours (5 PM - 9 AM):WARNING → Email (investigate during next business day)CRITICAL → PagerDuty (wake up on-call)Weekend:WARNING → Email onlyCRITICAL → PagerDuty (unless planned maintenance)Real-World Routing Example

```yaml
# Alertmanager routing configuration
route:  receiver: default-email
  routes:
    # Security alerts always page
    - match:
        team: security
        severity: critical
      receiver: pagerduty-security
      continue: true  # Also send to Slack
    # Developer alerts to Slack during business hours
    - match:
        team: dev
      receiver: slack-dev
      active_time_intervals:
        - business-hours
    # Database alerts to dedicated channel
    - match:
        component: database
        severity: critical
      receiver: pagerduty-dba
```

**Career Application**:

* **Resume skill**: "Configured multi-channel alerting with Alertmanager"
* **Interview story**: "I implemented severity-based alert routing to reduce on-call fatigue while maintaining security incident response"
* **Professional judgment**: Understanding when to page vs. when to email is critical operational maturity

</details>

<details>

<summary><strong>Challenge 2: Modify Grafana Alert Thresholds</strong></summary>

**Objective**: Explore Grafana threshold configuration and trigger custom alerts to Discord.

**Documentation**: [Grafana Configure Thresholds](https://grafana.com/docs/grafana/latest/panels-visualizations/configure-thresholds/)

**Implementation Tasks**:

Review Threshold TypesBase Threshold: Default condition for all dataAbsolute Threshold: Fixed value (e.g., CPU > 90%)Percentage Threshold: Relative to baseline (e.g., 20% increase)Configure Custom Alert Threshold

```
# In Grafana:
# 1. Edit Fail2Ban panel
# 2. Navigate to Alert tab
# 3. Modify existing rule or create new
# Example custom conditions:
- Condition: avg() OF query(fail2ban_banned, 5m) > 10  Meaning: Alert if average banned count exceeds 10 over 5 minutes
- Condition: diff() OF query(fail2ban_banned, 5m) > 5  Meaning: Alert if banned count increased by 5+ in 5 minutes
- Condition: rate() OF query(authentication_failures, 1m) > 50  Meaning: Alert if authentication failures exceed 50/minute
```

Trigger Alert to Discord

```bash
# Modify your existing alert rule:
# 1. Lower threshold temporarily for testing
#    Example: banned > 0 instead of banned > 5
# 2. Trigger Fail2Ban event (failed SSH)
# 3. Wait for evaluation period
# 4. Verify Discord notification
# 5. Restore production threshold
```

Understand Critical vs. Warning Relationship

```
# In Grafana threshold configuration:
Thresholds:
 - Base: 0 (Green - Normal operation)
 - Warning: 5 (Yellow - Elevated activity)
 - Critical: 10 (Red - Active attack)
```

Relationship by Default:

* Thresholds are cumulative
* Critical must be > Warning
* Warning must be > Base
* Visual color coding helps operators quickly assess state

Configuration Example:

* Base: 0-4 banned IPs (Green)
* Warning: 5-9 banned IPs (Yellow)
* Critical: 10+ banned IPs (Red)

Implement Multi-Level Alerting

```yaml
# Configure multiple alert rules with different severity:
Alert 1: SSH Brute Force WARNING
 Condition: banned > 5
 Notification: Slack (info channel)
 Message: "Elevated SSH activity - investigating"

Alert 2: SSH Brute Force CRITICAL
 Condition: banned > 20
 Notification: PagerDuty (pages on-call)
 Message: "ACTIVE ATTACK - Immediate response required"

Alert 3: SSH Authentication Surge
 Condition: auth_failures > 100 in 5 min
 Notification: Security team email
 Message: "Unusual authentication pattern detected"
```

Test Threshold Modifications

## Testing workflow:

1. Create test alert rule with low threshold
2. Trigger condition (manual Fail2Ban event)
3. Verify alert evaluation (check Grafana alert state)
4. Confirm notification delivery (check Discord)
5. Document behavior (what triggered, when, how)
6. Adjust threshold based on findings
7. Repeat testing with production-realistic conditions

**Real-World Tuning Process**:

> "Alert tuning is iterative. You start with conservative thresholds, observe what fires in production, and adjust based on:
>
> * False positive rate (alerts that weren't actionable)
> * False negative rate (incidents you missed)
> * Operational impact (team interruption vs. risk tolerance)
>
> There's no perfect threshold—only thresholds optimized for your environment and risk profile."

**Career Impact**:

* Demonstrates **operational maturity**: Understanding alert tuning shows production experience
* Shows **analytical thinking**: Balancing sensitivity vs. specificity requires judgment
* Proves **tool proficiency**: Grafana alerting is standard enterprise monitoring skill

</details>

<details>

<summary><strong>Challenge 3: Design Alert Escalation Workflow</strong></summary>

**Objective**: Design a complete alert escalation strategy for a hypothetical production environment.

**Scenario**:

> You're the Linux security engineer for a financial services company with:
>
> * 500+ Linux servers (web, database, batch processing)
> * 24/7 operations (global customer base)
> * Compliance requirements (PCI-DSS, SOC 2)
> * Three-person security team (you + 2 engineers)
> * On-call rotation (weekly rotation, 24/7 coverage)

**Design Requirements**:

Define Alert Categories

Operational Alerts:

* Service failures
* Resource exhaustion
* Performance degradation

Security Alerts:

* Intrusion detection (Fail2Ban)
* Privilege escalation attempts
* File integrity violations

Compliance Alerts:

* Failed access to cardholder data
* Audit log tampering
* Encryption key access

Create Severity Matrix

CRITICAL:

* Definition: Immediate business impact, active security incident
* Response Time: 15 minutes
* Notification: PagerDuty (phone + SMS + app)
* Escalation: If not acknowledged in 15 min, escalate to manager
* Examples:
  * Active SQL injection attack
  * Database server compromised
  * Complete service outage

WARNING:

* Definition: Potential issue developing, no immediate impact
* Response Time: 1 hour during business hours
* Notification: Slack + Email
* Escalation: If unresolved in 4 hours, escalate to senior engineer
* Examples:
  * Elevated failed authentication attempts
  * Disk usage approaching 85%
  * Performance degradation (not critical yet)

INFO:

* Definition: Situational awareness, no action required
* Response Time: Next business day
* Notification: Email digest (daily summary)
* Escalation: None - informational only
* Examples:
  * Routine Fail2Ban blocks (isolated bots)
  * Successful patch deployments
  * Regular backup completions

Design Routing Logic

```yaml
# Alertmanager routing configuration
route:  receiver: default-email
  group_by: ['alertname', 'severity']
  group_wait: 30s        # Wait 30s to group related alerts
  group_interval: 5m     # Send updates every 5 minutes
  repeat_interval: 4h    # Re-send every 4 hours if not resolved
  routes:
    # Security critical - always page
    - match:
        team: security
        severity: critical
      receiver: pagerduty-security
      continue: true              # Also send to Slack
    - match:
        team: security
        severity: critical
      receiver: slack-security-critical
    # Security warnings - business hours only
    - match:
        team: security
        severity: warning
      receiver: slack-security
      active_time_intervals:
        - business-hours
    # Security info - daily digest
    - match:
        team: security
        severity: info
      receiver: email-security-digest
      group_interval: 24h         # Daily summary

# Active time intervals definition
time_intervals:
 - name: business-hours
   time_intervals:
     - weekdays: ['monday:friday']
       times:
         - start_time: '09:00'
           end_time: '17:00'
       location: 'America/New_York'
```

Implement Escalation Procedures

## Primary Engineer (15 min response):

1. PagerDuty alert sent
2. Phone call + SMS + app push
3. Engineer acknowledges via PagerDuty app
4. Begins investigation

## If Not Acknowledged (15 min timeout):

5. Escalate to Secondary Engineer
6. Same notification methods
7. Secondary acknowledges or escalates

## If Still Not Acknowledged (30 min total):

8. Escalate to Security Manager
9. Manager notified via multiple channels
10. Emergency response procedures activated

Escalation Tracking:

* All escalations logged in SIEM
* Incident ticket automatically created
* Post-incident review required for all escalations

Define On-Call Compensation

## Financial services standard:

* On-call stipend: $500/week (just for being on-call)
* Alert response: Additional compensation per incident
* Weekend response: 2x weekday rate
* Holiday response: 3x weekday rate

Rationale:

* Acknowledges impact on personal life
* Incentivizes quality alert tuning (fewer false positives)
* Industry standard for 24/7 operations

Document Runbooks for Common Alerts

## Alert: SSH Brute Force CRITICAL

### Symptom:

* 20+ IPs banned by Fail2Ban in 10 minutes

### Immediate Actions:

1. Verify banned IPs using fail2ban-client
2. Check if any legitimate IPs affected
3. Review /var/log/secure for successful authentications
4. Validate firewall rules are active

### Investigation:

5. Run whois on attacking IPs
6. Check threat intelligence feeds
7. Correlate with other security events
8. Document attack pattern

### Escalation Criteria:

* If attack continues despite bans → Escalate to senior
* If data exfiltration suspected → Escalate to incident response team
* If affected production systems → Notify infrastructure team

**Deliverable**: Write up your complete alert escalation design and post it in the Discord group for peer review.

**Discussion Points**:

* How did you balance security sensitivity with alert fatigue?
* What factors influenced your severity definitions?
* How would your design handle a major incident (e.g., active breach)?
* What compliance considerations affected your choices?

**Career Value**:

* **Architecture design**: Shows ability to design enterprise-scale systems
* **Operational thinking**: Demonstrates understanding of 24/7 operations
* **Compliance awareness**: Shows consideration of regulatory requirements
* **Team leadership**: Preparing you for senior/lead engineer responsibilities

</details>

{% hint style="success" %}
✅ **Outcome**:

* Deep understanding of enterprise alerting platforms (Alertmanager, Grafana)
* Experience with multi-channel notification routing strategies
* Alert threshold tuning and optimization skills
* Escalation workflow design for production environments
* Professional documentation and runbook creation
* Evidence of senior-level operational thinking and architecture design
{% endhint %}
{% endstep %}
{% endstepper %}

***

{% hint style="info" %}
🎓 **Professional Development**

These labs mirror actual security operations workflows—from intrusion detection through metrics collection to automated alerting and incident response.

> _You've built log aggregation pipelines, configured metrics-based alerting, and implemented notification routing using the same tools and processes required in Security Operations Centers (SOCs) and enterprise monitoring teams._

**Career Impact**: Add these skills to your project section on your resume:

* **"Fail2Ban intrusion detection configuration"**
* **"Grafana dashboard creation and alerting"**
* **"Telegraph + InfluxDB metrics pipeline"**
* **"Multi-channel alert routing and notification"**
* **"Security event monitoring and incident detection"**
{% endhint %}

***
