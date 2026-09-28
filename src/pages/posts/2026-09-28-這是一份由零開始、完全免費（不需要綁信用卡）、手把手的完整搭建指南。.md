---
layout: ../../layouts/PostLayout.astro
title: 這是一份由零開始、完全免費（不需要綁信用卡）、手把手的完整搭建指南。
pubDate: 2026-09-28
description: free webpage
tags:
  - webpage
  - free webpage
---

這是一份**由零開始、完全免費（不需要綁信用卡）、手把手**的完整搭建指南。

完成之後，你會擁有：

1. **GitHub 倉庫**：存放你的網站所有原始碼與筆記。
2. **Cloudflare Pages**：全球加速的免費靜態託管（自動給予 `xxx.pages.dev` 網址）。
3. **Astro 專案**：SEO 滿分、速度極快的內容引擎。
4. **Sveltia CMS**：像 WordPress 一樣的視覺化寫作後台（只需打開 `your-site.pages.dev/admin` 即可寫文章或貼入 AI 生成的 Markdown）。

### 第一階段：註冊 2 個必備的免費帳號（5 分鐘）

#### 步驟 1：註冊 GitHub 帳號

GitHub 用來存放網站代碼與文章，所有雲端部署都會跟它連動。

1. 前往[github.com](https://github.com/?utm_source=gemini)點擊 **Sign up**。
2. 輸入你的 Email、設定密碼、輸入使用者名稱（Username）。
3. 完成郵件驗證碼確認，進入 GitHub 首頁。

#### 步驟 2：註冊 Cloudflare 帳號

Cloudflare 負責免費幫你構建網站、分配免費網址、提供全球 CDN 與防護。

1. 前往[cloudflare.com](https://www.cloudflare.com/?utm_source=gemini)點擊 **Sign Up**。
2. 輸入 Email 與設定密碼。
3. 收驗證電郵，點擊連結完成驗證。

### 第二階段：在 GitHub 建立 Astro 專案（10 分鐘，免裝本地環境）

如果你不想在本地電腦裝 Node.js、Terminal 敲代碼，最快、最不易出錯的方法是**直接在 GitHub 透過網頁版編輯器初始化**：

#### 步驟 3：建立新的 GitHub Repository

1. 登入 GitHub，右上角點擊 **「+」** $\rightarrow$ **New repository**。
2. 設定：
    - **Repository name**: 例如 `ai-research-hub`（純英文小寫與減號）。
    - **Public / Private**: 建議選 **Public**（或者 Private 都可以，Cloudflare 免費層都支援）。
    - 勾選 **Add a README file**。
3. 點擊綠色按鈕 **Create repository**。

#### 步驟 4：建立 Astro 基本結構

在剛建好的倉庫頁面，**直接按一下鍵盤上的 `.`（點號鍵）**，瀏覽器會立即啟動網頁版 VS Code。

在左側檔案樹依序新建以下 3 個核心檔案：

1. **新建 `package.json`**（在根目錄）：
JSON{
  "name": "ai-research-hub",
  "type": "module",
  "version": "0.0.1",
  "scripts": {
    "dev": "astro dev",
    "start": "astro dev",
    "build": "astro build",
    "preview": "astro preview"
  },
  "dependencies": {
    "astro": "^4.15.0"
  }
}

2. **新建 `src/pages/index.astro`**（先建立 `src` 資料夾，再建 `pages` 資料夾，再建檔案）：
HTML---
// src/pages/index.astro
const posts = await Astro.glob('./posts/\*.md');
---
<html lang="zh-HK">
  <head>
    <meta charset="utf-8" />
    <title>AI 研究與里程碑</title>
    <style>
      body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; max-width: 800px; margin: 40px auto; padding: 0 20px; line-height: 1.6; color: #222; }
      h1 { border-bottom: 2px solid #eee; padding-bottom: 10px; }
      .post-card { border: 1px solid #e1e4e8; border-radius: 8px; padding: 16px; margin-bottom: 16px; }
      .post-card a { text-decoration: none; color: #0366d6; font-size: 1.2rem; font-weight: bold; }
      .post-card p { margin: 8px 0 0; color: #555; }
      .admin-btn { display: inline-block; background: #000; color: #fff; padding: 8px 14px; border-radius: 6px; text-decoration: none; margin-bottom: 20px; }
    </style>
  </head>
  <body>
    <a href="/admin/" class="admin-btn">進入 CMS 發布後台</a>
    <h1>AI 研究筆記與個人里程碑</h1>
    <div>
      {posts.map(post => (
        <div class="post-card">
          <a href={post.url}>{post.frontmatter.title}</a>
          <p>{post.frontmatter.description}</p>
          <small>{post.frontmatter.pubDate} · 標籤: {post.frontmatter.tags?.join(', ')}</small>
        </div>
      ))}
    </div>
  </body>
</html>

3. **新建第一篇測試文章 `src/pages/posts/first-post.md`**：
Markdown---
title: "我的第一篇 AI 研究筆記"
pubDate: "2026-09-28"
description: "測試 Cloudflare Pages 與 Astro 的連動發布。"
tags: ["AI", "Astro"]
layout: "../../layouts/PostLayout.astro"
---

# 歡迎來到我的 AI 研究站

這是第一篇測試筆記。之後可以透過後台直接新增文章，或將 AI 對話輸出的 Markdown 貼進來！

4. **新建文章排版樣板 `src/layouts/PostLayout.astro`**（先建立 `layouts` 資料夾）：
HTML---
const { frontmatter } = Astro.props;
---
<html lang="zh-HK">
  <head>
    <meta charset="utf-8" />
    <title>{frontmatter.title}</title>
    <style>
      body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; max-width: 800px; margin: 40px auto; padding: 0 20px; line-height: 1.7; color: #222; }
      a { color: #0366d6; }
      pre { background: #f6f8fa; padding: 16px; border-radius: 6px; overflow-x: auto; }
    </style>
  </head>
  <body>
    <a href="/">← 返回首頁</a>
    <h1>{frontmatter.title}</h1>
    <p style="color: #666;">發布日期：{frontmatter.pubDate}</p>
    <hr/>
    <article>
      <slot />
    </article>
  </body>
</html>


#### 步驟 5：保存並提交到 GitHub

在網頁版編輯器左邊側邊欄點擊 **Source Control（分支圖示）** $\rightarrow$ 輸入訊息 `init astro project` $\rightarrow$ 點擊 **Commit & Push**（打勾按鈕）。

### 第三階段：在 Astro 中加入 Sveltia CMS 後台（5 分鐘）

我們利用完全免費、免伺服器的 **Sveltia CMS**，在網站建立一個 `/admin` 管理頁面。

在網頁版編輯器建立兩個檔案：

1. **新建 `public/admin/index.html`**：
HTML<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>內容管理後台</title>
    <!-- 載入 Sveltia CMS 核心腳本 -->
    <script src="https://unpkg.com/@sveltia/cms/dist/sveltia-cms.js" type="module"></script>
  </head>
  <body></body>
</html>

2. **新建 `public/admin/config.yml`**：
_(記得將裡面的 `<你的GitHub帳號>` 和 `<你的Repo名稱>` 替換成你剛剛建立的名稱)_
YAMLbackend:
  name: github
  repo: <你的GitHub帳號>/<你的Repo名稱>
  branch: main

media_folder: "public/images"
public_folder: "/images"

collections:
  - name: "posts"
    label: "AI 研究文章"
    folder: "src/pages/posts"
    create: true
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
    fields:
      - { label: "排版模板", name: "layout", widget: "hidden", default: "../../layouts/PostLayout.astro" }
      - { label: "文章標題", name: "title", widget: "string" }
      - { label: "發布日期", name: "pubDate", widget: "datetime", date_format: "YYYY-MM-DD", time_format: false }
      - { label: "文章簡介 (SEO)", name: "description", widget: "string" }
      - { label: "標籤 (Tags)", name: "tags", widget: "list" }
      - { label: "正文內容 (Markdown)", name: "body", widget: "markdown" }

3. **再次 Commit & Push** 提交代碼。

### 第四階段：連接 Cloudflare Pages 免費託管（3 分鐘）

1. 登入[Cloudflare Dashboard](https://dash.cloudflare.com/?utm_source=gemini)。
2. 在左側選單點擊 **Compute (Workers) > Workers & Pages**。
3. 點擊右上角 **Create application** $\rightarrow$ 選擇 **Pages** 分頁。
4. 點擊 **Connect to Git**（連接 GitHub）。
5. 授權並選擇剛才建立的 Repository（例如 `ai-research-hub`），點擊 **Begin setup**。
6. 設定構建參數：
    - **Project name**: 保持預設（這會決定你的 `xxx.pages.dev` 網址名稱）。
    - **Framework preset**: 下拉選單選擇 **Astro**。
    - **Build command**: 自動填入 `npm run build`（如無則手動填寫）。
    - **Build output directory**: 自動填入 `dist`（如無則手動填寫）。
7. 點擊 **Save and Deploy**。
8. 稍等約 1 分鐘，構建成功後，頁面上方會顯示一個成功的綠色勾號，並附上你的免費永久網址：
`[https://ai-research-hub.pages.dev](https://ai-research-hub.pages.dev)`。

### 第五階段：登入 CMS 後台發布文章

1. 打開瀏覽器，造訪你的後台網址：
`https://<你的專案名稱>.pages.dev/admin/`
2. 畫面會出現登入按鈕，點擊 **Sign in with GitHub** 授權。
3. 登入後你便會看到「AI 研究文章」管理面板：
    - 點擊 **New AI 研究文章**。
    - 把文章標題、發布日期填上。
    - **正文內容**：直接把 Gemini 產出的 Markdown 粘貼進去。
    - 點擊右上角 **Publish**（發布）。
4. Sveltia CMS 會自動將檔案透過 API 提交回你的 GitHub，Cloudflare Pages 會在背後自動觸發構建，1 分鐘後你的主站就會刷新顯示新文章！

### 日後運作工作流小結

Plaintext

```plain
對話叫 Gemini 按規範出 Astro Markdown ──> 打開 /admin 貼上發布 ──> Cloudflare 自動上線

```

整個流程不需要伺服器月費、不需要碰複雜的指令碼，隨時可以流暢記錄研究與里程碑。日後如果想綁定自訂 Domain，直接在 Cloudflare Pages 後台的 **Custom domains** 點擊加進去即可。
