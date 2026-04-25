# Deploy Token-Saving Agent To Hermes

This guide assumes Hermes is already installed.

## 1. Confirm Hermes

```bash
command -v hermes
hermes --version
hermes memory status
```

Expected memory status:

```text
Built-in: always active
Provider: (none - built-in only)
```

Built-in memory is enough for V1.

## 2. Back Up Current Hermes Memory

```bash
mkdir -p ~/.hermes/backups/token-agent
cp ~/.hermes/memories/USER.md ~/.hermes/backups/token-agent/USER.md.$(date +%Y%m%d-%H%M%S)
cp ~/.hermes/memories/MEMORY.md ~/.hermes/backups/token-agent/MEMORY.md.$(date +%Y%m%d-%H%M%S)
cp ~/.hermes/SOUL.md ~/.hermes/backups/token-agent/SOUL.md.$(date +%Y%m%d-%H%M%S)
```

## 3. Copy User And Memory Drafts

From this repo:

```bash
cp hermes-personal-agent/USER.md ~/.hermes/memories/USER.md
cp hermes-personal-agent/MEMORY.md ~/.hermes/memories/MEMORY.md
```

## 4. Add Token-Saving Behavior To `SOUL.md`

Edit:

```bash
nano ~/.hermes/SOUL.md
```

Suggested content:

```markdown
# Hermes Token-Saving Personal Agent

You are a warm, concise, Thai-first assistant for AI coding workflows.

Your main operating goal is to reduce token waste while staying useful.

Default behavior:

1. Search before reading large files or folders.
2. Read only the smallest useful context.
3. Summarize large context before using it.
4. Keep normal answers concise and actionable.
5. Preserve technical accuracy, errors, failing tests, file paths, and changed files.
6. Do not store secrets, API keys, passwords, raw logs, or one-off debugging details in memory.
7. Before adding tools, MCP servers, or persistent memory, explain why they are necessary.
8. Prefer built-in memory for V1. Add external memory only after measuring a real need.
9. Use `tokscale` or available usage data to measure token use before optimizing further.
10. Add output compression and retrieval tools only one at a time.
```

Hermes reloads `SOUL.md` each message, so a restart is usually not needed.

## 5. Run First Test Session

```bash
hermes chat -q "สรุปกติกาการประหยัด token ของคุณจาก memory และ persona ตอนนี้ให้สั้น ๆ"
```

Good signs:

- It answers in Thai.
- It mentions search-before-read.
- It mentions small curated memory.
- It warns against unnecessary MCP/tools.
- It keeps the answer concise.

## 6. Daily Usage Prompt

Use this at the start of coding sessions:

```text
ช่วยทำงานนี้โดยประหยัด token: search ก่อนอ่านไฟล์, อ่านเฉพาะ context ที่จำเป็น, สรุป output ยาว ๆ, และอย่าเพิ่ม memory/tool/MCP ถ้ายังไม่มีเหตุผลชัดเจน
```

## 7. Roll Back

If the behavior feels worse, restore the latest backup:

```bash
cp ~/.hermes/backups/token-agent/USER.md.YYYYMMDD-HHMMSS ~/.hermes/memories/USER.md
cp ~/.hermes/backups/token-agent/MEMORY.md.YYYYMMDD-HHMMSS ~/.hermes/memories/MEMORY.md
cp ~/.hermes/backups/token-agent/SOUL.md.YYYYMMDD-HHMMSS ~/.hermes/SOUL.md
```

