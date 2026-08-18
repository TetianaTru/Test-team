---
name: qa-test-conditions-creation
description: Create traceable, atomic test conditions from a requirements analysis produced by Skill 02. Use only after Skill 01 has collected the ADO User Story and Skill 02 has created requirements_{ISSUEKEY}.md.
recommended_model: sonnet
recommended_effort: medium
skill_01_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\01_ADO-data-collection
skill_02_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\02_requirements-analysis
skill_03_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\03_test-conditions-creation
skill_output_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs
---

# QA Test Conditions Creation

Create atomic, traceable test conditions from a requirements analysis that has completed the Skill 01 and Skill 02 workflow.

## Workflow dependency

Run the skills in this order:

1. Skill 01 - ADO data collection:
   `C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\01_ADO-data-collection`
2. Skill 02 - Requirements analysis:
   `C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\02_requirements-analysis`
3. Skill 03 - Test conditions creation:
   `C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\03_test-conditions-creation`

Skill 03 must not run directly from ADO data or from `user_story_{ISSUEKEY}.md`. Its authoritative input is the Skill 02 requirements analysis.

Expected hierarchy:

```text
C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs\
  stories\
    {EPIC-KEY}\
      epic.md
      {FEATURE-KEY}\
        feature.md
        {ISSUEKEY}\
          user_story_{ISSUEKEY}.md
          requirements_{ISSUEKEY}.md
          test-cases\
          tasks\
```

## Goal

1. Locate and validate `requirements_{ISSUEKEY}.md`.
2. Read the reviewed requirements and their decisions.
3. Derive only test conditions supported by those requirements.
4. Preserve traceability to the exact requirement and source quote.
5. Record requirements that cannot yet produce an objective condition.
6. Save `test_conditions_{ISSUEKEY}.md` beside the requirements file.

## Input

Accept one of:

- An ADO User Story URL containing a numeric work-item ID.
- A numeric User Story ID.
- A direct path to `requirements_{ISSUEKEY}.md`.

For an ID or URL, search under:

`C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs\stories`

Expected filename:

`requirements_{ISSUEKEY}.md`

If exactly one matching file is found, use it. If none or more than one is found, stop and ask the user for the exact path.

## Data-source and safety rules

- Read only the local `requirements_{ISSUEKEY}.md` produced by Skill 02.
- Do not retrieve ADO data again.
- Do not use a browser, web search, or another external source.
- Do not change anything in ADO.
- Do not modify Skill 01 or Skill 02 output files.
- Write only the Skill 03 output described here.
- Do not infer behavior from comments, designs, relations, or metadata that Skill 02 did not approve as a requirement.

## Core quality rules

Every test condition must be:

- **Atomic** - one independently observable pass/fail outcome.
- **Traceable** - bound to one `REQ-N` and its exact source wording.
- **Unambiguous** - the expected result has one material interpretation.
- **Executable** - the required setup and expected result are stated or approved.
- **Independent** - it does not require another condition to pass first.
- **Non-duplicative** - it does not repeat the same setup and expected result.

An interaction may produce more than one condition when the requirement establishes multiple independently observable outcomes.

Do not use vague expected-result words such as:

- correctly
- properly
- appropriate
- as needed
- quickly
- smoothly
- conveniently
- looks good
- works normally

Use the exact observable value, state, label, order, message, or navigation destination established by the requirement.

## Workflow

### Step 1: Locate and validate the Skill 02 file

Validate all of the following:

1. The file exists and is named `requirements_{ISSUEKEY}.md`.
2. YAML frontmatter contains:
   - `issue_key` matching `{ISSUEKEY}`
   - `source_file: user_story_{ISSUEKEY}.md`
   - `generated_by: qa-requirements-analysis`
   - one of the supported statuses below
3. At least one `### REQ-N` section exists.
4. Every requirement section contains:
   - `**Statement:**`
   - `**Source quote:**`
   - `**Analysis status:**`

