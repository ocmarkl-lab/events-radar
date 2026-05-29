# Events Radar — project notes

Single-file site: `index.html`. The live webpage is served from the `main` branch.

## Workflow: adding events

When Oliver adds a new event and approves it (i.e. confirms the card looks right):

1. Commit the change to the active feature branch.
2. **Automatically merge that branch into `main` and push** — no need to ask again.
   This is standing, durable permission to publish approved events to `main` (the live site).
3. Report the merge/push result.

"Approved" means Oliver has explicitly confirmed the event card (e.g. "looks good",
"push it", "ship it", "yes"). If the event has only been drafted but not yet approved,
keep it on the feature branch and wait for approval before touching `main`.

When updating an event, also keep these in sync in `index.html`:
- Header stats: events tracked, priority (score 3) count, geo split.
- The relevant month-section `.count`.
- Footer "N events tracked".
- The "Updated DD Mon YYYY" date in the subtitle.
