# SOC Incident Report 002: Brute-Force Attack Detection and Investigation

**Analyst Role:** Aspiring Tier 1 SOC Analyst  
**Incident Type:** Repeated Failed Authentication Attempts  
**Investigation Environment:** Windows Endpoint and Wazuh SIEM  
**Incident Classification:** Controlled Laboratory Simulation  
**Investigation Status:** Completed

---

## 1. Executive Summary

This investigation examined five consecutive failed authentication attempts against a local Windows test account named `SOC-Lab-Test`.

The activity was deliberately generated in a controlled laboratory environment to simulate a repeated password-failure pattern commonly associated with brute-force attacks.

Windows Security Event ID 4625 recorded all five failed login attempts. Wazuh SIEM successfully collected the events and generated five corresponding alerts using Rule ID 60122, with alert level 5.

The source address was identified as `::1`, the IPv6 loopback address, indicating local authentication activity rather than an external network connection.

No evidence of an external attacker or successful unauthorized access was established.

## 2. Investigation Objectives

The objectives of this investigation were to:

- Generate five controlled failed login attempts against a local test account.
- Identify and examine Windows Security Event ID 4625.
- Analyse authentication failure details and status codes.
- Verify that Wazuh collected the generated events.
- Confirm whether Wazuh generated alerts for all five events.
- Identify the targeted account and source address.
- Document the findings from a Tier 1 SOC Analyst perspective.

## 3. Investigation Environment

| Component | Details |
|---|---|
| Endpoint | Windows workstation |
| Test Account | SOC-Lab-Test |
| SIEM Platform | Wazuh |
| Wazuh Agent | Windows-Laptop |
| Windows Event ID | 4625 |
| Wazuh Rule ID | 60122 |
| Wazuh Alert Level | 5 |
| Number of Failed Attempts | 5 |

## 4. Simulation Methodology

A controlled simulation was performed against the local account `SOC-Lab-Test`.

Five intentionally incorrect password attempts were made using the Windows `runas` command.

Each attempt returned Windows error code `1326`, indicating that the supplied username or password was incorrect.

The simulation successfully generated five failed authentication attempts without requiring an external system or attacking a real user's account.

## 5. Windows Security Event Investigation

Windows Security logs recorded five failed logon events corresponding to the simulation.

### 5.1 Event Findings

| Event Field | Observed Value |
|---|---|
| Event ID | 4625 |
| Target Account | SOC-Lab-Test |
| Logon Type | 2 |
| Failure Reason | Unknown user name or bad password |
| Status | 0xC000006D |
| SubStatus | 0xC000006A |
| Source Network Address | ::1 |

### 5.2 Technical Analysis

Event ID 4625 represents a failed logon attempt.

The status code `0xC000006D` indicates a logon failure, while substatus `0xC000006A` indicates that the password was incorrect.

Logon Type 2 represents an interactive logon.

The source address `::1` is the IPv6 loopback address. This indicates that the authentication activity originated locally on the Windows endpoint rather than from an identified external IP address.

## 6. Wazuh SIEM Detection and Analysis

The Windows endpoint successfully forwarded the five failed authentication events to Wazuh.

The following search query was used to locate the relevant alerts:

`data.win.system.eventID:4625 AND data.win.eventdata.targetUserName:SOC-Lab-Test`

Wazuh returned five matching alerts.

### 6.1 Detection Results

| Detection Field | Observed Value |
|---|---|
| Total Events Detected | 5 |
| Windows Event ID | 4625 |
| Wazuh Rule ID | 60122 |
| Wazuh Alert Level | 5 |
| Rule Description | Logon Failure - Unknown user or bad password |
| Target Account | SOC-Lab-Test |
| Source Address | ::1 |
| Wazuh Agent | Windows-Laptop |

### 6.2 Detection Analysis

Wazuh successfully detected all five failed authentication events generated during the controlled simulation.

Each event was associated with Rule ID 60122 and alert level 5.

The results demonstrate that the SIEM successfully collected the Windows authentication events and generated corresponding security alerts.

## 7. Evidence Collected

The following screenshots were collected during the investigation:

1. `01-wazuh-alert-overview-1.png` — Wazuh alert overview, part 1.
2. `02-wazuh-alert-overview-2.png` — Wazuh alert overview, part 2.
3. `03-wazuh-alert-details-1.png` — Wazuh alert details, part 1.
4. `04-wazuh-alert-details-2.png` — Wazuh alert details, part 2.
5. `05-wazuh-alert-details-3.png` — Wazuh alert details, part 3.
6. `06-windows-failed-login-events.png` — Windows Security Event Viewer showing the five failed login events.

## 8. Key Investigation Findings

The investigation established that:

1. Five controlled failed authentication attempts were generated against `SOC-Lab-Test`.
2. Windows recorded all five attempts as Event ID 4625.
3. SubStatus `0xC000006A` indicated an incorrect password.
4. The source address `::1` identified the activity as local authentication.
5. Wazuh detected all five events and generated five corresponding alerts.
6. All five alerts were associated with Rule ID 60122 and alert level 5.

## 9. Security Assessment

Repeated failed authentication attempts can indicate a brute-force attack, particularly when a high volume of failures targets one or more accounts.

However, the activity observed in this investigation was deliberately generated as a controlled laboratory exercise.

The source address `::1` indicates local loopback activity, and the evidence does not establish an external attacker, remote source IP, or successful account compromise.

**Assessment:** Controlled repeated authentication failures successfully detected by Wazuh. No confirmed external attack or unauthorized access was established.

## 10. Recommended SOC Analyst Actions

If similar activity were observed in a production environment, a Tier 1 SOC Analyst should:

- Review the frequency and timing of failed authentication events.
- Identify the affected user account and source IP address.
- Correlate authentication failures with successful logons.
- Examine related endpoint and network security events.
- Determine whether the activity originates locally or remotely.
- Escalate suspicious or persistent authentication activity according to the organisation's incident response procedures.

## 11. Conclusion

This investigation successfully demonstrated the generation, collection, detection, and analysis of repeated Windows authentication failures using Windows Security Event Viewer and Wazuh SIEM.

All five controlled failed login attempts were recorded as Event ID 4625 and detected by Wazuh through Rule ID 60122.

The exercise strengthened practical skills in Windows event analysis, authentication monitoring, SIEM alert investigation, and incident documentation.

**Final Outcome:** Successful detection and investigation of five controlled failed authentication events.

---

**Report:** SOC Incident Report 002  
**Project:** SOC Practical Lab 02 — Brute-Force Attack Detection and Investigation  
**Platform:** Windows Event Viewer and Wazuh SIEM
