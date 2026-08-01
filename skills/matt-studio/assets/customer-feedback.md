# Customer Feedback Change Control

Customer feedback may arrive during any Matt Studio stage. Treat it as input to an approved baseline, not as an automatic requirement or a reason to replay the whole pipeline.

## Authority and baseline

- The Product Owner accepts, rejects, or requests discovery for customer feedback.
- A named specialist reviewer approves changes in governed areas such as safety, security, privacy, compliance, accessibility, clinical policy, or financial controls when project documentation requires it.
- An approved baseline identifies the exact prototype version, spec issue/revision, and relevant implementation state.
- A customer-demo prototype completes at least one customer review round before its discovery gate closes.
- Approval establishes a versioned baseline; it does not prevent later change.

## Natural-language hook

When the Product Owner says **“process customer feedback”** or **“process customer feedback <issue-or-URL>”**, treat that as explicit invocation of this workflow.

- With an issue or URL, fetch its complete body, comments, labels, and referenced baselines.
- With raw feedback, remove identifying, personal, secret, or confidential information; prepare a draft using the configured tracker template; and ask before publishing it.
- Without either, ask for exactly one issue, URL, or feedback item.
- Load `triage` and `matt-studio`, plus any skill required by the recommended route.
- Recommend a route and wait for Product Owner approval before changing an approved baseline.
- Once the Product Owner approves the recommendation, treat that approval as explicit invocation of the selected route; do not require another slash command.

## Intake record

Each feedback item identifies:

- the sanitized feedback and desired outcome;
- the exact prototype version or commit reviewed, when applicable;
- the governing spec issue and revision, when one exists;
- the affected screen, workflow, domain concept, or implementation behaviour;
- sanitized evidence such as an annotated screenshot;
- possible safety, security, privacy, compliance, accessibility, data, or other governed impact.

New feedback starts in the configured `needs-triage` state. During triage, apply exactly one category role (`bug` or `enhancement`) and one triage-state role. Follow the `triage` skill's AI disclaimer rule for every AI-authored tracker issue or comment.

## Route by the earliest invalidated artifact

Re-enter at the earliest stage whose approved output no longer holds, then run only affected downstream work.

| Classification | Earliest route | Downstream work |
| --- | --- | --- |
| Baseline defect: implementation differs from approved behaviour | Implementation ticket | Implement and code-review the fix; preserve the approved spec |
| Presentation-only polish with no behaviour, accessibility, or governed impact | Current prototype or focused change ticket | Revise the visual baseline when needed, then implement and review affected UI |
| Clear workflow/design change with no unresolved decision | Versioned prototype/spec amendment | Update affected tickets and tests, then implement and review |
| Ambiguous, cross-cutting, or outcome-level request | Focused Wayfinder map | Resolve the fog, publish a spec amendment, ticket affected slices, implement, and review |
| Governed change involving safety, security, privacy, compliance, accessibility, data, or equivalent project concerns | Focused Wayfinder research/decision work plus named specialist review | Rebaseline every affected prototype, spec, test, and implementation artifact |
| Insufficient information | Triage `needs-info` | Resume routing after the reporter supplies the missing evidence |
| Rejected request | Triage `wontfix` | Preserve the rationale and baseline |

If the original Wayfinder map is open and the feedback belongs within its destination, add or revise child tickets there. After that map completes, prefer a new focused map to reopening and replaying the historical map.

## Product Owner checkpoint

Before applying feedback, present one recommendation containing:

1. classification and rationale;
2. the earliest stage to re-enter;
3. prototype, spec, tickets, tests, and code likely affected;
4. specialist review needs;
5. work that remains valid and will not be replayed.

Wait for the Product Owner to accept, reject, or redirect that recommendation. Customer approval and Product Owner acceptance are distinct.

## Versioning and traceability

- Preserve every approved prototype and spec baseline.
- Give customer-reviewable prototypes a visible version and link them to an exact commit or immutable artifact.
- Publish a revision or focused amendment rather than silently changing history.
- Link the feedback item to every resulting Wayfinder map, spec amendment, implementation ticket, and pull request.
- Update the spec when intended workflow, behaviour, governed constraints, data handling, or acceptance criteria change. Fix the code when code alone differs.
- Code review compares work with the latest approved baseline and routes newly discovered product requests through this workflow.

## Completion

A feedback item is complete when its disposition is recorded, accepted changes are linked, required specialist approval is present, affected downstream work is complete, and the resulting prototype/spec version is identifiable. Unaffected historical work remains closed and valid.
