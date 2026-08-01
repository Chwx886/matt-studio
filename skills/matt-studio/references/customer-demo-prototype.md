# Customer-Demo Prototype Gate

Load this branch when a product effort includes customer-visible workflow or design and the Product Owner wants a reviewable demonstration.

## Position in the pipeline

The customer-demo prototype is a **Stage 1 discovery artifact**. Build it after the decisions that determine its screens and transitions, review it with a customer, and settle accepted feedback before `to-spec`.

- In Wayfinder, put the requirement in the map destination and create a late `prototype` child ticket. Wire every decision that determines the integrated flow as a blocker.
- In a small direct-grilling effort, build and review the artifact after the decisions resolve and before invoking `to-spec` in the same discovery context.
- Keep the map or discovery effort open through at least one customer review round. Approval establishes a baseline; it does not promise that the product will never change.

`to-spec` consumes and links the approved artifact. It does not create the first customer-reviewable version.

## Artifact contract

Produce one portable, self-contained `.html` file that a reviewer can open directly in a current browser without a build command, local server, login, backend, or network connection unless the discovery decisions explicitly require an exception.

The file must:

- embed its CSS, JavaScript, fixtures, and required visual assets;
- use synthetic data and simulate writes, integrations, messages, payments, analytics, emergency actions, and other external effects;
- carry a visible prototype label and version;
- render every distinct customer-visible screen in the agreed prototype scope;
- make every agreed branch and consequential state reachable without asking the reviewer to reproduce hidden preconditions;
- include a clearly separate reviewer control that can jump to any screen or scenario, switch important personas/configurations, reset state, and return to the normal start;
- support the primary flow through normal in-app controls rather than functioning only as a screen gallery;
- preserve enough responsive, keyboard, focus, contrast, and screen-reader behaviour for accessibility feedback to be meaningful;
- identify assumptions and intentionally simulated behaviour inside reviewer notes;
- contain no real customer data, personal data, credentials, secrets, or live destructive actions.

“Every screen” means every distinct screen and agreed consequential state, not every combinatorial permutation. Record that boundary explicitly in the prototype ticket.

## Build discipline

The integrated demonstration answers one question: **is the approved workflow and design coherent when experienced end to end?** Load the `prototype` skill and treat this contract as the effort-specific shape. Use multiple visual variants only where a live design decision genuinely requires comparison; the integrated baseline itself does not need three arbitrary variants.

Use the project's design system and approved assets when they exist. Otherwise use the discovery-approved references and label placeholders. The prototype remains non-production code: favour a clear state model and inspectable state over production architecture, persistence, backend fidelity, tests, or abstractions.

When an exact screen, branch, copy decision, or design input is still foggy, create or graduate the upstream decision ticket. The prototype ticket must not silently invent a product decision merely to appear complete.

## Customer review round

A review round is complete when:

1. the customer receives an immutable version of the file;
2. the reviewed version is recorded;
3. feedback is captured through the repository's customer-feedback intake;
4. the Product Owner accepts, rejects, or defers every item;
5. accepted changes are incorporated or linked as explicit blockers;
6. required specialist review is recorded for safety, security, privacy, compliance, accessibility, or other governed concerns.

Customer feedback informs the baseline; the Product Owner controls it.

## Capture and handoff

Capture the prototype on a throwaway prototype branch, outside the production branch, and identify an exact commit or immutable artifact URL. Put that context pointer and a one-line verdict on the prototype ticket and Wayfinder map. The approved spec must link the same version.

The gate is complete only when every in-scope screen and branch is reachable, the review round has a disposition for every item, accepted changes are reflected, and the exact approved artifact can be retrieved later.
