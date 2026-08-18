---
name: qa-test-cases-creation
description: Create deterministic, executable manual test cases from test_conditions_{ISSUEKEY}.md produced by Skill 03. Use only after Skills 01, 02, and 03 have completed.
recommended_model: sonnet
recommended_effort: high
skill_01_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\01_ADO-data-collection
skill_02_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\02_requirements-analysis
skill_03_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\03_test-conditions-creation
skill_04_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\skills\04_test-cases-creation
skill_output_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs
---

# QA Test Cases Creation

Create self-contained, repeatable manual test cases from the reviewed test conditions produced by Skill 03.

## Workflow dependency

Run the skills in this order:

1. Skill 01 - collect ADO data locally.
2. Skill 02 - analyze requirements and record decisions.
3. Skill 03 - create atomic test conditions.
4. Skill 04 - create executable test cases.

Skill 04 must not derive test cases directly from ADO, the User Story file, or the requirements file. Its authoritative behavior input is `test_conditions_{ISSUEKEY}.md`.

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
          test_conditions_{ISSUEKEY}.md
          test-cases\
          tasks\
```

## Goal

1. Locate and validate the Skill 03 output.
2. Convert supported test conditions into deterministic manual test cases.
3. Keep every action atomic and every expected result observable.
4. Preserve traceability to the test condition and parent requirement.
5. Record concrete data, state, UI labels, ordering, units, and cleanup when supported.
6. Save one local Markdown file per test case plus an index.
7. Prepare ADO suite-placement metadata without changing ADO.

## Input

Accept one of:

- An ADO User Story URL containing a numeric work-item ID.
- A numeric User Story ID.
- A direct path to `test_conditions_{ISSUEKEY}.md`.

For an ID or URL, search under:

`C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs\stories`

Expected filename:

`test_conditions_{ISSUEKEY}.md`

If exactly one file matches, use it. If none or more than one matches, stop and ask for the exact path.

## Data-source and safety rules

- Read the local Skill 03 output only for test behavior.
- The Skill 01 `feature.md` and `user_story_{ISSUEKEY}.md` may be read only to obtain the exact Feature title, User Story title, and Figma-link presence for placement metadata. They must not add or change expected behavior.
- Do not retrieve ADO or Figma data.
- Do not use a browser, web search, or another external source.
- Do not change anything in ADO.
- Do not create Test Plans, Test Suites, Test Cases, links, or comments in ADO.
- Do not modify Skill 01, 02, or 03 outputs.
- Write only under the User Story's existing `test-cases` folder.
- ADO publication is a separate action and requires an explicit user request and confirmation.
- If the source references a Figma design but the Skill 03 conditions do not contain the UI detail needed for an executable case, report the missing local design information. Do not fetch or guess it.

## Project authoring principles

Each test case must be:

- **Self-contained** - executable without reading the User Story or another test case.
- **Repeatable** - produces the same observable result from the defined starting state.
- **Focused** - covers one coherent scenario theme.
- **Independent** - does not depend on another test case having run.
- **Traceable** - identifies its Skill 03 test condition and parent requirement.
- **Concrete** - uses exact controls, values, units, statuses, messages, and ordering when specified.
- **Grounded** - contains no behavior absent from Skill 03.

Do not invent expected behavior. If a label, limit, state, calculation, error, or interaction is unclear, do not create unsupported steps. Record the condition as blocked in the index.

## Workflow

### Step 1: Locate and validate Skill 03 output

Validate:

1. Filename is `test_conditions_{ISSUEKEY}.md`.
2. Frontmatter contains:
   - `issue_key` matching `{ISSUEKEY}`
   - `source_file: requirements_{ISSUEKEY}.md`
   - `generated_by: qa-test-conditions-creation`
   - `status: Draft`
   - `total_conditions` greater than zero
3. A `## Test Conditions` section exists.
4. Every condition row contains:
   - ID in the format `TC-REQ-N.N`
   - Test condition
   - Input / Setup
   - Expected result
   - Technique
   - Source quote
