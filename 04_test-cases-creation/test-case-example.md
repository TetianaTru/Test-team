# Worked Example - Test Case Creation

This example shows how Skill 04 converts related Skill 03 conditions into one focused, four-step UI test case without inventing behavior.

## Example Skill 03 input

```markdown
---
issue_key: 42001
source_file: requirements_42001.md
generated_by: qa-test-conditions-creation
status: Draft
total_conditions: 3
---

# Test Conditions: 42001 - Display Workstation details

### REQ-1 - Workstation details

| ID | Test condition | Input / Setup | Expected result | Technique | Source quote |
|---|---|---|---|---|---|
| TC-REQ-1.1 | Navigate to the dedicated Workstation details page | Open a Workstation inventory record | The dedicated Workstation details page is displayed | Scenario | "redirected to the dedicated Workstation details page" |
| TC-REQ-1.2 | Display the Components tab as the default open tab | Open the Workstation details page | The Components tab is open | Scenario | "Components - opened by default" |
| TC-REQ-1.3 | Display the Modules tab as disabled | Display the Workstation details page | The Modules tab is disabled | Scenario | "Modules - disabled" |
```

## Example index

```markdown
---
issue_key: 42001
source_file: ..\test_conditions_42001.md
generated_by: qa-test-cases-creation
generated_at: 2026-08-18T14:00:00Z
status: Draft
total_source_conditions: 3
covered_conditions: 3
total_test_cases: 1
---

# Test Case Index: 42001 - Display Workstation details

## Recommended ADO Placement

| Level | Name |
|---|---|
| Test Plan | Integrated Multi-Parallel Bioreactor Release |
| Feature suite | Inventory Management |
| User Story suite | 42001: Display Workstation details |

## Test Cases

| Logical ID | Title | Priority | Source conditions | File |
|---|---|---|---|---|
| TC-42001-001 | Display Workstation details - Workstation record - Open - Default view - Details and tab states are displayed | 1 | TC-REQ-1.1, TC-REQ-1.2, TC-REQ-1.3 | [Open](./TC-42001-001.md) |

## Condition Coverage

| Test condition | Test case | Coverage |
|---|---|---|
| TC-REQ-1.1 | TC-42001-001 | Covered |
| TC-REQ-1.2 | TC-42001-001 | Covered |
| TC-REQ-1.3 | TC-42001-001 | Covered |
```

## Example test case

```markdown
---
logical_id: TC-42001-001
issue_key: 42001
generated_by: qa-test-cases-creation
generated_at: 2026-08-18T14:00:00Z
state: Design
priority: 1
source_file: ..\test_conditions_42001.md
source_conditions:
  - TC-REQ-1.1
  - TC-REQ-1.2
  - TC-REQ-1.3
status: Draft
---

# Display Workstation details - Workstation record - Open - Default view - Details and tab states are displayed

## Description

Verifies that opening a Workstation inventory record displays its dedicated details page with the "Components" tab open and the "Modules" tab disabled.

## Traceability

| Test condition | Requirement |
|---|---|
| TC-REQ-1.1 | REQ-1 |
| TC-REQ-1.2 | REQ-1 |
| TC-REQ-1.3 | REQ-1 |

## Steps

| Step | Action | Expected Result |
|---|---|---|
| 1 | **Preconditions:**<br>- The "Inventory Management" page is displayed.<br>- A Workstation inventory record is available. | |
| 2 | Open the Workstation inventory record. | The dedicated Workstation details page is displayed. |
| 3 | Inspect the selected tab on the Workstation details page. | The "Components" tab is displayed as open. |
| 4 | Inspect the "Modules" tab. | The "Modules" tab is displayed as disabled. |

## Cleanup

No cleanup required.

## Pass Criterion

The dedicated Workstation details page is displayed, the "Components" tab is open, and the "Modules" tab is disabled.

## ADO Placement

| Field | Value |
|---|---|
| Test Plan | Integrated Multi-Parallel Bioreactor Release |
| Feature suite | Inventory Management |
| User Story suite | 42001: Display Workstation details |
| ADO Test Case ID | Not assigned |
| User Story link type after publication | Tested by |
```

## Patterns demonstrated

- Three closely related UI conditions are combined into one focused case.
- Every source condition maps to a distinct expected-result step.
- The Preconditions block is Step 1.
- The case contains four meaningful steps without artificial actions.
- Exact UI labels are quoted.
- Expected results are observable and contain no `should` or `must`.
- No route, selector, user role, data value, or interaction is invented.
- ADO placement is documented without changing ADO.
