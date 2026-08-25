# End-of-shift handovers · Cloudora

## Shift 1 · 2026-08-25

SHIFT HANDOVER - 08/25/2026

SHIFT STATUS
Alerts investigated: 3
Resolved: 2
On Hold: 1
Escalated: 0

--------------------------------------------------

1. PUA REMEDIATED - MAN-WS-204 / helen.dray

Verdict: True Positive - Remediated
Severity: Low
State: Resolved

PUA:Win32/BundleLoader detected in freepdf_setup.exe.

17:41:52 UTC - Installer downloaded.
05:31:02 UTC - PUA detected.
05:31:04 UTC - Quarantine successful on attempt 1.
07:31:04 UTC - Rescan CLEAN with no reappearance.

No evidence of persistence or continued impact. No escalation required.

User education recommended regarding free/bundled installers and use of approved software.

Tuning: None recommended. Detection behaved as intended.

--------------------------------------------------

2. GUEST ACCOUNT SIGN-IN - ewalsh.ext@cloudora.io

Verdict: Insufficient Data
Severity: Medium
State: On Hold / Monitoring
Escalation: Not currently escalated

Original 21:47 UTC sign-in from 198.51.100.77 matched Eleanor Walsh's established London, macOS/Safari activity.

Morning handover confirms Eleanor is an authorized Bexley consultant working during go-live week. Lack of MFA is expected under the documented Bexley guest-access policy.

During investigation, a separate anomalous sign-in was identified:

IP: 192.0.2.146
Device: Windows
Browser: Chrome
Network: Unknown / hosting infrastructure

This does not match Eleanor's established sign-in pattern. Search of available sign-in logs found no other activity from 192.0.2.146.

Additional proxy/egress review identified:

Oct. 7 11:02 UTC - ewalsh.ext session accessed 34 documents in the Payroll-Mapping SharePoint library over approximately 25 minutes.

Oct. 8 23:52 UTC - ewalsh.ext session performed OAuth consent for "Bexley Sync Utility."

Proxy logs do not provide the originating source IP, so these sessions cannot currently be tied to 192.0.2.146.

NEXT SHIFT ACTIONS:

- Confirm with Eleanor/Bexley whether the 192.0.2.146 Windows/Chrome session was authorized.
- Obtain full session/authentication details for 192.0.2.146.
- Correlate the anomalous session with SharePoint and OAuth activity.
- Determine whether client/payroll data was viewed, downloaded, modified, deleted, or shared.
- Continue monitoring for 192.0.2.146, unfamiliar IPs/devices, unusual SharePoint activity, large downloads, and OAuth consent activity.

ESCALATE TO vCISO IF:

- Eleanor denies the session.
- Unauthorized client/payroll data access is confirmed.
- Confirmed malicious activity expands beyond the current scope.
- Activity cannot be bounded with available evidence.

DO NOT CLOSE - follow-up required.

--------------------------------------------------

3. TCP PORT SWEEP - LDN-SCAN-01 / 203.0.113.44

Verdict: False Positive - Authorized Activity
Severity: Informational
State: Resolved
Resolution: Resolved by Change

LDN-SCAN-01 probed 3,800+ TCP ports across approximately 160 internal hosts using sequential SYN probes with no data transfer.

Normal scan window:
Monday 22:00 - Tuesday 03:00 UTC

CHG-2101 temporary window:
Sunday 23:00 - Monday 03:00 UTC

Observed activity:
Sunday 23:30 - Monday approximately 01:1x UTC

Source, traffic pattern, and timestamps matched the authorized vulnerability scan and approved CHG-2101 window.

No escalation or additional investigation required.

Tuning recommendation: Consider change-window-aware suppression/severity reduction only when the authorized scanner identity AND an active approved scan window match. Continue alerting outside approved windows or for unexpected sources.

--------------------------------------------------

NEXT SHIFT PRIORITY

Priority: ewalsh.ext@cloudora.io / Eleanor Walsh

PUA and TCP port-sweep investigations are complete.

Eleanor investigation remains open pending validation of the 192.0.2.146 session and correlation with subsequent SharePoint/OAuth activity.
