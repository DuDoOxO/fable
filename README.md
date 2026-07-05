# Claude Code 工作制度備份

2026-07-05 由 Fable 5 session 建立的跨專案工作制度，本 repo 是它的 git 備份。
活的版本在 `~/.claude/`（Claude Code 實際讀取的位置）；這裡是快照，改動請在活版本改完再同步過來 commit。

## 內容

| 目錄/檔案 | 對應的活版本位置 |
|---|---|
| `CLAUDE.global.md` | `~/.claude/CLAUDE.md`（全域路由與硬規則，每 session 自動載入） |
| `rules/` | `~/.claude/rules/`（診斷、調度、判斷、模板、維護、信、使用說明） |
| `commands/` | `~/.claude/commands/`（/hard、/second-opinion、/verify-done、/stuck） |
| `memory/` | `~/.claude/projects/-Users-enoch-fable/memory/` |

（各專案自己的 CLAUDE.md 不在本 repo——它們屬於各自的專案，跟著各專案的 git 走。）

## 還原（規則檔被改壞或搬新機器時）

```bash
cp CLAUDE.global.md ~/.claude/CLAUDE.md
cp rules/*.md ~/.claude/rules/
cp commands/*.md ~/.claude/commands/
```

## 同步備份（在活版本改了東西之後）

```bash
cd ~/fable
cp ~/.claude/CLAUDE.md CLAUDE.global.md && cp ~/.claude/rules/*.md rules/ && cp ~/.claude/commands/*.md commands/
git add -A && git commit -m "sync: 說明改了什麼"
```
