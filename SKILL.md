---
name: change-traceability
description: 'Maintain traceability for substantive software changes by selecting the appropriate documentation level, recording scope and decisions, tracking implementation and verification evidence, and keeping change records aligned with the actual result.'
---

# Change Traceability

Use this skill when a task changes business behavior, interfaces, data, protocols, security, workflows, or delivery scope, or when the user requests documentation traceability.

## Goals

- Make important changes explainable, reviewable, and recoverable.
- Keep requirements, decisions, implementation, verification, review status, risks, and remaining work connected.
- Reuse the project's existing documentation system and conventions.
- Avoid unnecessary documentation overhead for trivial changes.

## Workflow

1. Classify the requested change and identify whether traceability is required.
2. Select the minimum sufficient traceability level; if multiple conditions apply, use the highest level.
3. Read `references/documentation-discovery.md` before creating or relocating records. Discover the project's documentation location, naming conventions, templates, and collaboration records; distinguish explicit rules from incidental flat files.
4. Resolve whether to reuse an existing topic or create an independent record using `references/documentation-discovery.md`. Create missing parent directories before writing substantive records, before or during implementation. For a new topic without an applicable project convention, use `<docs-root>/<change-slug>/`; use `docs` as the root only when no documentation root exists. Reuse records for continuing work and related small fixes when their history and evidence remain clear, including legacy flat files.
5. Keep requirements, assumptions, decisions, progress, deviations, and risks synchronized with the actual implementation at the update points below.
6. Link acceptance criteria to concrete verification evidence.
7. Before delivery, verify that the intended files exist at the resolved paths, contain substantive content, and have working local links. Check that the record is complete, current, and does not claim unverified results. Report creation failures without silently falling back to flat files.
8. Report the selected level, updated records, evidence, unexecuted checks, deviations, risks, and remaining work.

Read the relevant reference only when the current task needs its detail:

- `references/traceability-levels.md` for level selection.
- `references/documentation-discovery.md` for placement precedence, topic reuse versus independent records, directory creation, naming, legacy records, and existing indexes.
- `references/lightweight-template.md` for a small change record.
- `references/requirements-template.md` for a full business requirements record.
- `references/progress-template.md` for implementation and verification progress.
- `references/decision-template.md` for material technical decisions.
- `references/acceptance-evidence.md` for mapping criteria to evidence.
- `references/deviation-and-risk.md` for assumptions, deviations, risks, and remaining work.

## Traceability levels

- **Level 0 — no new record:** trivial non-behavioral edits, such as formatting, comments, or simple renames that do not change contracts or behavior. Explain in the final report why no record was added.
- **Level 1 — lightweight trace:** localized bug fixes or small internal behavior changes that do not alter public contracts, protocols, data models, security boundaries, or concurrency semantics.
- **Level 2 — full business trace:** new functionality, workflow or state-model changes, public API/protocol/data changes, security or concurrency changes, migrations, cross-module changes, release-critical work, or an explicit request for complete documentation.
- **ADR — decision trace:** add an ADR-style record when a material technical choice has multiple viable alternatives, significant impact, or a meaningful rollback strategy. ADR may accompany Level 1 or Level 2.

## Record maintenance

For changes requiring a trace record, update it at meaningful points:

- When requirements, scope, or acceptance criteria change, update the requirements and change history before implementing the revised behavior. Distinguish confirmed changes from assumptions.
- After a meaningful implementation milestone or a material verification result changes, update progress, evidence, deviations, risks, and remaining work. Record milestones rather than every search, command, or tool call.
- Before pausing or handing off, leave the current stage, verified scope, blockers, relevant record/code links, and a concrete next action sufficient for another developer to resume from the records and code.
- At delivery, reconcile the final state with acceptance evidence and synchronize any existing index according to `references/documentation-discovery.md`.

Keep requirements focused on intended behavior and acceptance; put implementation history in progress. For Level 1, keep those roles in distinct sections of the same record, including explicit acceptance criteria. Adapt to project templates without adding duplicate records or unrelated stages.

Use the project's status vocabulary and qualify completion by the evidence available, such as implementation complete, local verification complete, or integration pending. Do not mark unmet in-scope acceptance criteria as complete. Keep current status and next actions up to date rather than copying the initial plan unchanged.

## Review boundary

Traceability does not replace code review, testing, security review, or business acceptance. This skill does not define or perform a full code-review process. When a project requires code review, or when review has occurred, record its status and material findings in the existing change record, pull request, issue, or final delivery report. Treat unresolved review findings as risks or remaining work. Never claim that complete documentation or passing tests proves that code review was completed.

Use project terminology and workflow for review status. If no convention exists, use a concise status such as `Not required`, `Not run`, `In progress`, or `Completed`, and include the scope, conclusion, and unresolved findings when applicable.

## Boundaries

- Project-specific instructions, paths, templates, terminology, and collaboration practices override the defaults in this skill.
- Do not invent a parallel documentation hierarchy when the project already has an applicable convention. A few flat documents alone do not establish a flat-only rule; use the topic-directory fallback for new changes in that case.
- Preserve historical record locations unless migration is requested. Do not duplicate an existing change record to fit the fallback layout.
- Do not create an empty template as the only trace record.
- Do not invent confirmed requirements from incomplete information; mark assumptions and items requiring confirmation.
- Do not hide scope changes, implementation deviations, unexecuted checks, failed checks, or unresolved risks.
- Do not claim acceptance or verification without concrete evidence.
- Keep the documentation proportional to the change; traceability is not a reason to expand implementation scope.

## Final delivery

The final report must state:

- selected traceability level and rationale;
- records created or updated and their paths or external locations;
- implementation and behavioral changes;
- acceptance, verification, and (when applicable) code-review evidence;
- checks not run and why;
- deviations, assumptions, risks, and remaining work;
- final documentation status;
- code-review status, if required or performed, without treating it as proof of overall correctness.
