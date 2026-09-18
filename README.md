# t3-threads

A Claude Code skill that lets agents hand off tasks and message each other through [T3 Code](https://github.com/pingdotgg/t3code) threads.

Each T3 Code thread is one agent, and its threadId is its address. An agent can start a thread with a task, send follow-ups to a thread it knows, and ask for the result to come back to its own thread.

## Requirements

- T3 Code running on the same machine (desktop app or `t3`).
- Node.js 24.13+ and pnpm.
- The `t3 thread` commands. They are not in a T3 Code release yet, so use the fork branch:

  ```bash
  git clone -b feat/cli-thread-start https://github.com/kakaponick/t3code ~/t3code
  cd ~/t3code && corepack enable && pnpm install
  ```

## Install

```bash
git clone https://github.com/kakaponick/t3-threads ~/.claude/skills/t3-threads
```

If t3code is not at `~/t3code`, edit the `T3=` line in `SKILL.md`.

New Claude Code sessions pick up the skill, including Claude agents inside T3 Code threads.

## Commands

`t3` below is `node ~/t3code/apps/server/src/bin.ts`.

```bash
t3 thread start <project> <prompt | -> [--model <slug>] [--provider <instance>] [--title <title>] [--json]
t3 thread send <threadId> <prompt | -> [--json]
```

- `start` creates a thread in a project already added to T3 Code (`t3 project add <path>`) and sends the first prompt. The default model and permissions come from the project settings.
- `send` continues a thread with its own model and modes. If the agent is working, it waits for the turn to finish and then sends, so the agent is never interrupted.

## Limitations

- An agent finds its own threadId only when it is a Claude agent (through `CLAUDE_CODE_SESSION_ID`). Agents on other providers need it passed in the prompt.
- The threadId lookup reads T3 Code's internal database, so a T3 Code update can break it.
- New threads start in the project checkout, not in a worktree.
