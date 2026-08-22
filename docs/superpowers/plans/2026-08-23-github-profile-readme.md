# GitHub Profile README Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 建立 `DoubleChuang/DoubleChuang` GitHub profile repo,內含 Kazuhito00 風格的 README(project pages + 自動更新部落格文章)。

**Architecture:** 本機 `~/Code/doublechuang-profile/` 建立 repo,寫好 README.md 與 GitHub Actions workflow(blog-post-workflow,抓 `doublechuang.github.io/feed.xml` 更新 `<!-- BLOG-POST-LIST:START -->` 區塊),push 到 GitHub 後由使用者確認首頁渲染。

**Tech Stack:** Markdown、shields.io badges、GitHub Actions(gautamkrishnar/blog-post-workflow@v1)

## Global Constraints

- 使用者需先在 GitHub 網頁手動建立空 repo `DoubleChuang`(public,無 README、無 .gitignore、無 license)— 若尚未建立,Task 2 前須請使用者完成
- repo 必須與帳號同名(`DoubleChuang`)才會顯示為 profile README
- README 語言:繁中 + 英文雙語,每區塊中英對照
- Project 卡片:標題連結 + 說明 + shields.io 技術徽章(方案 C)
- Blog 來源 feed:https://doublechuang.github.io/feed.xml
- 聯絡徽章:GitHub、部落格主站(doublechuang.github.io)
- git commit 身分:name=`DoubleChuang`,email=`doublechuang@users.noreply.github.com`

---

### Task 1: 建立 README.md(含 blog workflow 佔位區塊)

**Files:**
- Create: `/Users/double/Code/doublechuang-profile/README.md`

**Interfaces:**
- Produces: `README.md`,內容含 `<!-- BLOG-POST-LIST:START -->` / `<!-- BLOG-POST-LIST:END -->` 註解,供 Task 2 的 workflow 改寫

- [ ] **Step 1: 撰寫 README.md**

內容如下(完整檔案):

```markdown
# DoubleChuang (Double) 🦔

> When I Wrote It, Only God and I Knew the Meaning; Now God Alone Knows

## Project Pages(專案頁面)

- [![price-libra](https://img.shields.io/badge/price--libra-比價網站-2ea44f)](https://doublechuang.github.io/price-libra/)
  台灣各賣場商品價格比較網站
  Price comparison website for Taiwanese retailers
  [![HTML](https://img.shields.io/badge/-HTML-E34F26?logo=html5&style=flat)](https://doublechuang.github.io/price-libra/)
- [![time-chamber](https://img.shields.io/badge/time--chamber-自由潛水-4e9a06)](https://doublechuang.github.io/time-chamber/)
  自由潛水呼吸訓練 — 雙曲線時間艙
  Freediving breathing trainer — Hyperbolic Time Chamber
  [![HTML](https://img.shields.io/badge/-HTML-E34F26?logo=html5&style=flat)](https://doublechuang.github.io/time-chamber/)
- [![doublechuang.github.io](https://img.shields.io/badge/doublechuang.github.io-部落格-4078c0)](https://doublechuang.github.io/)
  個人部落格與網站
  Personal blog and website
  [![Jekyll](https://img.shields.io/badge/-Jekyll-CC0000?logo=jekyll&style=flat)](https://doublechuang.github.io/)

## Recent Blog Posts(最新文章)

<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->

## SNS / 聯絡(Contact)

[![GitHub](https://img.shields.io/badge/-GitHub-181717?logo=github&style=flat)](https://github.com/DoubleChuang)
[![Blog](https://img.shields.io/badge/-Blog-4078c0?logo=jekyll&style=flat)](https://doublechuang.github.io/)

## Technologies(技術)

[![Python](https://img.shields.io/badge/-Python-f9d64e?logo=python&style=flat)](https://github.com/DoubleChuang?tab=repositories&q=&type=&language=Python)
[![HTML](https://img.shields.io/badge/-HTML-E34F26?logo=html5&style=flat)](https://github.com/DoubleChuang?tab=repositories&q=&type=&language=HTML)
[![C](https://img.shields.io/badge/-C-222222?logo=c&style=flat)](https://github.com/DoubleChuang?tab=repositories&q=&type=&language=C)
[![C++](https://img.shields.io/badge/-C++-00599C?logo=c%2B%2B&style=flat)](https://github.com/DoubleChuang?tab=repositories&q=&type=&language=C++)
[![Go](https://img.shields.io/badge/-Go-00ADD8?logo=go&style=flat)](https://github.com/DoubleChuang?tab=repositories&q=&type=&language=Go)
[![JavaScript](https://img.shields.io/badge/-JavaScript-3577c4?logo=javascript&style=flat)](https://github.com/DoubleChuang?tab=repositories&q=&type=&language=JavaScript)
[![Shell](https://img.shields.io/badge/-Shell-4EAA25?logo=gnubash&style=flat)](https://github.com/DoubleChuang?tab=repositories&q=&type=&language=Shell)
```

