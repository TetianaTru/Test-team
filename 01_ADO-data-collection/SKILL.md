---
name: ado-issue-context
description: Collect ADO work-item data on the local computer and build an Epic-to-Feature-to-User-Story hierarchy. Use when the user provides an ADO work-item URL or ID, or asks to import or read an Epic, Feature, or User Story from ADO.
recommended_model: sonnet
recommended_effort: medium
output_location: C:\Users\olena.kliushnyk\Kliushnyk-work\Projects\6.SteppingStone\AI\.github\outputs
---

# ADO Issue Context

Collect data from Azure DevOps (ADO) using read-only MCP operations and save it as structured Markdown files on the local computer.

## Goal

For a requested Epic, Feature, or User Story:

1. Retrieve the work item from ADO.
2. Extract fields defined in `ADO-field-maps.md` in this skill's folder.
3. Resolve the applicable Epic -> Feature -> User Story hierarchy.
4. Create or update the corresponding local hierarchy under `output_location`.
5. Preserve source wording and values without requirement analysis or grooming.

## Safety and data-source rules

- Use only read-only ADO MCP operations.
- Never create, update, link, comment on, or otherwise modify an ADO work item.
- Do not use a browser, web search, or another external source to supplement ADO data.
- Local filesystem tools may be used to read `ADO-field-maps.md` and create the requested folders and Markdown files.
- Do not invoke this skill recursively when fetching a parent or child work item.
- Do not invent missing values or hierarchy links.

## Supported input

The trigger must contain one of:

- A numeric ADO work-item ID, for example `188021`.
- An ADO URL containing a work-item ID, for example:
  `https://dev.azure.com/sartorius-bps/NG_ACT%20Stepping%20Stone/_workitems/edit/188021`

Supported primary work-item types:

- Epic
- Feature
- User Story

`ADO-field-maps.md` also contains mappings for Bug and Test Case. Those mappings may be used when collecting related Bugs or Test Cases, but a Bug or Test Case does not replace the Epic -> Feature -> User Story hierarchy.

## Read-only ADO operations

Use the available read-only ADO MCP work-item tools:

- Retrieve a work item: `azure-devops-wit_work_item` with action `get` and `expand: All`.
- Retrieve comments: `azure-devops-wit_work_item` with action `list_comments`.
- Retrieve an attachment only after user approval: `azure-devops-wit_work_item_attachment`.

If tool names differ in the active environment, use the equivalent read-only ADO work-item retrieval, comment retrieval, and attachment download operations.

## Field mapping rules

1. Read `ADO-field-maps.md` from this skill's folder before extracting data.
2. Determine the work-item type from `fields.System.WorkItemType`.
3. Use the matching mapping section.
4. Extract every populated mapped field, including optional fields.
5. If a required field is absent, record the field as `Not provided in ADO`.
6. Do not record absent optional fields.
7. Preserve exact numbers, names, labels, statuses, dates, URLs, and requirement wording.
8. Convert ADO HTML or XML to readable Markdown without paraphrasing its text.
9. Preserve list order and step order.
10. For identity fields, preserve both display name and email when available.
11. For relations, extract the related work-item ID from the relation URL and retain relation type and comment when present.

## Workflow

### Step 1: Parse the requested work-item ID

Extract the final numeric ID from the supplied URL or use the supplied numeric ID.

If no ID can be determined, stop and ask the user for the ADO work-item URL or ID.

### Step 2: Fetch and classify the requested work item

Fetch the requested work item directly using the read-only ADO MCP work-item retrieval tool with full field and relation expansion.

Then:

1. Read `fields.System.WorkItemType`.
2. Confirm that a matching section exists in `ADO-field-maps.md`.
3. Extract all populated fields listed in that section.
4. Retrieve comments with the read-only comment operation.
5. Identify attachments from attachment relations and embedded attachment URLs.
6. Do not download attachments yet.

Apply hierarchy processing according to the requested type:

- **User Story:** resolve its parent Feature, then resolve that Feature's parent Epic.
- **Feature:** resolve its parent Epic. Do not search for a parent Feature.
- **Epic:** do not search for a parent Feature or Epic.
- **Bug or Test Case:** collect it only when its placement under an existing User Story is known from ADO relations or supplied by the user.

### Step 3: Resolve the parent Feature for a User Story

Skip this step unless the requested work item is a User Story.

