[English](README.md) · [العربية](i18n/README.ar.md) · [Español](i18n/README.es.md) · [Français](i18n/README.fr.md) · [日本語](i18n/README.ja.md) · [한국어](i18n/README.ko.md) · [Tiếng Việt](i18n/README.vi.md) · [中文 (简体)](i18n/README.zh-Hans.md) · [中文（繁體）](i18n/README.zh-Hant.md) · [Deutsch](i18n/README.de.md) · [Русский](i18n/README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# AWS Amplify Vue.js Starter Template

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

This repository provides a starter template for creating applications using Vue.js and AWS Amplify, emphasizing easy setup for authentication, API, and database capabilities.

## 🎯 Overview

This template equips you with a foundational Vue application integrated with AWS Amplify, streamlined for scalability and performance. It is ideal for developers looking to jumpstart their project with pre-configured AWS services like Cognito, AppSync, and DynamoDB.

In its current repository state, the app includes two practical tracks:

1. An active, single-page `OnlyIdeas` marketing/landing experience rendered by `src/App.vue`.
2. Legacy/template-style authenticated and CRUD components (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue`, etc.) that are present for extension but are not currently wired in `src/main.ts`.

### 🧭 At a glance

| Area | Current state |
| --- | --- |
| Frontend runtime entry | `src/main.ts` mounts `App.vue` directly |
| Backend stack | Amplify Gen 2 (`auth` + `data`) |
| Data models | `Idea` and `Creator` |
| Auth mode | Email login + Cognito User Pool authorization |
| CI/CD | Amplify Hosting pipeline via `amplify.yml` |
| i18n README files | Language links present; files under `i18n/` are pending population |

## ✨ Features

- 🔐 **Authentication**: Setup with Amazon Cognito for secure user authentication.
- ⚡ **API**: Ready-to-use GraphQL endpoint with AWS AppSync.
- 🗄️ **Database**: Real-time database powered by Amazon DynamoDB.
- 🧩 **Amplify Gen 2 backend definitions**:
  - `amplify/auth/resource.ts` defines email login.
  - `amplify/data/resource.ts` defines `Idea` and `Creator` models with owner authorization.
- 🛠️ **Production-ready frontend toolchain**:
  - Vue 3 + TypeScript + Vite.
  - Type-check and build scripts (`vue-tsc` + Vite build).
- 🚀 **Amplify Hosting CI/CD config** via `amplify.yml`.

## 🧱 Project Structure

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

## 🧰 Prerequisites

- Node.js 20+ recommended (project uses `@tsconfig/node20` and modern Vite tooling).
- npm 9+.
- AWS account access if running backend sandbox/deploy workflows.

## 🧪 Installation

```bash
npm install
```

If you plan to run the app with backend-connected functionality, provision or pull Amplify backend outputs first so `amplify_outputs.json` is available.

Common local backend workflow:

```bash
npx ampx sandbox
```

Or, for CI pipeline deployments (already configured in `amplify.yml`):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ Usage

| Task | Command |
| --- | --- |
| Start local development | `npm run dev` |
| Build production bundle (includes type-check) | `npm run build` |
| Preview production build | `npm run preview` |
| Run type-check only | `npm run type-check` |

## 🔧 Configuration

### Amplify runtime outputs

`src/main.ts` imports `../amplify_outputs.json` and calls:

```ts
Amplify.configure(outputs)
```

If `amplify_outputs.json` is missing, run backend provisioning/pull first. The file is intentionally gitignored (`amplify_outputs*`) so each environment generates its own outputs.

### Backend schema and auth

- Auth is email-based (`amplify/auth/resource.ts`).
- Data models currently include:
  - `Idea { content }`
  - `Creator { name, bio }`
- Default authorization mode is Cognito user pool (`userPool`) with owner-based rules.

### ⚙️ Environment variables

Use `.env` with `VITE_`-prefixed keys for frontend configuration. Update `env.d.ts` when adding new typed env vars.

## 📦 Examples

### Current default app behavior

The current app entry mounts `App.vue` directly:

```ts
createApp(App).mount('#app')
```

This means the landing-page UI is the default runtime experience.

### Using existing CRUD demo components

Repository components like `src/components/Ideas.vue` and `src/components/Creators.vue` already show `generateClient<Schema>()` and `observeQuery()` usage for Amplify Data.

If you want to switch to router-based authenticated pages, you can wire `src/router/index.js` back into `src/main.ts`. Note: this route guard uses an older `Auth.currentAuthenticatedUser()` style and may need updates against your installed Amplify version.

## 🚀 Deploying to AWS

For detailed instructions on deploying your application, refer to the [deployment section](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) of our documentation.

This repo also includes Amplify Hosting build config in `amplify.yml`.

## 🧑‍💻 Development Notes

- `docs/amplify-status.md` contains an operational snapshot of the hosted app (`onlyideas.art`).
- Legacy `.old` files exist in backend/frontend directories; keep them as historical reference unless intentionally cleaned up.
- There is currently no automated test harness configured in `package.json`.

Manual verification focus areas:

1. Authentication flow (if enabling/router wiring auth UI).
2. Idea and creator CRUD behavior against Amplify Data.
3. Production build integrity via `npm run build`.

## 🐞 Troubleshooting

- `Cannot find module '../amplify_outputs.json'`:
  - Generate backend outputs with Amplify sandbox/pull before starting the app.
- Authenticated routes do not work:
  - `main.ts` currently does not mount `src/router/index.js`; this is expected in the present landing-page setup.
- Build fails on types:
  - Run `npm run type-check` and fix TypeScript issues before bundling.
- CI backend deploy issues:
  - Validate `AWS_BRANCH` and `AWS_APP_ID` are available in Amplify Hosting environment variables.

## 🗺️ Roadmap

1. Finalize multilingual README files under `i18n/` to match the language navigation links above.
2. Decide whether router-based authenticated app mode should be restored as primary, or archived as legacy.
3. Add automated tests (recommended: Vitest + Vue Test Utils).
4. Clean up transitional `.old` files and commented tutorial scaffolding once no longer needed.
5. Enable additional production hardening noted in `docs/amplify-status.md` (for example AWS WAF).

## 🤝 Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening issues or pull requests.

This project has adopted the [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct). For more information see [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## 🛡️ Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
