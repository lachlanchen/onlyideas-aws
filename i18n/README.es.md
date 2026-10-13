[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://lazying.art)

# Plantilla inicial de AWS Amplify para Vue.js

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

Este repositorio proporciona una plantilla inicial para crear aplicaciones con Vue.js y AWS Amplify, con un enfoque en la configuración fácil de autenticación, API y capacidades de base de datos.

## 🎯 Visión general

Esta plantilla te da una aplicación base de Vue integrada con AWS Amplify, optimizada para escalabilidad y rendimiento. Es ideal para desarrolladores que quieran acelerar su proyecto con servicios de AWS preconfigurados como Cognito, AppSync y DynamoDB.

En su estado actual, el repositorio incluye dos flujos prácticos:

1. Una experiencia `OnlyIdeas` activa de una sola página para marketing/landing, renderizada por `src/App.vue`.
2. Componentes autenticados y CRUD con estilo heredado/plantilla (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue`, etc.) que están disponibles para extender, pero no están conectados actualmente en `src/main.ts`.

### 🧭 De un vistazo

| Área | Estado actual |
| --- | --- |
| Punto de entrada del frontend | `src/main.ts` monta `App.vue` directamente |
| Stack backend | Amplify Gen 2 (`auth` + `data`) |
| Modelos de datos | `Idea` y `Creator` |
| Modo de autenticación | Inicio de sesión por correo + autorización con Cognito User Pool |
| CI/CD | Pipeline de Amplify Hosting vía `amplify.yml` |
| Archivos README i18n | Enlaces de idioma presentes; archivos en `i18n/` pendientes de completar |

## ✨ Características

- 🔐 **Autenticación**: Configuración con Amazon Cognito para una autenticación de usuarios segura.
- ⚡ **API**: Endpoint de GraphQL listo para usar con AWS AppSync.
- 🗄️ **Base de datos**: Base de datos en tiempo real impulsada por Amazon DynamoDB.
- 🧩 **Definiciones de backend de Amplify Gen 2**:
  - `amplify/auth/resource.ts` define el inicio de sesión por correo.
  - `amplify/data/resource.ts` define los modelos `Idea` y `Creator` con autorización por propietario.
- 🛠️ **Cadena de herramientas frontend lista para producción**:
  - Vue 3 + TypeScript + Vite.
  - Scripts de comprobación de tipos y compilación (`vue-tsc` + build de Vite).
- 🚀 **Configuración de CI/CD de Amplify Hosting** mediante `amplify.yml`.

## 🧱 Estructura del proyecto

```text
onlyideas-aws/
├─ amplify/                    # Definiciones de backend de Amplify Gen 2
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # instantánea del estado de despliegue
├─ i18n/                       # destinos README multilingües (actualmente placeholders)
├─ public/
├─ src/
│  ├─ App.vue                  # UI de landing page activa
│  ├─ main.ts                  # arranque de app + Amplify.configure(outputs)
│  ├─ router/index.js          # flujo de router heredado (no montado actualmente)
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # pipeline de build/despliegue de Amplify Hosting
├─ package.json
└─ README.md
```

## 🧰 Requisitos previos

- Node.js 20+ recomendado (el proyecto usa `@tsconfig/node20` y herramientas modernas de Vite).
- npm 9+.
- Acceso a una cuenta de AWS si se ejecutan flujos de sandbox/despliegue del backend.

## 🧪 Instalación

```bash
npm install
```

Si planeas ejecutar la app con funcionalidad conectada al backend, provisiona o descarga primero los outputs de Amplify para que `amplify_outputs.json` esté disponible.

Flujo típico de backend local:

```bash
npx ampx sandbox
```

O, para despliegues de pipeline de CI (ya configurados en `amplify.yml`):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ Uso

| Tarea | Comando |
| --- | --- |
| Iniciar desarrollo local | `npm run dev` |
| Generar bundle de producción (incluye type-check) | `npm run build` |
| Previsualizar el build de producción | `npm run preview` |
| Ejecutar solo comprobación de tipos | `npm run type-check` |

## 🔧 Configuración

### Outputs de runtime de Amplify

`src/main.ts` importa `../amplify_outputs.json` y ejecuta:

```ts
Amplify.configure(outputs)
```

Si `amplify_outputs.json` falta, ejecuta primero el aprovisionamiento/pull del backend. Este archivo está intencionalmente en gitignore (`amplify_outputs*`) para que cada entorno genere sus propios outputs.

### Esquema de backend y autenticación

- La autenticación es por correo (`amplify/auth/resource.ts`).
- Los modelos de datos actualmente incluyen:
  - `Idea { content }`
  - `Creator { name, bio }`
- El modo de autorización por defecto es Cognito user pool (`userPool`) con reglas basadas en propietario.

### ⚙️ Variables de entorno

Usa `.env` con claves prefijadas por `VITE_` para la configuración del frontend. Actualiza `env.d.ts` al añadir nuevas variables de entorno tipadas.

## 📦 Ejemplos

### Comportamiento predeterminado actual de la app

La entrada actual de la app monta `App.vue` directamente:

```ts
createApp(App).mount('#app')
```

Esto significa que la UI de la landing page es la experiencia predeterminada en tiempo de ejecución.

### Uso de los componentes de demostración CRUD existentes

Componentes del repositorio como `src/components/Ideas.vue` y `src/components/Creators.vue` ya muestran ejemplos de uso de `generateClient<Schema>()` y `observeQuery()` para Amplify Data.

Si quieres pasar a páginas autenticadas basadas en router, puedes volver a enlazar `src/router/index.js` en `src/main.ts`. Nota: este guard de ruta usa el estilo antiguo `Auth.currentAuthenticatedUser()` y puede necesitar ajustes según la versión de Amplify instalada.

## 🚀 Despliegue en AWS

Para instrucciones detalladas sobre cómo desplegar tu aplicación, consulta la [sección de despliegue](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) de la documentación.

Este repositorio también incluye la configuración de build de Amplify Hosting en `amplify.yml`.

## 🧑‍💻 Notas de desarrollo

- `docs/amplify-status.md` contiene una instantánea operativa de la app alojada (`onlyideas.art`).
- Existen archivos `.old` heredados en los directorios de backend/frontend; mantenlos como referencia histórica salvo que se limpien intencionalmente.
- Actualmente no hay un conjunto de pruebas automatizadas configurado en `package.json`.

Áreas de verificación manual:

1. Flujo de autenticación (si activas y conectas la UI de autenticación con router).
2. Comportamiento CRUD de ideas y creadores sobre Amplify Data.
3. Integridad del build de producción a través de `npm run build`.

## 🐞 Solución de problemas

- `Cannot find module '../amplify_outputs.json'`:
  - Genera los outputs del backend con `sandbox/pull` de Amplify antes de iniciar la app.
- Las rutas autenticadas no funcionan:
  - `main.ts` actualmente no monta `src/router/index.js`; esto es esperado en la configuración de landing page actual.
- El build falla por tipos:
  - Ejecuta `npm run type-check` y corrige los errores de TypeScript antes de generar el bundle.
- Problemas de despliegue del backend en CI:
  - Comprueba que `AWS_BRANCH` y `AWS_APP_ID` estén disponibles en las variables de entorno de Amplify Hosting.

## 🗺️ Hoja de ruta

1. Finalizar los archivos README multilingües en `i18n/` para que coincidan con los enlaces de idioma del bloque anterior.
2. Decidir si el modo de app autenticada basada en router debe restaurarse como principal o mantenerse como legado.
3. Añadir pruebas automatizadas (recomendado: Vitest + Vue Test Utils).
4. Limpiar los archivos transicionales `.old` y el scaffolding tutorial comentado cuando ya no se necesiten.
5. Habilitar refuerzos de producción adicionales señalados en `docs/amplify-status.md` (por ejemplo AWS WAF).

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Lee [CONTRIBUTING.md](../CONTRIBUTING.md) antes de abrir incidencias o pull requests.

Este proyecto adopta el [Código de Conducta de AWS Open Source](https://aws.github.io/code-of-conduct). Para más información consulta [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md).

## 🛡️ Seguridad

Consulta [CONTRIBUTING](../CONTRIBUTING.md#security-issue-notifications) para más información.

## Licencia

Esta librería está licenciada bajo la Licencia MIT-0. Consulta el archivo LICENSE.


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
