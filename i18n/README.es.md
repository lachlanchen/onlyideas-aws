[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Plantilla inicial de AWS Amplify para Vue.js

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

Este repositorio proporciona una plantilla inicial para crear aplicaciones con Vue.js y AWS Amplify, con énfasis en una configuración sencilla para autenticación, API y capacidades de base de datos.

## Descripción general

Esta plantilla te ofrece una aplicación base de Vue integrada con AWS Amplify, optimizada para escalabilidad y rendimiento. Es ideal para desarrolladores que quieran arrancar su proyecto con servicios de AWS ya configurados como Cognito, AppSync y DynamoDB.

En el estado actual del repositorio, la app incluye dos líneas prácticas:

1. Una experiencia de marketing/landing `OnlyIdeas` activa de una sola página, renderizada por `src/App.vue`.
2. Componentes heredados/de tipo plantilla para autenticación y CRUD (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue`, etc.) que están presentes para ampliar, pero actualmente no están conectados en `src/main.ts`.

### De un vistazo

| Área | Estado actual |
| --- | --- |
| Punto de entrada del frontend | `src/main.ts` monta `App.vue` directamente |
| Stack de backend | Amplify Gen 2 (`auth` + `data`) |
| Modelos de datos | `Idea` y `Creator` |
| Modo de autenticación | Inicio de sesión por correo + autorización con Cognito User Pool |
| CI/CD | Pipeline de Amplify Hosting mediante `amplify.yml` |
| Archivos README i18n | Enlaces de idioma presentes; archivos en `i18n/` pendientes de completarse |

## Características

- 🔐 **Autenticación**: Configuración con Amazon Cognito para autenticación segura de usuarios.
- ⚡ **API**: Endpoint GraphQL listo para usar con AWS AppSync.
- 🗄️ **Base de datos**: Base de datos en tiempo real impulsada por Amazon DynamoDB.
- 🧩 **Definiciones de backend de Amplify Gen 2**:
  - `amplify/auth/resource.ts` define inicio de sesión por correo.
  - `amplify/data/resource.ts` define los modelos `Idea` y `Creator` con autorización por propietario.
- 🛠️ **Toolchain de frontend listo para producción**:
  - Vue 3 + TypeScript + Vite.
  - Scripts de type-check y build (`vue-tsc` + Vite build).
- 🚀 **Configuración de CI/CD de Amplify Hosting** mediante `amplify.yml`.

## Estructura del proyecto

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
│  ├─ App.vue                  # UI activa de landing page
│  ├─ main.ts                  # bootstrap de app + Amplify.configure(outputs)
│  ├─ router/index.js          # flujo de router heredado (actualmente no montado)
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # pipeline de build/deploy de Amplify Hosting
├─ package.json
└─ README.md
```

## Requisitos previos

- Node.js 20+ recomendado (el proyecto usa `@tsconfig/node20` y tooling moderno de Vite).
- npm 9+.
- Acceso a una cuenta de AWS si vas a ejecutar flujos de sandbox/deploy del backend.

## Instalación

```bash
npm install
```

Si planeas ejecutar la app con funcionalidad conectada al backend, primero aprovisiona o descarga los outputs de Amplify para que `amplify_outputs.json` esté disponible.

Flujo común de backend en local:

```bash
npx ampx sandbox
```

O, para despliegues de pipeline CI (ya configurados en `amplify.yml`):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## Uso

| Tarea | Comando |
| --- | --- |
| Iniciar desarrollo local | `npm run dev` |
| Compilar bundle de producción (incluye type-check) | `npm run build` |
| Previsualizar build de producción | `npm run preview` |
| Ejecutar solo type-check | `npm run type-check` |

## Configuración

### Outputs de runtime de Amplify

`src/main.ts` importa `../amplify_outputs.json` y llama a:

```ts
Amplify.configure(outputs)
```

Si falta `amplify_outputs.json`, ejecuta primero el aprovisionamiento/pull del backend. El archivo está intencionalmente en gitignore (`amplify_outputs*`) para que cada entorno genere sus propios outputs.

### Esquema de backend y auth

- La autenticación es por correo (`amplify/auth/resource.ts`).
- Los modelos de datos incluyen actualmente:
  - `Idea { content }`
  - `Creator { name, bio }`
- El modo de autorización por defecto es Cognito user pool (`userPool`) con reglas basadas en propietario.

### Variables de entorno

Usa `.env` con claves prefijadas con `VITE_` para la configuración del frontend. Actualiza `env.d.ts` al añadir nuevas variables de entorno tipadas.

## Ejemplos

### Comportamiento actual de la app por defecto

La entrada actual de la app monta `App.vue` directamente:

```ts
createApp(App).mount('#app')
```

Esto significa que la UI de la landing page es la experiencia de runtime por defecto.

### Uso de componentes demo CRUD existentes

Componentes del repositorio como `src/components/Ideas.vue` y `src/components/Creators.vue` ya muestran el uso de `generateClient<Schema>()` y `observeQuery()` para Amplify Data.

Si quieres volver a páginas autenticadas basadas en router, puedes conectar de nuevo `src/router/index.js` en `src/main.ts`. Nota: este guard de rutas usa el estilo antiguo `Auth.currentAuthenticatedUser()` y puede requerir actualizaciones según la versión de Amplify que tengas instalada.

## Despliegue en AWS

Para instrucciones detalladas sobre cómo desplegar tu aplicación, consulta la [sección de despliegue](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) de nuestra documentación.

Este repositorio también incluye la configuración de build de Amplify Hosting en `amplify.yml`.

## Notas de desarrollo

- `docs/amplify-status.md` contiene una instantánea operativa de la app alojada (`onlyideas.art`).
- Existen archivos `.old` heredados en directorios de backend/frontend; mantenlos como referencia histórica salvo que se limpien de forma intencional.
- Actualmente no hay un harness de pruebas automatizadas configurado en `package.json`.

Áreas de enfoque para verificación manual:

1. Flujo de autenticación (si habilitas/conectas la UI de auth con router).
2. Comportamiento CRUD de ideas y creadores contra Amplify Data.
3. Integridad del build de producción mediante `npm run build`.

## Solución de problemas

- `Cannot find module '../amplify_outputs.json'`:
  - Genera outputs del backend con sandbox/pull de Amplify antes de iniciar la app.
- Las rutas autenticadas no funcionan:
  - `main.ts` actualmente no monta `src/router/index.js`; esto es lo esperado en la configuración actual centrada en la landing page.
- El build falla por tipos:
  - Ejecuta `npm run type-check` y corrige los problemas de TypeScript antes de compilar.
- Problemas en despliegues de backend por CI:
  - Valida que `AWS_BRANCH` y `AWS_APP_ID` estén disponibles en las variables de entorno de Amplify Hosting.

## Hoja de ruta

1. Completar los archivos README multilingües en `i18n/` para que coincidan con los enlaces de idioma anteriores.
2. Decidir si el modo de app autenticada basada en router debe restaurarse como principal o archivarse como heredado.
3. Añadir pruebas automatizadas (recomendado: Vitest + Vue Test Utils).
4. Limpiar archivos transicionales `.old` y scaffolding tutorial comentado cuando ya no sean necesarios.
5. Habilitar endurecimientos adicionales de producción indicados en `docs/amplify-status.md` (por ejemplo, AWS WAF).

## Contribuciones

Las contribuciones son bienvenidas. Lee [CONTRIBUTING.md](../CONTRIBUTING.md) antes de abrir issues o pull requests.

Este proyecto ha adoptado el [Código de Conducta de Amazon Open Source](https://aws.github.io/code-of-conduct). Para más información, consulta [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md).

## Seguridad

Consulta [CONTRIBUTING](../CONTRIBUTING.md#security-issue-notifications) para más información.

## Licencia

Esta biblioteca está licenciada bajo la licencia MIT-0. Consulta el archivo [LICENSE](../LICENSE).
