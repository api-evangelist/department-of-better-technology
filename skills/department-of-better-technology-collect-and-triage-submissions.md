---
name: Collect and triage submissions
description: Create Screendoor responses, page through them, and triage with statuses and labels.
api: openapi/department-of-better-technology-screendoor-openapi.yml
operations: [createResponse, listResponses, updateResponse, addResponseLabels, createStatus]
---

# Collect and triage submissions

Use this skill to submit responses to a project's form and move them through a review workflow.

## Prerequisites

- A Screendoor API key passed as the `api_key` URL parameter.
- A `project_id` and knowledge of its form field ids (keys of the `response_fields` map).

## Steps

1. **Submit a response** with `createResponse` (`POST /projects/{project_id}/responses`),
   sending `response_fields` (a map of field id -> value). Optional flags: `skip_validation`,
   `skip_notifications`, `skip_email_confirmation`, plus an initial `status` and `labels`.
2. **List and page** with `listResponses` (`GET /projects/{project_id}/responses`). Use
   `page` and `per_page` (max 100); follow the `Link` response header (`next`/`prev`). Filter
   with `sort`, `direction`, `trash`, and `advanced_search`.
3. **Define workflow stages** (once per project) with `createStatus`
   (`POST /projects/{project_id}/statuses`).
4. **Advance a response** with `updateResponse` (`PUT /projects/{project_id}/responses/{response_id}`),
   setting `status` and/or `labels`. Use `force_validation` when you need validation enforced.
5. **Tag a response** with `addResponseLabels` (`POST /responses/{response_id}/labels`),
   sending `labels` (array of names).

## Rules

- Field values follow Screendoor's per-type encoding (checkboxes/radio use
  `{ "checked": [...] }`, date uses `{ "month","day","year" }`, price uses `{ "dollars","cents" }`).
- Deleting a response with `trashResponse` is recoverable via `recoverResponse`;
  `deleteResponseForever` is permanent.
