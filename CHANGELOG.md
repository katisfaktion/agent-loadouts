# agent-loadouts

## 0.3.0

### Minor Changes

- 238ebe2: - Move `nextjs-on-cloudflare` from `cloudflare/platform` into the opt-in `cloudflare/nextjs` group. The `vinext` choice now installs it for apps already on vinext, and the migration skill moves to a new `vinext-migration` choice. Projects that selected `cloudflare/nextjs: [vinext]` for the migration should switch to `vinext-migration`. Routing rows for these choices no longer list `platform`, so each appears only when its choice is selected.
- 4ef527c: - Add a logo-design loadout for SVG logos, repository icons and small-size checks.

## 0.2.0

### Minor Changes

- 81a2dea: - Add Codex plugin creation and instruction-writing skills to agent-authoring.
- 779f1c0: - Add a bun-cli loadout with Bun tooling, the Optique package companion and CLI development conventions.
- 4d55219: - Add a github-contributions loadout for template-aware issues and PRs, writing and semantic commits.
