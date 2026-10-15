[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner — apps and four buddies](https://lazying.art/media/brand/lazyingart-intro.gif)](https://lazying.art)

# AWS Amplify Vue.js 入门模板

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

该仓库提供了一个使用 Vue.js 与 AWS Amplify 构建应用程序的入门模板，重点在于简化认证、API 与数据库能力的接入。

## 🎯 概览

该模板为你提供了一个与 AWS Amplify 集成的 Vue 基础应用，并针对可扩展性与性能进行了优化。它非常适合希望使用 Cognito、AppSync 和 DynamoDB 等预配置 AWS 服务快速启动项目的开发者。

在当前仓库状态下，应用包含两个实际方向：

1. 由 `src/App.vue` 渲染的活跃、单页式 `OnlyIdeas` 营销/落地页体验。
2. 保留但当前未在 `src/main.ts` 接入的传统/模板风格已认证与 CRUD 组件（如 `src/router/index.js`、`src/components/Ideas.vue`、`src/components/Creators.vue` 等），用于后续扩展。

### 🧭 一览

| 区域 | 当前状态 |
| --- | --- |
| 前端运行入口 | `src/main.ts` 直接挂载 `App.vue` |
| 后端技术栈 | Amplify Gen 2（`auth` + `data`） |
| 数据模型 | `Idea` 与 `Creator` |
| 认证方式 | 邮箱登录 + Cognito User Pool 授权 |
| CI/CD | 通过 `amplify.yml` 使用 Amplify Hosting 流水线 |
| i18n README 文件 | 已有语言链接；`i18n/` 下文件待完善 |

## ✨ 功能

- 🔐 **认证**：通过 Amazon Cognito 配置安全的用户认证。
- ⚡ **API**：可直接使用的 AWS AppSync GraphQL 端点。
- 🗄️ **数据库**：由 Amazon DynamoDB 提供的实时数据库能力。
- 🧩 **Amplify Gen 2 后端定义**：
  - `amplify/auth/resource.ts` 定义邮箱登录。
  - `amplify/data/resource.ts` 定义了带拥有者授权的 `Idea` 和 `Creator` 模型。
- 🛠️ **可用于生产环境的前端工具链**：
  - Vue 3 + TypeScript + Vite。
  - 类型检查与构建脚本（`vue-tsc` + Vite build）。
- 🚀 通过 `amplify.yml` 提供 **Amplify Hosting CI/CD 配置**。

## 🧱 项目结构

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

## 🧰 先决条件

- 推荐使用 Node.js 20+（项目使用 `@tsconfig/node20` 与现代 Vite 工具链）。
- npm 9+。
- 如需运行后端沙箱/部署流程，需要具备 AWS 账户访问权限。

## 🧪 安装

```bash
npm install
```

如果你计划运行带后端联动功能的应用，请先部署或拉取 Amplify 后端输出，以确保 `amplify_outputs.json` 可用。

常见本地后端流程：

```bash
npx ampx sandbox
```

或用于 CI 流水线部署（`amplify.yml` 已完成配置）：

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ 使用方法

| 任务 | 命令 |
| --- | --- |
| 启动本地开发 | `npm run dev` |
| 构建生产包（含类型检查） | `npm run build` |
| 预览生产构建 | `npm run preview` |
| 仅运行类型检查 | `npm run type-check` |

## 🔧 配置

### Amplify 运行时输出

`src/main.ts` 会导入 `../amplify_outputs.json` 并调用：

```ts
Amplify.configure(outputs)
```

如果缺少 `amplify_outputs.json`，请先执行后端部署或拉取。该文件被有意加入 `.gitignore`（`amplify_outputs*`），因此每个环境都会生成自己的输出。

### 后端 schema 与授权

- 认证基于邮箱（`amplify/auth/resource.ts`）。
- 当前的数据模型包括：
  - `Idea { content }`
  - `Creator { name, bio }`
- 默认授权模式为 Cognito 用户池（`userPool`），采用基于 owner 的规则。

### ⚙️ 环境变量

前端配置请使用带有 `VITE_` 前缀的 `.env` 键。新增有类型的环境变量时，请同步更新 `env.d.ts`。

## 📦 示例

### 当前默认应用行为

当前应用入口会直接挂载 `App.vue`：

```ts
createApp(App).mount('#app')
```

这意味着落地页 UI 是默认运行时体验。

### 使用现有的 CRUD 演示组件

仓库中的 `src/components/Ideas.vue` 和 `src/components/Creators.vue` 已展示 `generateClient<Schema>()` 与 `observeQuery()` 在 Amplify Data 中的使用。

如果你想切换到基于路由的已认证页面，可将 `src/router/index.js` 再接回 `src/main.ts`。注意：该路由守卫使用了较旧的 `Auth.currentAuthenticatedUser()` 风格，可能需要根据你安装的 Amplify 版本进行更新。

## 🚀 部署到 AWS

有关部署应用的详细说明，请参考文档中的[部署章节](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws)。

本仓库还在 `amplify.yml` 中包含 Amplify Hosting 构建配置。

## 🧑‍💻 开发说明

- `docs/amplify-status.md` 包含已托管应用（`onlyideas.art`）的运行快照。
- 后端/前端目录中存在旧的 `.old` 文件；除非有意清理，否则建议保留作历史参考。
- `package.json` 当前未配置自动化测试工具。

手动验收重点：

1. 认证流程（如果启用了路由接入或认证界面）。
2. `Idea` 与 `creator` 在 Amplify Data 上的 CRUD 行为。
3. 通过 `npm run build` 验证生产构建完整性。

## 🐞 故障排查

- `Cannot find module '../amplify_outputs.json'`：
  - 在启动应用前先通过 Amplify sandbox/pull 生成后端输出。
- 已认证路由不生效：
  - `main.ts` 目前没有挂载 `src/router/index.js`；在当前落地页配置下这是预期行为。
- 构建类型检查失败：
  - 执行 `npm run type-check`，并在打包前修复 TypeScript 问题。
- CI 后端部署异常：
  - 核验 Amplify Hosting 环境变量中是否存在 `AWS_BRANCH` 与 `AWS_APP_ID`。

## 🗺️ 路线图

1. 完成 `i18n/` 下的多语言 README 文件，使其与上方语言导航链接一致。
2. 决定是否将基于路由的已认证应用模式恢复为主线，或归档为传统版本。
3. 补充自动化测试（推荐：Vitest + Vue Test Utils）。
4. 清理过渡期 `.old` 文件和注释式教程脚手架，在不再需要后执行。
5. 启用 `docs/amplify-status.md` 中提到的额外生产加固（例如 AWS WAF）。

## 🤝 贡献

欢迎贡献。提交 issue 或 pull request 前，请先阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

该项目采用 [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct)。更多信息请查看 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。

## 🛡️ 安全

更多信息请参见 [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications)。

## 许可证

该库采用 MIT-0 许可。详情见 LICENSE 文件。


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
