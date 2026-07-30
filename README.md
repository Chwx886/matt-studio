# Matt Studio

A reusable router skill for moving projects through the Matt Pocock engineering workflow:

1. Repository setup
2. Operational preflight
3. Discovery sized by decision fog (`grill-with-docs` or `wayfinder`)
4. Integrated product specification
5. End-to-end tracer tickets
6. Ticket-by-ticket implementation and two-axis code review

The router keeps UI prototypes in discovery, treats frontend and backend as one product contract by default, and favors vertical implementation slices.

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

## Repository layout

```text
skills/
└── matt-studio/
    └── SKILL.md
```
