# Prioritize Scan Findings

An agent skill that orders findings and root-cause groups into a work queue using the CERT/CC SSVC Deployer decision points (exploitation, exposure, automatability, human impact), plus KEV, EPSS, and reachability. Scanner severity is an input, not the answer. Every value records its source, date, and whether it was assumed.

## Use the skill

Use [`SKILL.md`](SKILL.md) in your agent’s skill directory under `prioritize-scan-findings/`. Provide the groups from [Group Findings by Root Cause](https://github.com/rafik-mikhail/group-findings-by-root-cause) and whatever asset context you have: exposure, environment, and criticality. Run it in `triage-order` mode before validation and in `remediation` mode after validation. An organization policy, if supplied, replaces the default table.

## Use as agent instructions

Copy the [Prioritize Scan Findings instruction gist](https://gist.github.com/rafik-mikhail/31414fded7450f3c390983722120d120) into your agent’s instruction file. The gist contains the same guidance with `applyTo: "**"` front matter for tools that support it. If your agent accepts plain Markdown instructions, copy the body without the front matter.

## Where it fits

This skill is step 4 of a seven-step workflow that takes a received scan report to fix pull requests. Steps 1–4 work on the whole report, steps 5–6 work on one finding at a time, and step 7 works on one batch at a time.

| Step | Skill | Works on |
|---|---|---|
| 1 | [Normalize Scan Report](https://github.com/rafik-mikhail/normalize-scan-report) | whole report |
| 2 | [Deduplicate Scan Findings](https://github.com/rafik-mikhail/deduplicate-scan-findings) | whole report |
| 3 | [Group Findings by Root Cause](https://github.com/rafik-mikhail/group-findings-by-root-cause) | whole report |
| 4 | **Prioritize Scan Findings** | whole report, then again for confirmed findings |
| 5 | [Validate Scan Finding](https://github.com/rafik-mikhail/validate-scan-finding) | one finding at a time |
| 6 | [Decide Finding Action](https://github.com/rafik-mikhail/decide-finding-action) | one finding at a time |
| 7 | [Batch Finding Fixes](https://github.com/rafik-mikhail/batch-finding-fixes) | one batch at a time |

Verification by rescan and closing with regression monitoring are not covered yet.

## How to test

Follow the [evaluation guide](TESTING.md).

## Notes

[NOTES.md](NOTES.md) covers gotchas, areas for improvement, and the sources behind the guidance.
