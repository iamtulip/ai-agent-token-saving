# GitHub Tools สำหรับประหยัด Token ใน Codex CLI และ Claude Code

> รวบรวม repo ที่เกี่ยวข้องกับการลด token, ลด context bloat, กรอง terminal output, ทำ semantic search เฉพาะไฟล์ที่เกี่ยวข้อง และวัดการใช้ token  
> เหมาะสำหรับใช้กับ **OpenAI Codex CLI**, **Claude Code**, **Gemini CLI**, **OpenCode**, **OpenClaw** และ workflow แบบ AI coding agent

อัปเดตลิงก์: 25 เมษายน 2026

---

## สรุปสั้น

ถ้าต้องการเริ่มแบบปลอดภัยและคุ้มที่สุด แนะนำลำดับนี้

1. ใช้ **RTK** หรือ **Snip** เพื่อลด terminal output ที่ส่งเข้า context
2. ใช้ **claude-code-lean** เพื่อจัด `CLAUDE.md`, `.claudeignore`, hooks และแนวทางใช้ Claude Code แบบประหยัด
3. ถ้าใช้ MCP หลายตัว ให้ใช้ **mcp-optimizer** หรือ **token-optimizer-mcp**
4. ถ้า codebase ใหญ่ ให้ใช้ **Lumen** หรือ **claude-context** เพื่อค้นเฉพาะไฟล์ที่เกี่ยวข้อง
5. ใช้ **Tokscale** หรือ **token-optimizer** เพื่อตรวจว่ากิน token จากจุดไหน

---

## 1. กลุ่มลด Terminal Output / Shell Output

