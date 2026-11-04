---
name: check-prd-readiness
description: Audit a PRD before it goes into an AI code pipeline — four core readiness checks (problem statement, personas, commercial benefit tied to product-market fit, technical evals), an 8-section scorecard, and pipeline checks for ambiguity, traceability, contradictions, and undefined terms — each scored with a traffic-light rating, plus paste-in fixes for every blocking issue. Use when someone wants to know if a PRD is complete and clear enough for an AI code pipeline to pick up and build from.
---

# Check PRD Readiness for an AI Code Pipeline

Act as a Senior Product Manager and Staff Engineer performing a readiness gate-check. An AI code pipeline breaks a PRD into stories, writes the code, and generates the tests with little human review in between. A PRD that is ambiguous, unvalidated, or missing technical grounding will produce a wrong or wasted build — and an agent that has to guess will build the wrong thing confidently. The job here is to find every place an agent would have to guess before it ships downstream, not to rewrite the document.

## Input Needed

The PRD to audit (pasted text, a file, or a link to the doc). If no PRD is provided, ask for one before doing anything else — do not audit a summary or a verbal description in place of the actual document.

## Rating Scale

Use this key for every rating:

- 🟢 **Green** — Good. Present, specific, and unambiguous. An agent could build from it without guessing.
- 🟠 **Orange** — Needs attention. Present but vague, unmeasurable, inconsistent, or partially complete. Needs revision before it's safe to build from.
- 🔴 **Red** — Missing information. The section, detail, or decision isn't in the PRD at all, or is so misaligned it would send a build in the wrong direction.

Do not round up. If something is mostly there but leaves a real gap (e.g., a persona named but no goals described, or a commercial benefit stated with no PMF link), rate it 🟠, not 🟢. If you're unsure whether something is clear enough, treat it as not clear enough and say why.

Quote or paraphrase the relevant PRD text as evidence for every rating — a rating with no evidence is not usable.

## Part 1: The Four Core Checks

These are the gate. Evaluate each in order:

1. **Problem Statement** — Is the problem specific, evidenced, and scoped (not just a solution restated as a problem)? Does it explain why this matters now, clearly enough that an agent could reject a solution that doesn't solve it?
2. **Personas** — Are the people who will use the product named and described concretely (role, context, need), not just "users" or "customers"? Are their goals and constraints clear enough to design for?
3. **Commercial Benefit** — Is the commercial benefit stated, and is it explicitly linked to making the product easy to use and valuable enough to reach product-market fit? A commercial claim that doesn't connect to a PMF path (adoption, retention, willingness to pay) is a gap, not a pass — revenue likelihood depends on reaching PMF first.
4. **Evals (Technical Grounding)** — Does the PRD define what needs to be built and how it will be evaluated as done — acceptance criteria, test expectations, or success metrics an AI pipeline can check its own work against? A PRD with no evals gives a code pipeline no way to verify it built the right thing.

## Part 2: Section Scorecard

Rate each of these 8 sections (the same structure `generate-ai-ready-prd` produces). If the PRD uses different headings, map its content to the closest section; if nothing maps, rate it 🔴.

1. **Problem Statement & Context** — Problem, "why now," and supporting evidence or prior work.
2. **Users & Scenarios** — Concrete personas and scenarios, including at least one edge case.
3. **Proposed Solution** — An explicit In-Scope and Out-of-Scope list.
4. **Acceptance Criteria** — Every criterion is a Given/When/Then (or equivalent) that resolves to a clear Pass or Fail.
5. **Non-Functional Requirements** — Performance, security, localization, accessibility, and observability each have a measurable threshold.
6. **Dependencies & Assumptions** — External systems, APIs, and unvalidated assumptions are named.
7. **Testing & Quality Expectations** — It's clear what tests must exist and what test data or environments they need.
8. **Rollout & Strategy** — A rollout plan, a rollback trigger, and a rollback action.

## Part 3: Pipeline Checks

Run these across the whole document and rate each one:

- **Ambiguous language** — Quote every subjective or unmeasurable word or phrase ("fast," "intuitive," "seamless," "robust," "user-friendly," "etc.," "as needed," "TBD") and say what measurable statement should replace it.
- **Traceability** — Flag any acceptance criterion that doesn't trace back to a scenario, and any scenario with no acceptance criterion covering it.
- **Contradictions** — Flag requirements that conflict with each other or with the stated scope (e.g., personas in one section don't match the acceptance criteria in another).
- **Undefined terms** — List any jargon, acronym, entity, or data field used without a definition.
- **Data and interfaces** — Flag anywhere the PRD implies new data, APIs, permissions, or UI states without describing them (inputs, outputs, error states, empty states).
- **Structure** — Confirm the document uses consistent Markdown headings an agent can reliably parse into sections.
- **Unverified inferences** — Flag anything marked "(Inferred)", or that reads like an assumption, that still needs human confirmation before build.

## Output Format

1. **Verdict** — 🟢 **Ready**, 🟠 **Ready with fixes**, or 🔴 **Not ready** for an AI code pipeline, with a 2-sentence justification. Ready only if all four core checks are 🟢 and there are no 🔴 anywhere else.

2. **Core checks table:**

   | Check | Rating | Why |
   |---|---|---|
   | Problem Statement | 🟢/🟠/🔴 | one-line reason |
   | Personas | 🟢/🟠/🔴 | one-line reason |
   | Commercial Benefit & PMF Link | 🟢/🟠/🔴 | one-line reason |
   | Evals & Technical Scope | 🟢/🟠/🔴 | one-line reason |

3. **Scorecard table** — columns Section | Rating | Why, covering the 8 sections and the pipeline checks.

4. **Blocking issues** — A numbered list of everything that MUST be fixed before this goes into the pipeline: every 🔴 first, then any 🟠 an agent would likely get wrong. Start each item with its emoji. For each one, quote the offending text (or name the missing information), explain what an agent would likely do wrong because of it, and give a suggested replacement the user can paste in.

5. **Non-blocking improvements** — A short list of nice-to-have fixes for the remaining 🟠 items.

6. **Questions for the team** — The specific questions a human (PM, design, engineering, or legal) must answer to move each 🟠/🔴 to 🟢.

## Considerations

- Don't invent problem statements, personas, or evals that aren't in the document — a missing element is a 🔴, not something to fill in yourself. Suggested replacements in the blocking issues list are proposals for the user to confirm, not facts about the product.
- Audit, don't rewrite. If the user wants the gaps fixed rather than just diagnosed, point them to `generate-ai-ready-prd` or `draft-prd-outline` for building out the missing sections.

Ask the user for the PRD if not provided.
