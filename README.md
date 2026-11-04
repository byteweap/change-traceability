# Change Traceability

[中文文档](README.zh-CN.md)

An AI skill for maintaining traceability across substantive software changes and adapting to existing project documentation conventions. It helps an AI agent decide whether a change needs documentation, how much documentation is appropriate, and how to keep records aligned with requirements, decisions, implementation, verification, deviations, risks, and remaining work.

## Install

Paste this into your coding agent:

```text
Install the change-traceability skill from https://github.com/byteweap/change-traceability
for me by running: npx skills add byteweap/change-traceability -g
```

Or run it yourself:

```bash
npx skills add byteweap/change-traceability -g
```

The CLI detects supported coding agents and installs the skill into their shared global skills directory. Omit `-g` to install it into the current project instead. To install manually, copy or link this repository into your agent's skills directory and keep `SKILL.md` at the skill root.

## Usage tutorial

After installation, invoke the skill explicitly:

```text
$change-traceability
```

Then describe the change, constraints, and any issue or acceptance criteria. For example:

```text
Use $change-traceability to implement support for retrying failed payments.
Inspect the existing documentation conventions first, record the requirements and
decisions, implement the change, run the relevant checks, and report evidence,
remaining risks, and unexecuted checks.
```

The skill inspects the host project's instructions and documentation layout first, then reuses existing `docs/`, ADR, RFC, issue, or pull-request conventions. It selects the smallest sufficient traceability level automatically; you can also specify Level 0, Level 1, Level 2, or ADR explicitly.

For substantive changes, ask it to keep the record synchronized during implementation:

```text
Keep the traceability record synchronized as you work. Update progress,
assumptions, deviations, decisions, and verification results; do not mark
checks as passed unless they were actually run.
```

Before delivery, verify that the final report includes the selected level and rationale, record locations, implementation changes, acceptance and verification evidence, unexecuted checks, deviations, risks, remaining work, and code-review status separately from test results and business acceptance.


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

## Keeping records useful

- Update requirements and change history before implementing revised scope or acceptance criteria. Update progress at meaningful milestones and when material verification results change, rather than logging every tool call.
- Before pausing or handing off, record the current stage, verified scope, blockers, relevant links, and a concrete next action so another developer can resume from the records and code.
- Lightweight records also include goals, scope, explicit acceptance criteria, implementation progress, and evidence. Root-cause analysis applies to defects; other small behavior changes do not need an invented root cause.
- Reuse an existing documentation index: add an entry when a record is created, reconcile it at delivery, and correct stale completion status when work reopens. Detailed progress stays in the record; no index is required when the project has none.

The [record maintenance rules](SKILL.md#record-maintenance), [progress template](references/progress-template.md), [lightweight template](references/lightweight-template.md), and [index guidance](references/documentation-discovery.md#existing-indexes) provide the details. These practices preserve the existing placement precedence and risk-based traceability levels.

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

## Automatic topic directories

For a new topic without an applicable project convention, the skill instructs the agent to create `<docs-root>/<change-slug>/` before writing records. It reuses the project's documentation root (for example, `doc/`); only a project without one defaults to `docs/`. A few existing flat Markdown files alone do not require future records to remain flat.

```text
docs/
├── quick-match-waiting/
│   ├── requirements.md
│   ├── progress.md
│   └── decision.md       # Optional ADR
└── dev-rollout-capacity-fix/
    └── change.md
```

- Level 0 creates no directory or record.
- Level 1 uses `change.md`; Level 2 uses `requirements.md` and `progress.md`.
- Add `decision.md` only when needed; reuse an established ADR location if one exists.
- Continuing a feature or making a small related fix reuses its topic when scope, history, and evidence remain clear. Changes needing independent goal, acceptance, or rollback tracking get separate topics linked to the relevant feature records.
- Topic reuse does not lower the traceability level or make past verification valid for new behavior. Distinct topics with colliding names use an issue identifier or descriptive qualifier.
- Historical flat records stay in place and are updated there for the same change. Migration requires a separate request.
- Explicit project placement rules, including flat-only and external issue/PR workflows, take precedence.
- Existing category layouts such as `docs/features/` and filenames such as `decisions.md` are preserved. The fallback adds no category layer between the documentation root and a topic.

The agent creates missing parent directories, writes substantive content, and checks the resulting files and local links. A creation failure must be reported with the path and cause; it must not silently fall back to a flat file. This is an instruction-based workflow, with no initialization script added. See [documentation discovery and placement](references/documentation-discovery.md) for the full rules and placement review cases.

## Repository layout

- `SKILL.md` — runtime instructions for an AI agent.
- `agents/openai.yaml` — optional OpenAI UI metadata.
- `references/` — level guidance, discovery rules, fallback templates, and evidence formats.

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
