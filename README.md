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

In a configured repository with one active Wayfinder map, you can also say:

```text
continue the work
```

The router resumes that map directly without requiring another slash command.

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
