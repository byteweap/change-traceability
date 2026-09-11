# Change Traceability

[中文文档](README.zh-CN.md)

An AI skill for maintaining traceability across substantive software changes and adapting to existing project documentation conventions. It helps an AI agent decide whether a change needs documentation, how much documentation is appropriate, and how to keep records aligned with requirements, decisions, implementation, verification, deviations, risks, and remaining work.

## Why this exists

AI-assisted development can produce a working patch without preserving why the change was made, what was explicitly out of scope, which assumptions were used, or what remains unverified. This skill turns traceability into a proportional, repeatable workflow without forcing every project into one documentation system.

## What it does

- Classifies changes by traceability risk.
- Selects Level 0, Level 1, Level 2, and optional ADR records.
- Discovers and reuses the host project's existing documentation conventions.
- Guides requirements, progress, decision, acceptance-evidence, deviation, and risk records.
- Keeps assumptions and scope changes visible.
- Distinguishes verification evidence, code-review status, and business acceptance.
- Produces an explicit final traceability summary, including unexecuted checks and remaining risks.

## What it does not do

This skill is not:

- a project-management system;
- a replacement for product requirements or business approval;
- a code-review, security-audit, or testing skill;
- a tool that invents requirements or verification results;
- a mandate to create documents for every trivial edit;
- permission to create a parallel documentation hierarchy over an existing project convention.

## Traceability model

| Level | Use when | Typical record |
|---|---|---|
| **Level 0** | Formatting, comments, typo fixes, or simple non-behavioral changes | No new record; explain why in the final report |
| **Level 1** | Localized bug fixes or small internal behavior changes | Lightweight change record |
| **Level 2** | New functionality, workflow/state changes, public interfaces, protocols, data, security, migrations, cross-module or release-critical changes | Full business trace |
| **ADR** | A material technical choice has viable alternatives or meaningful impact/rollback conditions | ADR-style decision record; may accompany Level 1 or Level 2 |

When multiple conditions apply, use the highest applicable level. An explicit user request for documentation raises the minimum to Level 1; a request for complete traceability uses Level 2.

## Documentation workflow

1. Classify the requested change.
2. Select the minimum sufficient traceability level.
3. Inspect project instructions, existing records, templates, and issue/PR conventions.
4. Create or update the appropriate record before or during implementation.
5. Keep scope, assumptions, decisions, progress, deviations, risks, and remaining work current.
6. Map acceptance criteria to concrete evidence.
7. Check documentation completeness before delivery.
8. Report the resulting record locations, evidence, limitations, and risks.

Traceability is a living record, not a retrospective summary written only after coding is finished.

## Language policy

The skill's default templates are written in English for portability. Generated records should follow this order:

1. the user's explicit language request;
2. the project's documented language convention;
3. the language of an existing record being updated;
4. the language used by the user in the current task.

Templates define structure, not output language. Do not create bilingual records unless requested or required by the project.

## Cross-project use

This skill can be used across projects, but it does not require every project to share the same documentation structure. It reuses the traceability method while adapting paths, templates, terminology, language, and collaboration workflows to the host project.

## Project adaptation

The host project's rules, paths, templates, terminology, and collaboration workflow take precedence. Before creating files, look for project instructions, `docs/`, `doc/`, `adr/`, `design/`, RFCs, similar records, PR templates, and issue templates. If no convention exists, use the smallest suitable fallback from `references/`.

## Repository layout

- `SKILL.md` — runtime instructions for an AI agent.
- `agents/openai.yaml` — optional OpenAI UI metadata.
- `references/` — level guidance, discovery rules, fallback templates, and evidence formats.

## Installation

Copy or link this directory into the skills directory supported by your AI agent. Keep the directory name `change-traceability` and ensure `SKILL.md` remains at its root.

For Codex-style local skills, the resulting layout is typically:

```text
<skills-root>/change-traceability/SKILL.md
```

## Usage tutorial

### 1. Install the skill

Copy or link this repository into the skills directory used by your AI agent, then restart or refresh the agent if required:

```text
<skills-root>/change-traceability/SKILL.md
```

For Codex-style agents, the skill can usually be invoked explicitly with:

```text
$change-traceability
```

### 2. Ask the agent to trace a change

Include the requested behavior, constraints, and any available issue or acceptance criteria. For example:

```text
Use $change-traceability to implement support for retrying failed payments.
Inspect the existing documentation conventions first, record the requirements and
decisions, implement the change, run the relevant checks, and report evidence,
remaining risks, and unexecuted checks.
```

The skill will first inspect the host project's instructions and documentation layout. It reuses existing `docs/`, ADR, RFC, issue, or pull-request conventions instead of creating a parallel hierarchy.

### 3. Choose the appropriate traceability level

The agent selects the smallest sufficient level automatically, or you can state one explicitly:

- Use **Level 0** for formatting, comments, or other non-behavioral edits.
- Use **Level 1** for a localized bug fix or small internal behavior change.
- Use **Level 2** for new features, public interfaces, data or protocol changes, migrations, security, workflow changes, cross-module work, or release-critical changes.
- Add an **ADR** when the implementation involves a material technical choice with meaningful alternatives or rollback implications.

Example:

```text
Use Level 2 traceability for this API change. Capture the scope, non-goals,
acceptance criteria, compatibility impact, verification evidence, and risks.
```

### 4. Maintain the record while implementing

For a substantive change, ask the agent to keep the record current during implementation:

```text
Keep the traceability record synchronized as you work. Update progress,
assumptions, deviations, decisions, and verification results; do not mark
checks as passed unless they were actually run.
```

A typical Level 1 record covers the problem, root cause, fix boundary, verification evidence, impact, assumptions, risks, and remaining work. A Level 2 change normally maps each acceptance criterion to a concrete test, command, review, artifact, or explicitly documented manual check.

### 5. Review the final report

Before delivery, confirm that the response identifies:

- the selected traceability level and rationale;
- records created or updated and their locations;
- implementation and behavioral changes;
- acceptance and verification evidence;
- checks that were not run and why;
- deviations, assumptions, risks, and remaining work; and
- code-review status separately from test results and business acceptance.

If no project convention exists, the bundled files under [`references/`](references/) provide minimal fallback templates.

## Validation

Run the Skill Creator validator when available:

```bash
python3 /path/to/skill-creator/scripts/quick_validate.py /path/to/change-traceability
```

Also review whitespace and repository changes:

```bash
git diff --check
```

The validator checks packaging and frontmatter; it does not determine whether a trace record is substantively complete.

## References

- [Traceability levels](references/traceability-levels.md)
- [Documentation discovery and placement](references/documentation-discovery.md)
- [Lightweight template](references/lightweight-template.md)
- [Full requirements template](references/requirements-template.md)
- [Progress template](references/progress-template.md)
- [Decision template](references/decision-template.md)
- [Acceptance evidence](references/acceptance-evidence.md)
- [Deviations and risks](references/deviation-and-risk.md)

## License

Add the repository's chosen open-source license before public release. No license is implied by this repository until one is added.
