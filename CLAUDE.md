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
description: "100 字以內;痛點 + 解法,首句能單獨上 SERP(中文全形約 40 字後易被截,關鍵資訊放前面)"
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
- **硬性檢查:每個 H2 對應一個獨立的搜尋意圖。** 答完該意圖就收,不為湊長度展開;
  兩個 H2 若會被同一組關鍵字命中,合併或砍掉一個。
  推送前一律在內部列出「H2 → 目標搜尋詞」對照表自檢;**正文超過 4,000 字時,
  把這張表輸出給使用者**供人工判斷。字數超標不傷讀者,同一件事講兩遍才傷。
- 長度(觀察指標,非放行門檻):一般文章 2,500–4,000 字;
  多爭點、跨階段、開類別首篇等複雜主題上限 5,000 字。
  **超過 5,000 字不放行,改為評估拆分**(此條是門檻)。
- 字數定義(避免規格再次失效):正文(不含 front matter),去掉 markdown 連結語法
  與 `#*` 等標記符號後的字元數,**含中文標點**。一行指令:
  `python -c "import io,re,sys; s=io.open(sys.argv[1],encoding='utf-8').read().split('---',2)[2]; s=re.sub(r'\[([^\]]*)\]\([^)]*\)',r'\1',s); print(len(re.sub(r'[#*\`>\-\s]','',s)))" _posts/檔名.md`
- ⚠️ 舊規格為「1500–2500 字」,實際近八篇為 3,033–6,683 字,已於 2026-08-24 依實況修正。
  `2026-07-04-workplace-bullying`(6,683)、`2026-07-11-nominee-registration`(5,268)
  兩篇超過 5,000 字上限,列為 **legacy 例外**,新文適用新規;要不要回頭拆分未決,不順手做。
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

### 法規查證來源路由
| 查什麼 | 去哪查 |
|---|---|
| 法律、法規命令(民法、消保法、各種辦法) | taiwan-legal-db MCP |
| **定型化契約應記載及不得記載事項、行政指導、預告修正** | **內政部主管法規共用系統 / 行政院定型化契約專頁,MCP 不適用** |
| 兩者皆查無 | 標【資料不足,無法確認】 |

⚠️ **不得以「MCP 查無」推論該規定不存在。** 應記載事項是行政公告,不在全國法規
資料庫的法律/命令層(實測 `search_regulations("預售屋")` 只回傳兩部辦法,
「預售屋買賣定型化契約」為 0 筆)。把 MCP 當唯一入口會把「查不到」誤判成
「沒這規定」,誤判成本最高。

預售屋履約擔保修正的追蹤見 `_drafts/TODO-presale-escrow-amendment-tracking.md`;
民法 1223 特留分修法見 `_drafts/TODO-1223-amendment-tracking.md`。

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
- [ ] description ≤ 100 字元
- [ ] 正文 2,500–4,000 字(複雜主題上限 5,000;超過則評估拆分)、純 H2/H3、無 emoji
- [ ] 每個 H2 對應一個獨立搜尋意圖，無兩個 H2 撞同一組關鍵字(超過 4,000 字需輸出對照表)
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
