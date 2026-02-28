[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Стартовый шаблон AWS Amplify для Vue.js

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

Этот репозиторий предоставляет стартовый шаблон для создания приложений на Vue.js и AWS Amplify с упором на простой запуск аутентификации, API и базы данных.

## Обзор

Этот шаблон дает базовое Vue-приложение с интеграцией AWS Amplify, оптимизированное для масштабируемости и производительности. Он подходит разработчикам, которые хотят быстро начать проект с заранее настроенными сервисами AWS, такими как Cognito, AppSync и DynamoDB.

В текущем состоянии репозитория приложение включает два практических направления:

1. Активный одностраничный интерфейс маркетинга/лендинга `OnlyIdeas`, который рендерится через `src/App.vue`.
2. Унаследованные/шаблонные компоненты с аутентификацией и CRUD (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue` и т. д.), которые доступны для расширения, но сейчас не подключены в `src/main.ts`.

### Кратко

| Область | Текущее состояние |
| --- | --- |
| Точка входа фронтенда | `src/main.ts` монтирует `App.vue` напрямую |
| Backend-стек | Amplify Gen 2 (`auth` + `data`) |
| Модели данных | `Idea` и `Creator` |
| Режим аутентификации | Вход по email + авторизация через Cognito User Pool |
| CI/CD | Пайплайн Amplify Hosting через `amplify.yml` |
| i18n-файлы README | Ссылки на языки присутствуют; файлы в `i18n/` ожидают заполнения |

## Возможности

- 🔐 **Аутентификация**: настройка Amazon Cognito для безопасной аутентификации пользователей.
- ⚡ **API**: готовый к использованию GraphQL endpoint на AWS AppSync.
- 🗄️ **База данных**: база данных в реальном времени на Amazon DynamoDB.
- 🧩 **Backend-описания Amplify Gen 2**:
  - `amplify/auth/resource.ts` определяет вход по email.
  - `amplify/data/resource.ts` определяет модели `Idea` и `Creator` с авторизацией владельца.
- 🛠️ **Готовый к продакшену frontend toolchain**:
  - Vue 3 + TypeScript + Vite.
  - Скрипты проверки типов и сборки (`vue-tsc` + Vite build).
- 🚀 **Конфигурация CI/CD для Amplify Hosting** через `amplify.yml`.

## Структура проекта

```text
onlyideas-aws/
├─ amplify/                    # Backend-описания Amplify Gen 2
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # снимок статуса развертывания
├─ i18n/                       # многоязычные README (сейчас заполнители)
├─ public/
├─ src/
│  ├─ App.vue                  # активный UI лендинга
│  ├─ main.ts                  # bootstrap приложения + Amplify.configure(outputs)
│  ├─ router/index.js          # устаревший router flow (сейчас не подключен)
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # пайплайн сборки/деплоя Amplify Hosting
├─ package.json
└─ README.md
```

## Предварительные требования

- Рекомендуется Node.js 20+ (проект использует `@tsconfig/node20` и современный toolchain Vite).
- npm 9+.
- Доступ к аккаунту AWS, если вы запускаете sandbox/deploy workflow для backend.

## Установка

```bash
npm install
```

Если вы планируете запускать приложение с функциональностью, подключенной к backend, сначала разверните или подтяните выходные данные Amplify, чтобы был доступен `amplify_outputs.json`.

Типичный локальный workflow для backend:

```bash
npx ampx sandbox
```

Или для деплоя в CI-пайплайне (уже настроен в `amplify.yml`):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## Использование

| Задача | Команда |
| --- | --- |
| Запустить локальную разработку | `npm run dev` |
| Собрать production-бандл (включая проверку типов) | `npm run build` |
| Предпросмотр production-сборки | `npm run preview` |
| Только проверка типов | `npm run type-check` |

## Конфигурация

### Runtime outputs Amplify

`src/main.ts` импортирует `../amplify_outputs.json` и вызывает:

```ts
Amplify.configure(outputs)
```

Если `amplify_outputs.json` отсутствует, сначала выполните provisioning/pull backend. Файл намеренно игнорируется git (`amplify_outputs*`), поэтому каждое окружение генерирует свои outputs.

### Backend-схема и аутентификация

- Аутентификация основана на email (`amplify/auth/resource.ts`).
- Модели данных сейчас включают:
  - `Idea { content }`
  - `Creator { name, bio }`
- Режим авторизации по умолчанию: Cognito user pool (`userPool`) с правилами на основе владельца.

### Переменные окружения

Используйте `.env` с ключами, начинающимися с `VITE_`, для конфигурации frontend. Обновляйте `env.d.ts` при добавлении новых типизированных env-переменных.

## Примеры

### Текущее поведение приложения по умолчанию

Текущая точка входа приложения монтирует `App.vue` напрямую:

```ts
createApp(App).mount('#app')
```

Это означает, что UI лендинга является опытом выполнения по умолчанию.

### Использование существующих CRUD-демо компонентов

Компоненты репозитория, такие как `src/components/Ideas.vue` и `src/components/Creators.vue`, уже показывают использование `generateClient<Schema>()` и `observeQuery()` для Amplify Data.

Если вы хотите переключиться на аутентифицированные страницы на базе router, можно снова подключить `src/router/index.js` в `src/main.ts`. Примечание: этот route guard использует более старый стиль `Auth.currentAuthenticatedUser()` и может потребовать обновлений под установленную у вас версию Amplify.

## Развертывание в AWS

Подробные инструкции по развертыванию приложения смотрите в [разделе deployment](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) документации.

В этом репозитории также есть конфигурация сборки Amplify Hosting в `amplify.yml`.

## Заметки по разработке

- `docs/amplify-status.md` содержит операционный снимок размещенного приложения (`onlyideas.art`).
- В каталогах backend/frontend есть устаревшие `.old`-файлы; оставляйте их как историческую справку, если не выполняется намеренная очистка.
- В `package.json` сейчас не настроен автоматизированный тестовый контур.

Зоны фокуса для ручной проверки:

1. Поток аутентификации (если включаете/подключаете auth UI через router).
2. CRUD-поведение для идей и создателей с Amplify Data.
3. Целостность production-сборки через `npm run build`.

## Устранение неполадок

- `Cannot find module '../amplify_outputs.json'`:
  - Сгенерируйте backend outputs через sandbox/pull Amplify перед запуском приложения.
- Аутентифицированные маршруты не работают:
  - `main.ts` сейчас не монтирует `src/router/index.js`; это ожидаемо для текущей конфигурации лендинга.
- Сборка падает по типам:
  - Запустите `npm run type-check` и исправьте ошибки TypeScript перед сборкой.
- Проблемы CI-деплоя backend:
  - Проверьте, что `AWS_BRANCH` и `AWS_APP_ID` доступны в переменных окружения Amplify Hosting.

## Дорожная карта

1. Завершить многоязычные README-файлы в `i18n/`, чтобы они соответствовали ссылкам навигации по языкам выше.
2. Решить, должен ли режим аутентифицированного приложения на базе router снова стать основным, или его следует оставить как legacy.
3. Добавить автоматизированные тесты (рекомендуется: Vitest + Vue Test Utils).
4. Очистить переходные `.old`-файлы и закомментированные tutorial scaffolding-блоки, когда они станут не нужны.
5. Включить дополнительные меры production-hardening, отмеченные в `docs/amplify-status.md` (например, AWS WAF).

## Вклад

Вклады приветствуются. Перед созданием issue или pull request прочитайте [CONTRIBUTING.md](../CONTRIBUTING.md).

Этот проект принял [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct). Подробности см. в [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md).

## Безопасность

Подробности см. в [CONTRIBUTING](../CONTRIBUTING.md#security-issue-notifications).

## Лицензия

Эта библиотека лицензирована по лицензии MIT-0. См. файл LICENSE.
