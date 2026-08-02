# Matt Studio

A reusable router skill for moving projects through the Matt Pocock engineering workflow:

1. Repository setup and customer-feedback hooks
2. Operational preflight
3. Discovery sized by decision fog (`grill-with-docs` or `wayfinder`)
4. Portable customer-demo prototypes and review gates when customer-visible workflow/design is in scope
5. Integrated product specification
6. End-to-end tracer tickets
7. Ticket-by-ticket implementation and two-axis code review
8. Controlled feedback re-entry at the earliest invalidated stage

The router keeps customer-demo prototypes in discovery, treats frontend and backend as one product contract by default, favors vertical implementation slices, and preserves unaffected approved work when feedback arrives later.

## Prerequisites

Install the underlying skills from [`mattpocock/skills`](https://github.com/mattpocock/skills). This repository supplies the router, not the pipeline stages themselves.

## Install globally

```bash
npx skills add Chwx886/matt-studio --skill matt-studio --global
```

Pi discovers global skills under `~/.agents/skills/`. Start the router with:

```text
/skill:matt-studio
```

## Continue Wayfinder work across sessions

In a configured repository with one active Wayfinder map, start a fresh agent session and say:

```text
continue the work
```

The router distinguishes two situations:

- If the authenticated tracker user has exactly one open, claimed child ticket, it resumes that in-progress ticket.
- If that user has no claimed ticket, it lets Wayfinder select and claim the next frontier ticket normally.

If several tickets are claimed, the router asks which named ticket to resume. It never takes over another collaborator's claim and does not change Wayfinder's own selection or resolution mechanism.

Before ending a session midway through a ticket, say:

```text
checkpoint this ticket
```

The router records a compact checkpoint on the ticket and leaves it open and claimed. After a computer restart, a new session can reconstruct the work from the tracker and repository by saying `continue the work`; Pi's session-resume command is not required. Uncommitted work remains local to its checkout, so use a committed and pushed branch—with approval—when continuation must work from another machine.

## Customer-demo prototypes

For customer-visible products, Matt Studio can make one portable, self-contained clickable HTML file a late discovery gate. The artifact demonstrates every agreed screen and consequential branch with synthetic data, includes reviewer navigation, completes at least one customer review round, and becomes an exact versioned input to `to-spec`.

## Customer-feedback hook

Configured repositories can accept:

```text
process customer feedback <issue-or-URL>
```

The hook captures sanitized feedback, waits for Product Owner disposition, and re-enters at the earliest invalidated stage. Clear fixes can go directly to implementation; decision fog receives a focused Wayfinder map. Unaffected work is not replayed.

Existing configured repositories are offered the hook as a migration the next time the router inspects them.

## Repository layout

```text
skills/
└── matt-studio/
    ├── SKILL.md
    ├── assets/       # tracker intake and repo-policy seeds
    └── references/   # customer-demo and hook setup branches
```
