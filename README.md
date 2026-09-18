# t3-threads

Claude Code skill: agents hand off tasks and message each other through [T3 Code](https://github.com/pingdotgg/t3code) threads.

## Install

Needs T3 Code desktop, Node.js 24.13+ and git. Run in Git Bash:

```bash
git clone --depth 1 -b feat/cli-thread-start https://github.com/kakaponick/t3code ~/t3code
cd ~/t3code && npx --yes pnpm@11.10.0 install --filter "t3..."
git clone https://github.com/kakaponick/t3-threads ~/.claude/skills/t3-threads
```

Check: `node ~/t3code/apps/server/src/bin.ts thread start --help` prints usage.

Keep T3 Code desktop updated: the CLI shares its database.

## Commands

`t3` = `node ~/t3code/apps/server/src/bin.ts`

```bash
t3 thread start <project> <prompt> [--model <slug>] [--title <title>] [--json]
t3 thread send <threadId> <prompt> [--now] [--json]
```

- `start`: new thread in a project added to T3 Code.
- `send`: next message to a thread. Waits until the agent finishes its turn.
- `send --now`: sends right away into the running turn.
- `-` instead of `<prompt>` reads stdin.
