# Test Conditions - Technique Reference

Apply a technique only when the reviewed requirement establishes the inputs, conditions, boundaries, states, or outcomes needed by that technique.

## General rule

A technique helps decompose approved behavior; it must not expand scope.

Generate a condition only when:

1. A Skill 02 requirement or user-approved resolution supports it.
2. Its setup can be established without invention.
3. Its expected result is objective and observable.

If a technique identifies an unsupported path, do not invent the outcome. For an `Accepted open` requirement, record the affected aspect as not covered.

## Scenario-based condition

Use `Scenario` for straightforward behavior that does not require a formal technique, including:

- Displaying a specified record, field, tab, column, or label
- Performing one specified action
- Navigating to a specified destination
- Displaying items in a specified order
- Showing a specified state without a transition

Derive one condition per independently observable result.

Do not create invalid-action or error scenarios unless the requirement specifies their expected outcome.

## Equivalence Partitioning (EP)

Use `EP` when a requirement defines classes of input or state that should produce different behavior.

Examples of supported partitions:

- Populated and empty, when both outcomes are defined
- Authorized and unauthorized, when both access outcomes are defined
- Supported and unsupported formats, when handling is defined
- Named statuses with different expected displays or actions

Coverage:

- Create at least one condition for each explicitly defined or user-approved partition.
- Preserve exact representative values when the requirement supplies them.

Do not automatically add:

- null or undefined
- wrong data types
- malformed values
- out-of-range values
- unauthorized roles

These are conditions only when the reviewed requirements establish their expected behavior.

## Boundary Value Analysis (BVA)

Use `BVA` when a requirement defines a measurable boundary such as:

- Minimum or maximum value
- Length or item count
- Date or time limit
- Capacity
- Ordered position with boundary-specific behavior

Choose values appropriate to the domain:

| Domain | Typical values when supported |
|---|---|
| Inclusive integer boundary | immediately below, at, immediately above |
| String length | one below, at, one above |
| Date/time | immediately before, at, immediately after |
| Collection size | below, at, above the stated limit |

Generate only values whose expected outcome is defined or approved. If the requirement defines behavior at the limit but not outside it, test the limit and do not invent rejection behavior outside it.

## State Transition Testing (ST)

Use `ST` when a requirement defines:

1. A starting state
2. An event or trigger
3. A resulting state

Optional elements may include a guard, reverse transition, or invalid transition, but they generate conditions only when their expected behavior is defined.

Examples:

- `Offline` plus connection restored produces `Connected`
- `Pending` plus approval produces `Approved`
- Loading completion changes `Loading` to `Loaded`

Simple page display, navigation, or visibility is not automatically a state transition. Use `Scenario` unless the requirement explicitly makes the before-state and after-state material.

Coverage:

- Generate one condition for each explicitly defined valid transition.
- Generate invalid or reverse-transition conditions only when their outcomes are specified.

## Decision Table Testing (DT)

Use `DT` when two or more independent conditions jointly determine an outcome.

Process:

1. List only conditions stated or directly established by the reviewed requirement.
2. Identify possible, in-scope combinations.
3. Map each supported combination to its specified outcome.
4. Merge combinations when one condition does not affect the result.
5. Generate one condition for each remaining supported rule.

Do not automatically generate all mathematical combinations when some are impossible, out of scope, or have no specified outcome.

## Technique selection

| Requirement pattern | Technique |
|---|---|
| Direct display, action, order, or navigation | Scenario |
| Defined input or state classes with different outcomes | EP |
| Explicit limit, range, threshold, or capacity | BVA |
| Explicit start state, trigger, and resulting state | ST |
| Multiple conditions jointly determine an outcome | DT |

When more than one technique applies, use the smallest set that covers the stated behavior without duplicate conditions.

## Condition categories

Technique and condition intent are separate. A condition may have one of these intents:

| Intent | Use when |
|---|---|
| Positive | A supported input, action, or state produces the required outcome |
| Negative | A defined invalid or disallowed case produces its specified outcome |
| Boundary | A specified boundary or adjacent value has a defined outcome |
| Error | A stated failure produces a specified response |
| Transition | A defined event moves the entity between specified states |
| Decision | A supported combination produces a specified result |

Do not assign a negative, error, boundary, transition, or decision intent merely to broaden coverage.

## Final filter

Remove a proposed condition when any answer is `No`:

1. Does one reviewed requirement support the setup?
2. Does that requirement or an approved resolution support the expected result?
3. Is the expected result observable?
4. Does the condition add distinct coverage?
5. Is it within scope?
