# Field Maps
Type-specific field mappings for ADO data extraction.
---
## User Story
| Field Name | ADO Path | Required |
|---|---|---|
| Work Item ID | `id` | Yes |
| Work Item URL | `_links.html.href` | Yes |
| Title | `fields.System.Title` | Yes |
| Work Item Type | `fields.System.WorkItemType` | Yes |
| State | `fields.System.State` | Yes |
| Reason | `fields.System.Reason` | No |
| Description / User Story | `fields.System.Description` | Yes |
| Acceptance Criteria | `fields.Microsoft.VSTS.Common.AcceptanceCriteria` | Yes |
| Area Path | `fields.System.AreaPath` | Yes |
| Iteration Path | `fields.System.IterationPath` | Yes |
| Assigned To | `fields.System.AssignedTo.displayName` | No |
| Assigned To Email | `fields.System.AssignedTo.uniqueName` | No |
| Story Points | `fields.Microsoft.VSTS.Scheduling.StoryPoints` | No |
| Value Area | `fields.Microsoft.VSTS.Common.ValueArea` | No |
| Parent Work Item ID | `fields.System.Parent` | No |
| Approved By | `fields.Custom.ApprovedBy.displayName` | No |
| Created Date | `fields.System.CreatedDate` | Yes |
| Created By | `fields.System.CreatedBy.displayName` | No |
| Changed Date | `fields.System.ChangedDate` | Yes |
| Changed By | `fields.System.ChangedBy.displayName` | No |
| Revision | `fields.System.Rev` | No |
| Comment Count | `fields.System.CommentCount` | No |
| Board Column | `fields.System.BoardColumn` | No |
| Child Work Items | `relations[rel="System.LinkTypes.Hierarchy-Forward"]` | No |
| Related Work Items | `relations[rel="System.LinkTypes.Related"]` | No |
| Parent Relation | `relations[rel="System.LinkTypes.Hierarchy-Reverse"]` | No |
| Successor Work Items | `relations[rel="System.LinkTypes.Dependency-Forward"]` | No |
| Linked Pull Requests | `relations[rel="ArtifactLink"][attributes.name="Pull Request"]` | No |
| Polarion Requirement Links | `fields.System.Description` *(parsed from HTML links)* | No |
| Figma Design Link | `fields.Microsoft.VSTS.Common.AcceptanceCriteria` *(parsed from HTML link)* | No |

---
## Feature
| Field Name | ADO Path | Required |
|---|---|---|
| Work Item ID | `id` | Yes |
| Work Item URL | `_links.html.href` | Yes |
| Title | `fields.System.Title` | Yes |
| Work Item Type | `fields.System.WorkItemType` | Yes |
| State | `fields.System.State` | Yes |
| Reason | `fields.System.Reason` | No |
| Description / Feature Details | `fields.System.Description` | Yes |
| Area Path | `fields.System.AreaPath` | Yes |
| Iteration Path | `fields.System.IterationPath` | Yes |
| Assigned To | `fields.System.AssignedTo.displayName` | No |
| Assigned To Email | `fields.System.AssignedTo.uniqueName` | No |
| Start Date | `fields.Microsoft.VSTS.Scheduling.StartDate` | No |
| Target Date | `fields.Microsoft.VSTS.Scheduling.TargetDate` | No |
| Effort | `fields.Microsoft.VSTS.Scheduling.Effort` | No |
| Priority | `fields.Microsoft.VSTS.Common.Priority` | No |
| Value Area | `fields.Microsoft.VSTS.Common.ValueArea` | No |
| Tags | `fields.System.Tags` | No |
| Parent Work Item ID | `fields.System.Parent` | No |
| Created Date | `fields.System.CreatedDate` | Yes |
| Created By | `fields.System.CreatedBy.displayName` | No |
| Changed Date | `fields.System.ChangedDate` | Yes |
| Changed By | `fields.System.ChangedBy.displayName` | No |
| Revision | `fields.System.Rev` | No |
| Comment Count | `fields.System.CommentCount` | No |
| Board Column | `fields.System.BoardColumn` | No |
| Child Work Items | `relations[rel="System.LinkTypes.Hierarchy-Forward"]` | No |
| Parent Relation | `relations[rel="System.LinkTypes.Hierarchy-Reverse"]` | No |
| Problem Statement | `fields.System.Description` *(parsed from HTML section)* | No |
| Feature Scope and Overview | `fields.System.Description` *(parsed from HTML section)* | No |
| In-Scope Items | `fields.System.Description` *(parsed from HTML section)* | No |
| Out-of-Scope Items | `fields.System.Description` *(parsed from HTML section)* | No |
| Success Criteria | `fields.System.Description` *(parsed from HTML section)* | No |
| Polarion Requirement Links | `fields.System.Description` *(parsed from HTML links)* | No |

