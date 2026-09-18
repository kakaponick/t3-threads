---
name: t3-threads
description: Talk to other agents through T3 Code threads. Use to hand off a task to a new agent, or to message an existing thread by its threadId.
---

# T3 threads

Each T3 Code thread is one agent; its **threadId** is its address. A message starts that agent's next turn. Needs T3 Code (desktop app or `t3`) running on this machine.

```bash
T3="node $HOME/t3code/apps/server/src/bin.ts"
```

## Hand off

```bash
$T3 thread start <project-path> - --model <slug> --title "<title>" --json <<'EOF'
From thread <your-threadId>
<task>
EOF
```

- Prints `{"threadId", ...}`: keep it, it is the only way to reach that agent later.
- The new agent sees only this prompt: make it self-contained (goal, context, files, done criteria).
- Project must already be in T3; add it with `$T3 project add <path>`.
- `--model`: omit for project default, or e.g. `claude-opus-5`, `claude-fable-5-1`. A wrong slug fails with the available list.

## Message

```bash
$T3 thread send <threadId> - --json <<'EOF'
From thread <your-threadId>
<message>
EOF
```

- Delivers after the receiver finishes its current turn, so it can block for minutes; run it in the background when the receiver is mid-task.
- Fails while the receiver awaits an approval or answer; the user resolves that in T3.

## Replies

Your own threadId (empty output: you are outside T3):

```bash
node -e 'const db=new (require("node:sqlite").DatabaseSync)(require("os").homedir()+"/.t3/userdata/state.sqlite",{readOnly:true});console.log(db.prepare("select thread_id from provider_session_runtime where json_extract(resume_cursor_json, ?) = ?").get("$.resume",process.env.CLAUDE_CODE_SESSION_ID)?.thread_id??"")'
```

- Put it on the first line, `From thread <id>`, and ask for the result with `$T3 thread send <id> -`.
- Outside T3, sign as `From <role>` and expect no reply.
- After a handoff that expects a reply, end your turn: the reply arrives as your next message, and a `send` to you waits for your turn to end.