Resolve the parent in this order:

1. Read `fields.System.Parent`.
2. If unavailable, inspect `relations[rel="System.LinkTypes.Hierarchy-Reverse"]`.
3. Fetch the referenced parent with the read-only work-item retrieval tool.
4. Confirm that `fields.System.WorkItemType` is `Feature`.

If the referenced parent is not a Feature, do not assume a Feature. Ask:

> No parent Feature was detected for User Story `{ISSUEKEY}`. What is its Feature ID?

### Step 4: Fetch Feature details

If a Feature was requested directly or resolved in Step 3:

1. Fetch it directly with the read-only ADO work-item retrieval tool if it has not already been fetched.
2. Extract all populated Feature fields defined in `ADO-field-maps.md`.
3. Retrieve its comments.
4. Identify, but do not download, its attachments.
5. Do not invoke this skill recursively.

### Step 5: Resolve the parent Epic from the Feature

Skip this step when the requested work item is already an Epic.

Resolve the Epic from the Feature retrieved or identified in Steps 2-4:

1. Read the Feature's `fields.System.Parent`.
2. If unavailable, inspect the Feature's `relations[rel="System.LinkTypes.Hierarchy-Reverse"]`.
3. Fetch the referenced parent with the read-only work-item retrieval tool.
4. Confirm that `fields.System.WorkItemType` is `Epic`.

If no Epic is found or the referenced parent is not an Epic, do not assume an ID. Ask:

> No parent Epic was detected for Feature `{FEATURE-KEY}`. What is its Epic ID?

### Step 6: Fetch Epic details

If an Epic was requested directly or resolved in Step 5:

1. Fetch it directly with the read-only ADO work-item retrieval tool if it has not already been fetched.
2. Extract all populated Epic fields defined in `ADO-field-maps.md`.
3. Retrieve its comments.
4. Identify, but do not download, its attachments.
5. Do not invoke this skill recursively.

### Step 7: Handle attachments

Create an inventory of attachments before writing files. Include:

- File name
- Attachment URL
- Source work-item ID
- Location in the source, such as relation, description, acceptance criteria, reproduction steps, or test steps

If no attachments exist, continue without asking.

If attachments exist, ask one question:

> Attachments were found in the ADO work items. Should they be downloaded to the local output folders?

If the user declines:

- Do not download them.
- Keep their metadata and URLs in the corresponding Markdown file.

If the user approves:

- Download each attachment with the read-only attachment tool.
- Save it under an `attachments` folder beside the Markdown file for its source work item.
- Use the original filename when available.
- Reference the local relative path and source URL in the Markdown file.

### Step 8: Build the local folder structure

All paths below are relative to `output_location`.

For a complete User Story hierarchy:

```text
stories\
  {EPIC-KEY}\
    epic.md
    {FEATURE-KEY}\
      feature.md
      {ISSUEKEY}\
        user_story_{ISSUEKEY}.md
        attachments\
        test-cases\
        tasks\
```

For an Epic-only request:

```text
stories\
  {EPIC-KEY}\
    epic.md
    attachments\
```

For a Feature request:

```text
stories\
  {EPIC-KEY}\
    epic.md
    {FEATURE-KEY}\
      feature.md
      attachments\
```

Create `attachments` only when an attachment is downloaded. For a User Story, always create `test-cases` and `tasks`.

## Output rules

### Common rendering rules

- Use the field order from the applicable section of `ADO-field-maps.md`.
- Render scalar fields in a `## Fields` table.
- Render long-form content, parsed HTML sections, relations, comments, and attachments as separate sections.
- Do not place large HTML/XML values directly in the fields table.
- Use `Not provided in ADO` only for missing required fields.
- Write `No comments.` when the comment count is zero.
- If the reported comment count differs from retrieved comments, record both values without inventing missing comments.
- Write `No acceptance criteria specified.` when a User Story has no acceptance criteria.
- Preserve each comment's author, creation date, and text.
- Preserve raw source URLs.

### `epic.md` template

```markdown
# Epic {EPIC-KEY}: {TITLE}

## Fields
| Field Name | Value |
|---|---|
| Work Item ID | ... |

## Description
...

## Extracted Description Sections
### Business Value
...
### End-to-End Flow
...
### In-Scope Items
...
### Out-of-Scope Items
...
### Open Questions
...

## Features
| ID | Title | State | Local File |
|---|---|---|---|
| ... | ... | ... | ... |

## Relations
...

## Comments
...

## Attachments
...
```