| Repo | ใช้กับ | จุดเด่น | ลิงก์ |
|---|---|---|---|
| `rtk-ai/rtk` | Claude Code, OpenCode, ใช้ร่วมกับ Codex ได้ผ่าน proxy/hook | CLI proxy สำหรับกรองและย่อ output จากคำสั่ง dev เช่น `ls`, `tree`, `git diff`, `test` | [rtk-ai/rtk](https://github.com/rtk-ai/rtk) |
| `edouard-claude/snip` | Claude Code, Cursor, Copilot, Gemini, Codex workflow | CLI proxy เขียนด้วย Go ใช้ YAML filter กำหนดวิธีตัด output | [edouard-claude/snip](https://github.com/edouard-claude/snip) |
| `claudioemmanuel/squeez` | Claude Code, Copilot CLI, OpenCode, Gemini CLI, Codex CLI | Hook-based token compressor, dedup output, compression สำหรับ bash output | [claudioemmanuel/squeez](https://github.com/claudioemmanuel/squeez) |

### ใช้เมื่อไร

ใช้กลุ่มนี้เมื่อ AI ของเราสั่งคำสั่งแล้วได้ output ยาวมาก เช่น

```bash
npm test
pytest
git diff
git status
tree
ls -R
```

ปัญหาที่ช่วยแก้คือ terminal output ยาว ๆ ถูกส่งเข้า context ทำให้ token หมดเร็ว

---

## 2. กลุ่ม Semantic Search / อ่านเฉพาะไฟล์ที่เกี่ยวข้อง

| Repo | ใช้กับ | จุดเด่น | ลิงก์ |
|---|---|---|---|
| `ory/lumen` | Claude Code, Codex, OpenCode | local semantic search ช่วยให้ agent หาไฟล์ที่เกี่ยวข้องก่อนอ่านทั้งโปรเจกต์ | [ory/lumen](https://github.com/ory/lumen) |
| `zilliztech/claude-context` | Claude Code / MCP | Code search MCP สำหรับทำให้ agent ค้น codebase ได้แม่นขึ้น | [zilliztech/claude-context](https://github.com/zilliztech/claude-context) |

### ใช้เมื่อไร

เหมาะกับโปรเจกต์ใหญ่ เช่น

- monorepo
- Next.js + Supabase + API routes จำนวนมาก
- โปรเจกต์ grading platform
- repo ที่มีหลาย module หลายโฟลเดอร์

แนวคิดคือให้ AI ค้นหาไฟล์ที่เกี่ยวข้องก่อน แล้วค่อยเปิดอ่านเฉพาะส่วนจำเป็น แทนการโหลดไฟล์จำนวนมากเข้าบริบท

---

## 3. กลุ่ม Claude Code Token Optimization โดยตรง

| Repo | ใช้กับ | จุดเด่น | ลิงก์ |
|---|---|---|---|
| `farhan523/claude-code-lean` | Claude Code | Toolkit รวม guides, skills, hooks และ templates เพื่อลด token/cost | [farhan523/claude-code-lean](https://github.com/farhan523/claude-code-lean) |
| `nadimtuhin/claude-token-optimizer` | Claude Code | Prompt/template สำหรับย่อเอกสารและตั้งค่า docs ให้ Claude Code ใช้ token น้อยลง | [nadimtuhin/claude-token-optimizer](https://github.com/nadimtuhin/claude-token-optimizer) |
| `affaan-m/everything-claude-code` | Claude Code | เอกสารรวมแนวทาง token optimization สำหรับ Claude Code | [docs/token-optimization.md](https://github.com/affaan-m/everything-claude-code/blob/main/docs/token-optimization.md) |

### ใช้เมื่อไร

ใช้เมื่อมีปัญหาเหล่านี้

- `CLAUDE.md` ยาวเกินไป
- Claude Code จำบริบทเก่าเยอะเกิน
- session ยาวแล้วช้าลง
- ต้อง `/compact` บ่อย
- agent อ่านไฟล์ไม่ตรงจุด

---

## 4. กลุ่ม MCP Optimization

| Repo | ใช้กับ | จุดเด่น | ลิงก์ |
|---|---|---|---|
| `o8n/mcp-optimizer` | Claude Code + MCP | วิเคราะห์ MCP server ว่ากิน context เท่าไร และตัวไหนควรปิด/ลด | [o8n/mcp-optimizer](https://github.com/o8n/mcp-optimizer) |
| `ooples/token-optimizer-mcp` | Claude Code + MCP | MCP สำหรับ caching, compression และ smart tool intelligence | [ooples/token-optimizer-mcp](https://github.com/ooples/token-optimizer-mcp) |

### ใช้เมื่อไร

เหมาะเมื่อใช้ MCP หลายตัว เช่น

- filesystem MCP
- GitHub MCP
- database MCP
- browser/search MCP
- memory MCP

ถ้าเปิด MCP มากเกินไป context อาจบวมตั้งแต่เริ่ม session

---

## 5. กลุ่มลด Output Style / ทำให้ AI ตอบสั้นลง

| Repo | ใช้กับ | จุดเด่น | ลิงก์ |
|---|---|---|---|
| `M4cs/compressoor` | Codex, Claude Code | Runtime policy สำหรับทำให้ output สั้นลงโดยยังอ่านรู้เรื่อง | [M4cs/compressoor](https://github.com/M4cs/compressoor) |
| `JuliusBrussee/caveman` | Claude Code, Codex plugin/workflow | ลด output token โดยบังคับ style ให้สั้นมาก | [JuliusBrussee/caveman](https://github.com/juliusbrussee/caveman) |
| `o4f6bgpac3/concise` | Claude Code, Codex, Cline, Cursor | Skill/plugin สำหรับตอบสั้น ลด output tokens | [o4f6bgpac3/concise](https://github.com/o4f6bgpac3/concise) |
| `chimera-defi/token-reduce-skill` | Claude/Codex workflow | Skill สำหรับ low-token repo discovery, QMD-first retrieval และ Bash/Glob guardrails | [chimera-defi/token-reduce-skill](https://github.com/chimera-defi/token-reduce-skill) |

### ใช้เมื่อไร

ใช้เมื่อ AI พูดเยอะเกินไป เช่น

- อธิบายแผนยาวทุกครั้งก่อนแก้โค้ด
- สรุปซ้ำ ๆ
- ตอบด้วยข้อความยาวมากทั้งที่ต้องการแค่ diff หรือ patch
- ทำให้ output token สูงเกินจำเป็น

---

## 6. กลุ่มวัด Token Usage / ตรวจว่า Token หายไปไหน

| Repo | ใช้กับ | จุดเด่น | ลิงก์ |
|---|---|---|---|
| `junhoyeo/tokscale` | Claude Code, Codex, Gemini, Cursor, OpenClaw ฯลฯ | CLI สำหรับ tracking token usage จากหลายเครื่องมือ | [junhoyeo/tokscale](https://github.com/junhoyeo/tokscale) |
| `alexgreensh/token-optimizer` | Claude Code / AI coding workflow | หา ghost tokens, config บวม, skill ไม่ใช้, memory ซ้ำ, context decay | [alexgreensh/token-optimizer](https://github.com/alexgreensh/token-optimizer) |

### ใช้เมื่อไร

ใช้เมื่อไม่แน่ใจว่า token หมดจากอะไร เช่น

- output จาก terminal
- context จากไฟล์
- MCP tools
- memory
- `CLAUDE.md`
- agent instructions
- session เก่าที่ยาวมาก

---

## 7. กลุ่ม Format ประหยัด Token สำหรับ Structured Data

| Repo | ใช้กับ | จุดเด่น | ลิงก์ |
|---|---|---|---|
| `RudsonCarvalho/terse-format` | LLM pipelines, structured prompts, tool payloads | format แทน JSON ที่ลด token ได้โดยยังอ่านง่าย | [RudsonCarvalho/terse-format](https://github.com/RudsonCarvalho/terse-format) |

### ใช้เมื่อไร

เหมาะกับกรณีที่ส่งข้อมูลแบบ JSON เข้า LLM บ่อย เช่น

```json
{
  "items": [
    {
      "id": 1,
      "name": "example",
      "status": "active"
    }
  ]
}
```

ถ้าข้อมูลมี key ซ้ำจำนวนมาก การเปลี่ยน format อาจช่วยลด token ได้

---

## 8. กลุ่มรวม Resources / Settings / Issues ที่เกี่ยวข้อง

| Repo / Issue | ใช้กับ | จุดเด่น | ลิงก์ |
|---|---|---|---|
| `RoggeOhta/awesome-codex-cli` | Codex CLI | รวมเครื่องมือ skills plugins และ resources สำหรับ Codex CLI | [RoggeOhta/awesome-codex-cli](https://github.com/RoggeOhta/awesome-codex-cli) |
| `fcakyon/claude-codex-settings` | Claude Code, Codex | ตัวอย่าง settings, skills, plugins, hooks และ agents ที่ผู้ใช้จัดไว้ใช้จริง | [fcakyon/claude-codex-settings](https://github.com/fcakyon/claude-codex-settings) |
| `openai/codex#19001` | Codex CLI | Issue เสนอให้ Codex รองรับ RTK เพื่อลด shell output token | [openai/codex issue #19001](https://github.com/openai/codex/issues/19001) |
| `openai/codex#13222` | Codex CLI | Issue ขอ token usage breakdown เพื่อดูว่า token ถูกใช้ตรงไหน | [openai/codex issue #13222](https://github.com/openai/codex/issues/13222) |

---

## Recommended Setup สำหรับ VS Code + Codex + Claude Code

### ขั้นที่ 1: เริ่มจากลด output ใน terminal

เลือกอย่างใดอย่างหนึ่ง

- [rtk-ai/rtk](https://github.com/rtk-ai/rtk)
- [edouard-claude/snip](https://github.com/edouard-claude/snip)
- [claudioemmanuel/squeez](https://github.com/claudioemmanuel/squeez)

เหมาะกับงานที่ AI สั่ง command แล้ว output ยาวมาก

---

### ขั้นที่ 2: จัดเอกสาร context ของ Claude Code

ใช้

- [farhan523/claude-code-lean](https://github.com/farhan523/claude-code-lean)
- [everything-claude-code token optimization](https://github.com/affaan-m/everything-claude-code/blob/main/docs/token-optimization.md)

สิ่งที่ควรทำ

- ทำ `CLAUDE.md` ให้สั้น
- ใช้ `.claudeignore`
- แยก session ตามงาน
- ใช้ `/clear` เมื่อเปลี่ยนหัวข้อ
- ใช้ `/compact` เมื่อ session ยาว
- ระบุไฟล์เป้าหมายให้ AI ชัดเจน

---

### ขั้นที่ 3: ถ้าโปรเจกต์ใหญ่ ให้เพิ่ม semantic search

ใช้

- [ory/lumen](https://github.com/ory/lumen)
- [zilliztech/claude-context](https://github.com/zilliztech/claude-context)

แนวคิดคือให้ AI ค้นหาไฟล์ที่เกี่ยวข้องก่อน ไม่อ่านทั้ง repo

---

### ขั้นที่ 4: ถ้าใช้ MCP เยอะ ให้ audit MCP

ใช้

- [o8n/mcp-optimizer](https://github.com/o8n/mcp-optimizer)
- [ooples/token-optimizer-mcp](https://github.com/ooples/token-optimizer-mcp)

เป้าหมายคือดูว่า MCP ตัวไหนเพิ่ม context มากเกินไป

---

### ขั้นที่ 5: ติดตามผลจริง

ใช้

- [junhoyeo/tokscale](https://github.com/junhoyeo/tokscale)
- [alexgreensh/token-optimizer](https://github.com/alexgreensh/token-optimizer)

เพื่อตอบคำถามว่า token หายไปที่ไหน

---

## ไฟล์ที่ควรมีใน repo ส่วนตัว

แนะนำโครงสร้างแบบนี้

```text
ai-agent-token-saving-notes/
├── README.md
├── tools/
│   ├── terminal-output-compression.md
│   ├── claude-code-optimization.md
│   ├── codex-cli-optimization.md
│   └── mcp-optimization.md
└── experiments/
    ├── rtk-test.md
    ├── snip-test.md
    └── lumen-test.md
```

หรือถ้าต้องการแบบง่ายที่สุด ใช้ไฟล์นี้เป็น `README.md` ได้เลย

---

## หมายเหตุด้านความปลอดภัย

ก่อนติดตั้ง repo ใด ๆ ควรตรวจสอบ

- จำนวน stars/forks ไม่ใช่หลักฐานว่าปลอดภัยเสมอ
- อ่าน `README.md`, `install.sh`, `package.json`, `Cargo.toml`, `pyproject.toml` ก่อนรัน
- หลีกเลี่ยงการรันคำสั่ง install ที่ใช้ `curl | bash` โดยไม่อ่าน script
- ทดลองใน repo ทดสอบก่อนนำไปใช้กับงานจริง
- อย่าใส่ API key หรือ secret ลงในไฟล์ที่ AI อ่านโดยไม่จำเป็น
- ถ้าติดตั้ง hook ควรตรวจว่า hook อ่าน/ส่งข้อมูลออกไปที่ไหน

---

## Quick Links

### Terminal Output Compression

- [RTK - Rust Token Killer](https://github.com/rtk-ai/rtk)
- [Snip](https://github.com/edouard-claude/snip)
- [Squeez](https://github.com/claudioemmanuel/squeez)

### Semantic Search

- [Lumen](https://github.com/ory/lumen)
- [Claude Context](https://github.com/zilliztech/claude-context)

### Claude Code

- [Claude Code Lean](https://github.com/farhan523/claude-code-lean)
- [Claude Token Optimizer](https://github.com/nadimtuhin/claude-token-optimizer)
- [Everything Claude Code - Token Optimization](https://github.com/affaan-m/everything-claude-code/blob/main/docs/token-optimization.md)

### MCP

- [MCP Optimizer](https://github.com/o8n/mcp-optimizer)
- [Token Optimizer MCP](https://github.com/ooples/token-optimizer-mcp)

### Output Style Compression

- [Compressoor](https://github.com/M4cs/compressoor)
- [Caveman](https://github.com/juliusbrussee/caveman)
- [Concise](https://github.com/o4f6bgpac3/concise)
- [Token Reduce Skill](https://github.com/chimera-defi/token-reduce-skill)

### Measurement

- [Tokscale](https://github.com/junhoyeo/tokscale)
- [Token Optimizer](https://github.com/alexgreensh/token-optimizer)

### Structured Data

- [TERSE Format](https://github.com/RudsonCarvalho/terse-format)

### Codex Resources

- [Awesome Codex CLI](https://github.com/RoggeOhta/awesome-codex-cli)
- [Claude Codex Settings](https://github.com/fcakyon/claude-codex-settings)
- [Codex Issue #19001 - RTK Integration](https://github.com/openai/codex/issues/19001)
- [Codex Issue #13222 - Token Usage Breakdown](https://github.com/openai/codex/issues/13222)

---

## คำแนะนำส่วนตัวสำหรับงาน AI Coding Agent

สำหรับงานที่ใช้ทั้ง Codex และ Claude Code ไม่ควรติดตั้งทุกตัวพร้อมกัน เพราะอาจทำให้ระบบซับซ้อนและ debug ยาก

แนะนำเริ่มจากชุดเล็กนี้ก่อน

```text
1. rtk-ai/rtk หรือ edouard-claude/snip
2. farhan523/claude-code-lean
3. junhoyeo/tokscale
4. ory/lumen เมื่อ repo ใหญ่ขึ้น
```

เมื่อทดสอบแล้วค่อยเพิ่ม MCP optimizer หรือ output style compressor ตามปัญหาจริง

---

## License / Credit

ไฟล์นี้เป็นบันทึกสรุปส่วนตัวสำหรับศึกษาและจัดระบบเครื่องมือประหยัด token  
ลิงก์ทั้งหมดชี้กลับไปยัง GitHub repository ต้นทางของผู้พัฒนาแต่ละราย
