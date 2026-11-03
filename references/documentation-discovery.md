# Documentation Discovery and Placement

Before creating a record, inspect project instructions, existing documentation, similar feature records, templates, PR/Issue conventions, ADR or RFC directories, and any generated documentation policy.

## Placement precedence

Prefer, in order, subject to the user's explicit request:

1. the project's explicitly documented location and template;
2. an existing record for the same change, including a legacy flat file;
3. the project's established docs, ADR, RFC, issue, or pull-request workflow;
4. a topic directory at `<docs-root>/<change-slug>/` using this skill's templates when no applicable project convention exists.

An established convention may be an explicit instruction, a template, or a consistent structure used by comparable records. A few Markdown files directly under `docs/` are not by themselves a flat-only convention. If the project explicitly requires flat files or external issue/PR records, follow that rule instead of creating topic directories. Keep an established ADR or RFC location and link to it rather than duplicating the record.

Reuse the project's documentation root, such as `doc/` or `docs/`. If none exists, use `docs/` relative to the target project root, not the skill installation directory. Do not create a second root alongside the existing one. Adapt headings and filenames to the project. Bind records to the relevant issue, PR, branch, commit, or release when that is already customary.

## Default topic layout

Use one directory per change, not one shared directory for every change to a business module. The fallback layout is:

```text
docs/
├── quick-match-waiting/
│   ├── requirements.md
│   ├── progress.md
│   └── decision.md       # Only when a material decision needs an ADR
└── dev-rollout-capacity-fix/
    └── change.md
```

| Level | Default files for a new change |
|---|---|
| Level 0 | No directory or record |
| Level 1 | `change.md`, based on the lightweight template |
| Level 2 | `requirements.md` and `progress.md`, based on the corresponding templates |
| ADR | Add `decision.md` when needed, unless the project has an ADR location |

Keep verification evidence and risks in these records; add other files only when the content warrants them. Do not pre-create empty directories or placeholder documents for future work.

Use a short, stable lowercase kebab-case slug describing the change, such as `quick-match-waiting`. Search record contents and issue/PR associations before choosing a path: matching filenames alone do not establish that two records describe the same change. Reuse the directory on subsequent work for that change. For a distinct change with a colliding slug, add its issue identifier or a descriptive qualifier and check again; never overwrite an unrelated record or generate a fresh timestamp directory on each run.

## Creation and verification

1. Select the level and resolve the documentation root, change identity, and destination using the precedence above.
2. For local records, create missing parent directories with the available filesystem tool (for example, `mkdir -p` with a properly quoted resolved path). Directory creation is part of writing the authorized record; do not require a separate confirmation merely because the directory is new.
3. Write substantive task-specific content, updating the existing record where applicable. Do not overwrite existing content with blank templates. Cross-link related records using paths relative to their actual locations.
4. Verify the intended files exist, contain the relevant content, and that local links resolve. Report their actual paths in the final delivery.
5. If directory creation or writing fails, report the path, cause, and any partial output. Do not silently place the record in the documentation root, claim success, or work around an explicit project restriction.

## Historical records

Leave existing flat files in place by default. Continue updating them for the same change; new unrelated changes use topic directories when the fallback applies. Link to relevant historical records instead of copying them. A feature's general documentation can be linked from multiple distinct change records without merging their histories.

Migrate historical files only when requested. Identify the source-to-destination mapping, update affected relative links and indexes, and verify references after the move. Adopting the fallback for new changes does not itself authorize moving old files.

## Placement review cases

Use these cases to review changes to the placement rules; they are expected outcomes, not evidence of an executed agent test.

| Situation | Expected outcome |
|---|---|
| Empty project, Level 1 | Create `docs/<change-slug>/change.md` with substantive content |
| Existing `doc/` root, no applicable structure | Create `doc/<change-slug>/`, without a second `docs/` root |
| A few unrelated flat files, new Level 2 change | Preserve them; create the new topic directory with requirements and progress |
| Explicit flat-only or external issue/PR convention | Follow that convention; do not create fallback topic directories |
| Existing topic directory or legacy flat record for the same change | Update it in place; do not create a duplicate |
| Distinct change with the same slug | Choose a stable disambiguated slug; preserve the existing record |
| Level 0 | Create no directory or record |
| Directory creation fails | Report the failure and partial output; do not silently fall back to flat files |
