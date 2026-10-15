[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner — apps and four buddies](https://lazying.art/media/brand/lazyingart-intro.gif)](https://lazying.art)

# AWS Amplify Vue.js 스타터 템플릿

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

이 저장소는 Vue.js와 AWS Amplify로 애플리케이션을 만드는 스타터 템플릿으로, 인증·API·데이터베이스 기능을 빠르게 구성할 수 있도록 설계되었습니다.

## 🎯 개요

이 템플릿은 확장성과 성능을 고려해 최적화된, AWS Amplify가 통합된 Vue 기반 기본 앱을 제공합니다. Cognito, AppSync, DynamoDB 같은 사전 구성 AWS 서비스를 활용해 프로젝트를 빠르게 시작하려는 개발자에게 적합합니다.

현재 저장소 상태에서는 앱이 다음 두 가지 실용적인 트랙을 포함합니다.

1. `src/App.vue`로 렌더링되는 현재 동작 중인 단일 페이지 `OnlyIdeas` 마케팅/랜딩 경험.
2. `src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue` 등과 같은 기존 인증/CRUD 컴포넌트(레거시/템플릿 형식).
   이 컴포넌트들은 확장용으로 남아 있으며 현재는 `src/main.ts`에 연결되어 있지 않습니다.

### 🧭 한눈에 보기

| 영역 | 현재 상태 |
| --- | --- |
| 프런트엔드 런타임 엔트리 | `src/main.ts`가 `App.vue`를 직접 마운트 |
| 백엔드 스택 | Amplify Gen 2 (`auth` + `data`) |
| 데이터 모델 | `Idea`와 `Creator` |
| 인증 방식 | 이메일 로그인 + Cognito User Pool 인증 |
| CI/CD | `amplify.yml` 기반 Amplify Hosting 파이프라인 |
| i18n README 파일 | 언어 링크 존재; `i18n/` 하위 파일은 아직 채움 대기 |

## ✨ 주요 기능

- 🔐 **인증**: Amazon Cognito 기반의 안전한 사용자 인증 설정.
- ⚡ **API**: AWS AppSync 기반 즉시 사용 가능한 GraphQL 엔드포인트.
- 🗄️ **데이터베이스**: Amazon DynamoDB 기반 실시간 데이터베이스.
- 🧩 **Amplify Gen 2 백엔드 정의**:
  - `amplify/auth/resource.ts`에서 이메일 로그인 정의.
  - `amplify/data/resource.ts`에서 owner 권한을 사용하는 `Idea`와 `Creator` 모델 정의.
- 🛠️ **운영 준비형 프런트엔드 툴체인**:
  - Vue 3 + TypeScript + Vite.
  - 타입 검사 및 빌드 스크립트 (`vue-tsc` + Vite build).
- 🚀 `amplify.yml` 기반 **Amplify Hosting CI/CD 설정**.

## 🧱 프로젝트 구조

```text
onlyideas-aws/
├─ amplify/                    # Amplify Gen 2 백엔드 정의
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # 배포 상태 스냅샷
├─ i18n/                       # 다국어 README 대상(현재는 placeholder)
├─ public/
├─ src/
│  ├─ App.vue                  # 현재 동작 중인 랜딩 페이지 UI
│  ├─ main.ts                  # 앱 부트스트랩 + Amplify.configure(outputs)
│  ├─ router/index.js          # 레거시 라우터 플로우(현재 마운트되지 않음)
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # Amplify Hosting 빌드/배포 파이프라인
├─ package.json
└─ README.md
```

## 🧰 사전 요구 사항

- Node.js 20+ 권장 (프로젝트에서 `@tsconfig/node20`과 최신 Vite 툴링 사용).
- npm 9+.
- 백엔드 샌드박스/배포 워크플로우를 실행하려면 AWS 계정 접근 권한 필요.

## 🧪 설치

```bash
npm install
```

백엔드와 연동된 기능으로 앱을 실행하려면 `amplify_outputs.json`을 먼저 사용할 수 있도록 Amplify 백엔드를 프로비저닝하거나 pull해야 합니다.

일반적인 로컬 백엔드 워크플로:

```bash
npx ampx sandbox
```

또는 CI 배포용 파이프라인(`amplify.yml`에서 이미 구성됨):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ 사용법

| 작업 | 명령 |
| --- | --- |
| 로컬 개발 시작 | `npm run dev` |
| 운영 빌드 생성(타입 검사 포함) | `npm run build` |
| 운영 빌드 미리보기 | `npm run preview` |
| 타입 검사만 실행 | `npm run type-check` |

## 🔧 구성

### Amplify 런타임 outputs

`src/main.ts`는 `../amplify_outputs.json`을 가져와 다음을 호출합니다.

```ts
Amplify.configure(outputs)
```

`amplify_outputs.json`이 없으면 먼저 백엔드를 프로비저닝/pull 해야 합니다. 이 파일은 의도적으로 gitignore(`amplify_outputs*`)되어 있어 환경별로 각자의 outputs가 생성됩니다.

### 백엔드 스키마 및 인증

- 인증은 이메일 기반입니다 (`amplify/auth/resource.ts`).
- 현재 데이터 모델은 다음을 포함합니다:
  - `Idea { content }`
  - `Creator { name, bio }`
- 기본 인증 모드는 Cognito user pool (`userPool`)의 owner 기반 규칙입니다.

### ⚙️ 환경 변수

프런트엔드 구성은 `VITE_` 접두사가 붙은 키를 `.env`에 사용합니다. 새로 타입이 지정된 env 변수를 추가할 때는 `env.d.ts`도 업데이트하세요.

## 📦 예시

### 현재 기본 앱 동작

현재 앱 엔트리는 `App.vue`를 직접 마운트합니다.

```ts
createApp(App).mount('#app')
```

즉, 랜딩 페이지 UI가 기본 런타임 경험입니다.

### 기존 CRUD 데모 컴포넌트 사용

`src/components/Ideas.vue` 및 `src/components/Creators.vue` 같은 레포지토리 컴포넌트에는 Amplify Data의 `generateClient<Schema>()`와 `observeQuery()` 사용 예제가 이미 들어 있습니다.

라우터 기반 인증 페이지로 전환하려면 `src/router/index.js`를 다시 `src/main.ts`에 연결할 수 있습니다. 참고: 이 라우트 가드는 이전 `Auth.currentAuthenticatedUser()` 스타일을 사용하므로 설치된 Amplify 버전에 맞게 업데이트가 필요할 수 있습니다.

## 🚀 AWS 배포

애플리케이션 배포 절차는 문서의 [deployment section](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws)를 참고하세요.

이 저장소에는 `amplify.yml`에 포함된 Amplify Hosting 빌드 구성도 있습니다.

## 🧑‍💻 개발 메모

- `docs/amplify-status.md`에는 운영 중인 앱(`onlyideas.art`)의 스냅샷이 포함되어 있습니다.
- 백엔드/프런트엔드 디렉터리에 레거시 `.old` 파일이 남아 있습니다. 의도적으로 정리되지 않는 한 이력 참고용으로 유지하세요.
- 현재 `package.json`에는 자동 테스트 하네스가 구성되어 있지 않습니다.

수동 점검 핵심 항목:

1. 인증 흐름 (인증 UI를 사용하도록 라우터를 연결할 때).
2. Amplify Data의 Idea와 Creator에 대한 CRUD 동작.
3. `npm run build`를 통한 운영 빌드 무결성.

## 🐞 문제 해결

- `Cannot find module '../amplify_outputs.json'`:
  - 앱을 시작하기 전에 Amplify sandbox/pull로 백엔드 outputs를 생성하세요.
- 인증 라우트가 동작하지 않음:
  - 현재 `main.ts`는 `src/router/index.js`를 마운트하지 않으므로, 현재 랜딩 페이지 구성에서는 정상입니다.
- 타입 오류로 빌드 실패:
  - `npm run type-check`를 실행하고 TypeScript 문제를 해결한 후 번들링하세요.
- CI 백엔드 배포 문제:
  - Amplify Hosting 환경 변수에서 `AWS_BRANCH`와 `AWS_APP_ID`가 설정되어 있는지 확인하세요.

## 🗺️ 로드맵

1. 위의 언어 링크에 맞춰 `i18n/` 하위 다국어 README 파일을 완료합니다.
2. 라우터 기반 인증 앱 모드를 기본으로 되돌릴지, 레거시로 보관할지 결정합니다.
3. 자동화 테스트 추가(권장: Vitest + Vue Test Utils).
4. 더 이상 필요하지 않은 전환용 `.old` 파일 및 주석 처리된 튜토리얼 스캐폴딩을 정리합니다.
5. `docs/amplify-status.md`에 언급된 추가 운영 강화 항목(예: AWS WAF) 활성화.

## 🤝 기여

기여를 환영합니다. 이슈 또는 풀 리퀘스트를 열기 전 [CONTRIBUTING.md](CONTRIBUTING.md)를 읽어주세요.

이 프로젝트는 [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct)를 채택했습니다. 자세한 내용은 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)를 참고하세요.

## 🛡️ 보안

자세한 내용은 [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications)를 참고하세요.

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## License

이 라이브러리는 MIT-0 라이선스입니다. 자세한 내용은 LICENSE 파일을 확인하세요.
