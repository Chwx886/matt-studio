# Install the Customer-Feedback Repository Hook

Load this branch during Matt Studio setup or migration when the repository lacks `docs/agents/customer-feedback.md` or an equivalent natural-language hook.

## Inspect

Read the repository's configured tracker doc, triage-label mapping, and whichever root instruction file already exists (`CLAUDE.md` first, otherwise `AGENTS.md`). Determine the actual `needs-triage` label and tracker type. Preserve existing user-authored instructions.

## Present and confirm

Show the Product Owner the proposed instruction block, policy path, and tracker template path. Ask before writing files, creating missing labels, committing, or pushing. Declining the optional hook does not block the rest of Matt Studio.

Use this instruction block under the existing `## Agent skills` section:

```markdown
### Process customer feedback

When the user says “process customer feedback” or “process customer feedback <issue-or-URL>,” treat it as an explicit request to run the repository's customer-feedback change-control workflow. Follow `docs/agents/customer-feedback.md`.

Recommend the earliest stage whose approved output is invalidated and wait for Product Owner approval before changing a baseline. Once approved, execute the selected Matt Studio route directly and rerun only affected downstream stages; do not require another slash command or replay unaffected work.
```

## Write

1. Add or update the instruction block in the existing root instruction file. Do not create the other instruction-file convention beside it.
2. Copy [`../assets/customer-feedback.md`](../assets/customer-feedback.md) to `docs/agents/customer-feedback.md` unless the repository already has a customized equivalent. Merge upgrades rather than overwriting project-specific governance.
3. Add one intake template matching the configured tracker:
   - GitHub: render [`../assets/customer-feedback-github.yml.tmpl`](../assets/customer-feedback-github.yml.tmpl) to `.github/ISSUE_TEMPLATE/customer-feedback.yml`.
   - GitLab: render [`../assets/customer-feedback-gitlab.md.tmpl`](../assets/customer-feedback-gitlab.md.tmpl) to `.gitlab/issue_templates/Customer Feedback.md`.
   - Local Markdown: render [`../assets/customer-feedback-local.md.tmpl`](../assets/customer-feedback-local.md.tmpl) to `.scratch/_templates/customer-feedback.md`.
   - Other tracker: adapt the same fields to its native issue template or document the manual intake shape in `docs/agents/issue-tracker.md`.
4. Replace every `{{NEEDS_TRIAGE_LABEL}}` placeholder with the mapped tracker label.
5. Add a short `## Customer feedback` section to `docs/agents/issue-tracker.md` naming the template and policy.
6. For a hosted tracker, verify the rendered initial-state label exists. Ask before creating it.

For raw feedback, the hook drafts and sanitizes the intake record, then asks before publishing. Tracker content must not contain personal data, secrets, credentials, or confidential customer material.

## Upgrade behaviour

An existing configured repository may contain part of the hook. Compare by meaning, add only missing pieces, and retain local additions. A repository-specific policy is authoritative over the bundled seed.

Setup is complete when the trigger is present, the policy and tracker template are reachable, no placeholders remain, referenced labels exist (for hosted trackers), and the root instruction file contains only one customer-feedback hook.
