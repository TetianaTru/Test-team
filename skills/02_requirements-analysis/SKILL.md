---
name: qa-requirements-analysis
description: Analyze a User Story collected by the ado-issue-context skill for testability, ambiguity, contradictions, and requirement gaps. Present findings for user decisions, then save a traceable requirements analysis beside the User Story file. Use only after Skill 01 has collected the ADO hierarchy and user_story_{ISSUEKEY}.md.
recommended_model: opus
recommended_effort: high
skill_01_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\01_ADO-data-collection
skill_02_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\02_requirements-analysis
skill_01_output_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs
---

# QA Requirements Analysis

Analyze requirements from a User Story previously collected by Skill 01. Identify testability issues without changing source requirements, present findings to the user, wait for decisions, and then save the reviewed requirements.

## Dependency on Skill 01

This skill must run after:

`C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\01_ADO-data-collection`

Skill 01 creates User Story files using this structure:

```text
C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs\
  stories\
    {EPIC-KEY}\
      epic.md
      {FEATURE-KEY}\
        feature.md
        {ISSUEKEY}\
          user_story_{ISSUEKEY}.md
          test-cases\
          tasks\
```

Skill 01 files do not require YAML frontmatter. Validate their content using the heading and Fields table described below.

## Goal

1. Locate the User Story file produced by Skill 01.
2. Extract all verifiable requirements and out-of-scope statements.
3. Analyze each requirement using four QA questions and applicable test-design heuristics.
4. Identify only relevant, evidence-based gaps, ambiguities, and contradictions.
5. Present findings and wait for user decisions.
6. Save a traceable requirements file in the same folder as the User Story.

## Input

Accept one of:

- An ADO User Story URL containing a numeric work-item ID.
- A numeric User Story ID.
- A direct path to `user_story_{ISSUEKEY}.md`.

For an ID or URL, search under:

`C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs\stories`

Expected filename:

`user_story_{ISSUEKEY}.md`

If exactly one matching file is found, use it. If none or more than one is found, stop and ask the user for the exact path.

## Data-source and safety rules

- Read only the local files produced by Skill 01.
- Do not retrieve ADO data again.
- Do not use a browser, web search, or another external source.
- Do not change anything in ADO.
- Do not modify Skill 01 output files.
- Write only the requirements-analysis file described by this skill.
- Treat comments as context, not automatically as approved requirements.
- Treat the Description and Acceptance Criteria as the primary requirement sources.

## Workflow

### Step 1: Locate and validate the Skill 01 file

Validate all of the following:

1. The file exists and is named `user_story_{ISSUEKEY}.md`.
2. The first heading follows:
   `# User Story {ISSUEKEY}: {Title}`
3. The `## Fields` table contains:
   - `Work Item ID` matching `{ISSUEKEY}`
   - `Work Item Type` equal to `User Story`
4. At least one of these sections contains source text:
   - `## Description`
   - `## Acceptance Criteria`

If validation fails, stop and report:

- The invalid or missing file
- The failed check
- That Skill 01 must be run first or the correct file path must be supplied

### Step 2: Extract source statements

Extract:

- Each independently verifiable statement from `## Acceptance Criteria`
- Verifiable system behavior from `## Description`
- Explicit preconditions stated in either section
- Explicit out-of-scope statements

Do not treat these as requirements unless the User Story explicitly establishes them as required behavior:

- Metadata from the Fields table
- Relations
- Pull requests
- Comments
- Design or reference links by themselves

Comments may reveal a potential contradiction or clarification, but label the finding as comment-derived and ask the user whether it is authoritative.

### Step 3: Assign stable requirement IDs

Assign sequential IDs in source order:

`REQ-1`, `REQ-2`, `REQ-3`, and so on.

Rules:

