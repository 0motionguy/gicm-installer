# @gicm/installer

A command-line installer that copies the gICM skill pack into OpenClaw's skills folder.

Each gICM package ships a `skill/` folder with a `SKILL.md` that OpenClaw can read. The installer finds those folders in your `node_modules` (or a local monorepo checkout) and copies them to `~/.openclaw/skills/<name>`, where OpenClaw looks for skills. It leaves `openclaw.json`, `SOUL.md` and `MEMORY.md` alone. It is for OpenClaw users who want the gICM skills without copying folders by hand.

Part of [gICM](https://github.com/0motionguy/gICM), a family of OpenClaw agent packages.

## What it does

- **Install:** copies the skill folders of eleven gICM packages (router, goldfish, soul, memory, cache, context, shield, orchestrator, dashboard, polyclaw-pro, opus67), or just the ones you name.
- **List:** `--list` shows which skills are already installed in the target folder.
- **Preview:** `--dry-run` reports what would be copied without writing anything.
- **Redirect:** `--target <dir>` installs somewhere other than `~/.openclaw/skills`.
- **Find sources:** looks for each package's `skill/` folder in `node_modules/@gicm/<package>/`, then in `packages/gicm-<package>/`, both relative to the working directory.
- **Use as a library:** exports `installAll`, `installSkill`, `checkInstalled`, `getOpenClawSkillsDir` and the `GICM_PACKAGES` list.

## Example

```bash
node dist/cli.js --list
node dist/cli.js router shield --dry-run
node dist/cli.js --target ./my-skills
```

The `gicm-install` binary name in `package.json` maps to the same entry point.

## How it's built

TypeScript (ESM) bundled with tsup into two entries, `index` (library) and `cli` (with a Node shebang), and tested with vitest. It uses only `fs`, `path` and `os`; zod is declared but the installer does not need it for copying.

## Repository layout

- `src/index.ts`: package list, source lookup, install and list functions
- `src/cli.ts`: argument parsing and console output
- `src/__tests__/`: vitest suite (19 test cases)

## Run it locally

```bash
pnpm install
pnpm build   # tsup -> dist/index.js and dist/cli.js
pnpm test    # vitest (add --run for a single pass)
```

## Status

Beta. Version 1.0.0, a single extracted release (February 2026) with a test suite. Known gap: the third source lookup uses `__dirname` inside an ESM build, where it is undefined, so it throws instead of returning "not found" when the first two lookups miss.
