# Deploying to Cloudflare

This site is deployed as a Cloudflare Worker (assets-only) named `the-isle-guide`,
configured in [wrangler.jsonc](wrangler.jsonc). It serves the repo root directly —
no build step.

Live URL: https://the-isle-guide.kuniokun84.workers.dev

## 1. Commit and push your changes

```bash
git add <files>
git commit -m "your message"
git push origin main
```

## 2. Log in to Cloudflare (first time only, or after the token expires)

```bash
npx --yes wrangler login
```

This opens a Cloudflare OAuth link in your browser. Approve it there — if the
link times out before you click it, just re-run the command to get a fresh one.

Verify you're logged in:

```bash
npx --yes wrangler whoami
```

Credentials are cached locally (`~/.wrangler/config/default.toml`), so you
normally only need to log in once per machine.

## 3. Deploy

```bash
npx --yes wrangler deploy
```

Wrangler uploads only the files that changed since the last deploy and prints
the live URL and a version ID when done.

## Notes

- `wrangler.jsonc`'s `name` must match the Worker name in the Cloudflare
  dashboard.
- There's no CI/CD — deploys are manual, run from a local checkout after
  pushing to `main`.
