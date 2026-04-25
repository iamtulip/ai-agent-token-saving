# Token-Saving Agent Runbook

This runbook turns the notes in this repo into a repeatable workflow.

## What This Repo Is

This is a personal research and configuration note repository for a token-saving AI agent.

It is not a packaged CLI tool. The practical output is a small Hermes agent setup, plus experiment notes that help decide whether measurement, compression, retrieval, or MCP tools are worth adding.

## 1. Clone Or Open The Repo

```bash
git clone https://github.com/iamtulip/ai-agent-token-saving.git
cd ai-agent-token-saving
```

If the repo already exists locally:

```bash
cd /mnt/d/ai-agent-token-saving-notes
git pull
```

## 2. Read The V1 Contract

Start with:

- `TOKEN-SAVING-AGENT-V1.md`
- `hermes-personal-agent/TOKEN-SAVING-POLICY.md`

These files define the agent's main behavior:

- Search before reading.
- Keep context small.
- Compress noisy output.
- Keep memory curated.
- Measure token usage before adding more tools.
- Add MCP servers only when needed.

## 3. Deploy To Hermes

Follow:

- `DEPLOY-HERMES.md`

Short version:

```bash
cp hermes-personal-agent/USER.md ~/.hermes/memories/USER.md
cp hermes-personal-agent/MEMORY.md ~/.hermes/memories/MEMORY.md
cp hermes-personal-agent/SOUL.md ~/.hermes/SOUL.md
```

Before overwriting existing Hermes files, make a backup:

```bash
mkdir -p ~/.hermes/backups/token-agent
cp ~/.hermes/memories/USER.md ~/.hermes/backups/token-agent/USER.md.$(date +%Y%m%d-%H%M%S)
cp ~/.hermes/memories/MEMORY.md ~/.hermes/backups/token-agent/MEMORY.md.$(date +%Y%m%d-%H%M%S)
cp ~/.hermes/SOUL.md ~/.hermes/backups/token-agent/SOUL.md.$(date +%Y%m%d-%H%M%S)
```

## 4. Verify Hermes Behavior

Run:

```bash
hermes chat -q "สรุปกติกาการประหยัด token ของคุณตอนนี้ให้สั้น ๆ"
```

Good signs:

- It answers in Thai.
- It mentions search before reading.
- It mentions small curated memory.
- It warns against unnecessary tools, MCP, or external memory.
- It keeps the answer concise.

## 5. Measure Baseline First

Before adding output compression or retrieval, run one normal task and record the result in:

- `experiments/01-baseline-token-measurement.md`

Record:

- Date.
- Tool.
- Model.
- Task.
- Input tokens.
- Output tokens.
- Cached tokens if available.
- Tool output volume.
- Quality notes.

## 6. Test One Compression Tool

Choose only one first:

- `rtk`
- `snip`

Use:

- `experiments/02-rtk-test.md`
- `experiments/03-snip-test.md`

Keep the tool only if it reduces token use without hiding errors, failing tests, paths, or changed files.

## 7. Add Retrieval Later

Add semantic search only when the agent starts reading too much code.

First candidate:

- `Lumen`

Use:

- `experiments/04-lumen-test.md`

## 8. Daily Usage Prompt

Use this at the start of a Hermes coding session:

```text
ช่วยทำงานนี้โดยประหยัด token: search ก่อนอ่านไฟล์, อ่านเฉพาะ context ที่จำเป็น, สรุป output ยาว ๆ, และอย่าเพิ่ม memory/tool/MCP ถ้ายังไม่มีเหตุผลชัดเจน
```

## 9. Decision Rule

Promote a rule or tool only when it passes all three checks:

1. Token usage improves.
2. Answer or debugging quality stays good.
3. The setup remains easy to understand and roll back.

