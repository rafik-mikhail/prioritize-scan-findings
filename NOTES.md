# Notes: Prioritize scan findings

## Gotchas

- **Run it twice.** Before validation it orders the review queue. After validation it sets fix timing. A refuted finding has no remediation priority.
- **EPSS is a forecast.** It estimates exploitation activity in the next 30 days for CVEs. It does not apply to custom-code SAST or most IaC findings. FIRST says thresholds are local; about the 90th percentile (0.04) matches the workload of a CVSS-Critical-only policy, and 0.008 matches CVSS High and above. FIRST also says multiplying EPSS by CVSS is never a good idea.
- **KEV means exploited.** Treat a KEV listing as active exploitation regardless of EPSS or CVSS.
- **Human Impact needs the user.** The agent cannot know mission or safety impact. Ask, and mark assumptions.
- **Mitigations change inputs.** A firewall rule can move exposure from open to controlled, but only while the rule stays in place.
- **The table is SSVC Deployer v2 as published by CERT/CC.** CISA's Track/Track*/Attend/Act tree is a coordinator view for government use; the deployer view fits teams that run the affected software.
- **Fix-first trusts the tests, not the version number.** Endor Labs found that 76% of 1,250 vulnerabilities were fixable with a minor or patch upgrade, but 94% of those upgrades had at least one potentially breaking change. Without tests in CI, the shortcut does not apply.
- **Release age.** Renovate recommends a 14-day `minimumReleaseAge` for third-party packages, so a registry can pull a malicious release before you adopt it. For urgent items, record the trade-off instead of waiting.
- **A sample judges the rule, not the findings.** Five validated findings say whether a rule is noisy here; they never dismiss the unsampled ones. Google's Tricorder disables analyzers whose "not useful" rate exceeds 10%, which is a useful benchmark for when a rule needs tuning.
- **Defer means tracked, not reviewed.** CISA's SSVC `Track` means no action now, reassessment when new information arrives, and remediation within standard timelines. Validating deferred items first wastes review time on the lowest-priority work.
- **Combinations raise priority.** Wiz ranks findings higher when they combine, such as an exposed workload with a critical vulnerability and high privileges. Exposure here comes from asset context; this skill does not build a graph of related findings.

## Areas for improvement

- Add a lookup script that fetches KEV and EPSS for all CVEs once and records dates.
- Add an asset-context file format (exposure, environment, criticality) so the agent stops asking per finding.
- Add a reachability import for SCA tools that report it.
- Calibrate the local rules against your own incident history.
- Use a vendor remediation-risk rating (for example, Endor's upgrade impact analysis) as an extra fix-first check.
- Size rule samples with stratified sampling and a confidence bound instead of a fixed 5.

## Sources

- [CERT/CC SSVC Deployer decision model](https://certcc.github.io/SSVC/howto/deployer_tree/): decision points, values, outcomes, and the full table.
- [CISA SSVC](https://www.cisa.gov/resources-tools/resources/stakeholder-specific-vulnerability-categorization-ssvc): CISA's tree and outcomes.
- [FIRST: Using EPSS](https://www.first.org/epss/using-epss) and [EPSS FAQ](https://www.first.org/epss/faq.html): thresholds and misuse.
- [CISA KEV catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog): authoritative list of exploited vulnerabilities.
- [FIRST CVSS v4.0 User Guide](https://www.first.org/cvss/v4.0/user-guide): base, threat, and environmental metrics.
- [DefectDojo RBVM buyer's guide](https://defectdojo.com/hubfs/Whitepapers/RBVM_Buyers_Guide_DefectDojo_Pro.pdf): combining KEV, EPSS, asset context, and reachability.
- [Renovate: automerge](https://docs.renovatebot.com/key-concepts/automerge/) and [upgrade best practices](https://docs.renovatebot.com/upgrade-best-practices/): automerge patch and minor with tests; 14-day release age.
- [Endor Labs: open-source risk](https://www.endorlabs.com/learn/address-open-source-risks-with-endor-labs) and [upgrade impact analysis](https://docs.endorlabs.com/risk-remediation/upgrade-impact-analysis): 76% fixable by minor or patch, 94% with a potentially breaking change; remediation risk ratings.
- [Snyk deployment recommendations](https://docs.snyk.io/developer-tools/integrations/scm-integrations/deployment-recommendations): patch and minor upgrades by default, staged rollout.
- [Lessons from building static analysis tools at Google (CACM)](https://cacm.acm.org/research/lessons-from-building-static-analysis-tools-at-google/): effective false positives, the 10% threshold.
- [CISA SSVC](https://www.cisa.gov/resources-tools/resources/stakeholder-specific-vulnerability-categorization-ssvc): the `Track` decision.
- [Wiz: vulnerability prioritization](https://www.wiz.io/academy/vulnerability-management/vulnerability-prioritization): toxic combinations.
