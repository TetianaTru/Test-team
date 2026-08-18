# Requirements Analysis Rules

Use these rules during Step 4 of `qa-requirements-analysis`. Apply a technique only when its applicability conditions are met.

## General review principle

A missing detail is a finding only when it materially affects:

- The expected system behavior
- The objective pass/fail result
- Required test setup, data, role, or permission
- A relevant boundary or state combination established by the requirement

Do not flag every conceivable edge case. Do not expand the User Story's scope.

## Test-design techniques

### Equivalence Partitioning

Apply when a requirement defines or relies on an input, output, category, status, or domain whose members may behave differently.

Check whether the source distinguishes the relevant classes, for example:

- Valid and invalid formats
- Empty and populated values
- Authorized and unauthorized roles
- Connected and Offline statuses
- Available and Unavailable statuses

Flag a missing class only when:

1. The class is relevant to the stated behavior, and
2. Different handling would change the expected result.

Do not invent unsupported classes solely to broaden test coverage.

### Boundary Value Analysis

Apply when the source defines a measurable boundary, such as:

- Minimum or maximum value
- Count or length
- Date or time limit
- Timeout
- Capacity
- Ordered position

Choose boundary points appropriate to the data type:

| Domain | Typical points |
|---|---|
| Integer inclusive range | just below, at, just above each boundary |
| Decimal range | nearest meaningful precision below, at, and above |
| Date/time | immediately before, at, and immediately after |
| String length | one below, at, and one above the length boundary |
| Collection size | empty, one, at limit, above limit when relevant |

Do not use `min - 1` or `max + 1` mechanically when the domain is not integer-based.

Flag only when boundary inclusion or adjacent behavior is material and unclear.

### State Transition Testing

Apply when the requirement defines an entity state, a trigger, and a resulting state or status.

Relevant elements:

1. Starting state
2. Trigger or event
3. Resulting state
4. Guard condition, when stated or necessary
5. Invalid transition behavior, when relevant to the story

Words such as `Connected`, `Offline`, `Available`, `Unavailable`, `Approved`, or `Paused` are state indicators.

Actions such as displaying a page, opening a view, or navigating are not automatically state transitions.

Flag a missing element only when it prevents determining the expected transition or pass/fail result.

### Decision Table Testing

Apply when two or more independent conditions jointly determine an outcome.

Process:

1. Identify only conditions stated or directly implied by the requirement.
2. Enumerate meaningful combinations.
3. Mark combinations explicitly covered by the source.
4. Flag an uncovered combination only when it is possible, in scope, and could produce a different outcome.

Do not require impossible or explicitly out-of-scope combinations.

### CRUD Scope Check

Apply when the story concerns creating, reading, updating, or deleting a persistent entity.

Do not assume all four operations belong in one story.

Flag an absent operation only when:

- The wording implies it should be supported, or
- Another source statement conflicts about whether it is supported.

Otherwise, record no CRUD finding.

## Requirement quality criteria

Evaluate each requirement against these properties:

| Property | Pass condition | Finding trigger |
|---|---|---|
| Testable | Objective pass/fail behavior is available | Outcome is subjective or cannot be observed |
| Unambiguous | One materially valid interpretation | Multiple interpretations produce different expected results |
| Atomic | One independently verifiable outcome per ID | Independent outcomes need separate setup or assertions |
| Complete enough | Information needed for the stated behavior is present | A missing detail blocks or materially weakens verification |
| Consistent | Compatible with other source statements | Two statements cannot both hold under the same conditions |
| Traceable | Grounded in an exact source quote | No source wording supports the statement |

A requirement may remain in the final file with an `Accepted open` or `Deferred` issue. In that case, it does not need to pass every property, but its uncertainty must remain visible.

## Vague-language signals

Review these phrases in context:

- appropriate
- correct
- usable
- user-friendly
- fast
- seamless
- handle
- support
- as needed
- all places
- and so on

Do not flag a word automatically. Flag it only if the surrounding source does not provide an objective interpretation and different interpretations affect testing.

## Finding types

### GAP

Use when necessary information is absent.

Examples:

- A status outcome is specified for one in-scope condition but not another possible in-scope condition.
- A required role or precondition is missing and access changes expected behavior.
- A boundary exists but inclusion at the boundary is unclear.

### AMBIGUITY

Use when wording permits multiple materially different interpretations.

Examples:

- An unclear pronoun could refer to two components.
- A term is used with two different meanings.
- An action's target is not identifiable.

### CONTRADICTION

Use when two statements prescribe incompatible behavior under the same conditions.

Before reporting a contradiction, verify that the statements do not apply to different states, roles, time periods, or scopes.

## Severity guide

| Severity | Use when | Impact |
|---|---|---|
| Blocker | Requirements contradict each other or no objective behavior can be determined | Reliable tests cannot be designed |
| High | A missing or ambiguous in-scope path could cause acceptance of materially wrong behavior | Major test coverage or product-risk gap |
| Medium | Core behavior is testable, but an important setup, state combination, or postcondition is unclear | Partial coverage or inconsistent interpretation |
| Low | Clarification improves precision but does not block core verification | Minor assumption remains |

Severity describes analysis impact, not implementation priority.

## Suggested-resolution rules

A suggested resolution must:

- Be neutral
- Preserve the source's apparent intent
- Avoid architecture or implementation choices
- Ask one focused business-behavior question
- Include proposed wording only when it is directly grounded in the source

Good:

> Should an Offline Workstation and all its movement components become Unavailable immediately when the connection is lost?

Avoid:

> Should this use WebSockets with a five-second debounce?

The second question introduces implementation details not established by the requirements.

## Analysis example

**Source:**

> When a Workstation is Offline, the Workstation and all of its bioreactor movement components are considered and displayed as Unavailable.

**Analysis:**

- Testable: Yes; both status and display are observable.
- Preconditions: A Workstation and its movement components exist, and the Workstation is Offline.
- Relevant technique: State transition testing may apply if another requirement defines how the Workstation becomes Offline.
- Do not require CRUD analysis because no persistent-entity operation is described.
- Do not invent timeout behavior unless timing affects the stated acceptance result.

**Possible finding only if the source does not define the trigger elsewhere:**

```text
[GAP] [MEDIUM] REQ-N — Offline transition trigger is not defined
Source quote: "When a Workstation is Offline..."
Detail: The expected Unavailable status is clear after the Workstation is Offline,
but the source does not identify which event establishes Offline status. This may
prevent consistent setup and verification of the transition.
Suggested resolution: Which event or condition causes a Workstation to enter
Offline status?
```