- One ID represents one independently verifiable outcome.
- Preserve the exact source quote for every ID.
- IDs are stable once first presented to the user.
- Never reuse an ID.
- If the user approves splitting a requirement, use sub-IDs such as `REQ-5a` and `REQ-5b`.
- If the user approves retiring or merging a requirement, retain its original ID in the Resolved Issues Log as `Retired` or `Merged into REQ-N`.
- Out-of-scope statements receive `OOS-1`, `OOS-2`, and so on; do not run the four-question review on them.

### Step 4: Analyze each requirement internally

For every `REQ-N`, answer these questions internally:

| Question | Purpose |
|---|---|
| Q1. Can it be verified? | Confirm there is an objective pass/fail outcome. |
| Q2. What is needed to verify it? | Identify only required source-stated or genuinely necessary preconditions, data, roles, permissions, and environment. |
| Q3. Where can the stated behavior fail? | Identify relevant boundaries, negative paths, or state combinations implied by the requirement. |
| Q4. What relevant behavior is unspecified? | Identify missing information that prevents or materially weakens verification. |

Do not show four questions for every requirement automatically. Use the answers to create findings only where a material issue exists.

Apply only relevant techniques from `review-rules.md`:

- Equivalence Partitioning
- Boundary Value Analysis
- State Transition Testing
- Decision Table Testing
- CRUD scope check

Do not force a technique onto a requirement when its trigger conditions are absent.

### Step 5: Classify findings

Use these finding types:

- `[GAP]` — required information is absent and materially affects verification
- `[AMBIGUITY]` — the source permits multiple materially different interpretations
- `[CONTRADICTION]` — two source statements cannot both be true under the same conditions

Assign severity using `review-rules.md`:

- Blocker
- High
- Medium
- Low

Every finding must include:

```text
[TYPE] [SEVERITY] REQ-N — One-line title
Source quote: "Exact source wording"
Detail: Why this affects verification
Suggested resolution: A neutral clarification question or proposed wording
```

Do not create a finding merely because additional test cases could exist. Create one only when the missing information changes the expected behavior, pass/fail result, or required test setup.

### Step 6: Present findings and wait

Present:

1. Total requirements reviewed
2. Total out-of-scope statements
3. Finding counts by type and severity
4. Each finding with a stable identifier such as `ISSUE-1`

End with:

> Review complete. Please resolve or explicitly accept each listed issue. Refer to the issue identifiers in your response.

Stop. Do not create the requirements file until the user responds.

If there are no findings, ask the user to confirm that the extracted requirements list may be saved.

### Step 7: Apply user decisions

For each finding, record one decision:

- `Resolved` — apply user-approved wording or clarification
- `Accepted open` — preserve the source requirement and record the unresolved issue
- `Deferred` — preserve the source requirement and record that a decision is pending

Rules:

- Do not rewrite source requirements without explicit user approval.
- If the user cannot answer or asks to skip, use `Accepted open` unless the user explicitly says `Deferred`.
- Do not remove a requirement unless the user explicitly approves retirement.
- Do not add a new requirement unless the user explicitly approves it and identifies its business source.
- Preserve original source quotes even when approved wording changes.

### Step 8: Save the requirements analysis

Save beside the Skill 01 User Story file as:

`requirements_{ISSUEKEY}.md`

Do not overwrite an existing file without explicit user approval.

Use one of these statuses:

- `Reviewed` — no open or deferred issues
- `Reviewed with accepted open issues` — one or more findings were accepted open
- `Draft - decisions pending` — one or more findings were deferred

## Output format

