# GitHub Profile README (DoubleChuang/DoubleChuang)

日期:2026-08-23

## 目標

建立 GitHub 個人資料 README(repo `DoubleChuang/DoubleChuang`),仿照 Kazuhito00 的風格,
展示 project pages(price-libra、time-chamber、主站)與部落格(doublechuang.github.io)連結,
部落格最新文章透過 GitHub Actions 自動更新。

## 背景

- 目前 `DoubleChuang/DoubleChuang` repo 不存在(404),GitHub 首頁無 README
- `price-libra` repo(HTML)→ https://doublechuang.github.io/price-libra/「Price Libra - 比價網站」
- `time-chamber` repo(HTML)→ https://doublechuang.github.io/time-chamber/「Freediving - Hyperbolic Time Chamber」(Pause Breathe)
- `doublechuang.github.io` 是 Jekyll 部落格,有 RSS feed:https://doublechuang.github.io/feed.xml
- 部落格目前只有一篇 2019 年 "TEST" 文章,Recent Posts 初期只會顯示它

## 需求

1. 新建 GitHub profile repo `DoubleChuang/DoubleChuang`(需使用者在 GitHub 網頁手動建立,因本機無 gh CLI)
2. README 內容:繁中+英文雙語
3. 版面區塊(完整 Kazuhito00 風格):
   - 頂部:名字 + tagline「When I Wrote It, Only God and I Knew the Meaning; Now God Alone Knows」
   - Project Pages(專案頁面):price-libra、time-chamber、主站 doublechuang.github.io
     - 每個 project:標題連結 + 說明 + shields.io 技術徽章(方案 C)
   - Recent Blog Posts(最新文章):自動更新
   - SNS / 聯絡:GitHub 徽章 + 部落格主站徽章
   - Technologies(技術徽章):Python、HTML、C、Go、JavaScript 等(shields.io)
4. Blog 自動更新:
   - GitHub Actions 使用 `gautamkrishnar/blog-post-workflow`
   - feed 來源:https://doublechuang.github.io/feed.xml
   - 排程 + push 觸發,改寫 README 中 `<!-- BLOG-POST-LIST:START -->` 區塊
5. 本機 repo 位於 `~/Code/doublechuang-profile/`

## 版面結構

```markdown
# DoubleChuang 名字 + tagline

## Project Pages(專案頁面)
- price-libra — 比價網站 | [HTML badge] → link
- time-chamber — 自由潛水呼吸訓練 | [HTML badge] → link
- 主站 doublechuang.github.io — 部落格 & 個人網站 | [Jekyll badge] → link

## Recent Blog Posts(最新文章)
<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->

## SNS / 聯絡
- GitHub badge → https://github.com/DoubleChuang
- 部落格 badge → https://doublechuang.github.io/

## Technologies(技術徽章)
- Python、HTML、C、Go、JavaScript、Shell 等 shields.io badges
```

## 實作步驟

1. 使用者在 GitHub 網頁建立空 repo `DoubleChuang`(public, 無 README)
2. 本機 `~/Code/doublechuang-profile/`:
   - `README.md`:依上述版面撰寫
   - `.github/workflows/blog-post-workflow.yml`:blog-post-workflow 設定
3. `git init` + commit + push 到 `DoubleChuang/DoubleChuang`
4. 驗證:等 Actions 第一次跑完,確認 GitHub 首頁 README 渲染、Recent Blog Posts 出現 "TEST" 文章

## 驗證標準

- GitHub 首頁 https://github.com/DoubleChuang 顯示 README
- 三個 project 連結可點、徽章正常顯示
- Recent Blog Posts 區塊出現來自 feed.xml 的文章
- 工作流程檔無語法錯誤(可先 `actionlint` 或手動檢查)