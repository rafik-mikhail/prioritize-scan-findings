# Notes: Prioritize scan findings

## Gotchas

- **Run it twice.** Before validation it orders the review queue. After validation it sets fix timing. A refuted finding has no remediation priority.
- **EPSS is a forecast.** It estimates exploitation activity in the next 30 days for CVEs. It does not apply to custom-code SAST or most IaC findings. FIRST says thresholds are local; about the 90th percentile (0.04) matches the workload of a CVSS-Critical-only policy, and 0.008 matches CVSS High and above. FIRST also says multiplying EPSS by CVSS is never a good idea.
- **KEV means exploited.** Treat a KEV listing as active exploitation regardless of EPSS or CVSS.
- **Human Impact needs the user.** The agent cannot know mission or safety impact. Ask, and mark assumptions.
- **Mitigations change inputs.** A firewall rule can move exposure from open to controlled, but only while the rule stays in place.
- **The table is SSVC Deployer v2 as published by CERT/CC.** CISA's Track/Track*/Attend/Act tree is a coordinator view for government use; the deployer view fits teams that run the affected software.

## Areas for improvement

- Add a lookup script that fetches KEV and EPSS for all CVEs once and records dates.
- Add an asset-context file format (exposure, environment, criticality) so the agent stops asking per finding.
- Add a reachability import for SCA tools that report it.
- Calibrate the local rules against your own incident history.

## Sources

- [CERT/CC SSVC Deployer decision model](https://certcc.github.io/SSVC/howto/deployer_tree/): decision points, values, outcomes, and the full table.
- [CISA SSVC](https://www.cisa.gov/resources-tools/resources/stakeholder-specific-vulnerability-categorization-ssvc): CISA's tree and outcomes.
- [FIRST: Using EPSS](https://www.first.org/epss/using-epss) and [EPSS FAQ](https://www.first.org/epss/faq.html): thresholds and misuse.
- [CISA KEV catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog): authoritative list of exploited vulnerabilities.
- [FIRST CVSS v4.0 User Guide](https://www.first.org/cvss/v4.0/user-guide): base, threat, and environmental metrics.
- [DefectDojo RBVM buyer's guide](https://defectdojo.com/hubfs/Whitepapers/RBVM_Buyers_Guide_DefectDojo_Pro.pdf): combining KEV, EPSS, asset context, and reachability.