Omit an optional extracted section when it is not present in ADO.

### `feature.md` template

```markdown
# Feature {FEATURE-KEY}: {TITLE}

## Fields
| Field Name | Value |
|---|---|
| Work Item ID | ... |

## Description
...

## Extracted Description Sections
### Problem Statement
...
### Feature Scope and Overview
...
### In-Scope Items
...
### Out-of-Scope Items
...
### Success Criteria
...

## User Stories
| ID | Title | State | Local File |
|---|---|---|---|
| ... | ... | ... | ... |

## Relations
...

## Comments
...

## Attachments
...
```

Omit an optional extracted section when it is not present in ADO.

### `user_story_{ISSUEKEY}.md` template

```markdown
# User Story {ISSUEKEY}: {TITLE}

## Fields
| Field Name | Value |
|---|---|
| Work Item ID | ... |

## Description
...

## Acceptance Criteria
...

## Polarion Requirement Links
...

## Figma Design Link
...

## Relations
...

## Comments
...

## Attachments
...
```

Omit optional sections when they are not present in ADO.

## Existing-file behavior

- Never overwrite a pre-existing file without explicit user approval.
- If `epic.md` exists and the Feature row is missing, append only that Feature row.
- If `feature.md` exists and the User Story row is missing, append only that User Story row.
- Do not append a duplicate table row.
- If a required parent file exists, read it to determine whether the row is already present.
- If `user_story_{ISSUEKEY}.md` exists, stop and report:
  `user_story_{ISSUEKEY}.md already exists`.
- If an existing file has no expected Features or User Stories table, stop and report that it cannot be updated safely without user approval.

## Deduplication without changing source meaning

Deduplication removes only exact duplicate text:

- If identical text appears in Description and a comment, keep it in Description and add a note in the comment entry: `Duplicate of Description; text omitted.`
- If identical acceptance-criteria text appears in both the dedicated field and Description, keep it under Acceptance Criteria and omit only the exact duplicate.
- Do not merge similar but non-identical requirements.
- Do not rewrite, summarize, or choose one wording over another.
- Keep comment authors and dates even when duplicate comment text is omitted.

## Error handling

| Situation | Action |
|---|---|
| Work-item ID is missing | Ask for an ADO URL or numeric ID |
| Work item is not found | Report the ID and ask the user to verify it and their access |
| ADO MCP is unavailable | Report: `ADO MCP not available - check credentials and ADO configuration.` |
| Work-item type has no field map | Report the type and stop |
| Parent type does not match the expected hierarchy | Report the actual type and ask for the correct parent ID |
| Required field is absent | Write `Not provided in ADO` |
| No comments exist | Write `No comments.` |
| No acceptance criteria exist | Write `No acceptance criteria specified.` |
| Attachment cannot be downloaded | Preserve its metadata and source URL; report the download error |
| Existing file cannot be updated safely | Stop and request user approval before overwriting or restructuring |

## Verification

Before completing:

1. Re-read every created or modified Markdown file.
2. Compare each populated ADO field with the applicable `ADO-field-maps.md` section.
3. Confirm every extracted value appears exactly once, except where a hierarchy table intentionally references it.
4. Confirm descriptions and acceptance criteria are complete and not truncated.
5. Confirm relation IDs and types match the ADO response.
6. Confirm retrieved comment count is documented.
7. Confirm every identified attachment is documented and downloads match the user's choice.
8. Confirm no write operation was performed in ADO.

## Quality checklist

- [ ] Requested work-item ID and type confirmed
- [ ] Correct field-map section used
- [ ] Applicable hierarchy resolved and type-checked
- [ ] Epic folder and `epic.md` created or safely updated
- [ ] Feature folder and `feature.md` created or safely updated when applicable
- [ ] User Story folder and `user_story_{ISSUEKEY}.md` created when applicable
- [ ] `test-cases` and `tasks` folders created for a User Story
- [ ] Description captured in full
- [ ] Acceptance criteria captured in full when applicable
- [ ] Comments retrieved and documented
- [ ] Attachments inventoried and handled according to user choice
- [ ] Exact duplicates removed without paraphrasing
- [ ] Created and modified files verified against ADO field mappings
- [ ] No ADO data changed