5. Every condition ID is unique.
6. The number of condition rows equals `total_conditions`.

If validation fails, stop and report the exact failure and required action.

### Step 2: Load placement metadata

Read the local hierarchy to obtain:

- Feature title from `feature.md`
- User Story ID and title from `user_story_{ISSUEKEY}.md` or the Skill 03 heading

Record this recommended ADO placement:

```text
Test Plan: Integrated Multi-Parallel Bioreactor Release
Level 1 static suite: {Feature title}
Level 2 requirement-based suite: {ISSUEKEY}: {User Story title}
```

This is placement metadata only. Do not inspect or modify ADO.

For future regression publication, record the documented structure as guidance:

```text
Test Plan: Regression_ NG_ACT Stepping Stone High-Level Software Subsystem
Level 1 static suite: Integrated Multi-Parallel Bioreactor Release
Level 2 static suite: {Feature title}
Level 3 static suite: {Functionality-based grouping}
```

Regression Level 3 suites are grouped by functionality, not one suite per User Story.

### Step 3: Determine test-case eligibility

Ensure every executable Skill 03 condition maps to one test case. A test case may cover one condition or a small group of closely related conditions under the combination rules below.

Do not exclude display or presence conditions. Project instructions prefer a separate UI coverage case where related UI conditions can be verified together.

Multiple conditions may be combined into one test case only when all are true:

1. They form one coherent scenario or UI inspection.
2. Combining them helps meet the four-step minimum without mixing unrelated behavior.
3. Each source condition remains mapped to a distinct step and expected result.
4. The resulting case remains focused and independently executable.

Keep materially different behaviors in separate cases, including when applicable:

- Read-only presentation
- Edit and cancel
- Successful edit and completion
- Save, close, and reopen persistence
- Unsaved-change dialog branches
- Required-field validation
- Format, range, combination, and boundary validation
- Creation, update, rename, and removal
- Re-indexing or event ordering
- Execution status transitions

Generate these only when Skill 03 contains corresponding conditions.

### Step 4: Assign IDs and titles

#### Logical ID

Use:

`TC-{ISSUEKEY}-{NNN}`

Examples:

- `TC-188021-001`
- `TC-188021-002`

The ADO numeric Test Case ID does not exist until publication and must never be invented.

#### Title

Use a consistent general-to-specific form:

`{User Story title} - {Object or component} - {Operation} - {Mode or variant} - {Outcome}`

Rules:

- Name the exact object under test.
- Include the significant operation and state.
- End with the outcome.
- Put exact UI labels in double quotes.
- Use standard English capitalization and articles.
- Do not use vague titles such as `Check functionality`, `Negative pass`, or `Various validations`.
- For a future Bug-based flow, use `Bug-driven TC - {short description}`.

### Step 5: Assign metadata

Each test case includes:

| Field | Rule |
|---|---|
| State | `Design` unless the project workflow explicitly requires another state |
| Priority 1 | Core happy path, primary CRUD, persistence, or critical execution behavior |
| Priority 2 | UI/layout, secondary workflow, boundary, unsaved changes, or rename behavior |
| Priority 3 | Lower-risk negative combinations or supplementary validation |
| Description | One sentence stating what the case proves |
| Traces to | One or more Skill 03 condition IDs and their parent `REQ-N` IDs |

Priority is risk-based. Do not assign a priority solely from the technique name.

### Step 6: Write preconditions and data

Step 1 must be a dedicated `Preconditions:` block. Its Expected Result cell may be empty.

List dependencies in this order when applicable:

1. Required inventory and master data
2. Recipe or batch state
3. Required upstream steps
4. Exact state and values of the object under test
5. Execution status

Preconditions must be factual and concrete. Do not write `appropriate data exists`.

Use exact values from Skill 03. When Skill 03 permits a representative value but does not specify one:

- Use a realistic value.
- Mark it `[test data]`.
- Do not imply that the example is a product requirement.

