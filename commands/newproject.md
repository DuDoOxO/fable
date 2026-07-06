---
description: 新專案開場協議——從模糊想法到可動工：三問定需求、MVP 砍功能、沿用既有棧、mockup 先行、地基一次到位
---

開一個新專案：$ARGUMENTS。嚴格照順序執行，第 0–2 步完成前**禁止寫任何實作 code**。

## 第 0 步：產品三問（一次問完，等回答）

1. 給誰用？（一種人就好）
2. 第一版只解決什麼**單一問題**？
3. 怎樣算成功——使用者打開畫面要看到什麼、做到什麼？

使用者答得模糊就給 2–3 個具體選項讓他挑，不要拿模糊需求開工。

## 第 1 步：MVP 切割（砍功能是價值，不是損失）

列出所有想像得到的功能 → 只留**一條主流程**（使用者從進來到得到價值的最短路徑）進第一版。
被砍的全部寫進 TODO.md 存檔，跟使用者確認：「第一版只做 X，Y/Z 之後再說，OK？」

## 第 2 步：技術選型（預設沿用既有棧，偏離要理由）

| 類型 | 預設棧 | 照抄結構的範例 repo |
|---|---|---|
| 後端 API | Go + Gin + fx + GORM（SQLite 起步）＋ 需要時 gqlgen | ~/Paraloka/ikigaido-backend、~/gozar/moodecho |
| Web 前端 | React + Vite + TS + Tailwind | ~/men/checkin |
| 手機 App | Flutter + Riverpod | ~/Paraloka/ikigaido |
| 爬蟲/工具 | Python | ~/coupon |
| 部署 | Dockerfile 單容器起步（Railway/Render），長大再 compose/k8s | ~/haez/passbye |

沿用棧 = 使用者既有的維護知識全部適用。想偏離：一句理由 + 問過使用者才准。
動手前派 Explore 去範例 repo 抄目錄結構與慣例，不要自己發明。

## 第 3 步：mockup 先行（有 UI 的專案必做）

先產純靜態、可點的 mockup.html 給使用者看過、改到點頭，才寫真 code。
使用者的驗收方式是打開畫面——mockup 上改一輪，比寫完 code 再改便宜十倍。

## 第 4 步：地基一次到位（第一天就做，欠著必還利息）

git init + .gitignore、README（一條指令跑起來）、**CLAUDE.md（出生就有）**（≤30 行，含近親 repo）、
最小可跑骨架 + 一條煙霧測試、Dockerfile。
驗收條件：`git clone` 後照 README 一條指令能跑起來看到畫面/回應。

## 第 5 步：進入正常開發

第一個里程碑照 /hard 的格式先寫驗收條件，實作照 delegation.md 派工。
