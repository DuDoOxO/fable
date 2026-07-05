# 快速診斷：這個 harness 的三大漏洞（2026-07-05，Fable 5 盤點）

依據：`~/.claude/history.jsonl` 875 條 prompts、15 個專案目錄、`~/.claude/settings.json`。
後面所有規則檔都是為了修這三個洞。改規則前先回來看這份，確認沒有偏離原始病因。

## 漏洞 1：零 CLAUDE.md → 每個 session 從頭探索 repo（最大 token 漏洞）

**證據**：抽查最常用的 5 個專案（men/checkin、zentier/customer-management、checkin、haez/passbye、mans/manner-maketh-man）都沒有 CLAUDE.md；user 層級也沒有。（2026-07-05 後續盤點：pod-web 有一份 2025-04 的舊 CLAUDE.md，是唯一例外；多數專案已由當日 session 補建，見 letter.md 交接區。）每個新 session 都要重新 ls、grep、讀檔才知道專案結構、怎麼跑、怎麼測。

**修法**：
1. user 層級 `~/.claude/CLAUDE.md` 已建立（路由到 `~/.claude/rules/`）。
2. **每次進入一個還沒有 CLAUDE.md 的專案，在完成當次任務後，主動花一次探索的成本把結果寫成該專案的 `CLAUDE.md`**（30 行以內：技術棧、怎麼啟動、怎麼測、目錄地圖、已知地雷）。跟使用者說一句「我順手建了 CLAUDE.md，之後的 session 會更快」即可，不用先問。
3. 探索用 Explore subagent 做，主對話只收結論（見 [delegation.md](delegation.md)）。

**判準**：一個專案的第二個 session 開場不應該再出現「讓我先看一下專案結構」的整輪探索。有，就是 CLAUDE.md 沒寫好或沒寫。

## 漏洞 2：主對話下場做大量讀取與 debug 迴圈 → context 膨脹、失焦

**證據**：歷史裡大量長迭代 debug（「還是一樣耶」「好了 但卡片還是一樣」連環）；使用者查 `/usage` 17 次、`/rate-limit-options` 6 次 —— rate limit 是真實約束，主對話每多讀一個整檔，就是使用者本週少一點可用額度。

**修法**：大量讀取、掃 repo、查網頁、批次改檔一律派 subagent，主對話只進結論。具體觸發條件與派工格式見 [delegation.md](delegation.md)。debug 超過兩輪沒進展視為「方向錯了」訊號，處理方式見 [judgment.md](judgment.md)。

**判準**：主對話單一回合讀超過 3 個完整檔案、或同一個 bug 修到第 3 輪還在原地，就是踩到這個洞。

## 漏洞 3：宣稱修好但沒驗證 → 使用者手動打臉、來回返工

**證據**：歷史模式高度重複 —— 模型說改好了 → 使用者開畫面看 → 「還是依樣耶 要重啟前端？」→ 再來一輪。每一輪返工 = 燒 token + 失焦 + 使用者信任下降。使用者的驗收方式就是打開畫面看，不是讀 diff。

**修法**：
1. 「完成」的定義不是「改完了」，是「驗過了」。前端改動至少要：確認 dev server 有熱重載或明講「需要重啟前端」；能實跑就實跑（`/verify` skill 或 curl / 開瀏覽器截圖）。
2. 不能實跑時，明確告訴使用者**驗證這一項要看哪裡、預期看到什麼**（一句話），讓一次人工驗證就足夠。
3. 完成判準的完整 rubric 見 [judgment.md](judgment.md) 的「何時算真的完成」。

**判準**：回覆裡出現「應該可以了」「這樣就會正常了」而沒有附驗證結果或明確驗證指引，就是違規。

## 附註：使用者輪廓（寫規則時的預設讀者之外，還要懂服務對象）

- 繁體中文、短訊息、產品導向；在乎「畫面上看起來對」多於程式碼優雅。
- 常直接貼 console 輸出或錯誤截圖，期待你自己判讀。
- 回覆風格：先講結果、短、避免術語轟炸；問題一次問清楚不要擠牙膏。
