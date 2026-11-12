# Draft Retirement Gate

## Objective

When a design draft is converted into a formal spec (an RFC, or an implementation guide revision), the draft has served its purpose. This gate lets the user **remove the consumed draft(s)** and **cleanse every reference** that points at them, so the `.platonic/` world stays closed: no redundant exploratory documents and no dangling links to files that no longer exist.

Platonic Coding treats specs as an authoritative, self-consistent world. A draft left behind after formalization is harmless clutter; a *reference* to a deleted draft is a broken link in that world — it misleads readers and breaks `specs-validate-consistency` / `specs-refine`. Retirement must therefore handle **both** the file and its references as one atomic cleanup.

## When This Gate Runs

Trigger **once**, immediately after a draft → formal-spec conversion succeeds, before the phase handoff:

- BRAINSTORM mode post-draft routing paths **1, 2, 3, 4** (draft formalized into a new or updated RFC), and path **5** (draft folded into an IG revision via `impl-update-guide`) — see `references/brainstorm/brainstorm.md`.
- Phase 1 step 4/5 (Generate RFC → `specs-refine`) — see `references/workflow/workflow-phase-1.md`.
- A direct "formalize this draft" / "convert draft to RFC" request.

Do **not** run it when a draft was merely created, revised, or reviewed without being formalized, and not for `analysis/` reports (those are inputs to recovery, not consumed drafts).

<GATE-INSTRUCTION>
This is a **gate**. Call the `ask_user` tool with the retirement question — do NOT write it as plain text. Plain-text questions do not pause the loop; `ask_user` pauses and resumes on the same turn once the user answers. See `SKILL.md` → "Confirmation Gates & Clarifications" and `references/brainstorm/brainstorm.md` → GATE-INSTRUCTION.
</GATE-INSTRUCTION>

## Inputs

- **Consumed draft(s)**: the draft file(s) whose content was just formalized (e.g. `.platonic/drafts/2026-11-12-auth-design.md`). Determine these from the conversion step — never guess.
- **Formal target**: the RFC ID (`RFC-NNN-<name>` / `RFC-<PREFIX>-NNN-<name>`) or IG ID produced from the draft.
- **Drafts path**: `.platonic/drafts` by default (or `{{DRAFTS_PATH}}` from `.platonic.yml`).
- **Project root**: for the reference scan.

## The Gate

Before the gate, present a one-line summary: which draft(s) were consumed, and what they became (e.g. "`.platonic/drafts/2026-11-12-auth-design.md` → `RFC-007-auth`"). Then **call `ask_user`** with a single question:

> Question text: "Draft `<path>` was formalized as `<RFC/IG-ID>`. Retire it now? (A) Remove the draft and cleanse all references to it — recommended; (B) Keep the draft, but cleanse references anyway; (C) Keep the draft and leave references as-is; (D) Choose specific drafts to remove (for merged/partial conversions)."

Options (lead with the recommendation):

| # | Choice | Effect |
|---|--------|--------|
| A | **Remove + cleanse** (recommended) | Delete the consumed draft(s) and remove/repoint every reference to them |
| B | **Keep + cleanse** | Leave the file(s), but remove/repoint references so nothing points at a draft that will later be deleted |
| C | **Keep as-is** | No changes — skip retirement entirely |
| D | **Select drafts** | User names which of several consumed drafts to retire; useful when drafts were merged or only partially formalized |

**Mode interaction:**
- **Quick-pass / "no confirmations"**: deletion is destructive, so do **not** auto-delete. Default to choice **C** (keep as-is) and state that retirement was skipped; the user can run it explicitly later. Validation gates are never skipped by quick-pass — and this gate is a safety gate.
- **Explicit "remove the draft" / "clean up drafts" request**: treat as pre-consent for choice A — still confirm the exact file list, then execute.
- **Path 3 (update existing RFC) / path 5 (update existing IG)**: the draft may be only *partially* consumed (some sections still exploratory). Offer choice D by default so the user can keep the un-consumed draft instead of losing exploration notes.

Only proceed with file/reference changes on choices A, B, or D. On C, stop.

## Step 1: Confirm the Removal Set

Resolve the exact list of draft files to delete:

- Include only drafts **actually consumed** by this conversion. If several drafts were merged into one RFC, list all of them.
- **Never** remove an unrelated draft, a draft still under review, or a draft that only shares a topic keyword.
- If a draft was only partially formalized, exclude it (or keep it per choice D) rather than deleting content that has no formal home yet.
- Check tracking: use `git rm <path>` for a tracked draft, plain `rm <path>` for an untracked one (BRAINSTORM mode only commits drafts when the user asks). Do not remove the `drafts/` directory or its `README.md`.

Record the removed filenames verbatim — they are the keys for the cleanse scan.

