# tclaw.tw — 張桐嘉律師個人事務所網站

公開網站,GitHub Pages 自動部署。內容為法律知識科普,**無客戶資料**。

## 技術棧
- Jekyll 3.10.0(綁定 github-pages gem 232,與 GH Pages 環境一致)
- Ruby 3.1(CI workflow 指定)
- Markdown:kramdown 2.4.0(GFM input、auto_ids、rouge 3.30.0 高亮)
- Plugins:僅 `jekyll-feed`、`jekyll-sitemap`(github-pages 限制,**不可加自訂 plugin**)
- 無 npm/yarn,純 Ruby Jekyll
- Gemfile.lock 在 .gitignore(GH Pages 環境自行解析鎖版本)

## 目錄
- `_posts/` — 文章源檔
- `_layouts/` — `article.html`、`default.html`、`page.html`
- `_includes/` — `nav.html`、`footer.html`
- 根層靜態頁:`articles.html`、`search.html`(自製搜尋)、`disclaimer.md`、`privacy.md`、`404.html`
- `search-index.json` — 搜尋索引

## 文章規格

### 檔名
`_posts/YYYY-MM-DD-{slug}-legal-guide.md`
- slug:全小寫英文 + 連字號;統一以 `-legal-guide.md` 結尾
- 檔名 slug = URL slug(permalink `/articles/:title/`,`:title` 取自檔名)

### YAML front matter(必要欄位)
```yaml
---
title: "中文標題(含 SEO 鈎子,例如疑問句、數字、地名)"
description: "80 字以內;痛點 + 解法,首句能單獨上 SERP"
date: YYYY-MM-DD
category: 從下方既有 6 種挑選
---
```

### category 既有清單(2026-06-15 統計,共 39 篇)
| category | 篇數 |
|---|---|
| 不動產法律 | 13 |
| 強制執行 | 6 |
| 一般民事 | 5 |
| 車禍糾紛 | 4 |
| 勞資爭議 | 4 |
| 家事法 | 3 |
| 刑事辯護 | 3 |
| 財富傳承 | 1 |

⚠️ 分類用詞有歷史不一致(例:遺囑 / 特留分歸入「不動產法律」或「一般民事」,離婚歸「家事法」)。**新增文章請從上表 8 種挑選,勿自創新類**;若需新增分類,先盤點現有文章是否該重新分類。

「財富傳承」於 2026-06-15 刻意新增,為 tclaw.tw 財富傳承內容系列旗艦線(跨公司法/稅法/信託/繼承,不純屬家事或不動產)。後續同系列文章歸此類;既有遺囑/特留分/贈與子女文章是未來歸併候選(見下方技術債)。

### 分類重整 TODO(技術債)
現有分類有歷史誤分,建議擇期重整:
- 遺囑、特留分、繼承糾紛 → 應從「不動產法律」「一般民事」抽出,
  歸「家事法」或新增「繼承法」
- 離婚相關 → 確認是否誤歸「一般民事」
重整前需:(a) 盤點全部 32 篇 (b) 確認 URL 不變(category 不影響 permalink)
(c) 評估是否需新增「繼承法」分類
本對齊任務不順手做,記為待辦。

### 內文規格
- 長度:1500–2500 字
- 標題:H2(`##`)為主,必要時 H3(`###`);**無 H1**(留給 YAML title)、**無 emoji**
- 強調用 `**粗體**`,例如 `**地雷一:形式要件的微小瑕疵。**`
- 每段 3–5 句,避免整篇全條列
- E-E-A-T 標註(Google 品質訊號):
  - **Experience**:開頭需體現執業實務經驗,具體手法不限 — 以實例切入、引述常見諮詢情境、糾正普遍誤解、或直接揭示判決趨勢
  - 避免 AI 通用導語(「在現代社會中…」「隨著…」「許多人都會遇到…」)
  - **Expertise**:條文引用精準(例「民法第 1198 條」),不可虛構條號
  - **Authoritativeness**:關鍵主張附法源(條文 / 最高法院實務見解)
  - **Trustworthiness**:必要時提醒個案情形需專業評估,不下保證

### 內容紅線
- 不寫具體個案當事人(姓名、住址、可識別細節);舉例請通則化或匿名
- 不引用未公開或無法驗證的判決字號;引用務必真實可查(對齊全域 CLAUDE.md「法院字號真實性」)
- 不下絕對保證語(「一定贏」、「百分之百」)
- 法條提及採最新版本;修法頻繁領域(稅法、勞基法)應聯網查證
- ⚠️ 整站不用 emoji(品牌調性)

## 在地化策略
- 事務所定位高雄,文章在以下情境**自然點出地名**:
  - 涉及法院管轄(高雄地院、橋頭簡易庭、高雄高分院)
  - 涉及在地特有案件樣態(港都地產、加工出口區勞資)
  - 文末律師簡介或 CTA 區塊
- **不強迫每篇加「高雄」關鍵字**(會 over-optimization);以內容自然性為準,SEO 是副產物

## 發佈流程

### 本地預覽
```bash
bundle install              # 首次或 Gemfile 改動後
bundle exec jekyll serve    # http://127.0.0.1:4000
```

### 部署(自動)
- push `main` → GitHub Actions(`.github/workflows/jekyll.yml`)build + deploy 到 GitHub Pages
- 環境:Ubuntu + Ruby 3.1,`JEKYLL_ENV=production`
- ⚠️ **無 staging 分支,main 即正式環境**,推送前自行檢查

### 推送前檢查
- [ ] front matter 四欄位齊全(title / description / date / category)
- [ ] category 為既有 6 種之一
- [ ] description ≤ 80 字元
- [ ] 1500–2500 字、純 H2/H3、無 emoji
- [ ] 法條 / 判決字號已查證
- [ ] 無真實當事人資訊
- [ ] 本地 `jekyll serve` 預覽無 build error
- [ ] 圖片、內鏈、外鏈正常

## 與 content-pipeline skill 協作
撰寫新文章請先 invoke `content-pipeline` skill — 它涵蓋選題、痛點矩陣、長尾關鍵字、批量選題、Threads 拆稿等完整流程。
- 本 CLAUDE.md 為**規格約束**(寫成怎樣才合規)
- `content-pipeline` 為**流程工具**(從選題到發佈)

## 已知限制
- github-pages gem 限制可用 plugin;需要自訂功能要改用 GitHub Actions 自行 build
- `Gemfile.lock` 在 .gitignore — 本機 gem 升級不影響線上版本
- 自製搜尋(`search.html` + `search-index.json`),新增文章後須確認搜尋索引產生