---
## Epic
| Field Name | ADO Path | Required |
|---|---|---|
| Work Item ID | `id` | Yes |
| Work Item URL | `_links.html.href` | Yes |
| Title | `fields.System.Title` | Yes |
| Work Item Type | `fields.System.WorkItemType` | Yes |
| State | `fields.System.State` | Yes |
| Reason | `fields.System.Reason` | No |
| Description / Epic Details | `fields.System.Description` | Yes |
| Area Path | `fields.System.AreaPath` | Yes |
| Iteration Path | `fields.System.IterationPath` | Yes |
| Assigned To | `fields.System.AssignedTo.displayName` | No |
| Assigned To Email | `fields.System.AssignedTo.uniqueName` | No |
| Start Date | `fields.Microsoft.VSTS.Scheduling.StartDate` | No |
| Target Date | `fields.Microsoft.VSTS.Scheduling.TargetDate` | No |
| Effort | `fields.Microsoft.VSTS.Scheduling.Effort` | No |
| Priority | `fields.Microsoft.VSTS.Common.Priority` | No |
| Value Area | `fields.Microsoft.VSTS.Common.ValueArea` | No |
| Tags | `fields.System.Tags` | No |
| Parent Work Item ID | `fields.System.Parent` | No |
| Created Date | `fields.System.CreatedDate` | Yes |
| Created By | `fields.System.CreatedBy.displayName` | No |
| Changed Date | `fields.System.ChangedDate` | Yes |
| Changed By | `fields.System.ChangedBy.displayName` | No |
| Revision | `fields.System.Rev` | No |
| Comment Count | `fields.System.CommentCount` | No |
| Board Column | `fields.System.BoardColumn` | No |
| Child Work Items | `relations[rel="System.LinkTypes.Hierarchy-Forward"]` | No |
| Related Work Items | `relations[rel="System.LinkTypes.Related"]` | No |
| Parent Relation | `relations[rel="System.LinkTypes.Hierarchy-Reverse"]` | No |
| Business Value | `fields.System.Description` *(parsed from the Value HTML section)* | No |
| End-to-End Flow | `fields.System.Description` *(parsed from HTML section)* | No |
| In-Scope Items | `fields.System.Description` *(parsed from HTML section)* | No |
| Out-of-Scope Items | `fields.System.Description` *(parsed from HTML section)* | No |
| Open Questions | `fields.System.Description` *(parsed from HTML section)* | No |

