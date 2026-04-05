# 王之開源（Gate of Open Source）

![banner](assets/banner.png)

[繁體中文](#這是什麼) | [English](#english)

> *「你竟敢在沒有查閱寶庫的情況下開發？天下所有的開源寶物皆屬於我。」*

## 這是什麼

你是不是每次都在 Twitter 或 Hacker News 上偶然看到別人分享了一個超讚的開源工具，然後才發現「靠，這個東西三個月前就有了，我到底在幹嘛」？

這個 AI Skill 幫你解決這個問題。裝好之後打 `/oss`，它會：

1. **讀你的技術棧** — 快速掃 package.json / Cargo.toml，知道你用了什麼
2. **搜 GitHub + Hacker News** — 找這週跟你技術棧相關的熱門專案
3. **擺出來問你** — 「這些有沒有感興趣的？」
4. **你指哪個，它去比對你的實際代碼** — 讀對應的代碼，誠實告訴你值不值得

**它只會給你報告，不會幫你安裝任何東西。畢竟王只鑑定，不親自動手。**

## 跟其他「找開源」的工具有什麼不同

大多數推薦工具的做法是：看你用了 React → 推薦 React 生態的熱門庫 → 給你一大堆。

問題是：**熱門不代表你需要。** AI 不知道你的代碼長什麼樣就推薦，等於瞎推。

這個工具的做法是兩階段：
- **第一階段**（便宜）：掃技術棧 → 搜 → 擺出來讓你選
- **第二階段**（精準）：你指的那幾個 → 去讀你的實際代碼比對 → 誠實回報

你指到哪，它才讀到哪。不浪費 token，不浪費你的時間。

**「都不需要」和「你的代碼已經夠好」都是完全正常的結果。**

## 它怎麼運作

```
Phase 1（便宜，每次都跑）
      ↓
讀技術棧（package.json / Cargo.toml）
      ↓
搜 GitHub + Hacker News
      ↓
按類別擺出來
      ↓
問你：「有沒有感興趣的？」

Phase 2（你指了才跑）
      ↓
讀你指定領域的實際代碼（2-3 個檔案）
      ↓
比對：你現在的代碼 vs 這個庫
      ↓
誠實回報：值不值得、遷移成本、風險
      ↓
結果存進 .oss-profile.yml（下次不重複評估）
```

### 鑑定等級

| 等級 | 意思 |
|------|------|
| SSR | 比對過你的代碼，確認能解決真實問題，遷移成本合理 |
| SR | 值得花時間試用 |
| R | 有潛力，先加個書籤 |
| N | 有疑慮，先別碰 |
| 廢鐵 | 不安全，狗都不用 |

## 安裝

一行指令，裝完就能用：

### Claude Code

```bash
curl -o ~/.claude/commands/oss.md https://raw.githubusercontent.com/maxihermit/gate-of-oss/main/oss.md
```

### OpenAI Codex

```bash
mkdir -p .codex/skills/oss
curl -o .codex/skills/oss/SKILL.md https://raw.githubusercontent.com/maxihermit/gate-of-oss/main/oss.md
```

### Cursor

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/oss.mdc https://raw.githubusercontent.com/maxihermit/gate-of-oss/main/oss.md
```

### GitHub Copilot

把 `oss.md` 的內容**追加**到 `.github/copilot-instructions.md` 的最後面。

### 其他平台

直接把 `oss.md` 的內容貼到對話裡，一樣能用。

## 怎麼用

```
/oss                          # 掃技術棧 → 搜 → 擺出來問你
/oss all                      # 你有 5 個專案？一次全掃
/oss audit owner/repo         # 老闆說要用這個套件，先幫我查一下會不會爆炸
/oss typescript               # 只看特定語言的
/oss 簡報工具                   # 我要做簡報，有沒有什麼好東西
/oss init                     # 告訴它你的專案在做什麼，推薦會更準
```

### 每天自動跑

在 Claude Code 裡說：

```
幫我建一個排程任務，每天早上跑 /oss all
```

從此每天早上打開電腦就有一份鑑定報告等你。比看新聞有用多了。

## 報告長什麼樣子

Phase 1（自動跑）：
```
我的寶庫裡有幾個東西可能跟 my-web-app 有關：

═══════════════════════════════════════
  my-web-app — React + Node.js 電商網站
═══════════════════════════════════════

  AI/LLM
  ─────────────────────────────────────
  ⭐ 15,234  vercel/ai — AI SDK，串流 + 重試
  ⭐  3,200  instructor-ai/instructor — 結構化 LLM 輸出

  UI
  ─────────────────────────────────────
  ⭐ 82,000  shadcn/ui — 可複製貼上的 UI 元件

  已排除：12 個（已在用、無授權、已封存）
═══════════════════════════════════════

有沒有感興趣的？指給我看，我去比對你的實際代碼。
```

你說「vercel/ai 看一下」，Phase 2（去讀代碼）：
```
  ┌─ vercel/ai — 做 AI 功能的 SDK
  │  ⭐ 15,234  MIT  2 天前更新  156 人貢獻
  │  SSR
  │
  │  你現在的代碼：自己寫 fetch 串 OpenAI，80 行，串流斷了沒重試
  │  用了之後：用 useChat hook，3 行，自動串流 + 重試
  │  值不值得：省 80 行 + 解決穩定性問題，遷移 30 分鐘
  │
  │  安全嗎：MIT ✓ | 漏洞: 0 | 有人維護 ✓
  └─────────────────────────────────────
```

你說「都不需要」：
```
═══════════════════════════════════════
  好，寶庫先收著。下週再看。
═══════════════════════════════════════
```

## 資安與隱私

- Phase 1 **只讀 package.json 這類設定檔**（便宜、快速）
- Phase 2 **你指定才讀代碼**（只讀你指到的那個領域的 2-3 個檔案）
- **不讀** .env、密碼、金鑰
- **不安裝**任何東西，只給你報告
- **不會把你的代碼傳到外面**（只有套件名稱會送到 GitHub/OSV API）
- 評估結果存在 `.oss-profile.yml`，下次不重複評估，你可以隨時刪
- 規則全寫在 `oss.md` 裡，你可以自己看完每一行

## GITHUB_TOKEN（選用）

不設也能用，但一小時只能搜 60 次。設了可以搜 5,000 次。如果你每天自動跑的話建議設一下。

```bash
export GITHUB_TOKEN=ghp_...
```

---

## English

> *"You dare develop without consulting the treasury? All the world's open-source treasures belong to me."*

An AI skill that searches GitHub and Hacker News for trending projects, shows you what's relevant to your stack, and — when you point at something — reads your actual code to tell you honestly whether it's worth using.

**It only reports. It never installs anything.**

### What makes this different

Most tools: scan your stack → dump a list of popular libraries → you figure out which ones matter.

This tool: scan your stack → show you what's out there → **you pick what's interesting** → it reads your code and tells you "worth it" or "your code is already fine."

**Two phases: cheap browsing, then targeted deep-dive only where you care.**

### How it works

1. **Phase 1** (cheap, always runs): reads tech stack → searches → presents a categorized menu → asks "anything interesting?"
2. **Phase 2** (you trigger): reads your actual code for the areas you pointed at → honest before/after verdict
3. Results saved to `.oss-profile.yml` so it never re-evaluates the same thing

### Install

**Claude Code:**
```bash
curl -o ~/.claude/commands/oss.md https://raw.githubusercontent.com/maxihermit/gate-of-oss/main/oss.md
```

**Codex:** `mkdir -p .codex/skills/oss && curl -o .codex/skills/oss/SKILL.md https://raw.githubusercontent.com/maxihermit/gate-of-oss/main/oss.md`

**Cursor:** `mkdir -p .cursor/rules && curl -o .cursor/rules/oss.mdc https://raw.githubusercontent.com/maxihermit/gate-of-oss/main/oss.md`

**Copilot:** Append `oss.md` contents to `.github/copilot-instructions.md`.

### Usage

```
/oss                          # Scan stack → show menu → you pick → deep verify
/oss all                      # Scan all your projects at once
/oss audit owner/repo         # "Boss wants this lib, check if it'll blow up"
/oss typescript               # Filter by language
/oss presentation tools       # Search specific topic
/oss init                     # Describe your project for better results
```

### Daily auto-scan

In Claude Code: `Help me create a scheduled task that runs /oss all every morning`

### Grades

| Grade | Meaning |
|-------|---------|
| SSR | Verified against your code. Solves a real problem. Migration justified. |
| SR | Likely helps after code review. Worth evaluating. |
| R | Has potential. Bookmark it. |
| N | Has concerns. Don't touch it yet. |
| Junk | Unsafe. Not even my dog would use it. |

### Security & Privacy

- Phase 1 only reads config files (package.json, etc.) — cheap and fast
- Phase 2 reads code **only for areas you point at** (2-3 files per area)
- Never reads `.env`, passwords, API keys
- Never installs anything (that's your call)
- Only package names go to external APIs (GitHub/OSV)
- Verdicts cached in `.oss-profile.yml` (deletable anytime)
- Full prompt in `oss.md` — read every line yourself

## License

MIT

---

*如果你笑出來了，請給一顆星星。If this made you laugh, please star it.*
