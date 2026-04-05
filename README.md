# 王之開源（Gate of Open Source）

![banner](assets/banner.png)

[繁體中文](#這是什麼) | [English](#english)

> *「你竟敢在沒有查閱寶庫的情況下開發？天下所有的開源寶物皆屬於我。」*

## 這是什麼

你是不是每次都在 Twitter 或 Hacker News 上偶然看到別人分享了一個超讚的開源工具，然後才發現「靠，這個東西三個月前就有了，我到底在幹嘛」？

這個 AI Skill 幫你解決這個問題。裝好之後打 `/oss`，它會自動去 GitHub 和 Hacker News 幫你找最近熱門的開源專案，跟你說哪些對你的專案有用，然後幫你做資安檢查。

**它只會給你報告，不會幫你安裝任何東西。畢竟王只鑑定，不親自動手。**

## 它怎麼運作

```
你正在開發專案，忙到沒空看 GitHub
      ↓
/oss 幫你去寶庫翻
      ↓
比對你的專案用了什麼技術、缺了什麼
      ↓
檢查安全性：授權條款、已知漏洞、有沒有人在維護
      ↓
給你鑑定結果
```

### 鑑定等級

| 等級 | 意思 |
|------|------|
| SSR | 直接解決你的問題，安全，不用馬上就是跟錢過不去 |
| SR | 值得花時間試用 |
| R | 有潛力，先加個書籤 |
| N | 有疑慮，先別碰，碰了可能會後悔 |
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
/oss                          # 幫你找有用的新工具
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

```
我的寶庫裡有幾個東西可能適合 my-web-app：

═══════════════════════════════════════
  my-web-app — 電商網站
═══════════════════════════════════════

  SSR  ⭐ 15,234  vercel/ai — 3 行就能做串流 AI 回應
  SR   ⭐ 82,000  shadcn/ui — 不用再自己刻 UI 元件

  ┌─ vercel/ai — 做 AI 功能的 SDK
  │  ⭐ 15,234  MIT  2 天前更新  156 人貢獻
  │  SSR
  │
  │  你現在：自己寫 fetch 串 OpenAI，80 行，串流不穩定
  │  用了之後：用 useChat hook，3 行，自動串流 + 重試
  │  差在哪：省 80 行程式碼，穩定性大幅提升
  │
  │  安全嗎：MIT ✓ | 漏洞: 0 | 有人維護 ✓
  └─────────────────────────────────────
```

## 資安與隱私

- 只讀 package.json 這類設定檔，**不讀你的程式碼**（我們對你的程式碼沒興趣）
- **不讀** .env、密碼、金鑰（我們又不是駭客）
- **不安裝**任何東西，只給你報告（安不安裝你自己決定）
- **不會把你的程式碼傳到外面**
- 規則全寫在 `oss.md` 裡，你可以自己看完每一行，不到 130 行

## GITHUB_TOKEN（選用）

不設也能用，但一小時只能搜 60 次。設了可以搜 5,000 次。如果你每天自動跑的話建議設一下。

```bash
export GITHUB_TOKEN=ghp_...
```

---

## English

> *"You dare develop without consulting the treasury? All the world's open-source treasures belong to me."*

An AI skill that searches GitHub and Hacker News for trending open-source projects, tells you which ones are useful for your project, and checks if they're safe to use.

**It only reports. It never installs anything.**

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
/oss                          # Find useful new tools
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
| SSR | Solves your problem. Safe. Use it or you're leaving money on the table. |
| SR | Worth trying. |
| R | Has potential. Bookmark it. |
| N | Has concerns. Don't touch it yet. |
| Junk | Unsafe. Not even my dog would use it. |

### Security & Privacy

- Only reads config files — never your source code (we don't care about your code)
- Never reads .env, passwords, API keys (we're not hackers)
- Never installs anything (that's your call)
- Never sends your code anywhere
- Full prompt is in `oss.md` — under 130 lines, read it yourself

## License

MIT

---

*如果你笑出來了，請給一顆星星。If this made you laugh, please star it.*
