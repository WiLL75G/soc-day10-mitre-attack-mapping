# MITRE ATT&CK Detection Coverage Assessment

Mapping the detections in a home SOC lab against the ATT&CK matrix to answer one question honestly: what would get through. Four techniques covered, five gaps found, roadmap written.

## At a Glance

| Field | Detail |
| --- | --- |
| Assessment Type | Detection coverage and gap analysis |
| Framework | MITRE ATT&CK Enterprise Matrix v14 |
| Detection Stack | Splunk Enterprise and Universal Forwarder |
| Tools Used | MITRE ATT&CK Navigator, Splunk |
| Scope | 9 techniques assessed against the lab environment |
| Outcome | 4 detected, 5 gaps, remediation roadmap delivered |

## What This Is

Every SOC believes it has coverage until someone asks what it would miss.

This assessment takes the Splunk alert rules built in the previous lab and maps them onto the ATT&CK matrix, then marks what is not there. The value is not the green squares. It is the red ones.

Scope stated plainly: this is a home lab, and 9 techniques is a deliberately narrow assessment scope, not the full Enterprise matrix. The 44 percent figure describes coverage within that scope only. Reporting it as an overall SOC coverage score would be a lie made of arithmetic.

## Navigator Baseline

![Blank Layer](./screenshots/01_blank_layer.png)

A blank Navigator layer was initialised as the starting canvas, with Enterprise Matrix scope confirmed before any marking.

Starting blank matters. Start from the detections you already have and you map what you built. Start from the matrix and you map what an attacker can do, which is a different question with a much less comfortable answer.

## Mapping Current Detections

![Current Detections 1](./screenshots/02_current_detections_1.png)

![Current Detections 2](./screenshots/02_current_detections_2.png)

Four techniques were marked as actively detected, each tied back to a specific Splunk alert rule from the previous build.

The rule for this step is that a technique only gets marked green if a named, running alert covers it. Not a rule that is planned. Not a rule that could be written. A coverage map built on intentions is worse than no map, because it produces confidence without capability.

## Gap Visualisation

![Gap Map 1](./screenshots/03_coverage_gap_map_1.png)

Five gaps were marked in red, cross referenced against high prevalence adversary techniques.

The distribution is the finding. The gaps are not scattered randomly, they cluster in Execution, Initial Access, Impact, and Exfiltration, which means these are not missing rules. They are missing tactics.

## Final Coverage Map

![Gap Map 2](./screenshots/03_coverage_gap_map_2.png)

The unified map was finalised showing detections and gaps together, and the layer exported to JSON for version control.

Exporting the layer is what makes this repeatable. A coverage map is a snapshot, and a snapshot only has meaning next to the one before it. Versioned layers turn a one time assessment into a measurable trend.

## Detection Coverage

| Technique ID | Technique | Tactic | Status |
| --- | --- | --- | --- |
| T1110 | Brute force | Credential Access | Detected |
| T1078 | Valid accounts | Initial Access | Detected |
| T1548 | Abuse elevation control mechanism | Privilege Escalation | Detected |
| T1053.003 | Scheduled task or job, cron | Persistence | Detected |
| T1566 | Phishing | Initial Access | Gap |
| T1059.001 | PowerShell | Execution | Gap |
| T1059 | Command and scripting interpreter | Execution | Gap |
| T1486 | Data encrypted for impact | Impact | Gap |
| T1567 | Exfiltration over web service | Exfiltration | Gap |

## Coverage Metrics

| Metric | Value |
| --- | --- |
| Techniques assessed | 9 |
| Techniques detected | 4 |
| Coverage within assessed scope | 44 percent |
| Gaps identified | 5 |
| Detection source | Splunk Enterprise, forwarder based build |
| Layer artefact | layer_detection_coverage.json |

## What the Gaps Actually Mean

Read the map as an attacker and the problem is obvious.

Coverage sits entirely in authentication and persistence. Getting in is watched. Escalating is watched. Staying is watched.

Nothing watches execution, and nothing watches data leaving.

That shape means an attacker who arrives with valid credentials, which is the most common way anyone arrives, can run whatever they like through PowerShell and walk data out over a web service without generating a single alert. The lab would record them logging in and then go quiet for the entire rest of the intrusion.

Coverage concentrated at the front of the kill chain feels like security and is not. The gaps are at the end, which is where the damage happens.

## Remediation Roadmap

| Priority | Gap | Technique | Detection to Build |
| --- | --- | --- | --- |
| 1 | PowerShell execution | T1059.001 | Script Block Logging into Splunk, Event ID 4104 |
| 2 | Script interpreters | T1059 | Process creation logging with SPL rules |
| 3 | Web based exfiltration | T1567 | Outbound traffic anomaly detection |
| 4 | Ransomware behaviour | T1486 | File integrity monitoring alerts |
| 5 | Phishing | T1566 | Email gateway alerting and attachment scanning |

Execution is first because it is the widest gap and the cheapest to close. Script Block Logging is a policy change, not a purchase, and it turns the darkest part of the map green faster than anything else on the list.

## Analyst Findings

Four techniques confirmed detected by named, active Splunk rules.

Five gaps identified spanning Execution, Initial Access, Impact, and Exfiltration.

Coverage concentrated in authentication and persistence, absent in execution and exfiltration.

PowerShell monitoring absent despite being among the most used techniques in real intrusions.

No capability against ransomware behaviour or web based data exfiltration.

## Recommended Response

Close the execution gap before building anything else, because execution is the step every intrusion passes through.

Deploy PowerShell Script Block Logging and route Event ID 4104 into Splunk.

Add process creation telemetry to cover the wider scripting interpreter case.

Build outbound anomaly detection so data leaving is a detectable event.

Keep the Navigator layer in version control and re assess quarterly to measure whether coverage is moving.

## What This Lab Demonstrates

Auditing your own detections against an adversary framework rather than assuming coverage.

Refusing to mark a technique detected without a named, running rule behind it.

Reading a coverage map as a kill chain and identifying where an intrusion goes dark.

Prioritising remediation by attacker prevalence and cost to implement, not by matrix order.

Producing a versioned Navigator artefact that turns coverage into a trackable metric.

Reporting a coverage percentage with its scope attached instead of inflating it.

## Repository Structure

```
mitre-attack-detection-coverage-lab/
├── README.md
├── layer/
│   └── layer_detection_coverage.json
└── screenshots/
    ├── 01_blank_layer.png
    ├── 02_current_detections_1.png
    ├── 02_current_detections_2.png
    ├── 03_coverage_gap_map_1.png
    └── 03_coverage_gap_map_2.png
```

---

[![LinkedIn](https://img.shields.io/badge/LinkedIn-WilliamInCyber-blue?style=flat&logo=linkedin)](https://linkedin.com/in/WilliamInCyber)
[![X](https://img.shields.io/badge/X-WilliamInCyber-black?style=flat&logo=x)](https://x.com/WilliamInCyber)