- [ ] **Step 2: 驗證 markdown 渲染**

Run: `npx -y markdownlint-cli2 README.md` 檢查語法(或至少用編輯器預覽,確認 badges URL 格式正確)
Expected: 無 lint error(或僅無關緊要警告);badge URL 為合法 `img.shields.io/badge/` 格式

- [ ] **Step 3: Commit**

```bash
git add README.md
git -c user.name=DoubleChuang -c user.email=doublechuang@users.noreply.github.com commit -m "feat: add profile README with project pages and blog section"
```

---

### Task 2: 建立 blog-post-workflow GitHub Actions

**Files:**
- Create: `/Users/double/Code/doublechuang-profile/.github/workflows/blog-post-workflow.yml`

**Interfaces:**
- Consumes: Task 1 的 `README.md`(含 `<!-- BLOG-POST-LIST:START/END -->` 註解)
- Produces: workflow 檔,push 到 GitHub 後會自動抓 feed 並更新 README

- [ ] **Step 1: 撰寫 workflow 檔**

```yaml
name: Latest blog post workflow
on:
  schedule:
    - cron: '0 0 * * *'
  workflow_dispatch:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  update-readme:
    name: Update this repo's README with latest blog posts
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: gautamkrishnar/blog-post-workflow@v1
        with:
          feed_list: "https://doublechuang.github.io/feed.xml"
```

- [ ] **Step 2: 驗證 YAML 語法**

Run: `ruby -e "require 'yaml'; YAML.load_file('.github/workflows/blog-post-workflow.yml'); puts 'YAML OK'"`(macOS 內建 ruby)
Expected: 輸出 `YAML OK`,無錯誤

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/blog-post-workflow.yml
git -c user.name=DoubleChuang -c user.email=doublechuang@users.noreply.github.com commit -m "ci: add blog post workflow to auto-update recent posts"
```

---

### Task 3: 推送並驗證

**Files:**
- Modify: 無(僅 git 操作)

**Interfaces:**
- Consumes: Task 1、2 的 commit
- Produces: GitHub repo `DoubleChuang/DoubleChuang` 上的 main branch

前置條件:**使用者已在 GitHub 網頁建立空 repo `DoubleChuang`**(若還沒建立,先暫停請使用者完成)

- [ ] **Step 1: 確認 remote 尚未設定**

Run: `git remote -v`
Expected: 無輸出或無 `origin`

- [ ] **Step 2: 加入 remote 並 push**

```bash
git remote add origin https://github.com/DoubleChuang/DoubleChuang.git
git branch -M main
git push -u origin main
```

Expected: push 成功,顯示 main → main

- [ ] **Step 3: 觸發 workflow 並確認**

Run: `git push` 已在 Step 2 觸發;請使用者到 https://github.com/DoubleChuang/actions 確認 `Latest blog post workflow` 執行成功(或手動點 Run workflow 觸發)

- [ ] **Step 4: 最終驗證**

請使用者開啟 https://github.com/DoubleChuang 確認:
1. README 顯示(名字、tagline、三個 project 卡片、SNS、Technologies)
2. Recent Blog Posts 區塊出現來自 feed.xml 的文章("TEST" 2019)
3. 三個 project 連結可點、badge 正常顯示
4. Actions 頁面 workflow 執行成功(無紅燈)

- [ ] **Step 5: 收尾 commit(若有 workflow 自動產生的 README 更新)**

若 Step 4 發現 workflow 已自動改寫 README,把變更 pull 回來並 commit(若無變更則跳過):

```bash
git pull --rebase origin main
git status --short
# 若上方有 README.md 變更:
git add README.md
git -c user.name=DoubleChuang -c user.email=doublechuang@users.noreply.github.com commit -m "docs: update blog posts via workflow"
git push
```