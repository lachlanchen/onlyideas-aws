[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# AWS Amplify Vue.js 入門範本

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

此儲存庫提供一個使用 Vue.js 與 AWS Amplify 建立應用程式的入門範本，重點在於快速完成驗證、API 與資料庫能力的初始化設定。

## 概覽

此範本提供與 AWS Amplify 整合的 Vue 基礎應用，並針對可擴展性與效能進行簡化配置。它非常適合希望透過預先設定的 AWS 服務（例如 Cognito、AppSync 與 DynamoDB）快速啟動專案的開發者。

在目前儲存庫狀態下，應用包含兩條實用路線：

1. 由 `src/App.vue` 渲染、目前啟用的單頁 `OnlyIdeas` 行銷／落地頁體驗。
2. 傳統／範本風格的已驗證與 CRUD 元件（`src/router/index.js`、`src/components/Ideas.vue`、`src/components/Creators.vue` 等）；這些元件保留供擴充，但目前尚未在 `src/main.ts` 中接線。

### 快速一覽

| 區域 | 目前狀態 |
| --- | --- |
| 前端執行入口 | `src/main.ts` 直接掛載 `App.vue` |
| 後端堆疊 | Amplify Gen 2（`auth` + `data`） |
| 資料模型 | `Idea` 與 `Creator` |
| 驗證模式 | 電子郵件登入 + Cognito User Pool 授權 |
| CI/CD | 透過 `amplify.yml` 的 Amplify Hosting pipeline |
| i18n README 檔案 | 已有語言連結；`i18n/` 下檔案仍待完整填充 |

## 功能

- 🔐 **驗證**：以 Amazon Cognito 完成安全的使用者驗證設定。
- ⚡ **API**：可直接使用的 AWS AppSync GraphQL 端點。
- 🗄️ **資料庫**：由 Amazon DynamoDB 提供即時資料庫能力。
- 🧩 **Amplify Gen 2 後端定義**：
  - `amplify/auth/resource.ts` 定義電子郵件登入。
  - `amplify/data/resource.ts` 定義 `Idea` 與 `Creator` 模型，並含擁有者授權規則。
- 🛠️ **可用於生產環境的前端工具鏈**：
  - Vue 3 + TypeScript + Vite。
  - 型別檢查與建置腳本（`vue-tsc` + Vite build）。
- 🚀 透過 `amplify.yml` 提供 **Amplify Hosting CI/CD 設定**。

## 專案結構

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

## 先決條件

- 建議使用 Node.js 20+（專案使用 `@tsconfig/node20` 與現代 Vite 工具鏈）。
- npm 9+。
- 若要執行後端 sandbox／部署流程，需要可用的 AWS 帳號權限。

## 安裝

```bash
npm install
```

若你打算執行含後端連線功能的應用，請先佈建或拉取 Amplify 後端輸出，讓 `amplify_outputs.json` 可用。

常見的本機後端流程：

```bash
npx ampx sandbox
```

或者，用於 CI pipeline 部署（`amplify.yml` 已完成設定）：

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## 使用方式

| 任務 | 指令 |
| --- | --- |
| 啟動本機開發 | `npm run dev` |
| 建置生產版本（含型別檢查） | `npm run build` |
| 預覽生產建置 | `npm run preview` |
| 僅執行型別檢查 | `npm run type-check` |

## 設定

### Amplify 執行期輸出

`src/main.ts` 會匯入 `../amplify_outputs.json` 並呼叫：

```ts
Amplify.configure(outputs)
```

若缺少 `amplify_outputs.json`，請先執行後端佈建／拉取流程。此檔案會刻意被 gitignore（`amplify_outputs*`），因此每個環境都會產生自己的輸出。

### 後端 schema 與授權

- 驗證為電子郵件模式（`amplify/auth/resource.ts`）。
- 目前資料模型包含：
  - `Idea { content }`
  - `Creator { name, bio }`
- 預設授權模式為 Cognito user pool（`userPool`），搭配 owner-based 規則。

### 環境變數

前端設定請使用帶有 `VITE_` 前綴的 `.env` 鍵值。新增具型別的環境變數時，請同步更新 `env.d.ts`。

## 範例

### 目前預設應用行為

目前應用入口會直接掛載 `App.vue`：

```ts
createApp(App).mount('#app')
```

這表示落地頁 UI 是預設的執行期體驗。

### 使用既有 CRUD 示範元件

儲存庫中的元件（如 `src/components/Ideas.vue` 與 `src/components/Creators.vue`）已展示 `generateClient<Schema>()` 與 `observeQuery()` 在 Amplify Data 的使用方式。

若你想切換回以路由為基礎的已驗證頁面，可將 `src/router/index.js` 重新接回 `src/main.ts`。注意：此路由守衛採用較舊的 `Auth.currentAuthenticatedUser()` 風格，可能需要依你安裝的 Amplify 版本更新。

## 部署到 AWS

如需部署應用的詳細說明，請參考文件中的[部署章節](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws)。

此儲存庫亦在 `amplify.yml` 內提供 Amplify Hosting 建置設定。

## 開發備註

- `docs/amplify-status.md` 包含已託管應用（`onlyideas.art`）的運作狀態快照。
- 後端／前端目錄中存在舊版 `.old` 檔案；除非有意清理，否則請作為歷史參考保留。
- `package.json` 目前尚未配置自動化測試框架。

手動驗證建議重點：

1. 驗證流程（若啟用／接線路由驗證 UI）。
2. `Idea` 與 `Creator` 在 Amplify Data 上的 CRUD 行為。
3. 透過 `npm run build` 確認生產建置完整性。

## 疑難排解

- `Cannot find module '../amplify_outputs.json'`：
  - 啟動應用前，先透過 Amplify sandbox／pull 產生後端輸出。
- 已驗證路由無法運作：
  - `main.ts` 目前未掛載 `src/router/index.js`；在目前落地頁設定下這是預期行為。
- 建置在型別檢查失敗：
  - 先執行 `npm run type-check`，修正 TypeScript 問題後再打包。
- CI 後端部署異常：
  - 請確認 Amplify Hosting 環境變數中有 `AWS_BRANCH` 與 `AWS_APP_ID`。

## 路線圖

1. 完成 `i18n/` 下各語系 README，對齊上方語言導覽連結。
2. 決定是否將路由式已驗證應用模式恢復為主流程，或歸檔為 legacy。
3. 補上自動化測試（建議：Vitest + Vue Test Utils）。
4. 清理過渡期 `.old` 檔案與註解化教學骨架（確認不再需要後執行）。
5. 啟用 `docs/amplify-status.md` 所述的其他生產強化項目（例如 AWS WAF）。

## 貢獻

歡迎貢獻。提交 issue 或 pull request 前，請先閱讀 [CONTRIBUTING.md](CONTRIBUTING.md)。

本專案採用 [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct)。更多資訊請見 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。

## 安全性

更多資訊請參考 [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications)。

## 授權

本程式庫採用 MIT-0 授權。詳見 LICENSE 檔案。
