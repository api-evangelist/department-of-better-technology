---
name: Build a Screendoor form
description: Create a Screendoor project and assemble its intake form field by field.
api: openapi/department-of-better-technology-screendoor-openapi.yml
operations: [createProject, listForms, createFormField, updateForm]
---

# Build a Screendoor form

Use this skill to stand up a new intake form in Screendoor.

## Prerequisites

- A Screendoor API key. Pass it on every request as the `api_key` URL parameter
  (Screendoor -> Settings -> API Keys). Base URL: `https://screendoor.dobt.co/api`.
- The `site_id` you are creating the project under.

## Steps

1. **Create the project** with `createProject` (`POST /sites/{site_id}/projects`), sending
   `name` and optional `description`. Keep the returned `id` as `project_id`.
2. **Find the initial form** with `listForms` (`GET /projects/{project_id}/forms`). The first
   form is the project's initial form.
3. **Add fields** with `createFormField` (`POST /projects/{project_id}/form/fields`), sending a
   `field_data` object (`field_type` such as `paragraph`, `text`, `checkboxes`, `address`,
   `file`; plus a `label`) and an optional zero-based `position`. Repeat per field.
4. **Bulk-edit the form** (optional) with `updateForm` (`PUT /projects/{project_id}/forms/{id}`)
   to replace the whole `field_data` array at once.

## Rules

- Errors come back as `{ "error": "..." }`; a `422` adds `{ "errors": { "<field>": [...] } }`.
- There is no idempotency key — do not blindly retry a `createProject`/`createFormField` that
  may have partially succeeded; list first and reconcile.