Supported status behavior:

| Skill 02 status | Skill 03 action |
|---|---|
| `Reviewed` | Proceed. |
| `Reviewed with accepted open issues` | Proceed, preserving accepted issues and applying the coverage rules in Step 3. |
| `Draft - decisions pending` | Stop. Ask the user to complete Skill 02 by resolving or explicitly accepting the deferred findings. |

If validation fails, stop and report the failed check and the action required.

### Step 2: Load requirements and decisions

For each requirement, extract:

| Element | Source |
|---|---|
| User Story title | `# Requirements Analysis: {ISSUEKEY} - {Title}` heading |
| Requirement ID | `### REQ-N` heading |
| Statement | `**Statement:**` |
| Source | `**Source:**` |
| Source quote | `**Source quote:**` |
| Preconditions | `**Preconditions:**` |
| Expected result | `**Expected result:**` |
| Error handling | `**Error handling:**` |
| Analysis status | `**Analysis status:**` |

Also read `## Findings and Decisions` so accepted open issues remain visible in the output.

Do not treat `## Out of Scope` statements as testable requirements.

### Step 3: Determine condition eligibility

Use the requirement's analysis status:

| Analysis status | Action |
|---|---|
| `Clear` | Generate supported test conditions. |
| `Resolved` | Generate conditions using the user-approved statement and resolution. Preserve the original source quote. |
| `Accepted open` | Generate only conditions whose setup and expected result remain objective. Record any unsupported aspect as `Not covered - accepted open issue`. |
| `Deferred` | Do not proceed; the file should have status `Draft - decisions pending` and must return to Skill 02. |

Never add `[ASSUMPTION]` behavior. If an expected result cannot be derived from the requirement, do not create that condition. Record the requirement or affected aspect in the coverage summary as not covered and state the related accepted issue.

### Step 4: Derive test conditions

For each eligible requirement:

1. Identify each independently observable behavior in the approved statement.
2. Use the requirement's Preconditions, Expected result, and Error handling only when they are specified.
3. Apply only techniques whose applicability rules are met in `checklist-techniques.md`.
4. Preserve exact labels, values, order, states, messages, and thresholds.
5. Generate negative, boundary, error, or state-transition conditions only when the requirement establishes their expected behavior.
6. Do not force a fixed number of conditions per requirement.

A straightforward display, action, or navigation requirement may need only one positive condition.

### Step 5: Write atomic condition records

Each condition must include:

- **ID**
- **Test condition**
- **Input / Setup**
- **Expected result**
- **Technique**
- **Source quote**

Use this ID scheme:

- `TC-REQ-1.1`, `TC-REQ-1.2`, and so on for `REQ-1`
- `TC-REQ-2.1`, `TC-REQ-2.2`, and so on for `REQ-2`

IDs must be sequential within each requirement and must not be reused.

Use `Not specified` for setup only when no setup is required to understand or execute an observation. Do not use `Not specified` as an expected result.

### Step 6: Save the output

Save beside the Skill 02 file as:

`test_conditions_{ISSUEKEY}.md`

Do not overwrite an existing file without explicit user approval.

Use this format:

```markdown
---
issue_key: {ISSUEKEY}
source_file: requirements_{ISSUEKEY}.md
generated_by: qa-test-conditions-creation
generated_at: {ISO-8601 datetime}
source_status: {SKILL_02_STATUS}
status: Draft
total_requirements: {N}
covered_requirements: {N}
total_conditions: {N}
---

# Test Conditions: {ISSUEKEY} - {Title}

## Test Conditions

### REQ-1 - {Requirement summary}

| ID | Test condition | Input / Setup | Expected result | Technique | Source quote |
|---|---|---|---|---|---|
| TC-REQ-1.1 | {single behavior to verify} | {required setup or input} | {one observable pass criterion} | {Scenario, EP, BVA, ST, or DT} | {exact supporting phrase} |

## Coverage Summary

| Requirement | Analysis status | Condition IDs | Coverage |
|---|---|---|---|
| REQ-1 | Clear | TC-REQ-1.1 | Covered |
| REQ-2 | Accepted open | None | Not covered - ISSUE-1 prevents an objective expected result |

## Accepted Open Issues

| Issue ID | Requirement | Impact on test conditions |
|---|---|---|
| ISSUE-1 | REQ-2 | {covered aspect or reason it is not covered} |
```

