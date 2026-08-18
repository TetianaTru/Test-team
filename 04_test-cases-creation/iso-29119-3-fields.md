# Test Case Fields and Project Writing Rules

This reference combines the local test-case structure with the important NG_ACT authoring rules from `TestDocsSpecification.docx` and `NG_ACT_Test_Case_Authoring_Instructions.docx`.

## Required local fields

| Field | Required | Rule |
|---|---|---|
| Logical ID | Yes | `TC-{ISSUEKEY}-{NNN}`; never invent an ADO numeric ID |
| Title | Yes | User Story title, object, operation, variant, and outcome |
| State | Yes | `Design` unless project workflow says otherwise |
| Priority | Yes | Risk-based value 1, 2, or 3 |
| Description | Yes | One sentence stating what the case proves |
| Traceability | Yes | Skill 03 condition IDs and parent requirement IDs |
| Preconditions | Yes | Dedicated Step 1 with concrete starting state |
| Test data | When needed | Exact values and units; representative values marked `[test data]` |
| Actions | Yes | Atomic, imperative, and based on exact UI labels |
| Expected results | Yes after each action | Observable, normally passive voice, no `should` or `must` |
| Cleanup | Yes | Concrete restoration or `No cleanup required.` |
| Pass criterion | Yes | Objective summary of mapped conditions |
| ADO placement | Yes | Recommended Test Plan and suite path; no ADO write |

## Title rules

Format:

`{User Story title} - {Object or component} - {Operation} - {Mode or variant} - {Outcome}`

Use exact UI labels in double quotes.

Good:

> Admin module: Display Movement Related Inventory - "Components" tab - Open Workstation details - Default view - Components tab is displayed

Avoid:

- Check functionality
- Negative pass
- Various validations
- Test page

For a future Bug-based workflow:

`Bug-driven TC - {short description}`

## Priority

| Priority | Meaning |
|---|---|
| 1 - High | Main functionality, primary CRUD, persistence, or critical execution behavior |
| 2 - Medium | Layout/UI, secondary workflows, boundaries, unsaved changes, or rename behavior |
| 3 - Low | Lower-risk negative combinations and supplementary validation |

## Description

Write one sentence that states what the case proves. Do not repeat all steps.

Example:

> Verifies that opening a Workstation record displays its dedicated details page with the "Components" tab selected by default.

## Preconditions

Step 1 is a dedicated `Preconditions:` block. Its Expected Result cell may be empty.

List applicable dependencies in this order:

1. Inventory and master data
2. Recipe or batch state
3. Upstream steps
4. Exact object values and status
5. Execution state

Use concrete statements. Avoid:

- Appropriate data exists
- A recipe is configured
- The system is ready

## Test data

- Preserve requirement values literally.
- Include units.
- Use stable canonical data across related cases when possible.
- Mark representative values absent from requirements as `[test data]`.
- Explain why boundary or partition values were selected.
- Do not use `some value` or an unexplained placeholder.

## Actions

Each step contains one atomic action in imperative form.

Preferred verbs:

- Click
- Select
- Enter
- Clear
- Toggle
- Hover
- Expand
- Collapse
- Scroll
- Inspect
- Navigate
- Open

Good:

> Click the "Components" tab.

Avoid:

- Try different combinations.
- Verify all fields work.
- Check validation for other parameters.
- Verify everything is displayed correctly.

## Expected results

Except for the Preconditions step, every step has an expected result.

Expected results:

- Describe observable state, values, ordering, mode, status, controls, calculations, or messages.
- Use passive voice where natural.
- Do not use `should` or `must`.
- Do not merely restate the action.
- Use exact messages when specified.
- Include formula and result when a calculation is required.

Good:

> The "Components" tab is displayed as the selected tab.

Bad:

> The tab should open correctly.

## Step count

- Minimum: 4, including Preconditions.
- Target maximum: 25.
- Absolute maximum: 50.

Combine only closely related conditions to reach four meaningful steps. Split unrelated validation families and oversized scenarios.

## UI coverage

When supported by Skill 03, a focused UI test case may cover:

- Tabs and default selected tab
- Navigation sections
- Header name, index, status, and actions
- Default values
- Editable and read-only fields
- Disabled or unavailable actions
- Tooltip content
- Ordering and linkage
- Defined `N/A` behavior

Map every inspected element to a separate Skill 03 condition and expected-result step.

## Editing and persistence

Cover these transitions only when Skill 03 establishes them:

- Enter edit mode
- Cancel
- Complete editing
- Acknowledge and complete
- Save and close
- Reopen
- Discard without changes
- Unsaved-change dialog branches

Persistence cases reopen the object and verify every important saved value.

## Validation

Create validation cases only for conditions already present in Skill 03:

- Required values
- Range errors
- Format errors
- Invalid combinations
- Circular dependencies
- Boundaries
- Uniqueness and length

Use exact input and exact expected error behavior. Do not assume that format and range errors behave alike.

## Independence and cleanup

Each test case:

- Defines all required starting state.
- Does not depend on another case.
- Restores shared data when modified.
- Ends in a known saved state.
- States the final status when relevant.

## ADO organization reference

Recommended release placement:

```text
Integrated Multi-Parallel Bioreactor Release
  {Feature title}
    {ISSUEKEY}: {User Story title}
      Test Case
```

Regression placement:

```text
Regression_ NG_ACT Stepping Stone High-Level Software Subsystem
  Integrated Multi-Parallel Bioreactor Release
    {Feature title}
      {Functionality-based suite}
        Test Case
```

After explicit publication, each ADO Test Case is linked to the User Story using `Tested by`.

Local authoring does not create or modify these ADO objects.

## Final quality check

- [ ] Correct placement metadata
- [ ] Focused, descriptive title
- [ ] One-line description
- [ ] Concrete, ordered preconditions
- [ ] One action per step
- [ ] Expected result after every action
- [ ] Exact UI labels and messages
- [ ] Exact values and units
- [ ] Separate materially different validation families
- [ ] Persistence verified by reopen when required
- [ ] Independent execution and cleanup
- [ ] No vague wording
- [ ] No unsupported behavior
