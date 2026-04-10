# mg-calculator

Customer-facing cost calculator for the Myrmex Group renovation lead flow. Single-page React SPA (React via CDN, no build step). Generates an indicative project cost range from user inputs, then captures name + email and POSTs to `mg-submit` for PDF generation, email delivery, and sheet logging.

Part of the Myrmex systems stack. Registered (pending) in `MG_SYS_020_Systems_Automations_Register` as part of `MYX-AUTO-007_Cost_Calculator_Submit`.

## Repo contents

Single file: `index.html` — contains the React app, all styles, all calculator logic, and the submission UI.

## Key functions inside `index.html`

| Function | Purpose |
|---|---|
| `recalc(S)` | Main cost engine — takes the state `S` and produces section totals, contingency, grand total |
| `calcDetailedBreakdown(S, est)` | Expands the cost engine output into the detailed report object (sections with line items, programme, variance drivers). Also attaches `est.ref` via `generateRef(S)`. |
| `calcTimeline(S)` | Programme / timeline builder — produces `preConstruction`, `construction`, `dlp`, `grandTotal` blocks with week ranges |
| `generateRef(S)` | Reference number generator — returns `MG-[SS][Z][TTTT]` (see Reference Protocol below) |
| `Calculator()` | Top-level React component — orchestrates the multi-step form UI and the submission flow |

## Submission flow

1. User completes all calculator sections
2. Clicks submit → opens contact modal
3. User enters name + email, clicks send
4. Frontend POSTs to `https://mg-submit-production.up.railway.app/submit` with `{ref, name, email, state: S, est}`
5. On success: shows confirmation card with ref, then auto-redirects to `https://myrmexgroup.co.uk` after 3 seconds
6. On failure: shows error card with fallback contact email

## Reference number protocol

Format: `MG-[SS][Z][TTTT]`

- `SS` — scope: `FR` / `PR` / `KB` / `SO` / `XX`
- `Z` — zone: `1` / `2` / `3` / `4` / `0` (unknown)
- `TTTT` — 4-char unique: 2 chars timestamp (monotonic per ms) + 2 chars crypto-random

Two refs generated ≥1 ms apart cannot collide. See `generateRef()` in `index.html` for the canonical implementation.

## Governance

- Visual design is governed by the website design direction brief (separate from the PDF's brand protocol). The calculator is on the website medium, so **near-black backgrounds, Orbitron, and glass panels ARE allowed here** — per `MG_MKT_010 §6` (Cross-Medium Rules), these are reserved for website use and prohibited in client documents.
- Any change to `generateRef()` must be coordinated with the backend validator in `mg-submit/index.js` which currently accepts `^MG-[A-Z0-9]{6,7}$`.

## Related docs

- `DEPLOY.md` — GitHub Pages deployment
