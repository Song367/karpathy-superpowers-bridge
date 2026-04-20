# karpathy-superpowers-bridge

A Codex-only skill that combines **Superpowers** workflow discipline with **Karpathy-style** restraint.

This repository publishes a single bridge skill for Codex. It is designed for teams who want:

- Superpowers to keep the hard process gates
- Karpathy-style rules to keep planning, review, and implementation lean
- one self-contained skill instead of managing a separate Karpathy install

Chinese version: [README.zh.md](./README.zh.md)

## What This Skill Does

`karpathy-superpowers-bridge` does not replace Superpowers.

Instead, it adds a compatibility layer:

- **Superpowers** remains the workflow engine
  - design before implementation
  - root-cause debugging before fixes
  - failing tests before production code
  - fresh verification before completion claims
- **Karpathy** remains the restraint layer
  - make assumptions explicit
  - prefer the simplest solution
  - keep diffs surgical
  - define success in verifiable terms

The bridge makes those two systems work together without turning every task into a bloated prompt or an over-engineered plan.

## Design Goals

This skill is built around four decisions:

1. User and project instructions still win.
2. Superpowers hard gates stay intact when they clearly apply.
3. Karpathy principles constrain how Superpowers is used, especially in planning, review, and subagent execution.
4. The bridge is self-contained. You do **not** need a separate Karpathy skill install for the core behavior.

## Prerequisite

Install **Superpowers** first.

This bridge assumes Superpowers is already installed and discoverable in Codex. Without Superpowers, the bridge can still be read, but its routing logic loses most of its value.

Official Superpowers Codex docs:

- [Superpowers for Codex](https://github.com/obra/superpowers/blob/main/docs/README.codex.md)
- [Official Codex install instructions](https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md)

As of **April 20, 2026**, the official quick-install entrypoint is:

```text
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md
```

If Superpowers changes its installation flow later, prefer the official docs above.

## Installation

### Step 1: Install Superpowers

Follow the official Superpowers Codex install instructions first.

### Step 2: Install This Bridge Skill

Choose the install location that matches your Codex environment.

#### Option A: Native skill discovery under `~/.agents/skills` (recommended for Codex CLI)

```bash
mkdir -p ~/.agents/skills/karpathy-superpowers-bridge
curl -o ~/.agents/skills/karpathy-superpowers-bridge/SKILL.md \
  https://raw.githubusercontent.com/Song367/karpathy-superpowers-bridge/main/skills/karpathy-superpowers-bridge/SKILL.md
```

#### Option B: `$CODEX_HOME/skills` or `~/.codex/skills` (use this if your Codex setup discovers skills there)

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/karpathy-superpowers-bridge"
curl -o "${CODEX_HOME:-$HOME/.codex}/skills/karpathy-superpowers-bridge/SKILL.md" \
  https://raw.githubusercontent.com/Song367/karpathy-superpowers-bridge/main/skills/karpathy-superpowers-bridge/SKILL.md
```

This path has been tested successfully in a Codex desktop environment using:

```text
~/.codex/skills/karpathy-superpowers-bridge/
```

### Step 3: Restart Codex

Start a new Codex session after installation so the skill can be discovered.

## Alternative Installation

If you prefer to clone the repository and copy or symlink the skill folder yourself:

```bash
git clone https://github.com/Song367/karpathy-superpowers-bridge.git
```

Then copy or symlink:

- `skills/karpathy-superpowers-bridge/`

into one of these locations:

- `~/.agents/skills/`
- `${CODEX_HOME:-$HOME/.codex}/skills/`

## How It Combines Superpowers and Karpathy

The bridge uses this authority model:

1. **User and project instructions**
2. **Applicable Superpowers hard gates**
3. **Karpathy constraints inside the active workflow**
4. Default assistant behavior

In practice, that means:

- Karpathy can make a Superpowers workflow leaner.
- Karpathy does **not** silently cancel a Superpowers hard gate.
- If a rigid Superpowers skill clearly applies, the workflow should still be followed.
- Inside that workflow, planning, review, and implementation are filtered toward simplicity and anti-scope-creep.

## What the Bridge Adds

The bridge focuses on the seams where Superpowers can become heavier than necessary:

- **Planning filter**
  - no speculative architecture
  - no future-proofing unless requested
  - no file splitting or extra abstraction without present-day need
- **Review filter**
  - missing extensibility is not automatically a defect
  - missing configurability is not automatically a defect
  - scope creep is treated as a bug, not a strength
- **Subagent filter**
  - ask instead of guessing
  - implement only the specified task
  - keep diffs surgical
  - report concerns instead of bluffing certainty

## What This Repository Does Not Include

For now, this repository ships **only the Codex version**.

It does **not** currently include:

- Claude Code packaging
- Cursor rules
- a standalone `CLAUDE.md`
- a separate Karpathy plugin

## Repository Layout

```text
README.md
README.zh.md
skills/
  karpathy-superpowers-bridge/
    SKILL.md
```

## Usage

Once installed, Codex can discover the skill when the task matches its description.

You can also mention it explicitly, for example:

```text
Use karpathy-superpowers-bridge for this task.
```

## License

No license file has been added yet. Add one before wider redistribution if you want to make reuse terms explicit.
