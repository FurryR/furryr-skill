# furryr-skill

A collection of agent skills.

## hurry-up

`hurry-up` is a maximum-speed execution mode. Invoke it with a requirement:

```text
/hurry-up add cursor pagination to the users endpoint
```

The agent reads the project's `AGENTS.md` first if one exists, batches its
reads, posts a one-shot plan to the user, and only then starts editing. It does
not wait for subagents or slow commands, does not write or run tests, and does
not perform other bug checks. Unverified work is appended to the project's
`AGENTS.md` under `## TODO - Bug checks`.

Before a large change, the agent must explain why the work will take a while
and ask for confirmation.

### Install

The repository must be public so `skills` and `cp`-less installs can fetch it.

```bash
npx skills add FurryR/furryr-skill --skill hurry-up -g
```

Add `-y` to skip the confirmation prompt in scripts:

```bash
npx skills add FurryR/furryr-skill --skill hurry-up -g -y
```

OpenCode slash commands come from command files, not skills. Install the
command as well:

```bash
mkdir -p ~/.config/opencode/commands
cp commands/hurry-up.md ~/.config/opencode/commands/
```

Claude Code and other agents that expose skills as slash commands need only
the skill. Optionally install the command there too:

```bash
mkdir -p ~/.claude/commands
cp commands/hurry-up.md ~/.claude/commands/
```

To install the skill manually, copy its directory:

```bash
# Claude Code
cp -r skills/hurry-up ~/.claude/skills/

# OpenCode
cp -r skills/hurry-up ~/.config/opencode/skills/

# Codex, Cursor, and other agents using the universal path
cp -r skills/hurry-up ~/.agents/skills/
```

### Invocation

```text
/hurry-up add cursor pagination to the users endpoint
```

In OpenCode, `/` lists commands and `@` loads skills. If the command is not
installed, use the skill directly:

```text
@hurry-up add cursor pagination to the users endpoint
```

### When not to use it

Use this skill only when speed matters more than verification. It intentionally
skips tests and bug checks. It is a poor fit for security-sensitive changes,
data migrations that need validation, or work where an incorrect result is
expensive.

## Publishing

Create the public repository and push the initial commit:

```bash
gh repo create FurryR/furryr-skill --public --source=. --push
```

Verify the published repo before announcing it:

```bash
npx skills add FurryR/furryr-skill --list
```

## License

MIT
