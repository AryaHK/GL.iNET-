# Project Map

## Repository role

This repository contains the live GL.iNET customer-service scheduling website and the embedded monthly scheduler used by the team.

## Main surfaces

### 1. Landing page

- `app/page.tsx`
- `app/globals.css`

Use these files for the outer website experience: brand presentation, hero area, navigation, steps/rules panels, and the scheduler iframe container.

### 2. Scheduler engine

- `智能客服排班-V1.html`
- `public/scheduler.html`

The single-file scheduler contains the actual roster generation logic, manual cell editing controls, exports, summary pills, and scheduling rules.

`智能客服排班-V1.html` is the editable source of truth.
`public/scheduler.html` is the copy served by the site.
Keep them synchronized after every scheduler change.

### 3. Shared save backend

- `app/api/current-schedule/route.ts`
- `db/schema.ts`
- `.openai/hosting.json`

These files support the shared "save current schedule / refresh latest schedule / clear current schedule" workflow.

## Change patterns

### If the request is visual branding

Start with:
- `app/page.tsx`
- `app/globals.css`

Then verify the scheduler iframe still fits and remains readable.

### If the request is about scheduling rules or table behavior

Start with:
- `智能客服排班-V1.html`

Then copy the final result to:
- `public/scheduler.html`

### If the request is about saved state or collaboration

Inspect:
- `app/api/current-schedule/route.ts`
- `db/schema.ts`

Make sure the front-end state shape remains compatible with stored payloads.

## User-facing features that should not silently regress

- Green-and-white brand styling
- Sticky date/header behavior in the schedule table
- Manual cell dropdown editing
- Manual `(Chat)` tag rendering
- PNG / Excel export
- Shared save, refresh, and clear actions
- Legal-holiday summary text

## Deployment note

The live site is deployed through OpenAI Sites and uses the project id stored in `.openai/hosting.json`.
When preparing a release, ensure the repository state and any built archive reflect the same final source.