Always include units when material: `°C`, `mL`, `mL/min`, `cycles/min`, `°`, `%`, `h`, `min`, or `day(s)`.

For a defined boundary, use below, at, and above values only when Skill 03 provides conditions for those outcomes.

### Step 7: Write actions and expected results

#### Actions

- One atomic user action per step.
- Use imperative verbs: `Click`, `Select`, `Enter`, `Clear`, `Toggle`, `Hover`, `Expand`, `Collapse`, `Scroll`, `Inspect`, `Navigate`, `Open`.
- Put exact UI labels in double quotes.
- Enumerate concrete combinations; never write `try different combinations`.
- Repetition such as `Repeat steps X-Y` is allowed only when the repeated range, changing values, and expected differences are explicit.

#### Expected results

Except for the Preconditions step, every step must have a non-empty, observable expected result.

- Use passive voice where natural.
- Do not use `should` or `must`.
- State the current mode, displayed values, editability, index, position, status, available actions, calculations, and exact messages when established.
- Do not merely repeat the action.
- Keep one independently observable assertion per step where practical.
- If one action explicitly causes several tightly coupled required outcomes, list each outcome as a separate bullet in the same Expected Result cell.

For calculations, include the established formula and concrete result.

### Step 8: Enforce test-case size

- Minimum: 4 steps, including the Preconditions step.
- Maximum target: 25 steps.
- Absolute maximum: 50 steps.

If a focused scenario has fewer than four natural steps, combine it with closely related conditions, usually a UI inspection case. Never add artificial actions solely to reach four steps.

If a case exceeds 25 steps, split it when coherent. It may remain up to 50 steps only when splitting would break one continuous scenario.

### Step 9: Preserve independence and cleanup

Every test case must:

- Create or describe all required state.
- End in a known state.
- Include cleanup when shared data is modified.
- State the expected final complete, incomplete, or incorrect status when relevant.
- Avoid leaving temporary test objects in shared recipes or environments.

Save-and-reopen cases must verify important saved values after reopening.

### Step 10: Save local output

Save files under:

`test-cases\`

Create:

```text
test-cases\
  test_cases_{ISSUEKEY}_index.md
  TC-{ISSUEKEY}-001.md
  TC-{ISSUEKEY}-002.md
  ...
```

Do not overwrite an existing test case or index without explicit user approval.

#### Test case file format

```markdown
---
logical_id: TC-{ISSUEKEY}-{NNN}
issue_key: {ISSUEKEY}
generated_by: qa-test-cases-creation
generated_at: {ISO-8601 datetime}
state: Design
priority: {1|2|3}
source_file: ..\test_conditions_{ISSUEKEY}.md
source_conditions:
  - TC-REQ-N.N
status: Draft
---

# {User Story title} - {Object} - {Operation} - {Variant} - {Outcome}

## Description

{One sentence stating what this test case proves.}

## Traceability

| Test condition | Requirement |
|---|---|
| TC-REQ-N.N | REQ-N |

## Steps

| Step | Action | Expected Result |
|---|---|---|
| 1 | **Preconditions:**<br>- {concrete state}<br>- {required data} | |
| 2 | {One atomic action using exact UI labels.} | {One observable result.} |
| 3 | {One atomic action.} | {One observable result.} |
| 4 | {One atomic action or inspection.} | {One observable result.} |

## Cleanup

{Concrete cleanup and final state, or `No cleanup required.`}

## Pass Criterion

{Objective summary tied to all mapped test conditions.}

## ADO Placement

| Field | Value |
|---|---|
| Test Plan | Integrated Multi-Parallel Bioreactor Release |
| Feature suite | {Feature title} |
| User Story suite | {ISSUEKEY}: {User Story title} |
| ADO Test Case ID | Not assigned |
| User Story link type after publication | Tested by |
```

#### Index format

```markdown
---
issue_key: {ISSUEKEY}
source_file: ..\test_conditions_{ISSUEKEY}.md
generated_by: qa-test-cases-creation
generated_at: {ISO-8601 datetime}
status: Draft
total_source_conditions: {N}
covered_conditions: {N}
total_test_cases: {N}
---

