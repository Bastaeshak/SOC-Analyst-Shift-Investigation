# SOC Engagement Portfolio: Cloudora

## What This Engagement Was

This project documents my work as a SOC analyst investigating alerts in the Cloudora security environment. I worked through a live-style SOC queue containing identity, endpoint, and network security alerts while using ServiceNow to document my investigations and decisions.

My responsibility was to investigate each alert, determine what actually happened, assign a verdict and severity, decide whether the incident should be closed, monitored, or escalated, and leave enough documentation for another analyst to retrace my investigation.

All activity and data in this engagement are synthetic, but I approached each alert as I would during an actual SOC shift.

---

## How I Worked

For every alert, I followed the same seven-part investigation structure:

1. **Alert** - What triggered, which system or account was involved, the source of the alert, and when the activity occurred.
2. **Hypothesis** - Developed both malicious and benign explanations before reaching a conclusion.
3. **Evidence Checked** - Reviewed the available logs, account history, endpoint activity, network activity, Help Desk records, shift handovers, and change documentation relevant to the alert.
4. **Verdict** - Classified the alert as True Positive, False Positive, Duplicate, Escalate, or Insufficient Data.
5. **Severity** - Assigned Informational, Low, Medium, or High severity using the provided impact and confidence matrix.
6. **Action** - Determined whether to close, monitor, request additional evidence, or escalate the incident.
7. **Justification** - Documented the evidence and reasoning behind my decision so another analyst could retrace my investigation.

Where appropriate, I also wrote detection tuning recommendations.

One of my main goals throughout the engagement was to avoid treating the alert itself as proof. I tried to correlate multiple sources of evidence and understand the operational context before reaching a verdict.

---

## What I Found

### Repeated Failed Sign-Ins - `jt-admin`

A series of failed authentication attempts occurred against the newly created privileged account `jt-admin`. Instead of assuming the failures represented a password attack, I correlated the authentication activity with the shift handover, corporate device information, office network activity, and Help Desk ticket `HD-5121`.

The evidence showed that James Turner was a new IT engineer experiencing first-day authentication problems while setting up his account. After a verified password reset through the Help Desk, a successful authentication followed.

**Verdict:** False Positive  
**QA Review:** Excellent

[View investigation](verdicts/CLD-0201.md)

---

### Potentially Unwanted Application - `MAN-WS-204`

Endpoint protection detected `PUA:Win32/BundleLoader` after `helen.dray` downloaded a free PDF converter. The PUA was successfully quarantined on the first attempt, and a follow-up rescan two hours later returned clean with no reappearance.

I correctly classified the detection as a True Positive because the PUA was genuinely present and detected. However, my severity assessment was too high. I assigned Low when the stronger decision was Informational because remediation had already succeeded and there was no remaining activity requiring SOC response.

**Verdict:** True Positive - Remediated  
**QA Review:** Needs Improvement

[View investigation](verdicts/CLD-0202.md)

---

### Guest Account Sign-In - `ewalsh.ext@cloudora.io`

A first-time sign-in properties alert fired for Eleanor Walsh, an external Bexley Payroll Integrations consultant.

The original sign-in matched Eleanor's previously observed IP address, London location, macOS device, and Safari browser. The shift handover also confirmed that Bexley was in go-live week and that the guest relationship operated under an older policy that did not enforce MFA.

During the investigation, I identified an additional sign-in from `192.0.2.146` using Windows and Chrome that did not match Eleanor's established activity. I searched the available sign-in history and found no previous activity from that IP.

I then correlated additional proxy/egress evidence and identified later `ewalsh.ext` activity involving 34 documents in the Payroll-Mapping SharePoint library and an OAuth consent for "Bexley Sync Utility." The available evidence could not tie those sessions directly to the anomalous IP, so I did not have enough evidence to classify the activity as either malicious or benign.

**Verdict:** Insufficient Data  
**QA Review:** Satisfactory

[View investigation](verdicts/CLD-0203.md)

---

### TCP Port Sweep - `LDN-SCAN-01`

A TCP port-sweep alert showed `LDN-SCAN-01 (203.0.113.44)` probing more than 3,800 TCP ports across approximately 160 internal hosts.

The activity initially appeared suspicious because it occurred outside the scanner's normal schedule. I checked the shift handover and identified change record `CHG-2101`, which documented that the vulnerability scan had been moved to an earlier window for that week.

I correlated the source IP, hostname, traffic pattern, and timestamps with the approved change window and confirmed that the activity was authorized vulnerability scanning.

I also recommended narrowly scoped detection tuning that considers both the approved scanner identity and an active change window rather than broadly suppressing alerts from the scanner.

**Verdict:** False Positive - Authorized Activity  
**QA Review:** Excellent

[View investigation](verdicts/CLD-0204.md)

---

## What I Would Do Differently

The biggest areas I would improve from my first shift are **severity calibration and investigation efficiency**.

On the PUA investigation, I correctly identified the detection as a True Positive and documented the full detection and remediation timeline. However, I assigned **Low** severity when **Informational** was more appropriate. The PUA had already been quarantined successfully on the first attempt, and the follow-up rescan was clean with no reappearance. I also spent more time investigating the alert than necessary when the available evidence already showed that remediation was successful and there was nothing further for the SOC to action.

On the guest account investigation, I correctly reached an **Insufficient Data** verdict and identified the anomalous IP, checked its history, and correlated additional SharePoint and OAuth activity. However, I assigned **Medium** severity based too heavily on the potential consequences of an account compromise. The triggering sign-in itself matched the user's established baseline properties, and compromise had not been confirmed. **Low severity with continued monitoring** would have been the more defensible decision based on the evidence available at the time.

At the same time, the investigations that received the strongest QA feedback reinforced what worked well. Correlating alerts with Help Desk records, shift handovers, change records, known infrastructure, and authentication history allowed me to rule out competing hypotheses instead of making decisions from the alert alone.

Going into future shifts, I will focus on three questions before assigning severity or continuing an investigation:

1. **What does the evidence confirm actually happened?**
2. **What impact is supported by the evidence right now?**
3. **Will additional investigation realistically change my verdict or required action?**

The main lesson I am carrying forward is that good SOC analysis is not about investigating every alert as deeply as possible. It is about gathering enough evidence to make a defensible decision, assigning severity based on demonstrated impact and confidence, and knowing when there is enough information to close, monitor, or escalate an alert.

---

## Skills Practiced

- SOC alert triage
- ServiceNow incident documentation
- Identity and authentication analysis
- Endpoint / AV investigation
- Network traffic analysis
- Proxy / egress log analysis
- Cross-source log correlation
- User and system baselining
- Hypothesis-driven investigation
- Severity and impact assessment
- Incident escalation decisions
- Shift handover documentation
- Change-management correlation
- Detection tuning
- Evidence-based incident closure

---

## Contents

**`verdicts/`** - One file per investigated alert containing my original analysis, evidence, verdict, severity, actions, justification, tuning recommendations, and QA feedback.

**`handovers.md`** - My end-of-shift handovers documenting completed investigations, open items, monitoring requirements, and follow-up actions for the next analyst.

---

## Engagement Status

**Shift 1 Complete**

- 4 alerts investigated
- 2 Excellent QA reviews
- 1 Satisfactory QA review
- 1 Needs Improvement QA review
- 36-minute mean time to verdict

Additional SOC shifts and investigations will be added to this repository as I continue the engagement.

---

## Disclaimer

All data in this engagement is synthetic, including reserved IP ranges and domains. No real Cloudora systems, users, credentials, or customer information are represented.

The investigation decisions, verdicts, documentation, and analysis in this repository are my own work completed during the SOC engagement.
