# verify.axeumai.com

Public receipt verification proxy for Axeum Technologies.

Routes `verify.axeumai.com/{receipt_id}` → axeumOS public verification API.

**Endpoints:**
- `GET /RCP-mt05uoxo-6340c2af44b9f5ed` — verify a specific receipt
- `GET /{receipt_id}` — verify any receipt
- `GET /key/yubihsm-0x4afb` — get the RSA-4096 public signing key
- `POST /offline` — offline verification (supply hash + signature)

## Deploying

Push to `master`. Vercel builds from GitHub and aliases `verify.axeumai.com`.

This was **not** true until 2026-08-19. The Vercel project had no git link, so
every deploy was a manual `vercel --prod` upload. Because the CLI stamps the
current commit SHA onto a deployment, the dashboard showed real commit hashes
and looked git-connected — which is how commit `886710b` sat committed and
un-deployed for 25 days, then surfaced unannounced the next time someone
deployed by hand.

If a change is on `master` but not on the site, check the project is still
linked before re-deploying by hand:

```bash
npx vercel project inspect axeum-verify   # expect a Git repository section
```

Falling back to `npx vercel --prod` hides the problem rather than fixing it.

## Editing the verification UI

`public/index.html` and `public/seal.html` are self-contained (inline CSS/JS).
Two things there are correctness constraints, not styling:

- **Never hardcode a check count.** Read it from the API response. `null` means
  a check did not run and is never a pass. The page previously printed
  `6/6 checks` for every receipt including failing ones.
- **QR sizing is a scannability constraint.** The bitmap is oversampled to
  296px and displayed at 74px of content — exactly 2px per module. Rendering at
  display size produced a QR that a decoder could not read at all. The padding
  behind the canvas must stay the QR's light colour; a dark ring inside the
  quiet zone stops phone cameras locking on.
