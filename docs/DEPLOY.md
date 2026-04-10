# mg-calculator — Deployment

## Platform

**GitHub Pages** — served directly from the `main` branch of `Moneygeneratingmachine369/mg-calculator`. Auto-deploys on push.

## Deploy URL

`https://moneygeneratingmachine369.github.io/mg-calculator/` (or custom domain if configured).

## How the deploy works

1. Push to `main`
2. GitHub Pages picks up `index.html` from the root of the repo
3. No build step — the file is served as-is
4. React, ReactDOM, and Google Fonts are loaded via CDN `<script>` / `<link>` tags in the `<head>`
5. Cache invalidation is controlled by GitHub Pages' default behaviour; a force-refresh in the browser usually picks up the latest version within a minute or two

## No environment variables

Unlike `mg-submit`, this repo has no secrets. The only hardcoded external URL is the submission endpoint:
```
https://mg-submit-production.up.railway.app/submit
```
If the backend ever moves, search `index.html` for this URL and update it.

## Custom domain setup (if not already done)

1. Add a `CNAME` file at the repo root containing the custom domain (e.g. `calc.myrmexgroup.co.uk`)
2. Configure DNS: add a `CNAME` record pointing `calc` to `moneygeneratingmachine369.github.io`
3. In GitHub → Settings → Pages → set the custom domain

## Rollback

`git revert` the bad commit and push. GitHub Pages will re-deploy within a minute.

## Governance

This service is customer-facing. Any change to the visible UI, cost logic, or reference number format should be logged in `MG_SYS_020_Systems_Automations_Register` so downstream systems (mg-submit, Make scenario, Sheet layout) stay aligned.
