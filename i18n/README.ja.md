[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://lazying.art)

# AWS Amplify Vue.js スターターテンプレート

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

このリポジトリは、Vue.js と AWS Amplify を使ったアプリケーション作成のためのスターターテンプレートです。認証、API、データベース機能のセットアップを簡単に行えるように設計されています。

## 🎯 概要

このテンプレートは、AWS Amplify と統合された Vue の基盤アプリを提供し、拡張性とパフォーマンスを意識した構成になっています。Cognito、AppSync、DynamoDB などの AWS サービスが事前設定されているため、開発者がプロジェクトを素早く立ち上げるのに適しています。

現在のリポジトリ状態では、アプリは実用的に 2 つのトラックを含みます。

1. `src/App.vue` で描画される、稼働中のシングルページ `OnlyIdeas` マーケティング/ランディング体験。
2. `src/router/index.js`、`src/components/Ideas.vue`、`src/components/Creators.vue` などの既存認証＋CRUD コンポーネント。
   これらは拡張用途で残されており、現時点では `src/main.ts` には接続されていません。

### 🧭 サクッと確認

| 領域 | 現在の状態 |
| --- | --- |
| フロントエンド実行エントリ | `src/main.ts` が `App.vue` を直接マウント |
| バックエンド構成 | Amplify Gen 2（`auth` + `data`） |
| データモデル | `Idea` と `Creator` |
| 認証方式 | メールログイン + Cognito User Pool 認可 |
| CI/CD | `amplify.yml` による Amplify Hosting パイプライン |
| i18n README | 言語リンクは存在。`i18n/` 配下のファイルはまだ中身を埋める段階 |

## ✨ 機能

- 🔐 **認証**: Amazon Cognito による安全なユーザー認証をセットアップ。
- ⚡ **API**: AWS AppSync を使った GraphQL エンドポイントをすぐに利用可能。
- 🗄️ **データベース**: Amazon DynamoDB を活用したリアルタイムデータベース。
- 🧩 **Amplify Gen 2 バックエンド定義**:
  - `amplify/auth/resource.ts` でメールログインを定義。
  - `amplify/data/resource.ts` でオーナー認可付きの `Idea` と `Creator` モデルを定義。
- 🛠️ **本番運用を想定したフロントエンドツールチェーン**:
  - Vue 3 + TypeScript + Vite。
  - 型チェックとビルドのスクリプト（`vue-tsc` + Vite build）。
- 🚀 `amplify.yml` による **Amplify Hosting CI/CD 設定**。

## 🧱 プロジェクト構成

```text
onlyideas-aws/
├─ amplify/                    # Amplify Gen 2 バックエンド定義
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # デプロイ状況スナップショット
├─ i18n/                       # 多言語 README（README ファイル群）
├─ public/
├─ src/
│  ├─ App.vue                  # 稼働中のランディングページ UI
│  ├─ main.ts                  # アプリ起動 + Amplify.configure(outputs)
│  ├─ router/index.js          # レガシールート（現時点では未マウント）
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # Amplify Hosting のビルド/デプロイ定義
├─ package.json
└─ README.md
```

## 🧰 前提条件

- Node.js 20+（推奨、`@tsconfig/node20` と Vite のモダンツールチェーンを使用）。
- npm 9+。
- バックエンドの sandbox / deploy ワークフローを実行する場合は AWS アカウントへのアクセス。

## 🧪 インストール

```bash
npm install
```

バックエンド接続機能を使ってアプリを動かす場合は、まず Amplify バックエンドを provision するか pull して `amplify_outputs.json` を準備してください。

よく使われるローカル backend ワークフロー:

```bash
npx ampx sandbox
```

または CI のデプロイ（`amplify.yml` ですでに設定済み）:

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ 使い方

| タスク | コマンド |
| --- | --- |
| ローカル開発の開始 | `npm run dev` |
| 本番バンドルのビルド（型チェック含む） | `npm run build` |
| 本番ビルドのプレビュー | `npm run preview` |
| 型チェックのみ実行 | `npm run type-check` |

## 🔧 設定

### Amplify ランタイム出力

`src/main.ts` は `../amplify_outputs.json` をインポートし、次を呼び出します。

```ts
Amplify.configure(outputs)
```

`amplify_outputs.json` がない場合は、先にバックエンドのプロビジョニングまたは pull を実行してください。このファイルは意図的に gitignore（`amplify_outputs*`）対象になっており、各環境で個別に outputs が生成される想定です。

### バックエンドスキーマと認証

- 認証はメールベース（`amplify/auth/resource.ts`）。
- 現在のデータモデル:
  - `Idea { content }`
  - `Creator { name, bio }`
- デフォルトの認可方式は Cognito user pool（`userPool`）で、オーナーベースのルールを使用。

### ⚙️ 環境変数

フロントエンドの設定には `VITE_` プレフィックス付きのキーを `.env` に定義します。新しい型付き環境変数を追加する場合は `env.d.ts` も更新してください。

## 📦 例

### 現在のデフォルトアプリ挙動

現在のアプリエントリは `App.vue` を直接マウントします。

```ts
createApp(App).mount('#app')
```

そのため、ランディングページ UI が既定の実行体験になります。

### 既存 CRUD デモコンポーネントの利用

`src/components/Ideas.vue` や `src/components/Creators.vue` には、Amplify Data の `generateClient<Schema>()` と `observeQuery()` の使用例が既に含まれています。

ルーター構成の認証ページに切り替えたい場合は、`src/router/index.js` を再度 `src/main.ts` に接続できます。注: このルートガードは古い `Auth.currentAuthenticatedUser()` スタイルを使っているため、インストール済み Amplify バージョンに合わせた更新が必要になることがあります。

## 🚀 AWS へのデプロイ

アプリケーションのデプロイ手順は、ドキュメントの [deployment section](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) を参照してください。

このリポジトリには `amplify.yml` による Amplify Hosting のビルド設定も含まれています。

## 🧑‍💻 開発ノート

- `docs/amplify-status.md` には、公開中のアプリ（`onlyideas.art`）の運用状況スナップショットが含まれます。
- バックエンド/フロントエンド配下にレガシーな `.old` ファイルが残っています。意図的に整理しない限りは履歴参照として保持してください。
- 現在 `package.json` には自動テストハーネスがありません。

手動検証の重点領域:

1. 認証フロー（認証 UI を有効化／ルーター接続を行う場合）。
2. Amplify Data に対する Idea と Creator の CRUD 挙動。
3. `npm run build` による本番ビルドの整合性。

## 🐞 トラブルシューティング

- `Cannot find module '../amplify_outputs.json'`:
  - アプリ開始前に Amplify sandbox/pull を実行してバックエンド outputs を生成してください。
- 認証付きルートが動作しない:
  - 現在の `main.ts` は `src/router/index.js` をマウントしていないため、これは現行のランディングページ構成として想定内です。
- 型エラーでビルドが失敗する:
  - `npm run type-check` を実行し、TypeScript の問題を解消してからバンドルしてください。
- CI でバックエンドデプロイが失敗する:
  - Amplify Hosting の環境変数で `AWS_BRANCH` と `AWS_APP_ID` が設定されているか確認してください。

## 🗺️ ロードマップ

1. 上記の言語リンクに合わせて、`i18n/` 配下の多言語 README ファイルを完成させる。
2. ルーターベースの認証アプリモードを主要運用として復元するか、レガシーとして保存するかを決定する。
3. 自動テストを追加（推奨: Vitest + Vue Test Utils）。
4. 不要になった移行用 `.old` ファイルとコメント化されたチュートリアルの土台コードを整理する。
5. `docs/amplify-status.md` で示された追加の本番堅牢化項目（例: AWS WAF）を有効化する。

## 🤝 コントリビューション

コントリビューションは歓迎します。Issue や Pull Request を開く前に [CONTRIBUTING.md](CONTRIBUTING.md) をお読みください。

このプロジェクトは [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct) を採用しています。詳細は [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) を参照してください。

## 🛡️ セキュリティ

詳しくは [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) を参照してください。

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## License

このライブラリは MIT-0 ライセンスで提供されています。詳細は LICENSE ファイルをご確認ください。
