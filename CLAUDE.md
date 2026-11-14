# evaluating-claude

## What this is

A short field guide to telling whether an AI deployed in real work actually
works, and keeping that answer trustworthy as prompts and models change. The
advisory synthesis of four runnable write-ups that live in their own repos.

## Build, test, run

No code and no check script. The site is Jekyll served by GitHub Pages from docs/.

## Layout

- docs/: the published guide (index.md, _layouts/, _config.yml).
- README.md: repo overview and checklist.
- LICENSE: MIT.

## House rules

- All work lands on the session branch, one commit per phase. No pull request unless asked. Never force-push or rewrite history.
- No GitHub Actions or other CI automation. Use the local check script.
- User-facing text has no em-dashes and keeps a PG-clean tone.
- Never skip or disable a test to get a pass.
- Published results, run data and pre-registrations are a research record and are never overwritten. New runs go in new folders.

## Invariants

- docs/ is the published guide. Root files such as this one are not published.
- The shared stylesheet and fonts are served by the hub site at
  /assets/research-notes.css and /assets/fonts/ and must be linked by those
  exact paths (see docs/_layouts/default.html).