---
## Bug
| Field Name | ADO Path | Required |
|---|---|---|
| Work Item ID | `id` | Yes |
| Work Item URL | `_links.html.href` | Yes |
| Title | `fields.System.Title` | Yes |
| Work Item Type | `fields.System.WorkItemType` | Yes |
| State | `fields.System.State` | Yes |
| Reason | `fields.System.Reason` | No |
| Reproduction Steps / Bug Details | `fields.Microsoft.VSTS.TCM.ReproSteps` | Yes |
| Area Path | `fields.System.AreaPath` | Yes |
| Iteration Path | `fields.System.IterationPath` | Yes |
| Assigned To | `fields.System.AssignedTo.displayName` | No |
| Assigned To Email | `fields.System.AssignedTo.uniqueName` | No |
| Priority | `fields.Microsoft.VSTS.Common.Priority` | No |
| Severity | `fields.Microsoft.VSTS.Common.Severity` | No |
| Value Area | `fields.Microsoft.VSTS.Common.ValueArea` | No |
| Found In Build | `fields.Microsoft.VSTS.Build.FoundIn` | No |
| Integration Build | `fields.Microsoft.VSTS.Build.IntegrationBuild` | No |
| Back-End Version | `fields.Custom.BEVersion` | No |
| Front-End Version | `fields.Custom.FEVersion` | No |
| WiX Version | `fields.Custom.WIXVersion` | No |
| Approved By | `fields.Custom.ApprovedBy.displayName` | No |
| Verified By | `fields.Custom.VerifiedBy.displayName` | No |
| Tags | `fields.System.Tags` | No |
| Created Date | `fields.System.CreatedDate` | Yes |
| Created By | `fields.System.CreatedBy.displayName` | No |
| Changed Date | `fields.System.ChangedDate` | Yes |
| Changed By | `fields.System.ChangedBy.displayName` | No |
| Activated Date | `fields.Microsoft.VSTS.Common.ActivatedDate` | No |
| Activated By | `fields.Microsoft.VSTS.Common.ActivatedBy.displayName` | No |
| Resolved Date | `fields.Microsoft.VSTS.Common.ResolvedDate` | No |
| Resolved By | `fields.Microsoft.VSTS.Common.ResolvedBy.displayName` | No |
| Closed Date | `fields.Microsoft.VSTS.Common.ClosedDate` | No |
| Closed By | `fields.Microsoft.VSTS.Common.ClosedBy.displayName` | No |
| Revision | `fields.System.Rev` | No |
| Comment Count | `fields.System.CommentCount` | No |
| Board Column | `fields.System.BoardColumn` | No |
| Board Lane | `fields.WEF_56E763701226443EB48C5213D45FB460_Kanban.Lane` | No |
| Child Work Items | `relations[rel="System.LinkTypes.Hierarchy-Forward"]` | No |
| Related Work Items | `relations[rel="System.LinkTypes.Related"]` | No |
| Test Cases | `relations[rel="Microsoft.VSTS.Common.TestedBy-Forward"]` | No |
| Fixed-in Commits | `relations[rel="ArtifactLink"][attributes.name="Fixed in Commit"]` | No |
| Linked Pull Requests | `relations[rel="ArtifactLink"][attributes.name="Pull Request"]` | No |
| Integrated Builds | `relations[rel="ArtifactLink"][attributes.name="Integrated in build"]` | No |
| Steps to Reproduce | `fields.Microsoft.VSTS.TCM.ReproSteps` *(parsed from HTML section)* | No |
| Actual Result | `fields.Microsoft.VSTS.TCM.ReproSteps` *(parsed from HTML section)* | No |
| Expected Result | `fields.Microsoft.VSTS.TCM.ReproSteps` *(parsed from HTML section)* | No |
| Logs | `fields.Microsoft.VSTS.TCM.ReproSteps` *(parsed from HTML content)* | No |
| Embedded Attachments | `fields.Microsoft.VSTS.TCM.ReproSteps` *(parsed from HTML image URLs)* | No |

---
## Test Case
| Field Name | ADO Path | Required |
|---|---|---|
| Work Item ID | `id` | Yes |
| Work Item URL | `_links.html.href` | Yes |
| Title | `fields.System.Title` | Yes |
| Work Item Type | `fields.System.WorkItemType` | Yes |
| State | `fields.System.State` | Yes |
| Reason | `fields.System.Reason` | No |
| Test Steps | `fields.Microsoft.VSTS.TCM.Steps` | Yes |
| Area Path | `fields.System.AreaPath` | Yes |
| Iteration Path | `fields.System.IterationPath` | Yes |
| Assigned To | `fields.System.AssignedTo.displayName` | No |
| Assigned To Email | `fields.System.AssignedTo.uniqueName` | No |
| Priority | `fields.Microsoft.VSTS.Common.Priority` | No |
| Automation Status | `fields.Microsoft.VSTS.TCM.AutomationStatus` | No |
| Created Date | `fields.System.CreatedDate` | Yes |
| Created By | `fields.System.CreatedBy.displayName` | No |
| Changed Date | `fields.System.ChangedDate` | Yes |
| Changed By | `fields.System.ChangedBy.displayName` | No |
| State Change Date | `fields.Microsoft.VSTS.Common.StateChangeDate` | No |
| Revision | `fields.System.Rev` | No |
| Comment Count | `fields.System.CommentCount` | No |
| Related Work Items | `relations[rel="System.LinkTypes.Related"]` | No |
| Tested Work Items / Bugs | `relations[rel="Microsoft.VSTS.Common.TestedBy-Reverse"]` | No |
| Preconditions | `fields.Microsoft.VSTS.TCM.Steps` *(parsed from the steps XML)* | No |
| Step ID | `fields.Microsoft.VSTS.TCM.Steps.steps.step[].@id` | No |
| Step Type | `fields.Microsoft.VSTS.TCM.Steps.steps.step[].@type` | No |
| Test Action | `fields.Microsoft.VSTS.TCM.Steps.steps.step[].parameterizedString[0]` | No |
| Expected Result | `fields.Microsoft.VSTS.TCM.Steps.steps.step[].parameterizedString[1]` | No |
| Step Description | `fields.Microsoft.VSTS.TCM.Steps.steps.step[].description` | No |
