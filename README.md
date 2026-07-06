# verify.axeumai.com

Public receipt verification proxy for Axeum Technologies.

Routes `verify.axeumai.com/{receipt_id}` → axeumOS public verification API.

**Endpoints:**
- `GET /RCP-2026-166684` — verify a specific receipt
- `GET /{receipt_id}` — verify any receipt
- `GET /key/yubihsm-0x4afb` — get the RSA-4096 public signing key
- `POST /offline` — offline verification (supply hash + signature)
