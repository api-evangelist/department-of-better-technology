---
name: Assign and review a submission
description: Route a Screendoor response to reviewers and record the review outcome.
api: openapi/department-of-better-technology-screendoor-openapi.yml
operations: [getResponse, listResponseAssignments, addResponseAssignments, updateResponse]
---

# Assign and review a submission

Use this skill to route an individual submission to reviewers and record the outcome.

## Prerequisites

- A Screendoor API key passed as the `api_key` URL parameter.
- The `response_id` of the submission and the `project_id` it belongs to.

## Steps

1. **Load full detail** with `getResponse` (`GET /projects/{project_id}/responses/{response_id}`)
   using `response_format=detailed` to include current assignees and archived versions.
2. **See who is assigned** with `listResponseAssignments` (`GET /responses/{response_id}/assignments`).
3. **Assign reviewers** with `addResponseAssignments` (`POST /responses/{response_id}/assignments`),
   sending `assignees` as an array of `"Type,id"` strings (e.g. `"User,10"` or `"Team,3"`).
   Use `replaceResponseAssignments` (`PUT`) to set the full assignee list at once.
4. **Record the outcome** with `updateResponse`
   (`PUT /projects/{project_id}/responses/{response_id}`), advancing `status` and applying
   `labels` that capture the review decision.

## Rules

- Assignee entries must be the exact `"Type,id"` form; `Type` is `User` or `Team`.
- All calls are api-key authenticated; a `401` means the `api_key` is missing or invalid,
  a `404` means the response or project id is wrong.
