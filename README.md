# CW Institution Pitch Checklist

Interactive single-page checklist for pitching Charity Week to institutions.
Deploys to Cloudflare Workers as static assets, exactly like `cweventschecklist`.

## Structure

```
cwpitchchecklist/
  public/
    index.html      <- the checklist (served at /)
  wrangler.toml     <- points wrangler at ./public
  .gitignore
  README.md
```

## Deploy (same flow as your events checklist)

### 1. Put these files in a new GitHub repo

From inside this folder:

```bash
git init
git add .
git commit -m "CW institution pitch checklist"
git branch -M main
git remote add origin https://github.com/sha1sheikh/cwpitchchecklist.git
git push -u origin main
```

Create the empty `cwpitchchecklist` repo on GitHub first (personal account `sha1sheikh`).

**Two-account gotcha (the one that blocked you last time):** Windows Credential Manager
may hand over the work account (`ShawonSheikh-acc`) and reject the push. If you get a
permission error, force this repo to use the personal identity:

```bash
git config user.name "sha1sheikh"
git config user.email "<your-personal-email>"
git remote set-url origin https://sha1sheikh@github.com/sha1sheikh/cwpitchchecklist.git
```

Then clear the cached GitHub entry in Windows Credential Manager and push again,
entering the `sha1sheikh` credentials (use a Personal Access Token as the password).

### 2. Connect the repo to Cloudflare

Cloudflare dashboard -> Workers & Pages -> Create -> Pages -> Connect to Git ->
pick `cwpitchchecklist` -> deploy.

If Cloudflare runs `npx wrangler deploy` (Workers build, like last time), the
`wrangler.toml` here already does the work: it points at `./public` so wrangler
finds the static files. No "could not detect a directory containing static files"
error this time.

The `name = "cwpitchchecklist"` in `wrangler.toml` becomes the worker name, so the
URL lands at:

```
cwpitchchecklist.shawon-sheikh247.workers.dev
```

Keep `name` unique so it doesn't overwrite `cweventschecklist`.

### 3. Future updates

Edit `public/index.html`, then:

```bash
git add .
git commit -m "update checklist"
git push
```

Cloudflare redeploys automatically.

## Optional: deploy straight from your machine (skip the dashboard)

```bash
npm install -g wrangler
wrangler login
wrangler deploy
```

## Notes

- Tick state saves per-device via localStorage. Each rep keeps their own progress.
- "Reset" clears all ticks for the next pitch.
- Works offline once loaded; prints cleanly; responsive on mobile.
