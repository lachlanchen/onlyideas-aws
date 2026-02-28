[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Шаблон AWS Amplify для Vue.js

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

Этот репозиторий предоставляет стартовый шаблон для создания приложений на Vue.js и AWS Amplify, с акцентом на простую настройку аутентификации, API и возможностей базы данных.

## 🎯 Обзор

Этот шаблон оснащен базовым приложением на Vue с интеграцией AWS Amplify, оптимизированным для масштабируемости и производительности. Он подходит для разработчиков, которые хотят быстро запустить проект с заранее настроенными сервисами AWS, такими как Cognito, AppSync и DynamoDB.

В текущем состоянии репозитория приложение содержит две практичные дорожки:

1. Активный одностраничный маркетинговый/landing-опыт `OnlyIdeas`, который рендерится через `src/App.vue`.
2. Устаревшие шаблонные компоненты с аутентификацией и CRUD (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue` и т.д.), которые присутствуют для расширения, но сейчас не подключены в `src/main.ts`.

### 🧭 Вкратце

| Область | Текущее состояние |
| --- | --- |
| Точка входа фронтенда | `src/main.ts` монтирует `App.vue` напрямую |
| Бэкенд-стек | Amplify Gen 2 (`auth` + `data`) |
| Модели данных | `Idea` и `Creator` |
| Режим аутентификации | Вход по email + авторизация через Cognito User Pool |
| CI/CD | CI/CD пайплайн Amplify Hosting через `amplify.yml` |
| Файлы i18n README | Ссылки на языки присутствуют; файлы в `i18n/` ждут заполнения |

## ✨ Функции

- 🔐 **Аутентификация**: настройка Amazon Cognito для безопасной аутентификации пользователей.
- ⚡ **API**: готовый к использованию GraphQL-эндпоинт AWS AppSync.
- 🗄️ **База данных**: БД в реальном времени на базе Amazon DynamoDB.
- 🧩 **Определения бэкенда Amplify Gen 2**:
  - `amplify/auth/resource.ts` задаёт вход по email.
  - `amplify/data/resource.ts` задаёт модели `Idea` и `Creator` с авторизацией по владельцу.
- 🛠️ **Инструментальная цепочка для production**:
  - Vue 3 + TypeScript + Vite.
  - Скрипты проверки типов и сборки (`vue-tsc` + сборка Vite).
- 🚀 **Конфигурация CI/CD Amplify Hosting** через `amplify.yml`.

## 🧱 Структура проекта

```text
onlyideas-aws/
├─ amplify/                    # Определения бэкенда Amplify Gen 2
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # снимок статуса развертывания
├─ i18n/                       # мультиязычные целевые README (в настоящее время placeholders)
├─ public/
├─ src/
│  ├─ App.vue                  # активный интерфейс лендинга
│  ├─ main.ts                  # инициализация приложения + Amplify.configure(outputs)
│  ├─ router/index.js          # устаревший роутерный поток (на данный момент не смонтирован)
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # pipeline сборки/развертывания Amplify Hosting
├─ package.json
└─ README.md
```

## 🧰 Предварительные требования

- Рекомендуется Node.js 20+ (проект использует `@tsconfig/node20` и современный инструментальный стек Vite).
- npm 9+.
- Доступ к аккаунту AWS, если вы запускаете резервный/деплой-процессы бэкенда.

## 🧪 Установка

```bash
npm install
```

Если вы планируете запускать приложение с привязкой к бэкенду, предварительно разверните или получите выходные данные Amplify, чтобы `amplify_outputs.json` был доступен.

Обычный локальный backend-процесс:

```bash
npx ampx sandbox
```

Или для CI-пайплайна развертывания (уже настроен в `amplify.yml`):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ Использование

| Задача | Команда |
| --- | --- |
| Запуск локальной разработки | `npm run dev` |
| Сборка production-бандла (включая проверку типов) | `npm run build` |
| Просмотр production-сборки | `npm run preview` |
| Только проверка типов | `npm run type-check` |

## 🔧 Конфигурация

### Рантайм outputs Amplify

`src/main.ts` импортирует `../amplify_outputs.json` и вызывает:

```ts
Amplify.configure(outputs)
```

Если `amplify_outputs.json` отсутствует, выполните сначала provisioning/pull бэкенда. Файл намеренно добавлен в `.gitignore` (`amplify_outputs*`), поэтому каждое окружение генерирует собственные outputs.

### Схема бэкенда и аутентификация

- Аутентификация основана на email (`amplify/auth/resource.ts`).
- Модели данных сейчас включают:
  - `Idea { content }`
  - `Creator { name, bio }`
- Режим авторизации по умолчанию — Cognito user pool (`userPool`) с правилами на основе владельца.

### ⚙️ Переменные окружения

Используйте `.env` с переменными, префиксированными `VITE_`, для конфигурации фронтенда. Обновляйте `env.d.ts` при добавлении новых типизированных переменных окружения.

## 📦 Примеры

### Текущее поведение приложения по умолчанию

Текущая точка входа приложения монтирует `App.vue` напрямую:

```ts
createApp(App).mount('#app')
```

Это означает, что интерфейс лендинга является стандартным runtime опытом.

### Использование существующих CRUD-демо-компонентов

Компоненты репозитория, такие как `src/components/Ideas.vue` и `src/components/Creators.vue`, уже показывают использование `generateClient<Schema>()` и `observeQuery()` для Amplify Data.

Если вы хотите переключиться на аутентифицированные страницы на основе router, вы можете подключить `src/router/index.js` обратно в `src/main.ts`. Примечание: этот route guard использует старый стиль `Auth.currentAuthenticatedUser()` и может потребовать обновлений под установленную версию Amplify.

## 🚀 Развертывание в AWS

Подробные инструкции по развертыванию приложения см. в разделе [deployment](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) в документации.

Этот репозиторий также включает конфигурацию сборки Amplify Hosting в `amplify.yml`.

## 🧑‍💻 Заметки по разработке

- `docs/amplify-status.md` содержит оперативный снимок развернутого приложения (`onlyideas.art`).
- В каталогах backend/frontend существуют legacy-файлы `.old`; оставляйте их как историческую справку, если только вы не убираете их намеренно.
- В `package.json` в данный момент не настроен автоматизированный тестовый контур.

Основные направления для ручной проверки:

1. Процесс аутентификации (если включаете/подключаете роутерную auth-логику).
2. Поведение CRUD для идеи и авторов в Amplify Data.
3. Корректность production-сборки через `npm run build`.

## 🐞 Устранение неполадок

- `Cannot find module '../amplify_outputs.json'`:
  - Сгенерируйте backend outputs с помощью sandbox/pull Amplify перед запуском приложения.
- Аутентифицированные маршруты не работают:
  - `main.ts` в текущей момент не монтирует `src/router/index.js`; это ожидаемо для текущей настройки лендинга.
- Сборка падает из‑за типов:
  - Запустите `npm run type-check` и исправьте ошибки TypeScript перед сборкой.
- Проблемы деплоя backend в CI:
  - Проверьте, что `AWS_BRANCH` и `AWS_APP_ID` доступны в переменных окружения Amplify Hosting.

## 🗺️ Дорожная карта

1. Довести до конца мультиязычные README в `i18n/`, чтобы они соответствовали ссылкам языковой навигации выше.
2. Решить, должен ли режим аутентифицированного приложения на основе router быть восстановлен как основной или оставлен legacy.
3. Добавить автоматические тесты (рекомендуется: Vitest + Vue Test Utils).
4. Убрать переходные `.old`-файлы и закомментированный учебный каркас после того, как они перестанут быть нужны.
5. Включить дополнительную production-hardening из `docs/amplify-status.md` (например, AWS WAF).

## 🤝 Вклад

Вклады приветствуются. Перед созданием issues или pull requests прочитайте [CONTRIBUTING.md](../CONTRIBUTING.md).

Этот проект принял [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct). Дополнительную информацию можно найти в [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md).

## 🛡️ Безопасность

Подробности см. в [CONTRIBUTING](../CONTRIBUTING.md#security-issue-notifications).

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## License

Эта библиотека распространяется под лицензией MIT-0. См. файл LICENSE.
