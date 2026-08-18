# Worked Example - Test Conditions Output

This example demonstrates the Skill 03 output contract and strict source binding.

## Example Skill 02 input

```markdown
---
issue_key: 42001
source_file: user_story_42001.md
generated_by: qa-requirements-analysis
status: Reviewed
---

# Requirements Analysis: 42001 - User login

### REQ-1

**Statement:** The login form accepts an email address in the format `local-part@domain`.

**Source quote:** "The login form accepts an email address in the format `local-part@domain`."

**Preconditions:** The login form is displayed.

**Expected result:** An email address in the specified format is accepted.

**Error handling:** Not specified

**Analysis status:** Clear

### REQ-2

**Statement:** A password containing fewer than 8 characters is rejected with the message `Password must contain at least 8 characters`.

**Source quote:** "A password containing fewer than 8 characters is rejected with the message `Password must contain at least 8 characters`."

**Preconditions:** The login form is displayed.

**Expected result:** A password shorter than 8 characters is rejected and the specified message is displayed.

**Error handling:** The specified validation message is displayed.

**Analysis status:** Clear

### REQ-3

**Statement:** After successful authentication, the user is redirected to the Dashboard page.

**Source quote:** "After successful authentication, the user is redirected to the Dashboard page."

**Preconditions:** The user submits credentials that authenticate successfully.

**Expected result:** The Dashboard page is displayed.

**Error handling:** Not specified

**Analysis status:** Clear

### REQ-4

**Statement:** Activating the password visibility control displays the password characters.

**Source quote:** "Activating the password visibility control displays the password characters."

**Preconditions:** A password is entered and hidden.

**Expected result:** The entered password characters are visible.

**Error handling:** Not specified

**Analysis status:** Clear
```

The input does not define behavior for malformed email addresses, passwords of 8 or more characters, failed authentication, a Dashboard route, or a second activation of the visibility control. Skill 03 must not add conditions for those behaviors.

## Example Skill 03 output

```markdown
---
issue_key: 42001
source_file: requirements_42001.md
generated_by: qa-test-conditions-creation
generated_at: 2026-08-18T14:00:00Z
source_status: Reviewed
status: Draft
total_requirements: 4
covered_requirements: 4
total_conditions: 5
---

# Test Conditions: 42001 - User login

## Test Conditions

### REQ-1 - Login email format

| ID | Test condition | Input / Setup | Expected result | Technique | Source quote |
|---|---|---|---|---|---|
| TC-REQ-1.1 | Accept an email in the specified format | Login form displayed; enter `user@example.com` | The email address is accepted | EP | "accepts an email address in the format `local-part@domain`" |

### REQ-2 - Password minimum-length rejection

| ID | Test condition | Input / Setup | Expected result | Technique | Source quote |
|---|---|---|---|---|---|
| TC-REQ-2.1 | Reject a password immediately below the 8-character minimum | Login form displayed; enter the 7-character password `Pass123` | The password is rejected | BVA | "A password containing fewer than 8 characters is rejected" |
| TC-REQ-2.2 | Display the specified message for a password shorter than 8 characters | Login form displayed; enter the 7-character password `Pass123` | The message `Password must contain at least 8 characters` is displayed | BVA | "with the message `Password must contain at least 8 characters`" |

### REQ-3 - Successful login destination

| ID | Test condition | Input / Setup | Expected result | Technique | Source quote |
|---|---|---|---|---|---|
| TC-REQ-3.1 | Navigate after successful authentication | Submit credentials that authenticate successfully | The Dashboard page is displayed | Scenario | "the user is redirected to the Dashboard page" |

### REQ-4 - Password visibility

| ID | Test condition | Input / Setup | Expected result | Technique | Source quote |
|---|---|---|---|---|---|
| TC-REQ-4.1 | Display entered password characters | Enter a password while characters are hidden; activate the password visibility control | The entered password characters are visible | Scenario | "displays the password characters" |

## Coverage Summary

| Requirement | Analysis status | Condition IDs | Coverage |
|---|---|---|---|
| REQ-1 | Clear | TC-REQ-1.1 | Covered |
| REQ-2 | Clear | TC-REQ-2.1, TC-REQ-2.2 | Covered |
| REQ-3 | Clear | TC-REQ-3.1 | Covered |
| REQ-4 | Clear | TC-REQ-4.1 | Covered |
```

## Patterns demonstrated

- Every condition is supported by one reviewed requirement.
- Each expected result contains one independently observable pass criterion.
- `REQ-2` produces two conditions because rejection and message display are separate outcomes explicitly established by the source.
- BVA is used only on the explicitly stated 8-character boundary.
- No exact route, unsupported input class, error behavior, or reverse toggle behavior is invented.
- The output uses the same filenames, metadata values, statuses, IDs, and columns defined in `SKILL.md`.

## Validation checklist

- [ ] Input filename is `requirements_{ISSUEKEY}.md`
- [ ] Input was generated by `qa-requirements-analysis`
- [ ] Source status permits processing
- [ ] Every condition traces to an exact requirement phrase
- [ ] Every expected result is objective
- [ ] Technique applicability is demonstrated
- [ ] No unsupported negative or error path is added
- [ ] Every requirement appears in the coverage summary
- [ ] IDs and frontmatter counts are correct
