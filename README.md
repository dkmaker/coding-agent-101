# coding-agent-101

Educational snippets, basic instructions, and test skills for AI coding agents.

This repo is a growing collection of small, copy-pasteable artifacts you can
hand to an agent (Claude Code, other CLI coding agents, etc.) to teach it a
pattern, set up a project convention, or demonstrate a skill.

Everything here is intentionally low-ceremony and aimed at beginners —
both humans learning how to direct coding agents, and agents learning
common workflows.

## Structure

- `basic-instructions/` — single-file prompts you can paste into an agent to
  make it do something concrete (set up project docs, scaffold a structure,
  etc.). Each file is self-contained.

More folders will be added as the collection grows (skills, output styles,
slash-command examples, hook examples, …).

## How to use

### Option 1 — paste directly

Open a file under `basic-instructions/`, copy its full contents, and paste
it into your agent. The file itself is the prompt.

### Option 2 — let the agent fetch it

Tell your agent to fetch the raw file from GitHub and execute it. Example:

```
Hent https://raw.githubusercontent.com/dkmaker/coding-agent-101/main/basic-instructions/projekt-setup-dansk.md
og følg instruktionerne i filen præcist.
```

## License

MIT — free to use, fork, adapt, share.