## Step 2: Cleanse References

Scan the **entire project** (not just `.platonic/`) for references to each removed draft. Search by full path, bare filename, and topic, in all git-tracked text files. Typical locations:

- Other design drafts in `.platonic/drafts/` (cross-links, "see also" pointers)
- RFCs in `.platonic/specs/` — especially `Background & Motivation`, `Relationship to Other RFCs`, and any `Source draft` / `Derived from` / `Origin` metadata lines
- Implementation guides in `.platonic/impl/` and their "Inputs"/"Source" sections
- Supporting docs: `rfc-index.md`, `rfc-history.md`, `rfc-namings.md`, per-namespace local indexes, `drafts/README.md`
- `.platonic/analysis/` reports, `.platonic.yml`, project `README`/docs, source-code comments, CI configs, and any markdown that cites the draft path

**Reference forms to match** (per removed draft):

- Full or relative paths: `.platonic/drafts/2026-11-12-auth-design.md`, `drafts/2026-11-12-auth-design.md`
- Bare filename: `2026-11-12-auth-design.md`
- Markdown links: `[design draft](../drafts/2026-11-12-auth-design.md)`, wiki links `[[...]]`, reference-style links
- Metadata / provenance lines: `**Source draft**: ...`, `Derived from: ...`, `Origin: ...`, `See draft ...`
- Prose pointers: "the auth design draft", "as explored in the draft", "per `<filename>`"

**Cleansing rules** (apply in this order so nothing dangles):

1. **Repoint provenance, don't just delete it.** Where a reference records where a spec came from, replace the draft pointer with the formal target ID — e.g. `**Source draft**: .platonic/drafts/...` → `**Source**: RFC-007-auth` (or a link `[RFC-007-auth](RFC-007-auth.md)` using the skill's cross-reference convention). Traceability survives through the RFC, not through the dead draft.
2. **Convert links to the new spec.** A markdown link whose target is the removed draft becomes a link to the formal RFC/IG (correct relative path per `assets/templates/template-rfc-standard.md` → Cross-References), or, if the link label was purely about the draft itself, drop the link and keep the label as plain text. **Never leave a link to a non-existent file.**
3. **Remove dead index/history/list rows.** Delete rows or bullet entries that list the removed draft from `rfc-index.md`, `drafts/README.md`, and similar generated listings. Then regenerate dynamic files via `specs-refine` (Step 3) so they are rebuilt from the formal specs, not hand-patched.
4. **Optional plain-text provenance.** If the user wants a historical record of the conversion, add a single **plain-text, non-link** line to `rfc-history.md` (e.g. `Formalized from design draft 2026-11-12-auth-design.md (retired)`). This is a record, not a reference — it must not be a markdown link to the removed file. Skip it if the user wants a full cleanse.
5. **Rewrite bare path mentions** to the RFC ID or remove the clause. Do not silently leave a path string that resolves to nothing.

**Guardrails:**

- Do not edit binary files, vendored dependencies, or `.git/` internals.
- Do not rewrite unrelated text that merely contains the word "draft" — only references to the *specific removed draft(s)*.
- Preserve surrounding formatting, tables, and heading structure.
- If a reference appears in a frozen/immutable RFC version file, do not mutate its content in place — report it as a residual and let the user decide (frozen RFCs are immutable per `template-rfc-standard.md`).

## Step 3: Regenerate & Verify

1. Run `specs-refine` (or at minimum `specs-generate-index` + `specs-generate-history` + `specs-validate-consistency`) so dynamic files reflect the post-retirement state and no broken cross-references remain.
2. Re-scan the project for each removed draft's filename/path. **Zero hits** to live files is the success criterion (the optional plain-text history line, if added, is an allowed exception — it is a record, not a link).
3. Confirm no markdown link in `.platonic/` resolves to a missing file.
4. Report: drafts removed, files edited for cleansing, references repointed/removed, and any residuals that need a decision.

## Output

- Consumed draft file(s) removed (choice A/D), or explicitly kept (choice B/C).
- A `.platonic/` tree with **no references to removed drafts** and **no dangling links**.
- Updated supporting docs (history/index/namings) regenerated via `specs-refine`.
- A short retirement summary in the phase handoff.

## Key Principles

- **Atomic** — remove the file and cleanse its references in the same operation; never one without the other.
- **Confirm before deleting** — always an `ask_user` gate; destructive actions are never auto-executed by quick-pass.
- **Repoint, don't orphan** — preserve provenance by pointing at the formal spec, not by leaving a dead link.
- **Closed world** — after retirement, every reference in `.platonic/` resolves; `specs-validate-consistency` passes.
- **Conservative scope** — only drafts actually consumed; when in doubt, keep the draft.
