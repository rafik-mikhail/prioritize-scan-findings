# Prioritize scan findings — evaluation guide

Version under test: `1.0.0`. Use fixture groups and clusters with fixed, recorded threat data (do not query live feeds during evaluation, so results are repeatable). Pre-register the expected outcome and the deciding signals.

| Fixture | Expected behavior |
|---|---|
| CVE in KEV, open exposure, automatable, high impact | `I`, KEV due date recorded. |
| CVE in KEV, small exposure, not automatable, low impact | Table gives `S`; KEV local rule raises it to `O`, labeled `local-rule`. |
| CVE not in KEV, EPSS 0.6 | Exploitation stays `none` or `public poc` per evidence; EPSS only orders within the outcome. |
| Critical CVSS-B, no exploitation, small exposure, low impact | `D`; reason states that severity alone does not set priority. |
| Unknown asset criticality | Human Impact `high`, marked `assumed`, listed as a question; status `provisional`. |
| Group with one `O` member and four `S` members | Group `O`; members keep their own outcomes. |
| Custom-code SAST finding with no CVE | Exploitation `none` unless specific evidence; no borrowed EPSS. |
| Secret reported valid, in a public repository | At least `O` by local rule. |
| Organization policy supplied | Policy applied and cited instead of the default table. |

## Scoring and release checks

Check every outcome against the table by code. Report wrong outcomes, missing sources or dates, and unlabeled assumptions separately. Fail the release on any EPSS-times-CVSS score, any unknown value resolved toward lower priority, or any group outcome that lowers a member.
