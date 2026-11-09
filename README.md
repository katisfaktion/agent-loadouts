# Agent loadouts

Reusable agent setups for the [loadouts CLI](https://github.com/katisfaktion/loadouts), built from the project-local loadouts used in `acm-website`.

Each loadout declares its components and routing, which tells an agent when to use them. You choose loadouts individually. Nothing is bundled implicitly, and no loadout depends on another.

## Quick start

Requires the loadouts CLI `v0.1.0` or later.

```sh
loadouts setup --catalog katisfaktion/agent-loadouts
loadouts init --harness claude-code,codex
loadouts add sdlc docs --catalog katisfaktion/agent-loadouts@v0.1.0

# Add only the technologies and tools the project actually uses.
loadouts add react testing browser --catalog katisfaktion/agent-loadouts@v0.1.0

loadouts status
loadouts doctor
```

Use `loadouts show <name>` to inspect a definition before adding it. Sources can also be written explicitly, for example `github:katisfaktion/agent-loadouts/react@v0.1.0`.

Commit `loadouts.json`, `loadouts.lock.json`, `skills-lock.json` and the harness configuration the CLI manages. After cloning, `loadouts install` restores the setup from the locked catalog resolutions. `loadouts update` refreshes sources within their declared refs. A fixed release tag never advances to another release on its own. Catalog commits pin the definitions, but reproducing each component exactly is best effort.

## Loadouts

Each `loadouts/<name>.yml` starts with a comment that explains what the loadout adds and why its choices are grouped the way they are.

### Workflow

| Loadout | Use for | Enabled by default | Optional |
| --- | --- | --- | --- |
| `sdlc` | Planning, implementation, debugging and review | Superpowers, Ponytail, Codex second opinion (Claude) | `methodology` → `feature-dev` (Claude); `review` → `code-review`, `security-hooks` |
| `docs` | Current library and framework docs | Context7 | — |
| `codebase-memory` | Code structure, dependencies, change impact | Project graph (needs the global CLI) | — |
| `engineering-practices` | Security, migrations, performance, observability, ADRs, shipping | Nothing | Each practice under `practices` |
| `learning` | Learning over several sessions; stress-testing plans | `teach`, `grill-me` | `discovery` → `interview-me` |
| `agent-authoring` | Skills, plugins, AGENTS.md / CLAUDE.md | skill-creator, plugin-dev, claude-md-management (Claude only) | — |

### Stack

| Loadout | Use for | Enabled by default | Optional |
| --- | --- | --- | --- |
| `react` | React/Next.js components and component tests | React best practices, React Testing Library | — |
| `testing` | Unit, integration and e2e tests | Vitest, Playwright best practices | Disable either runner |
| `browser` | Checking UI in a real browser | Playwright | `driver` → `chrome-devtools` |
| `design` | Designing and reviewing UI and design systems | frontend-design, web-design-guidelines, UI UX Pro Max | `component-workbench` → `storybook` |
| `shadcn` | shadcn/ui components and registries | shadcn skill and MCP server | — |
| `payload` | Payload CMS | Skills from the installed `payload` package | `content-import` → `cms-migration` |
| `cloudflare` | Workers, wrangler, bindings, Next.js on Cloudflare | Cloudflare platform skills | `nextjs` → `vinext` |

A `one_of` group, such as `sdlc/methodology`, `browser/driver` or `learning/discovery`, picks exactly one alternative, and `loadouts switch` changes it. An `any_of` group picks zero or more members, which you change with `loadouts enable` and `loadouts disable`:

```sh
loadouts switch browser/driver chrome-devtools
loadouts enable engineering-practices/practices security
loadouts enable design/component-workbench storybook
loadouts disable sdlc/review second-opinion
```

These choices are saved in the project's `loadouts.json`. A listed optional component stays off until you enable it.

## Prerequisites and harness differences

- **payload:** install the project's dependencies first. The skills come from the installed package and follow its version on `loadouts install`. Loadouts does not install the library. If the package is missing, the CLI reports it and syncs the skills once the package is installed.
- **codebase-memory:** install the global codebase-memory CLI first. Loadouts reuses that installation, adds the project integration and indexes the project.
- **Harnesses:** Claude Code and Codex use their native project configuration, and your personal configuration stays in place. Some entries are intentionally Claude-only (`agent-authoring`, `sdlc` feature-dev and review members). Choosing a Claude-only alternative does not provide a Codex equivalent.
- **shadcn:** keep the command as `npx shadcn@latest mcp`. The CLI adds the npm isolation prefix to the generated configuration, including in pnpm projects.
- **Project settings:** operational names, account settings and deployment permissions belong to the consuming project, not to this catalog.

## Editing and validation

Definitions live in `loadouts/<name>.yml`, and the filename must match the YAML `name`. Write a `one_of` group with a scalar default and an `any_of` group with an array default. Use `[]` when no member is on by default. Start each file with a short comment that says what the loadout adds on top of its components and why its groups are shaped the way they are.

```sh
loadouts validate loadouts/payload.yml
loadouts show payload --catalog /absolute/path/to/agent-loadouts
```

Every entry links the [YAML schema](https://raw.githubusercontent.com/katisfaktion/loadouts/v0.1.0/schemas/v1/loadout.schema.json) (format `v1`) pinned to the CLI `v0.1.0` tag. The schema belongs to the CLI repository and is not copied here. It provides editor completion and validation. The CLI also checks group defaults and routing targets. Catalog versions are independent of CLI versions. Leave the schema reference unchanged until a loadout adopts a newer format, and never move an existing release tag.

Passing validation only proves that a definition is well formed. It does not prove that every optional alternative works on a real project task, or that upstream sources haven't changed. Check new or changed components on real work before releasing.

To develop a project-specific variant, run `loadouts create <name> --from <name>`, edit `.loadouts/catalog/<name>.yml`, select it explicitly and check it on real work. `loadouts publish <name> --summary 'What was verified'` previews a contribution, and adding `--yes` pushes a branch and opens a PR.

## Releases

Catalog tags are `vMAJOR.MINOR.PATCH` and never move once published. `v0.1.0` is the first catalog release and targets the CLI `v0.1.0`.