Omit `## Accepted Open Issues` when the source status is `Reviewed`.

## Technique and source-binding rules

- Use `Scenario` for a direct action-to-outcome, display, ordering, or navigation check that does not require another formal technique.
- Use `EP`, `BVA`, `ST`, or `DT` only according to `checklist-techniques.md`.
- A source quote may support multiple conditions only when each condition verifies a different independently stated aspect.
- Do not derive a condition from a requirement's general topic when the expected behavior appears only in another requirement.
- Do not convert `Not specified` preconditions or error handling into invented checks.

## Prohibitions

- Do not generate conditions for behavior absent from the reviewed requirements.
- Do not invent routes, selectors, roles, permissions, states, test data, messages, thresholds, or failure handling.
- Do not add general QA advice.
- Do not test implementation details such as CSS classes, internal APIs, or database queries.
- Do not test standard browser or platform behavior unless the requirement changes it.
- Do not generate visual-quality, animation, timing, or performance conditions without a measurable requirement.
- Do not apply BVA without an explicit boundary.
- Do not generate invalid inputs or transitions without a specified expected result.
- Do not duplicate conditions.
- Do not generalize exact source values.
- Do not modify ADO or any Skill 01/02 output.

## Error handling

| Situation | Action |
|---|---|
| Skill 02 output is missing | Ask the user to run Skill 02 or supply the correct path. |
| Multiple matching files exist | Ask for the exact path. |
| Filename and `issue_key` differ | Report the mismatch and stop. |
| `generated_by` is not `qa-requirements-analysis` | Report that the input was not generated by the supported Skill 02. |
| Status is `Draft - decisions pending` | Stop and ask the user to finish Skill 02 decisions. |
| A requirement has no statement or source quote | Exclude it, report the validation failure, and stop. |
| An accepted-open requirement lacks an objective outcome | Record it as not covered; do not invent a condition. |
| Output already exists | Stop and request explicit overwrite approval. |

## Verification

Before saving:

1. Confirm the input is the Skill 02 file for the same issue key.
2. Confirm the Skill 02 status permits processing.
3. Confirm every generated condition maps to exactly one requirement.
4. Confirm every condition is supported by the requirement statement or approved resolution.
5. Confirm every expected result is objective and observable.
6. Confirm no out-of-scope statement became a condition.
7. Confirm every requirement appears in the coverage summary.
8. Confirm accepted open issues and their coverage impact are preserved.
9. Confirm IDs are unique and sequential.
10. Confirm `total_conditions` equals the number of condition rows.
11. Re-read the saved file and confirm no unsupported behavior was added.
12. Confirm Skill 01 and Skill 02 files remain unchanged.
13. Confirm ADO remains unchanged.

## Quality checklist

- [ ] Skill 02 source file located and validated
- [ ] Skill 02 status permits test-condition creation
- [ ] Requirements loaded in source order
- [ ] Deferred issues do not pass validation
- [ ] Accepted open issues remain visible
- [ ] Every condition is atomic and traceable
- [ ] Only applicable test-design techniques used
- [ ] No assumptions or unsupported negative paths added
- [ ] Every requirement represented in the coverage summary
- [ ] Condition IDs unique and sequential
- [ ] Frontmatter counts match the output
- [ ] Existing output not overwritten without approval
- [ ] Skill 01 and Skill 02 outputs unchanged
- [ ] ADO unchanged

## References

- `checklist-techniques.md` - technique applicability and derivation rules
- `checklist-example.md` - worked example matching this output contract
