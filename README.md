# Agent loadouts

A catalog of reusable agent setups for the [loadouts CLI](https://github.com/katisfaktion/loadouts). See that repository for installation and usage.

## Format

- `loadouts/<name>.yml` holds one loadout. The filename matches its `name`.
- Each file starts with a comment that says what the loadout adds and why its groups are shaped the way they are.
- Definitions follow the [`v1` schema](https://raw.githubusercontent.com/katisfaktion/loadouts/v0.1.0/schemas/v1/loadout.schema.json). Check a file with `loadouts validate loadouts/<name>.yml`.

## Releases

Tags are `vMAJOR.MINOR.PATCH` and never move. Versioning uses [Changesets](https://github.com/changesets/changesets): add one with `pnpm changeset`. The [release workflow](.github/workflows/release.yml) opens a version PR, and merging it creates the tag.
