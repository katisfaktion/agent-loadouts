---
"agent-loadouts": minor
---

- Move `nextjs-on-cloudflare` from `cloudflare/platform` into the opt-in `cloudflare/nextjs` group. The `vinext` choice now installs it for apps already on vinext, and the migration skill moves to a new `vinext-migration` choice. Projects that selected `cloudflare/nextjs: [vinext]` for the migration should switch to `vinext-migration`.