```markdown
---
issue_key: {ISSUEKEY}
source_file: user_story_{ISSUEKEY}.md
generated_by: qa-requirements-analysis
reviewed_at: {ISO-8601 datetime}
status: {STATUS}
total_requirements: {N}
open_issues: {N}
---

# Requirements Analysis: {ISSUEKEY} — {Title}

## Requirements List

| ID | Requirement | Source |
|---|---|---|
| REQ-1 | {verbatim or user-approved statement} | Acceptance Criteria |
| REQ-2 | {verbatim or user-approved statement} | Description |

## Requirement Details

### REQ-1

**Statement:** {verbatim or user-approved requirement}

**Source:** {section and source item}

**Source quote:** {exact verbatim text from the Skill 01 file}

**Preconditions:** {source-stated or user-approved value; otherwise "Not specified"}

**Expected result:** {source-stated or user-approved value; otherwise "Not specified"}

**Error handling:** {source-stated or user-approved value; otherwise "Not specified"}

**Analysis status:** {Clear | Resolved | Accepted open | Deferred}

## Out of Scope

| ID | Source statement |
|---|---|
| OOS-1 | {verbatim out-of-scope text} |

## Findings and Decisions

| Issue ID | Type | Severity | Requirement | Decision | Resolution |
|---|---|---|---|---|---|
| ISSUE-1 | GAP | Medium | REQ-1 | Resolved | {user-approved resolution} |

## Traceability

| Source item | Requirement IDs |
|---|---|
| Acceptance Criteria 1 | REQ-1, REQ-2 |
```

## Requirement wording rules

- Initial statements must preserve source wording.
- User-approved clarifications may be incorporated into `Statement`.
- `Source quote` must always remain verbatim.
- Do not turn an observation, comment, design link, or inferred behavior into a requirement without user approval.
- Do not generalize exact values, names, labels, statuses, messages, or thresholds.
- If no precondition, expected result, or error behavior is stated or approved, write `Not specified`.

## Error handling

| Situation | Action |
|---|---|
| Skill 01 output is missing | Ask the user to run Skill 01 or provide the correct path |
| Multiple matching User Story files exist | Ask for the exact path |
| Work Item ID does not match the filename | Report the mismatch and stop |
| Work Item Type is not User Story | Report the actual type and stop |
| Description and Acceptance Criteria are both empty | Report that no requirements source is available and stop |
| Existing `requirements_{ISSUEKEY}.md` is found | Stop and request approval before overwriting |
| A finding is unanswered | Record `Accepted open` or `Deferred` according to the user's instruction |
| A statement has no exact source quote | Do not include it as a requirement; present it as an ungrounded proposal |

## Prohibitions

- Do not change ADO.
- Do not fetch ADO or external data.
- Do not modify Skill 01 output files.
- Do not rewrite requirements without explicit user approval.
- Do not invent expected behavior, failure behavior, data, permissions, or thresholds.
- Do not ask implementation-design questions unless implementation details are explicitly part of the acceptance criterion.
- Do not flag standard platform behavior unless the User Story overrides it or it materially affects pass/fail behavior.
- Do not manufacture questions merely to produce findings.
- Do not silently resolve uncertainty.

## Verification

Before saving:

1. Confirm the source file was generated in Skill 01's output hierarchy.
2. Confirm every Acceptance Criteria statement maps to at least one `REQ-N` or is explicitly non-verifiable context.
3. Confirm every requirement has an exact source quote.
4. Confirm all out-of-scope statements are preserved.
5. Confirm IDs are unique and stable.
6. Confirm every finding has a recorded user decision.
7. Confirm the output status matches the recorded decisions.
8. Re-read the saved file and verify no source wording or exact value was unintentionally changed.

## Quality checklist

- [ ] Skill 01 source file located and validated
- [ ] Description and Acceptance Criteria reviewed
- [ ] Requirements extracted in source order
- [ ] Stable requirement IDs assigned
- [ ] Exact source quote retained for every requirement
- [ ] Only applicable test-design techniques used
- [ ] Findings are material and evidence-based
- [ ] User decisions recorded
- [ ] Out-of-scope statements preserved
- [ ] Output status reflects unresolved decisions
- [ ] `requirements_{ISSUEKEY}.md` saved beside the User Story
- [ ] Skill 01 files unchanged
- [ ] ADO unchanged

## Reference

Use `review-rules.md` in this skill's folder for technique applicability, quality criteria, and severity guidance.