# Test Case Index: {ISSUEKEY} - {User Story title}

## Recommended ADO Placement

| Level | Name |
|---|---|
| Test Plan | Integrated Multi-Parallel Bioreactor Release |
| Feature suite | {Feature title} |
| User Story suite | {ISSUEKEY}: {User Story title} |

## Test Cases

| Logical ID | Title | Priority | Source conditions | File |
|---|---|---|---|---|
| TC-{ISSUEKEY}-001 | {Title} | 1 | TC-REQ-1.1 | [Open](./TC-{ISSUEKEY}-001.md) |

## Condition Coverage

| Test condition | Test case | Coverage |
|---|---|---|
| TC-REQ-1.1 | TC-{ISSUEKEY}-001 | Covered |

## Blocked Conditions

| Test condition | Reason |
|---|---|
| {ID} | {Why an executable test case cannot be grounded} |
```

Omit `## Blocked Conditions` when all source conditions are covered.

## ADO publication rules

The source specifications state that published cases belong in ADO under the `NG_ACT Stepping Stone` project's Test Plans and should be linked to the User Story using `Tested by`.

Skill 04 only authors local files. If the user later explicitly requests publication:

1. Confirm the target project, Test Plan, suite path, and cases.
2. Check whether matching suites or test cases already exist.
3. Present the proposed ADO changes.
4. Obtain explicit confirmation before any write.
5. Use the project workflow's design state.
6. Link each created case to the User Story using `Tested by`.
7. Record the assigned ADO Test Case IDs locally only after successful creation.

## Prohibitions

- Do not access or modify ADO while authoring local files.
- Do not fetch Figma content.
- Do not invent UI labels, routes, roles, states, values, units, messages, calculations, or expected behavior.
- Do not derive new behavior from the source quote alone when the Skill 03 expected result does not support it.
- Do not add unsupported negative, boundary, validation, persistence, CRUD, or state-transition scenarios.
- Do not use vague actions or expected results.
- Do not create execution-order dependencies between test cases.
- Do not hide materially different validation families behind repetition.
- Do not overwrite existing files without approval.

## Error handling

| Situation | Action |
|---|---|
| Skill 03 output is missing | Ask the user to run Skill 03 or provide the correct path. |
| Multiple matching Skill 03 files exist | Ask for the exact path. |
| Metadata or counts are invalid | Report the failed validation and stop. |
| Feature or User Story title cannot be found locally | Stop and ask for the exact local hierarchy path. |
| A condition has no objective expected result | Record it as blocked; do not invent a test case. |
| A four-step case cannot be formed without artificial actions | Combine only with closely related conditions; otherwise record the limitation and ask the user. |
| Output file exists | Stop and request explicit overwrite approval. |
| User requests ADO publication | Follow the explicit publication gate above. |

## Verification

Before saving:

1. Confirm the source was generated by Skill 03 for the same issue key.
2. Confirm every source condition is covered once or recorded as blocked.
3. Confirm every case is self-contained and independent.
4. Confirm every action is atomic.
5. Confirm every action step has an observable expected result.
6. Confirm expected results use no `should` or `must`.
7. Confirm UI labels are quoted exactly when known.
8. Confirm exact source values and units are preserved.
9. Confirm each case has 4-50 steps and normally no more than 25.
10. Confirm priorities follow project risk rules.
11. Confirm cleanup and final state are defined.
12. Confirm IDs, filenames, traceability, and index counts match.
13. Confirm no unsupported behavior was added.
14. Confirm existing files were not overwritten without approval.
15. Confirm ADO and earlier skill outputs remain unchanged.

## References

- `iso-29119-3-fields.md` - project field and writing rules
- `test-case-example.md` - grounded example matching this skill
- `TestDocsSpecification.docx` - project test documentation and ADO organization rules
- `NG_ACT_Test_Case_Authoring_Instructions.docx` - detailed NG_ACT manual test authoring rules
