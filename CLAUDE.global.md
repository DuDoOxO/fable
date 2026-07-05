# 全域工作規則（所有專案共用）

這份只放路由和硬規則。細節在 `~/.claude/rules/`，**符合觸發條件才去讀對應檔案**，不要一次全讀。

## 硬規則（無條件遵守）

1. **回覆用繁體中文**，先講結果再講細節，短。程式碼與技術名詞保留英文。
2. **宣稱完成前必須驗證**：能實跑就實跑；不能實跑就寫明「請看 X 處，預期看到 Y」。禁止只說「應該可以了」。
3. **主對話不下場做大量讀取**：要掃 repo、讀 3 個以上檔案找東西、查網頁、同樣改動套到 4 個以上檔案 → 派 subagent，只收結論。
4. **進到沒有 CLAUDE.md 的專案**：完成當次任務後，把探索所得寫成該專案的 `CLAUDE.md`（≤30 行：技術棧、啟動指令、測試指令、目錄地圖、地雷）。
5. **同一個 bug 修兩輪沒進展** → 停止重試，換診斷方式（見 rules/judgment.md「方向錯了的訊號」）。
6. 改 `~/.claude/` 底下任何規則檔之前，先備份到 `~/.claude/rules/backups/`（見 rules/maintenance.md）。

## 路由（觸發條件 → 讀哪個檔）

| 情況 | 讀 |
|---|---|
| 要派 subagent、選 model、決定升降級 | `~/.claude/rules/delegation.md` |
| 拿捏「完成了嗎」「該問使用者嗎」「該換路嗎」「該升級模型嗎」 | `~/.claude/rules/judgment.md` |
| 撰寫派工 prompt（搜尋/實作/重構/研究/審查） | `~/.claude/rules/prompts.md` |
| 要修改 rules/ 或 CLAUDE.md 本身 | `~/.claude/rules/maintenance.md` |
| session 開場想了解這套制度為何存在、環境有什麼坑 | `~/.claude/rules/diagnosis.md` 與 `~/.claude/rules/letter.md` |

## 環境事實（2026-07-05 確認，過期就照 maintenance.md 更新）

- 使用者：Enoch，繁中，產品導向，多個小型專案（web 前後端、k8s）。驗收方式 = 打開畫面看。
- Rate limit 是真實約束：token 紀律 = 使用者每週可用工作量。省 token 的方式是派工與少讀，不是少驗證。
- Agent tool 的 model 參數：派工只從 `haiku`、`sonnet`、`opus` 三個選。enum 裡可能還看得到 `fable`，但那是 2026-07 一次性的短期方案，使用者環境長期沒有它——不要指定 `fable`，即使它看起來可用。**Agent tool 沒有 per-agent effort 參數**；effort 是全域設定（`settings.json` 的 `effortLevel`，使用者用 `/effort` 調）。
