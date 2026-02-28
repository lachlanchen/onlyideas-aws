[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# AWS Amplify Vue.js スターターテンプレート

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

このリポジトリは、Vue.js と AWS Amplify を使用したアプリケーションを作成するためのスターターテンプレートです。認証、API、データベース機能を簡単にセットアップできる点に重点を置いています。

## 概要

このテンプレートは、AWS Amplify と統合された Vue アプリケーションの基盤を提供し、スケーラビリティとパフォーマンスを意識して構成されています。Cognito、AppSync、DynamoDB などの AWS サービスがあらかじめ設定されているため、プロジェクトをすぐに開始したい開発者に最適です。

現在のリポジトリ状態では、アプリには実用的な 2 つのトラックがあります。

1. `src/App.vue` で描画される、稼働中の単一ページ `OnlyIdeas` マーケティング/ランディング体験。
2. 拡張用として残されているが、現状 `src/main.ts` には接続されていない、レガシー/テンプレート風の認証付きおよび CRUD コンポーネント（`src/router/index.js`、`src/components/Ideas.vue`、`src/components/Creators.vue` など）。

### 一目でわかる現状

| 項目 | 現在の状態 |
| --- | --- |
| フロントエンドのランタイムエントリ | `src/main.ts` が `App.vue` を直接マウント |
| バックエンド構成 | Amplify Gen 2（`auth` + `data`） |
| データモデル | `Idea` と `Creator` |
| 認証モード | メールログイン + Cognito User Pool 認可 |
| CI/CD | `amplify.yml` による Amplify Hosting パイプライン |
| i18n README ファイル | 言語リンクは存在するが、`i18n/` 配下のファイルは内容整備中 |

## 機能

- 🔐 **認証**: Amazon Cognito による安全なユーザー認証をセットアップ済み。
- ⚡ **API**: AWS AppSync の GraphQL エンドポイントをすぐに利用可能。
- 🗄️ **データベース**: Amazon DynamoDB を利用したリアルタイムデータベース。
- 🧩 **Amplify Gen 2 バックエンド定義**:
  - `amplify/auth/resource.ts` でメールログインを定義。
  - `amplify/data/resource.ts` でオーナー認可付きの `Idea` / `Creator` モデルを定義。
- 🛠️ **本番運用向けフロントエンドツールチェーン**:
  - Vue 3 + TypeScript + Vite。
  - 型チェックとビルドスクリプト（`vue-tsc` + Vite build）。
- 🚀 `amplify.yml` による **Amplify Hosting CI/CD 設定**。

## プロジェクト構成

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

## 前提条件

- Node.js 20 以上を推奨（プロジェクトは `@tsconfig/node20` と最新の Vite ツールチェーンを使用）。
- npm 9 以上。
- バックエンドの sandbox/deploy ワークフローを実行する場合は AWS アカウントへのアクセス。

## インストール

```bash
npm install
```

バックエンド接続機能を使ってアプリを実行する場合は、先に Amplify バックエンドの outputs を作成または取得し、`amplify_outputs.json` を用意してください。

ローカルでよく使うバックエンドワークフロー:

```bash
npx ampx sandbox
```

または CI パイプラインデプロイ（`amplify.yml` ですでに設定済み）の場合:

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## 使い方

| タスク | コマンド |
| --- | --- |
| ローカル開発を開始 | `npm run dev` |
| 本番バンドルをビルド（型チェックを含む） | `npm run build` |
| 本番ビルドをプレビュー | `npm run preview` |
| 型チェックのみ実行 | `npm run type-check` |

## 設定

### Amplify ランタイム出力

`src/main.ts` は `../amplify_outputs.json` をインポートし、次を呼び出します。

```ts
Amplify.configure(outputs)
```

`amplify_outputs.json` がない場合は、先にバックエンドのプロビジョニング/取得を実行してください。このファイルは意図的に gitignore（`amplify_outputs*`）されており、各環境でそれぞれ生成する想定です。

### バックエンドスキーマと認証

- 認証はメールベース（`amplify/auth/resource.ts`）。
- 現在のデータモデルは以下です。
  - `Idea { content }`
  - `Creator { name, bio }`
- 既定の認可モードは Cognito user pool（`userPool`）で、オーナーベースのルールを使用します。

### 環境変数

フロントエンド設定には、`VITE_` プレフィックス付きのキーを `.env` で使用します。新しい型付き環境変数を追加する場合は `env.d.ts` も更新してください。

## 例

### 現在のデフォルトアプリ挙動

現在のアプリエントリは `App.vue` を直接マウントします。

```ts
createApp(App).mount('#app')
```

このため、ランディングページ UI がデフォルトのランタイム体験になっています。

### 既存の CRUD デモコンポーネントを使う

`src/components/Ideas.vue` や `src/components/Creators.vue` などのリポジトリ内コンポーネントには、Amplify Data の `generateClient<Schema>()` と `observeQuery()` の利用例がすでに含まれています。

ルーターベースの認証ページに切り替えたい場合は、`src/router/index.js` を `src/main.ts` に再接続できます。注意: このルートガードは古い `Auth.currentAuthenticatedUser()` スタイルを使っており、インストール済みの Amplify バージョンに合わせた更新が必要になる場合があります。

## AWS へのデプロイ

アプリケーションをデプロイする詳細な手順は、ドキュメントの [deployment section](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) を参照してください。

このリポジトリには `amplify.yml` の Amplify Hosting ビルド設定も含まれています。

## 開発メモ

- `docs/amplify-status.md` には、ホスト済みアプリ（`onlyideas.art`）の運用スナップショットが含まれています。
- バックエンド/フロントエンドのディレクトリにレガシーな `.old` ファイルが存在します。意図的にクリーンアップしない限り、履歴参照として維持してください。
- 現在、`package.json` には自動テストハーネスが設定されていません。

手動確認で注力すべき領域:

1. 認証フロー（認証 UI を有効化/ルーター接続する場合）。
2. Amplify Data に対する Idea と Creator の CRUD 挙動。
3. `npm run build` による本番ビルド整合性。

## トラブルシューティング

- `Cannot find module '../amplify_outputs.json'`:
  - アプリ起動前に Amplify sandbox/pull でバックエンド outputs を生成してください。
- 認証付きルートが動作しない:
  - 現在 `main.ts` は `src/router/index.js` をマウントしていません。現行のランディングページ構成では想定どおりです。
- 型エラーでビルド失敗する:
  - バンドル前に `npm run type-check` を実行し、TypeScript の問題を修正してください。
- CI バックエンドデプロイの問題:
  - Amplify Hosting 環境変数で `AWS_BRANCH` と `AWS_APP_ID` が利用可能か確認してください。

## ロードマップ

1. 上記の言語ナビゲーションリンクに対応するよう、`i18n/` 配下の多言語 README を完成させる。
2. ルーターベースの認証アプリモードを主要構成として復元するか、レガシーとしてアーカイブするかを決定する。
3. 自動テストを追加する（推奨: Vitest + Vue Test Utils）。
4. 移行期の `.old` ファイルとコメント化されたチュートリアル足場コードを、不要になった時点で整理する。
5. `docs/amplify-status.md` に記載の追加の本番強化策（例: AWS WAF）を有効化する。

## コントリビュート

コントリビューションを歓迎します。issue や pull request を作成する前に [CONTRIBUTING.md](CONTRIBUTING.md) をご確認ください。

このプロジェクトは [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct) を採用しています。詳細は [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) を参照してください。

## セキュリティ

詳細は [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) を参照してください。

## ライセンス

このライブラリは MIT-0 ライセンスで提供されています。LICENSE ファイルを参照してください。
