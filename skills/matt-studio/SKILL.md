---
name: matt-studio
description: Route projects through the Matt Pocock studio pipeline, including discovery sizing, customer-demo prototype gates, feedback change control, specs, tracer tickets, implementation, and review. Use when the user says "matt studio," wants the studio workflow, says "continue/resume the work," wants to pause or checkpoint an active Wayfinder ticket, asks for a customer-reviewable clickable prototype, or says "process customer feedback."
---

# Matt Studio Workflow

Route a project through the shared Matt Pocock engineering skills. Most pipeline stages are user-invoked skills, so determine the current stage, do any safe preparation, and tell the user **exactly one command to type next** unless a natural-language hook below applies.

## Command syntax

Render the next command for the active harness:

- **Pi:** `/skill:<skill-name>`
- **Claude Code with the `mattpocock-studio` plugin:** `/mattpocock-studio:<skill-name>`
- **Another harness:** use that harness's skill-command syntax. If it is unknown, name the skill rather than inventing a command.

For example, setup is `/skill:setup-matt-pocock-skills` in Pi and `/mattpocock-studio:setup-matt-pocock-skills` in Claude Code.

## Natural-language Wayfinder continuation

This router adds ticket-aware continuation without changing Wayfinder's own frontier, claim, or resolution rules.

When the user says “continue the work,” “continue,” or “resume” without naming another target in a configured repository:

1. Query the configured tracker for open issues labelled `wayfinder:map`.
2. If no map exists, route the current pipeline stage normally. If several maps exist, ask which named map to resume, then apply the remaining steps within that map.
3. For the selected map, identify the current driving developer from the configured tracker and query its open child tickets already claimed by that developer. On a hosted tracker, a claim is an assignment to the authenticated tracker user; with local Markdown, it is an open ticket whose status is `claimed`. Never take over a ticket assigned to another collaborator. If the current identity cannot be determined safely, ask rather than guessing.
4. If exactly one claimed ticket exists:
   - Load the ticket, its latest `## Work checkpoint` if present, linked artifacts, and the current repository state. A checkpoint improves fidelity but is not required; when none exists, reconstruct from durable tracker and repository evidence and state any uncertainty before changing work.
   - If the ticket has gained an open blocker, name the blocker and ask whether to keep the ticket paused or release its claim; do not silently bypass the dependency.
   - Otherwise treat the phrase as explicit invocation of `wayfinder` with the selected map **and that named ticket**, then execute its **Work through the map** branch directly. The existing claim remains valid.
5. If several tickets are claimed by the current developer, ask which **named ticket** to resume.
6. If no ticket is claimed by the current developer, treat the phrase as explicit invocation of `wayfinder` with the selected map and no ticket, then execute its **Work through the map** branch directly; Wayfinder selects and claims the next frontier ticket as usual.

Do not ask for a slash command. Supplying a claimed ticket to Wayfinder resumes it; supplying only the map preserves Wayfinder's existing new-frontier mechanism.

## Natural-language Wayfinder checkpoint

When the user says “pause the work,” “stop for now,” or “checkpoint this ticket” while working a claimed Wayfinder ticket:

1. Reach a safe boundary for the current tool or tracker operation, then load the claimed ticket and inspect repository state.
2. Append a compact `## Work checkpoint` to the ticket's own comment/history containing:
   - work completed since the claim;
   - durable artifact pointers such as issue comments, paths, branches, or commits;
   - current provisional state, without presenting it as a resolved decision;
   - the exact next action;
   - blockers or open questions.
3. Reference existing artifacts rather than duplicating them, and exclude secrets, credentials, personal data, and confidential material.
4. Keep the ticket open and claimed. Do not add it to the map's Decisions-so-far or use Wayfinder's resolution sequence.
5. Ensure changed files are saved and report any uncommitted paths. Ask before committing or pushing; when continuation may happen from another checkout, make the artifacts durable only after that approval.
6. Return the named ticket and map links and tell the user that a fresh session in the configured repository can say **“continue the work.”**

The latest checkpoint is the handoff source for unfinished work. Wayfinder itself remains the source of truth for claiming and resolving tickets.

## Natural-language customer feedback

When the user says “process customer feedback” or “process customer feedback <issue-or-URL>” in a configured repository:

1. Read the repository's `docs/agents/customer-feedback.md` completely. If it is absent, read the bundled [customer-feedback policy](assets/customer-feedback.md) completely for this run.
2. Load `triage`, fetch or safely draft one feedback item, and identify the exact prototype/spec baseline under review.
3. Recommend the earliest invalidated stage and every affected downstream artifact, then wait for Product Owner approval.
4. Treat approval as explicit invocation of the selected route: load and execute the required skill directly, without another slash command, while preserving unaffected work.

Repository-specific policy wins over the bundled seed. Raw feedback is sanitized and shown to the Product Owner before it is published to a tracker.

## Pipeline

