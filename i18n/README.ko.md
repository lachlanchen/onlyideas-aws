[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# AWS Amplify Vue.js 스타터 템플릿

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

이 저장소는 Vue.js와 AWS Amplify를 사용해 애플리케이션을 만들기 위한 스타터 템플릿을 제공하며, 인증, API, 데이터베이스 기능을 손쉽게 설정할 수 있도록 구성되어 있습니다.

## 개요

이 템플릿은 AWS Amplify가 통합된 Vue 기반 애플리케이션의 기본 골격을 제공하며, 확장성과 성능을 고려해 간소화되어 있습니다. Cognito, AppSync, DynamoDB 같은 AWS 서비스를 사전 구성된 상태로 빠르게 시작하려는 개발자에게 적합합니다.

현재 저장소 상태에서 앱은 두 가지 실용적인 트랙을 포함합니다.

1. `src/App.vue`로 렌더링되는 활성 단일 페이지 `OnlyIdeas` 마케팅/랜딩 경험
2. 확장을 위해 포함되어 있으나 현재 `src/main.ts`에 연결되어 있지 않은 레거시/템플릿 스타일 인증 및 CRUD 컴포넌트 (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue` 등)

### 한눈에 보기

| 영역 | 현재 상태 |
| --- | --- |
| 프런트엔드 런타임 엔트리 | `src/main.ts`가 `App.vue`를 직접 마운트 |
| 백엔드 스택 | Amplify Gen 2 (`auth` + `data`) |
| 데이터 모델 | `Idea` 및 `Creator` |
| 인증 모드 | 이메일 로그인 + Cognito User Pool 권한 부여 |
| CI/CD | `amplify.yml` 기반 Amplify Hosting 파이프라인 |
| i18n README 파일 | 언어 링크는 존재하며 `i18n/` 하위 파일은 채워야 함 |

## 기능

- 🔐 **인증(Authentication)**: Amazon Cognito 기반의 안전한 사용자 인증 설정
- ⚡ **API**: AWS AppSync 기반 즉시 사용 가능한 GraphQL 엔드포인트
- 🗄️ **데이터베이스**: Amazon DynamoDB 기반 실시간 데이터베이스
- 🧩 **Amplify Gen 2 백엔드 정의**:
  - `amplify/auth/resource.ts`에서 이메일 로그인 정의
  - `amplify/data/resource.ts`에서 소유자 권한(owner authorization)을 갖는 `Idea`, `Creator` 모델 정의
- 🛠️ **프로덕션 준비형 프런트엔드 툴체인**:
  - Vue 3 + TypeScript + Vite
  - 타입 검사 및 빌드 스크립트 (`vue-tsc` + Vite build)
- 🚀 `amplify.yml`을 통한 **Amplify Hosting CI/CD 설정**

## 프로젝트 구조

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

## 사전 요구 사항

- Node.js 20+ 권장 (프로젝트는 `@tsconfig/node20` 및 최신 Vite 툴링 사용)
- npm 9+
- 백엔드 샌드박스/배포 워크플로를 실행하려면 AWS 계정 접근 권한 필요

## 설치

```bash
npm install
```

백엔드 연결 기능과 함께 앱을 실행할 계획이라면, 먼저 Amplify 백엔드 출력을 프로비저닝하거나 pull하여 `amplify_outputs.json`을 준비하세요.

일반적인 로컬 백엔드 워크플로:

```bash
npx ampx sandbox
```

또는 CI 파이프라인 배포용(`amplify.yml`에 이미 설정됨):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## 사용법

| 작업 | 명령어 |
| --- | --- |
| 로컬 개발 시작 | `npm run dev` |
| 프로덕션 번들 빌드 (타입 검사 포함) | `npm run build` |
| 프로덕션 빌드 미리보기 | `npm run preview` |
| 타입 검사만 실행 | `npm run type-check` |

## 구성

### Amplify 런타임 출력

`src/main.ts`는 `../amplify_outputs.json`을 import하고 다음을 호출합니다:

```ts
Amplify.configure(outputs)
```

`amplify_outputs.json`이 없다면 먼저 백엔드를 프로비저닝/pull 하세요. 이 파일은 환경별로 각자 생성되도록 의도적으로 gitignore(`amplify_outputs*`) 처리되어 있습니다.

### 백엔드 스키마 및 인증

- 인증은 이메일 기반입니다 (`amplify/auth/resource.ts`).
- 현재 데이터 모델은 다음을 포함합니다:
  - `Idea { content }`
  - `Creator { name, bio }`
- 기본 권한 부여 모드는 Cognito user pool(`userPool`)이며 소유자 기반 규칙을 사용합니다.

### 환경 변수

프런트엔드 구성에는 `VITE_` 접두사가 붙은 키를 `.env`에서 사용하세요. 새로운 타입드 환경 변수를 추가할 때는 `env.d.ts`를 함께 업데이트하세요.

## 예시

### 현재 기본 앱 동작

현재 앱 엔트리는 `App.vue`를 직접 마운트합니다:

```ts
createApp(App).mount('#app')
```

즉, 랜딩 페이지 UI가 기본 런타임 경험입니다.

### 기존 CRUD 데모 컴포넌트 사용

`src/components/Ideas.vue`, `src/components/Creators.vue` 같은 저장소 컴포넌트에는 Amplify Data용 `generateClient<Schema>()`, `observeQuery()` 사용 예시가 이미 포함되어 있습니다.

라우터 기반 인증 페이지로 전환하려면 `src/router/index.js`를 다시 `src/main.ts`에 연결하면 됩니다. 참고: 이 라우트 가드는 구버전 `Auth.currentAuthenticatedUser()` 스타일을 사용하므로, 설치된 Amplify 버전에 맞춘 업데이트가 필요할 수 있습니다.

## AWS 배포

애플리케이션 배포에 대한 자세한 지침은 공식 문서의 [deployment section](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws)을 참고하세요.

이 저장소에는 `amplify.yml`의 Amplify Hosting 빌드 설정도 포함되어 있습니다.

## 개발 노트

- `docs/amplify-status.md`에는 호스팅된 앱(`onlyideas.art`)의 운영 스냅샷이 포함되어 있습니다.
- 백엔드/프런트엔드 디렉터리에 레거시 `.old` 파일이 있습니다. 의도적으로 정리(cleanup)하기 전까지는 이력 참조용으로 유지하세요.
- 현재 `package.json`에는 자동화된 테스트 하네스가 구성되어 있지 않습니다.

수동 검증 중점 영역:

1. 인증 플로우(인증 UI를 활성화/라우터 연결하는 경우)
2. Amplify Data에 대한 아이디어/크리에이터 CRUD 동작
3. `npm run build`를 통한 프로덕션 빌드 무결성

## 문제 해결

- `Cannot find module '../amplify_outputs.json'`:
  - 앱 시작 전에 Amplify sandbox/pull로 백엔드 출력을 생성하세요.
- 인증 라우트가 동작하지 않음:
  - 현재 `main.ts`는 `src/router/index.js`를 마운트하지 않으며, 이는 현재 랜딩 페이지 구성에서 의도된 동작입니다.
- 타입 오류로 빌드 실패:
  - `npm run type-check`를 실행해 TypeScript 문제를 먼저 해결한 뒤 번들링하세요.
- CI 백엔드 배포 문제:
  - Amplify Hosting 환경 변수에서 `AWS_BRANCH`, `AWS_APP_ID`가 제공되는지 확인하세요.

## 로드맵

1. 위 언어 네비게이션 링크와 일치하도록 `i18n/` 하위 다국어 README 파일 완성
2. 라우터 기반 인증 앱 모드를 기본으로 복원할지, 레거시로 보관할지 결정
3. 자동화 테스트 추가 (권장: Vitest + Vue Test Utils)
4. 더 이상 필요하지 않은 전환기 `.old` 파일 및 주석 처리된 튜토리얼 스캐폴딩 정리
5. `docs/amplify-status.md`에 명시된 추가 프로덕션 하드닝(예: AWS WAF) 적용

## 기여

기여를 환영합니다. 이슈 또는 풀 리퀘스트를 열기 전에 [CONTRIBUTING.md](CONTRIBUTING.md)를 읽어 주세요.

이 프로젝트는 [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct)를 채택했습니다. 자세한 내용은 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)를 참고하세요.

## 보안

자세한 내용은 [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications)을 참고하세요.

## 라이선스

이 라이브러리는 MIT-0 라이선스로 제공됩니다. 자세한 내용은 LICENSE 파일을 참고하세요.
