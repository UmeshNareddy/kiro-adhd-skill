# i-have-adhd — Kiro & VS Code setup pack

Copy/paste instructions to make an AI coding agent answer in ADHD-friendly style:
action first, numbered steps, no filler. Skill content is the upstream MIT skill
from [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd) — see `LICENSE`,
unmodified (`skills/i-have-adhd/SKILL.md`).

---

## 1. Kiro (IDE and CLI)

### Option A — Import from GitHub (fastest, ~1 min)

1. Open the Kiro panel (left sidebar) → **Agent Steering & Skills**.
2. Click **+** → **Import a skill**.
3. Source: **GitHub**. Paste this exact URL (it must point at the skill FOLDER, not the repo root):

   ```
   https://github.com/UmeshNareddy/kiro-adhd-skill/tree/main/skills/i-have-adhd
   ```

4. Confirm. Kiro copies it to `~/.kiro/skills/i-have-adhd/`. Restart the chat session.

### Option B — Manual copy

```bash
mkdir -p ~/.kiro/skills
cp -R skills/i-have-adhd ~/.kiro/skills/
```

- `~/.kiro/skills/` = personal, every project.
- `.kiro/skills/` in a repo = workspace-only, team-shareable (workspace wins on name clash).

### How to use it in Kiro

- On demand: type `/i-have-adhd` in chat (slash command). With `disable-model-invocation: true` in the SKILL.md, it stays off until you invoke it.
- Auto-activation: Kiro loads the skill when your request matches its description.
- Turn it off mid-session: say `stop adhd mode` (or `normal mode`).
- Manage: Kiro panel → Agent Steering & Skills shows everything loaded.

### Always-on variant (Kiro)

If you want the rules in EVERY chat without invoking (this is what CLAUDE.md does elsewhere):

1. Create `.kiro/steering/adhd-output.md` in your project (or `~/.kiro/steering/` for all projects):

   ```markdown
   ---
   inclusion: always
   ---
   # Output style: ADHD-friendly

   Follow the rules in skills/i-have-adhd/SKILL.md at all times:
   lead with the next action; number multi-step tasks; end with one concrete
   next step; suppress tangents; restate state every turn; specific time
   estimates (minutes/hours, never "a bit"); show completed work concretely;
   matter-of-fact errors (cause + fix, no "Uh oh"); cap visible lists at 5
   items (never drop relevant items); no preamble, no recap, no closers.
   Break these rules only for: explicit "explain this" requests, destructive
   actions (confirm first), debug spirals (name the bad assumption, ask one
   question), real ambiguity (one short question), and when a rule would
   delete the answer (options requests: 2–4 ranked options, recommendation
   first).
   ```

2. Restart chat. Verify: first line of any answer should now be an action, never "Great question!".

---

## 2. VS Code (GitHub Copilot Chat)

### Option A — Skills CLI

```bash
npx skills add ayghri/i-have-adhd -a github-copilot -g   # -g = all projects; drop -g for current project only
```

### Option B — Manual copy

```bash
mkdir -p ~/.copilot/skills
cp -R skills/i-have-adhd ~/.copilot/skills/
```

Copilot also scans `.github/skills/`, `.claude/skills/`, `.agents/skills/` (per-project) and `~/.claude/skills/` / `~/.agents/skills/` (global) — same folder format, pick whichever your team already uses.

### How to use it in VS Code

1. Open Copilot Chat (Ctrl/Cmd+I or the Chat panel).
2. Type `/i-have-adhd` (+ optional extra text). It applies for that session.
3. Say `stop adhd mode` to switch back.

### Always-on variant (VS Code)

Append the rules block below to `.github/copilot-instructions.md` (per repo; Copilot injects it into every chat):

```markdown
## Output style: ADHD-friendly
Lead with the next action as the first line. Number multi-step tasks, fewest
steps that work. End with exactly one concrete next step (under 2 minutes).
No tangents (finish task 1, then offer task 2). Restate progress every turn
("Step 3 of 5 done: X. Next: Y"). Time estimates in minutes/hours, never
vague. Show what now works, concretely. Errors: cause + fix, matter-of-fact.
Visible lists capped at 5 items, most relevant first — never omit relevant
items. No preamble ("Great question", "Let me…"), no recaps, no closers
("Hope this helps"). Exceptions: full explanations when asked, confirmation
before destructive actions, one diagnostic question on debug spirals, ranked
options when options are the answer.
```

---

## 3. Verify it works (either tool)

Ask: "why is my npm build slow?"

- Working: first line is an action or command (e.g. "Run `npx tsc --noEmit --pretty` and time it…").
- Not working: reply starts with "Great question!" or "Let's think about…" → the skill did not load; re-check folder path and restart the chat session (skills are read at session start).

## 4. Uninstall

- Kiro: delete `~/.kiro/skills/i-have-adhd/` (or remove in the Agent Steering & Skills panel).
- VS Code/Copilot: delete `~/.copilot/skills/i-have-adhd/` (and the rules block from `.github/copilot-instructions.md` if added).

## Provenance & license

- Skill text: © Ayoub Ghriss, MIT — https://github.com/ayghri/i-have-adhd (keep the upstream copyright notice in LICENSE when redistributing).
- Kiro skills/steering and Copilot instructions: verified against Kiro docs (kiro.dev/docs/skills) and the upstream INSTALL.md.