| Step | Skill | Purpose |
|------|-------|---------|
| 0 | `setup-matt-pocock-skills` | Run once per repo. Configure the issue tracker, triage vocabulary, and domain-doc layout. |
| 0½ | Studio hooks and operational preflight | Scaffold customer-feedback change control, make setup durable, and ensure the selected tracker operation can succeed. |
| 1 | `grill-with-docs` or `wayfinder` | Resolve product and technical decisions at the scale of the decision fog, including any customer-demo prototype gate. |
| 2 | `to-spec` | Synthesize the resolved decisions and approved prototype into an integrated spec on the configured tracker. |
| 3 | `to-tickets` | Break the spec into tracer-bullet tickets with explicit blocking edges. |
| 4 | `implement` | Implement one unblocked ticket at a time. Repeat until the backlog is complete. |
| 5 | `code-review` | Review each implementation against repository standards and its latest approved baseline before merge. |

Optional: `triage` moves incoming issues and external pull requests through the triage state machine. It is useful once a project has external issue flow; skip it for a fresh solo project.

## Size stage 1 by decision fog

Route by unresolved decisions, not estimated lines of code:

- Use `grill-with-docs` when one coherent feature or MVP can be sharpened in the current session.
- Use `wayfinder` when the destination spans several independent decision areas or needs research and prototypes before a spec is honest. Strong signals include a greenfield product spanning product definition plus client/backend architecture, unresolved UI or state behavior, external platform/API research, or safety and privacy trade-offs.
- If the evidence is mixed, present it and ask one sizing question rather than silently defaulting.

Wayfinder orchestrates the other discovery modes: grilling resolves human product decisions, research resolves external facts, prototypes make appearance or behavior concrete, and tasks clear manual prerequisites. The map is complete when the destination is clear and no decision tickets remain.

## Customer-demo prototype gate

When customer-visible workflow or design is in scope and the Product Owner wants a demonstration, read the [customer-demo prototype contract](references/customer-demo-prototype.md) completely. Make the portable clickable HTML file a late Stage 1 gate: its determining decisions block it, at least one customer review round informs it, and an exact approved version exists before `to-spec`.

A backend-only, library, infrastructure, or otherwise non-visual effort does not need this gate unless the Product Owner asks for it.

## Shape product planning

Make decisions before `to-spec`; that skill synthesizes rather than explores.

- For a product spanning frontend and backend, default to one integrated product spec containing both sides and their interface. Split specs only for independently releasable scopes or genuinely separate lifecycles.
- Explore UI or state design during stage 1. In Wayfinder, create a `prototype` child ticket and capture the validated decision while keeping non-production prototype code off the production branch. Apply the customer-demo contract when the artifact must demonstrate the integrated experience to reviewers.
- In `to-tickets`, prefer end-to-end slices through the UI, service/API, data, and tests needed for one demonstrable behavior. Avoid frontend-first or backend-first horizontal phases.

## Route the project

1. Inspect the repository and configured tracker to identify the current stage:
   - No `.git/`: initialize Git, then route to step 0.
   - No `docs/agents/issue-tracker.md`: route to step 0.
   - An open `wayfinder:map` exists: resume it under the natural-language rule, or route to `wayfinder` with the named map when the user did not ask to resume directly.
   - Discovery requires a customer-demo artifact but has no approved version: remain in step 1 and route to its prototype gate.
   - Setup exists but no spec exists on the configured tracker: route to step 1.
   - A spec exists but tickets do not: route to step 3.
   - Open implementation tickets remain: route to step 4, followed by step 5 after each implementation cycle.
2. In a configured repository, inspect the existing root instruction file and `docs/agents/customer-feedback.md`. When the feedback hook is absent or partial, read the [repository-hook setup](references/customer-feedback-hook.md) completely, present the migration, and ask before writing. A declined hook does not block the pipeline.
3. Run operational preflight for hosted trackers after setup choices are confirmed:
   - Verify tracker CLI authentication and repository access.
   - Verify labels required by the next write exist. `to-spec` and `to-tickets` require the configured `ready-for-agent` label; Wayfinder requires `wayfinder:map`, `wayfinder:research`, `wayfinder:prototype`, `wayfinder:grilling`, and `wayfinder:task`; the feedback intake requires the mapped `needs-triage` label.
   - Ask before creating missing tracker labels or committing/pushing setup files. Skip tracker-label checks for local Markdown.
   - Preflight is complete when the selected skill can perform its first tracker write, every installed repo hook resolves to its policy/template, and setup files have a durable home.
4. When routing to step 1, apply the decision-fog sizing rules and customer-demo gate above. State why the selected branch fits.
5. Do safe preparation that does not replace the selected pipeline skill.
6. Give the user exactly one fully rendered command for the active harness unless a natural-language hook explicitly authorizes direct execution.
7. Preserve discovery context:
   - For direct grilling, keep the decisions and any approved customer-demo artifact in the same session for `to-spec`.
   - For Wayfinder, use its map as durable context across sessions. Once the map is complete, load the map, resolved decisions, customer-feedback dispositions, and exact approved prototype before `to-spec`; if the final ticket is live grilling, continue to `to-spec` in that session.
8. During setup, recommend the local Markdown tracker when the project has no hosted remote and the user does not plan to add one.

## Availability check

The shared skills normally live at `~/.agents/skills/<skill-name>/SKILL.md`. Claude's plugin may expose the same skills under `~/.claude/skills/mattpocock-studio/skills/`.

If a required skill is absent, identify that as the next blocker and offer to install it from `mattpocock/skills` with the Skills CLI. Do not route to an unavailable command.
