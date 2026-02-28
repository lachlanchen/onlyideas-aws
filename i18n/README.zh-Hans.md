[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# AWS Amplify Vue.js 入门模板

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

本仓库提供了一个用于构建 Vue.js 与 AWS Amplify 应用的入门模板，重点在于快速完成认证、API 和数据库能力的初始化。

## 概览

该模板为你提供了一个集成 AWS Amplify 的 Vue 基础应用，并针对可扩展性和性能进行了简化配置。它非常适合希望通过 Cognito、AppSync、DynamoDB 等预配置 AWS 服务快速启动项目的开发者。

在当前仓库状态下，应用包含两条实用路线：

1. 一个正在使用中的单页 `OnlyIdeas` 营销/落地页体验，由 `src/App.vue` 渲染。
2. 遗留/模板风格的认证与 CRUD 组件（`src/router/index.js`、`src/components/Ideas.vue`、`src/components/Creators.vue` 等）；这些组件可用于扩展，但目前尚未在 `src/main.ts` 中接入。

### 一览

| 区域 | 当前状态 |
| --- | --- |
| 前端运行时入口 | `src/main.ts` 直接挂载 `App.vue` |
| 后端栈 | Amplify Gen 2（`auth` + `data`） |
| 数据模型 | `Idea` 和 `Creator` |
| 认证模式 | 邮箱登录 + Cognito User Pool 授权 |
| CI/CD | 通过 `amplify.yml` 配置的 Amplify Hosting 流水线 |
| i18n README 文件 | 已有语言链接；`i18n/` 下文件待补全 |

## 功能特性

- 🔐 **认证**：使用 Amazon Cognito 快速搭建安全用户认证。
- ⚡ **API**：开箱即用的 AWS AppSync GraphQL 端点。
- 🗄️ **数据库**：基于 Amazon DynamoDB 的实时数据库。
- 🧩 **Amplify Gen 2 后端定义**：
  - `amplify/auth/resource.ts` 定义邮箱登录。
  - `amplify/data/resource.ts` 定义 `Idea` 与 `Creator` 模型，并采用 owner 授权。
- 🛠️ **生产级前端工具链**：
  - Vue 3 + TypeScript + Vite。
  - 类型检查与构建脚本（`vue-tsc` + Vite build）。
- 🚀 通过 `amplify.yml` 提供 **Amplify Hosting CI/CD 配置**。

## 项目结构

```text
onlyideas-aws/
├─ amplify/                    # Amplify Gen 2 后端定义
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # 部署状态快照
├─ i18n/                       # 多语言 README 目标目录（当前为占位）
├─ public/
├─ src/
│  ├─ App.vue                  # 当前启用的落地页 UI
│  ├─ main.ts                  # 应用引导 + Amplify.configure(outputs)
│  ├─ router/index.js          # 遗留路由流程（当前未挂载）
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # Amplify Hosting 构建/部署流水线
├─ package.json
└─ README.md
```

## 前置要求

- 建议 Node.js 20+（项目使用 `@tsconfig/node20` 与现代 Vite 工具链）。
- npm 9+。
- 如需运行后端 sandbox/部署流程，需要 AWS 账号权限。

## 安装

```bash
npm install
```

如果你计划运行连接后端功能的应用，请先预置或拉取 Amplify 后端输出文件，确保 `amplify_outputs.json` 可用。

常见本地后端工作流：

```bash
npx ampx sandbox
```

或者用于 CI 流水线部署（已在 `amplify.yml` 中配置）：

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## 使用

| 任务 | 命令 |
| --- | --- |
| 启动本地开发 | `npm run dev` |
| 构建生产包（包含类型检查） | `npm run build` |
| 预览生产构建 | `npm run preview` |
| 仅运行类型检查 | `npm run type-check` |

## 配置

### Amplify 运行时输出

`src/main.ts` 会导入 `../amplify_outputs.json` 并调用：

```ts
Amplify.configure(outputs)
```

如果缺少 `amplify_outputs.json`，请先执行后端预置/拉取。该文件已被有意加入 gitignore（`amplify_outputs*`），因此每个环境都会生成自己的输出。

### 后端 Schema 与认证

- 认证为邮箱方式（`amplify/auth/resource.ts`）。
- 当前数据模型包括：
  - `Idea { content }`
  - `Creator { name, bio }`
- 默认授权模式为 Cognito user pool（`userPool`），并使用基于 owner 的规则。

### 环境变量

前端配置请使用带 `VITE_` 前缀的 `.env` 键。新增类型化环境变量时，请同步更新 `env.d.ts`。

## 示例

### 当前默认应用行为

当前应用入口直接挂载 `App.vue`：

```ts
createApp(App).mount('#app')
```

这意味着落地页 UI 是默认运行时体验。

### 使用现有 CRUD 示例组件

仓库中的 `src/components/Ideas.vue` 和 `src/components/Creators.vue` 等组件，已经展示了 Amplify Data 的 `generateClient<Schema>()` 与 `observeQuery()` 用法。

如果你希望切换回基于路由的认证页面，可以把 `src/router/index.js` 重新接入 `src/main.ts`。注意：该路由守卫使用较旧的 `Auth.currentAuthenticatedUser()` 风格，可能需要根据你已安装的 Amplify 版本调整。

## 部署到 AWS

有关部署应用的详细说明，请参考文档中的 [deployment section](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws)。

本仓库也在 `amplify.yml` 中包含了 Amplify Hosting 构建配置。

## 开发说明

- `docs/amplify-status.md` 包含托管应用（`onlyideas.art`）的运行状态快照。
- 后端/前端目录中存在遗留 `.old` 文件；除非你有意清理，否则请保留作为历史参考。
- `package.json` 当前尚未配置自动化测试框架。

手动验证重点：

1. 认证流程（如果启用/重新接入基于路由的认证 UI）。
2. 面向 Amplify Data 的 Idea 与 Creator CRUD 行为。
3. 通过 `npm run build` 验证生产构建完整性。

## 故障排查

- `Cannot find module '../amplify_outputs.json'`：
  - 启动应用前，先通过 Amplify sandbox/pull 生成后端输出。
- 认证路由不可用：
  - `main.ts` 当前未挂载 `src/router/index.js`；在目前的落地页方案下这是预期行为。
- 构建类型失败：
  - 先执行 `npm run type-check`，修复 TypeScript 问题后再打包。
- CI 后端部署问题：
  - 检查 Amplify Hosting 环境变量中是否提供了 `AWS_BRANCH` 与 `AWS_APP_ID`。

## 路线图

1. 完善 `i18n/` 下的多语言 README 文件，使其与上方语言导航链接一致。
2. 决定是否将基于路由的认证应用模式恢复为主模式，或归档为遗留模式。
3. 添加自动化测试（推荐：Vitest + Vue Test Utils）。
4. 在不再需要后，清理过渡性的 `.old` 文件和注释化教程脚手架。
5. 启用 `docs/amplify-status.md` 中提到的更多生产加固项（例如 AWS WAF）。

## 贡献

欢迎贡献。提交 issue 或 pull request 前，请先阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

本项目采用了 [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct)。详见 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。

## 安全

更多信息请参见 [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications)。

## 许可证

本库采用 MIT-0 License。详见 LICENSE 文件。
