---
name: matt-studio
description: Route any project through the Matt Pocock studio pipeline, including sizing discovery for grill-with-docs versus Wayfinder and sequencing specs, tracer tickets, implementation, and review. Use when the user says "matt studio," wants the studio workflow, or says "continue/resume the work" in a repository already configured for it.
---

# Matt Studio Workflow

Route a project through the shared Matt Pocock engineering skills. Most pipeline stages are user-invoked skills, so determine the current stage, do any safe preparation, and tell the user **exactly one command to type next** unless the natural-language resume rule applies.

## Command syntax

Render the next command for the active harness:

- **Pi:** `/skill:<skill-name>`
- **Claude Code with the `mattpocock-studio` plugin:** `/mattpocock-studio:<skill-name>`
- **Another harness:** use that harness's skill-command syntax. If it is unknown, name the skill rather than inventing a command.

For example, setup is `/skill:setup-matt-pocock-skills` in Pi and `/mattpocock-studio:setup-matt-pocock-skills` in Claude Code.

## Natural-language resume

When the user says “continue the work,” “continue,” or “resume” without naming another target in a configured repository:

1. Query the configured tracker for open issues labelled `wayfinder:map`.
2. If exactly one exists, treat the phrase as explicit invocation of `wayfinder`: load that skill, load the map, and execute its **Work through the map** branch directly. Do not ask the user to type a slash command.
3. If several maps exist, ask which named map to resume.
4. If no map exists, route the current pipeline stage normally.

## Pipeline

| Step | Skill | Purpose |
|------|-------|---------|
| 0 | `setup-matt-pocock-skills` | Run once per repo. Configure the issue tracker, triage vocabulary, and domain-doc layout. |
| 0½ | Operational preflight | Make setup durable and ensure the selected tracker operation can succeed. |
| 1 | `grill-with-docs` or `wayfinder` | Resolve product and technical decisions at the scale of the decision fog. |
| 2 | `to-spec` | Synthesize the resolved decisions into an integrated spec and publish it to the configured tracker. |
| 3 | `to-tickets` | Break the spec into tracer-bullet tickets with explicit blocking edges. |
| 4 | `implement` | Implement one unblocked ticket at a time. Repeat until the backlog is complete. |
| 5 | `code-review` | Review each implementation against repository standards and its spec before merge. |

Optional: `triage` moves incoming issues and external pull requests through the triage state machine. It is useful once a project has external issue flow; skip it for a fresh solo project.

## Size stage 1 by decision fog

Route by unresolved decisions, not estimated lines of code:

- Use `grill-with-docs` when one coherent feature or MVP can be sharpened in the current session.
- Use `wayfinder` when the destination spans several independent decision areas or needs research and prototypes before a spec is honest. Strong signals include a greenfield product spanning product definition plus client/backend architecture, unresolved UI or state behavior, external platform/API research, or safety and privacy trade-offs.
- If the evidence is mixed, present it and ask one sizing question rather than silently defaulting.

Wayfinder orchestrates the other discovery modes: grilling resolves human product decisions, research resolves external facts, prototypes make appearance or behavior concrete, and tasks clear manual prerequisites. The map is complete when the destination is clear and no decision tickets remain.

## Shape product planning

Make decisions before `to-spec`; that skill synthesizes rather than explores.

- For a product spanning frontend and backend, default to one integrated product spec containing both sides and their interface. Split specs only for independently releasable scopes or genuinely separate lifecycles.
- Explore UI or state design during stage 1. In Wayfinder, create a `prototype` child ticket and capture the validated decision while keeping throwaway code off the production branch.
- In `to-tickets`, prefer end-to-end slices through the UI, service/API, data, and tests needed for one demonstrable behavior. Avoid frontend-first or backend-first horizontal phases.

## Route the project

1. Inspect the repository and configured tracker to identify the current stage:
   - No `.git/`: initialize Git, then route to step 0.
   - No `docs/agents/issue-tracker.md`: route to step 0.
   - An open `wayfinder:map` exists: resume it under the natural-language rule, or route to `wayfinder` with the named map when the user did not ask to resume directly.
   - Setup exists but no spec exists on the configured tracker: route to step 1.
   - A spec exists but tickets do not: route to step 3.
   - Open implementation tickets remain: route to step 4, followed by step 5 after each implementation cycle.
2. Run operational preflight for hosted trackers after setup choices are confirmed:
   - Verify tracker CLI authentication and repository access.
   - Verify labels required by the next write exist. `to-spec` and `to-tickets` require the configured `ready-for-agent` label; Wayfinder requires `wayfinder:map`, `wayfinder:research`, `wayfinder:prototype`, `wayfinder:grilling`, and `wayfinder:task`.
   - Ask before creating missing tracker labels or committing/pushing setup files. Skip tracker-label checks for local Markdown.
   - Preflight is complete when the selected skill can perform its first tracker write and the setup files have a durable home.
3. When routing to step 1, apply the decision-fog sizing rules above and state why the selected branch fits.
4. Do safe preparation that does not replace the selected pipeline skill.
5. Give the user exactly one fully rendered command for the active harness.
6. Preserve discovery context:
   - For direct grilling, tell the user to run `to-spec` in the same session.
   - For Wayfinder, use its map as durable context across sessions. Once the map is complete, load the map and resolved decisions before `to-spec`; if the final ticket is live grilling, continue to `to-spec` in that session.
7. During setup, recommend the local Markdown tracker when the project has no hosted remote and the user does not plan to add one.

## Availability check

The shared skills normally live at `~/.agents/skills/<skill-name>/SKILL.md`. Claude's plugin may expose the same skills under `~/.claude/skills/mattpocock-studio/skills/`.

If a required skill is absent, identify that as the next blocker and offer to install it from `mattpocock/skills` with the Skills CLI. Do not route to an unavailable command.
