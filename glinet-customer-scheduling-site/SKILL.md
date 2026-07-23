---
name: glinet-customer-scheduling-site
description: Maintain and extend the GL.iNET customer-service scheduling website and its embedded monthly scheduler. Use when Codex needs to update the landing page, adjust the scheduler UI or shift rules, keep `智能客服排班-V1.html` and `public/scheduler.html` in sync, modify shared-save behavior, or prepare the site for deployment.
---

# GL.iNET Customer Scheduling Site

## Overview

Use this skill to work safely on the GL.iNET customer-service scheduling site in this repository.
Treat the project as two connected surfaces: the marketing shell in `app/` and the actual scheduler engine in `智能客服排班-V1.html`, which must also stay synced to `public/scheduler.html`.

## Quick Start

1. Read `references/project-map.md` before changing structure, data flow, or deployment-sensitive behavior.
2. For scheduler-rule changes, edit `智能客服排班-V1.html` first, then mirror the same content to `public/scheduler.html`.
3. For landing-page or embedded-frame changes, edit `app/page.tsx` and `app/globals.css`.
4. For shared save, clear, or refresh behavior, inspect `app/api/current-schedule/route.ts` and `db/schema.ts`.
5. Before shipping, verify the generated page still loads the scheduler iframe correctly and that manual schedule edits persist as expected.

## Core Workflow

### Update scheduler rules or cell rendering

Edit `智能客服排班-V1.html` as the source of truth for:
- shift assignment logic
- rest-day handling
- summary pills
- manual tags such as `(Chat)`
- export, save, and table rendering behavior

After updating it, copy the same finalized HTML to `public/scheduler.html` so the deployed site and standalone scheduler stay aligned.

### Update the site shell

Use `app/page.tsx` for:
- top navigation
- hero copy
- rules or steps sections
- iframe placement and section anchors

Use `app/globals.css` for:
- brand colors
- typography
- compact card layout
- responsive behavior
- sticky navigation and section presentation

### Update shared collaboration state

The shared "save current schedule" feature is backed by:
- `app/api/current-schedule/route.ts`
- `db/schema.ts`
- `.openai/hosting.json`

Keep API payload shape and client-side persistence behavior compatible when changing save, refresh, or clear actions.

## Guardrails

- Preserve Chinese UI labels and scheduling semantics unless the request explicitly changes them.
- Do not change the scheduler by editing only one copy of the HTML.
- Keep manual overrides usable after generation; users expect dropdown edits to remain available in the final table.
- Treat exported PNG, Excel, and shared-save behaviors as user-facing features that need regression checking after UI edits.
- When changing summary text or badges, prefer hiding unwanted items instead of leaving stale counts visible.

## Key Files

- `app/page.tsx`: landing page and embedded scheduler frame
- `app/globals.css`: site-wide look and responsive styling
- `智能客服排班-V1.html`: scheduler source of truth
- `public/scheduler.html`: deployed scheduler copy
- `app/api/current-schedule/route.ts`: shared schedule read/write/clear API
- `db/schema.ts`: D1 table definition for saved schedule state
- `.openai/hosting.json`: Sites project binding and deploy metadata

## References

- Read `references/project-map.md` for the project structure, sync points, and expected edit order.
