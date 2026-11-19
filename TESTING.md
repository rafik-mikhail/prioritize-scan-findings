# Prioritize scan findings — evaluation guide

Version under test: `1.1.0`. Use fixture groups and clusters with fixed, recorded threat data (do not query live feeds during evaluation, so results are repeatable). Pre-register the expected outcome and the deciding signals.

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
| Lodash CVE, fix 4.17.21 within major 4, CI tests present | Route `fix-first`. |
| Same CVE, fix requires a major upgrade | Route `validate` (or `defer-track` if `D`). |
| Same CVE, no tests in CI | Route `validate`; the shortcut needs tests. |
| Fixing version published 3 days ago, outcome `scheduled` | `fix-first` noted with the release-age concern, or wait until 14 days; outcome `O` records the trade-off. |
| 30 clusters from one SAST rule, outcomes `S` and `D` | Route `sample-rule`, sample of 5 spread across files, including the top-ranked cluster. |
| One `O` cluster from a high-volume rule | Route `validate`, not sampled. |
| `D` SAST finding | Route `defer-track` with reassessment triggers; not sent to validation. |

## Scoring and release checks

Check every outcome against the table by code. Report wrong outcomes, missing sources or dates, and unlabeled assumptions separately. Fail the release on any EPSS-times-CVSS score, any unknown value resolved toward lower priority, or any group outcome that lowers a member.
