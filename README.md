# Claude Code 工作制度（Fable 5 toolkit）

2026-07-05~06 由 Fable 5 session 建立的跨專案工作制度：把強模型的判斷方式外化成
規則、指令與範例，讓之後的一般模型（Sonnet/Opus/Haiku）照著跑就能逼近強模型的表現。
活的版本在 `~/.claude/`（Claude Code 實際讀取的位置）；本 repo 是它的 git 備份與使用手冊。

## 怎麼用

### 平常：什麼都不用做

`~/.claude/CLAUDE.md` 每個 session 自動載入，模型會自己：派 subagent 掃 repo、完成前驗證、
給「到哪裡看、預期看到什麼」的驗收指引。沒照做就打一句「看 CLAUDE.md 硬規則 N」。

### 需要判斷力的時候：六個斜線指令

| 情況 | 指令 |
|---|---|
| 開全新的專案 | `/newproject 想法` ——三問定需求、MVP 砍功能、沿用既有棧、mockup 先行、地基一次到位 |
| 想看懂一個專案的全貌 | `/archmap` ——三層架構圖 + 精簡說明 + 白話版（黃金範例在 examples/） |
| 困難或高風險任務開場 | `/hard 任務描述` ——先方案比較、先定驗收條件、最後獨立驗收 |
| 鬼打牆、越修越糟 | `/stuck` ——停止重試，先驗證改動有生效、再往上游二分定位 |
| 想要第二雙眼睛 | `/second-opinion` ——派 fresh opus 對抗性挑錯 |
| 懷疑「完成」沒真的好 | `/verify-done` ——派 fresh agent 獨立驗收，不自驗 |

### 前端 debug：`claude-debug` 開場（按需 MCP）

```bash
claude-debug   # = claude --mcp-config ~/.claude/mcp/debug.json
```

只在這個 session 載入 Chrome DevTools MCP——模型自己開頁面看 console、network、截圖，
不用人肉貼 log。平常打 `claude` 完全不載，零 context 成本。alias 在 `~/.zshrc`。
之後要加別的按需 MCP，複製這模式：`~/.claude/mcp/<用途>.json` + 一個 alias。

### 硬仗開場組合技

`/model opus` + `/effort high`（打完調回來省額度）＋ plan mode（Shift+Tab）。

## 內容對照表

| 本 repo | 活版本位置 |
|---|---|
| `CLAUDE.global.md` | `~/.claude/CLAUDE.md`（全域路由與硬規則） |
| `rules/` | `~/.claude/rules/`（診斷、調度、判斷、模板、維護、信、使用說明 for-enoch.md） |
| `commands/` | `~/.claude/commands/`（上表六個指令） |
| `mcp/` | `~/.claude/mcp/`（按需 MCP 設定） |
| `examples/` | /archmap 的黃金範例（Fable 5 親筆，指令要求動筆前先讀它模仿密度） |
| `memory/` | `~/.claude/projects/-Users-enoch-fable/memory/` |

（各專案自己的 CLAUDE.md 不在本 repo——它們跟著各專案的 git 走。）

## 還原（規則檔被改壞或搬新機器時）

```bash
cp CLAUDE.global.md ~/.claude/CLAUDE.md
cp rules/*.md ~/.claude/rules/
mkdir -p ~/.claude/commands ~/.claude/mcp && cp commands/*.md ~/.claude/commands/ && cp mcp/*.json ~/.claude/mcp/
grep -q claude-debug ~/.zshrc || printf '\nalias claude-debug="claude --mcp-config ~/.claude/mcp/debug.json"\n' >> ~/.zshrc
```

## 同步備份（在活版本改了東西之後）

```bash
cd ~/fable
cp ~/.claude/CLAUDE.md CLAUDE.global.md && cp ~/.claude/rules/*.md rules/ \
  && cp ~/.claude/commands/*.md commands/ && cp ~/.claude/mcp/*.json mcp/
git add -A && git commit -m "sync: 說明改了什麼" && git push
```

改制度檔案前先讀 `rules/maintenance.md`（備份、權限分級、行數上限、read-back 驗證）。
