[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner — apps and four buddies](https://lazying.art/media/brand/lazyingart-intro.gif)](https://lazying.art)

# AWS Amplify Vue.js 入門模板

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

本儲存庫提供一個使用 Vue.js 與 AWS Amplify 建立應用程式的入門模板，重點在於簡化驗證、API 與資料庫功能的整合設定。

## 🎯 總覽

本模板提供一個與 AWS Amplify 整合的 Vue 基礎應用，並針對可擴充性與效能做了優化。對於想使用 Cognito、AppSync 與 DynamoDB 等預先設定好的 AWS 服務快速啟動專案的開發者來說，它是理想選擇。

在目前的儲存庫狀態下，應用包含兩條實際路徑：

1. 由 `src/App.vue` 呈現的主動、單頁式 `OnlyIdeas` 行銷／著陸頁體驗。
2. 保留但尚未在 `src/main.ts` 中接入的傳統／模板式已驗證與 CRUD 元件（如 `src/router/index.js`、`src/components/Ideas.vue`、`src/components/Creators.vue` 等），供後續擴展使用。

### 🧭 一覽

| 區域 | 當前狀態 |
| --- | --- |
| 前端執行進入點 | `src/main.ts` 直接掛載 `App.vue` |
| 後端技術棧 | Amplify Gen 2（`auth` + `data`） |
| 資料模型 | `Idea` 與 `Creator` |
| 驗證方式 | Email 登入 + Cognito User Pool 授權 |
| CI/CD | 透過 `amplify.yml` 的 Amplify Hosting 管線 |
| i18n README 檔案 | 已有語言連結；`i18n/` 下檔案待完成填寫 |

## ✨ 功能

- 🔐 **驗證**：使用 Amazon Cognito 建立安全的使用者驗證。
- ⚡ **API**：可直接使用的 AWS AppSync GraphQL 端點。
- 🗄️ **資料庫**：由 Amazon DynamoDB 提供的即時資料庫。
- 🧩 **Amplify Gen 2 後端定義**：
  - `amplify/auth/resource.ts` 定義 Email 登入。
  - `amplify/data/resource.ts` 定義具有擁有者授權的 `Idea` 與 `Creator` 模型。
- 🛠️ **可投入產品環境的前端工具鏈**：
  - Vue 3 + TypeScript + Vite。
  - 型別檢查與建置腳本（`vue-tsc` + Vite build）。
- 🚀 透過 `amplify.yml` 提供 **Amplify Hosting CI/CD 設定**。

## 🧱 專案結構

```text
onlyideas-aws/
├─ amplify/                    # Amplify Gen 2 backend definitions
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # deployment status snapshot
├─ i18n/                       # multilingual README targets (currently placeholders)
├─ public/
├─ src/
│  ├─ App.vue                  # active landing-page UI
│  ├─ main.ts                  # app bootstrap + Amplify.configure(outputs)
│  ├─ router/index.js          # legacy router flow (not mounted currently)
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # Amplify Hosting build/deploy pipeline
├─ package.json
└─ README.md
```

## 🧰 先決條件

- 建議使用 Node.js 20+（專案使用 `@tsconfig/node20` 與現代化 Vite 工具鏈）。
- npm 9+。
- 如需執行後端 sandbox 或部署流程，需有 AWS 帳號存取權限。

## 🧪 安裝

```bash
npm install
```

若你打算執行具備後端連動功能的應用，請先佈建或拉取 Amplify 後端輸出，以便 `amplify_outputs.json` 可供使用。

常見本機後端流程：

```bash
npx ampx sandbox
```

或是用於 CI 流程部署（`amplify.yml` 已設定）：

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ 使用方式

| 任務 | 指令 |
| --- | --- |
| 啟動本機開發 | `npm run dev` |
| 建置生產 bundle（含型別檢查） | `npm run build` |
| 預覽生產建置 | `npm run preview` |
| 僅執行型別檢查 | `npm run type-check` |

## 🔧 設定

### Amplify 執行時輸出

`src/main.ts` 會匯入 `../amplify_outputs.json` 並呼叫：

```ts
Amplify.configure(outputs)
```

若缺少 `amplify_outputs.json`，請先執行後端佈建／拉取。該檔案有意被加入 `.gitignore`（`amplify_outputs*`），因此每個環境都會產生自己的輸出。

### 後端 Schema 與授權

- 認證採用 Email（`amplify/auth/resource.ts`）。
- 目前的資料模型包含：
  - `Idea { content }`
  - `Creator { name, bio }`
- 預設授權模式為 Cognito 使用者池（`userPool`），採用以擁有者為基礎的規則。

### ⚙️ 環境變數

前端設定請使用 `VITE_` 前綴的 `.env` 金鑰。新增有型別的環境變數時，請同步更新 `env.d.ts`。

## 📦 範例

### 目前預設應用行為

目前應用入口直接掛載 `App.vue`：

```ts
createApp(App).mount('#app')
```

這代表落地頁介面就是預設的執行時體驗。

### 使用既有 CRUD 示範元件

像 `src/components/Ideas.vue` 與 `src/components/Creators.vue` 的元件已顯示 `generateClient<Schema>()` 與 `observeQuery()` 在 Amplify Data 的使用方式。

如果你想切換為基於路由的已驗證頁面，可將 `src/router/index.js` 再接回 `src/main.ts`。注意：這個路由守衛使用較舊的 `Auth.currentAuthenticatedUser()` 寫法，可能需要依你安裝的 Amplify 版本進行調整。

## 🚀 部署到 AWS

有關部署應用的詳細操作，請參考文件中的[部署章節](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws)。

此儲存庫也在 `amplify.yml` 中提供了 Amplify Hosting 建置設定。

## 🧑‍💻 開發說明

- `docs/amplify-status.md` 包含已上線應用（`onlyideas.art`）的營運快照。
- 後端／前端目錄中仍保留 `.old` 檔案；除非要刻意清理，否則建議作為歷史參考保留。
- `package.json` 目前尚未設定自動化測試框架。

手動驗證重點：

1. 驗證流程（若啟用/接上路由驗證 UI）。
2. `Idea` 與 `creator` 在 Amplify Data 上的 CRUD 行為。
3. 透過 `npm run build` 驗證生產建置完整性。

## 🐞 故障排除

- `Cannot find module '../amplify_outputs.json'`：
  - 在啟動應用前先透過 Amplify sandbox/pull 產生後端輸出。
- 已驗證路由無法運作：
  - `main.ts` 目前未掛載 `src/router/index.js`；在現有落地頁設定中這是預期行為。
- 建置失敗（型別）：
  - 執行 `npm run type-check`，並先修正 TypeScript 問題再進行打包。
- CI 後端部署異常：
  - 驗證 Amplify Hosting 環境變數中有無 `AWS_BRANCH` 與 `AWS_APP_ID`。

## 🗺️ 路線圖

1. 完成 `i18n/` 下的多語系 README 文件，與上方語言導覽連結一致。
2. 決定是否將基於路由的已驗證應用模式恢復為主線，或保留為傳統存檔模式。
3. 補齊自動化測試（建議：Vitest + Vue Test Utils）。
4. 清理 `.old` 過渡檔案與註解式教學腳手架，待不再需要後再移除。
5. 啟用 `docs/amplify-status.md` 中提到的額外生產防護（例如 AWS WAF）。

## 🤝 貢獻

歡迎貢獻。提交 Issue 或 Pull Request 前，請先閱讀 [CONTRIBUTING.md](CONTRIBUTING.md)。

本專案採用 [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct)。更多資訊請參閱 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。

## 🛡️ 安全性

更多資訊請參見 [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications)。

## 授權

本程式庫採用 MIT-0 授權。詳情請見 LICENSE 檔案。


## ❤️ Support

<!-- support:start -->
[![GitHub Sponsors](https://img.shields.io/badge/GitHub-Sponsors-EA4AAA?style=for-the-badge&logo=githubsponsors&logoColor=white)](https://github.com/sponsors/lachlanchen)
[![LazyingArt Support](https://img.shields.io/badge/LazyingArt-Support-0EA5E9?style=for-the-badge)](https://chat.lazying.art/donate)
[![PayPal Support](https://img.shields.io/badge/PayPal-Support-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen)
[![Stripe Support](https://img.shields.io/badge/Stripe-Support-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400)
<!-- support:end -->
