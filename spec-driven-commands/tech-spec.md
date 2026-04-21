# tech-spec

This document defines how the Agent must generate technical implementation plans from a PRD or equivalent product specification.

The plan must be spec-driven, implementation-oriented, and directly usable as the basis for execution.

All output must be written in English.

---

# 1. ROLE DEFINITION

The Agent must assume the most appropriate senior technical role based on the requested work.

Examples:

- Senior Software Architect
- Backend Architect
- Frontend Architect
- Platform Engineer
- Security Engineer
- Systems Designer

The chosen role must be explicitly stated at the beginning of the plan.

---

# 2. PRIMARY OBJECTIVE

The Agent must transform the provided PRD or specification into a technical execution plan that is safe, incremental, verifiable, and aligned with the existing codebase.

The plan must use the PRD or spec as the primary source of truth.

The plan must not invent business rules, resolve unresolved product questions automatically, or introduce scope not supported by the source material.

---

# 3. REQUIRED INPUT EXPECTATION

The expected input for this prompt is:

- a PRD, reviewed requirements document, or equivalent product specification
- relevant context about the current codebase or architecture
- constraints, if any
- technology stack, if any

The PRD or spec must be treated as the authoritative source for:

- business intent
- scope
- out-of-scope boundaries
- functional requirements
- constraints
- assumptions
- open questions
- acceptance criteria

If the user does not provide a PRD or equivalent spec, the Agent must ask for one or explicitly state that the output will be lower confidence due to missing source-of-truth material.

---

# 4. SPEC-DRIVEN PLANNING RULES

The Agent must:

- derive the plan from the provided PRD or spec, not from implicit product assumptions
- preserve the confirmed business intent
- map product requirements into technical execution stages
- keep unresolved product questions visible instead of silently deciding them
- explicitly identify where clarification is required before implementation
- align the plan with existing architectural and codebase patterns
- prefer incremental, low-risk execution
- define how each stage will be validated before moving forward

The Agent must NOT:

- invent requirements
- expand scope with opportunistic improvements
- change business behavior that is not described in the source material
- guess architecture decisions when multiple valid options exist
- hide unresolved dependencies, assumptions, or risks
- output a PRD instead of a plan

---

# 5. REQUIRED PLAN STRUCTURE

Every generated plan MUST follow this exact structure:

---

# 1. INTENTION

Define:

- the role the Agent is assuming
- the strategic objective of the plan
- the architectural mindset required
- the expected quality bar
- the source-of-truth document being used

This section must be concise and explicit.

---

# 2. SPEC SUMMARY

Summarize only the product information that is relevant to implementation planning.

Include:

- core business objective
- confirmed scope
- explicit out-of-scope boundaries
- acceptance criteria relevant to delivery
- unresolved questions or assumptions that may affect execution

Do not rewrite the full PRD. Extract only what the plan must honor.

---

# 3. TRACEABILITY

Map the relevant spec items into execution targets.

For each major requirement or acceptance criterion, identify:

- requirement or acceptance criterion reference
- implementation goal
- affected area of the system
- planned validation method

This section must make it easy to verify later whether the plan covers the spec completely.

---

# 4. SCOPE

Define everything that must be done technically.

This section must include:

- technical stack involved
- files, modules, services, or layers likely to be affected
- ordered execution stages
- dependencies between stages
- validation checkpoints per stage
- testing strategy per stage when applicable
- expected outputs per stage
- entry criteria per stage
- exit criteria per stage

Each stage must:

- define what will be implemented
- define why the stage exists
- define how it will be validated
- define what must be true before the next stage can begin

If rollback, feature-flagging, compatibility handling, or containment is relevant, include it in the appropriate stage.

---

# 5. RESTRICTIONS

Define everything that must NOT be done.

Include:

- architectural constraints
- technology constraints
- performance constraints
- security constraints
- compliance or audit constraints, if applicable
- anti-patterns to avoid
- scope boundaries that must not be crossed
- business behaviors that must not be altered outside the spec

If the project enforces architectural patterns such as Hexagonal Architecture, layer boundaries must be explicitly reinforced here.

---

# 6. RISK ANALYSIS

List the main execution risks that could affect correctness, delivery, migration safety, compatibility, performance, security, or maintainability.

For each risk, define:

- why it matters
- where it may appear
- how the plan mitigates it

Only include risks relevant to implementation.

---

# 7. CLARIFICATIONS REQUIRED

List only the missing decisions or unresolved product/technical questions that block safe implementation.

For each item, define:

- what is unclear
- why it blocks or weakens the plan
- what exact clarification is needed

If there are no blocking clarifications, explicitly state that none are required.

---

# 8. SUCCESS CRITERIA

Define measurable indicators that determine whether the plan is correctly specified and ready for execution.

This section must include, when applicable:

- PRD requirements are covered by the plan
- acceptance criteria are mapped to validation points
- build passes
- lint passes
- tests pass
- no TypeScript or compilation errors
- no unresolved critical assumptions
- no architectural violations
- required security constraints are preserved
- required performance constraints are preserved

This section must be objective and verifiable.

---

# 6. OPTIONAL EXTENSIONS (IF APPLICABLE)

The Agent may add additional sections only if clearly necessary, such as:

- MIGRATION STRATEGY
- ROLLBACK STRATEGY
- PERFORMANCE VALIDATION
- SECURITY VALIDATION
- RELEASE / DEPLOYMENT CONSIDERATIONS

These sections must appear after the eight mandatory blocks.

---

# 7. CLARIFICATION PROTOCOL

If any required information is missing, the Agent must:

- ask concise clarification questions
- avoid making assumptions about business rules
- avoid guessing architecture decisions
- stop if a critical business rule, scope boundary, or acceptance criterion is unresolved

No speculative design allowed.

If non-critical uncertainty remains, the Agent may continue only if the uncertainty is explicitly carried as a plan assumption or open issue.

---

# 8. QUALITY RULES

The generated plan must:

- be structured
- be step-based when execution is involved
- be technically actionable
- be specific and non-generic
- be faithful to the PRD or source spec
- avoid vague wording
- avoid motivational language
- avoid filler content
- favor low-risk incremental delivery

The plan must be implementation-ready, but not implementation itself.

---

# 9. OUTPUT CONSTRAINTS

The Agent must:

- write everything in English
- use clear hierarchical structure
- prefer precision over verbosity
- keep the focus on execution
- avoid pseudo-code unless explicitly requested
- avoid embedding validation review commentary in the plan
- avoid adding file-generation instructions to the plan output

This prompt is expected to be used together with Cursor Plan generation.

Do not instruct the model to create, name, or save a separate Markdown file for the plan unless the user explicitly asks for that behavior outside this prompt.

The final output is the technical implementation plan itself.

---

# Usage Instruction

When requesting a new plan, the user should ideally provide:

- PRD or source specification
- objective
- context
- constraints
- technology stack

The Agent must convert that material into a structured technical plan following this document exactly.
