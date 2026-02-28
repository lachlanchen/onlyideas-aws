[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# AWS Amplify Vue.js Starter-Template

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

Dieses Repository stellt ein Starter-Template für die Entwicklung von Anwendungen mit Vue.js und AWS Amplify bereit. Der Fokus liegt auf einer einfachen Einrichtung für Authentifizierung, API und Datenbankfunktionen.

## Überblick

Dieses Template bietet dir eine grundlegende Vue-Anwendung mit integrierter AWS-Amplify-Anbindung, optimiert für Skalierbarkeit und Performance. Es ist ideal für Entwicklerinnen und Entwickler, die ein Projekt mit vorkonfigurierten AWS-Services wie Cognito, AppSync und DynamoDB schnell starten möchten.

Im aktuellen Zustand des Repositories umfasst die App zwei praktische Stränge:

1. Eine aktive Single-Page-`OnlyIdeas`-Marketing-/Landing-Experience, gerendert von `src/App.vue`.
2. Legacy-/Template-artige authentifizierte und CRUD-Komponenten (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue` usw.), die zur Erweiterung vorhanden sind, aktuell aber nicht in `src/main.ts` eingebunden sind.

### Auf einen Blick

| Bereich | Aktueller Stand |
| --- | --- |
| Frontend-Runtime-Einstiegspunkt | `src/main.ts` mountet `App.vue` direkt |
| Backend-Stack | Amplify Gen 2 (`auth` + `data`) |
| Datenmodelle | `Idea` und `Creator` |
| Auth-Modus | E-Mail-Login + Cognito-User-Pool-Autorisierung |
| CI/CD | Amplify-Hosting-Pipeline über `amplify.yml` |
| i18n-README-Dateien | Sprachlinks vorhanden; Dateien unter `i18n/` warten auf Befüllung |

## Features

- 🔐 **Authentifizierung**: Einrichtung mit Amazon Cognito für sichere Benutzeranmeldung.
- ⚡ **API**: Sofort nutzbarer GraphQL-Endpunkt mit AWS AppSync.
- 🗄️ **Datenbank**: Echtzeitdatenbank auf Basis von Amazon DynamoDB.
- 🧩 **Amplify-Gen-2-Backend-Definitionen**:
  - `amplify/auth/resource.ts` definiert E-Mail-Login.
  - `amplify/data/resource.ts` definiert `Idea`- und `Creator`-Modelle mit Owner-Autorisierung.
- 🛠️ **Produktionsreife Frontend-Toolchain**:
  - Vue 3 + TypeScript + Vite.
  - Type-Check- und Build-Skripte (`vue-tsc` + Vite-Build).
- 🚀 **Amplify-Hosting-CI/CD-Konfiguration** über `amplify.yml`.

## Projektstruktur

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

## Voraussetzungen

- Node.js 20+ empfohlen (das Projekt nutzt `@tsconfig/node20` und moderne Vite-Tooling-Standards).
- npm 9+.
- Zugriff auf ein AWS-Konto, falls du Backend-Sandbox-/Deploy-Workflows ausführst.

## Installation

```bash
npm install
```

Wenn du die App mit angebundenen Backend-Funktionen ausführen möchtest, stelle zuerst Amplify-Backend-Outputs bereit oder ziehe sie lokal, damit `amplify_outputs.json` verfügbar ist.

Typischer lokaler Backend-Workflow:

```bash
npx ampx sandbox
```

Oder für CI-Pipeline-Deployments (bereits in `amplify.yml` konfiguriert):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## Verwendung

| Aufgabe | Befehl |
| --- | --- |
| Lokale Entwicklung starten | `npm run dev` |
| Produktions-Bundle bauen (inkl. Type-Check) | `npm run build` |
| Produktions-Build lokal ansehen | `npm run preview` |
| Nur Type-Check ausführen | `npm run type-check` |

## Konfiguration

### Amplify-Runtime-Outputs

`src/main.ts` importiert `../amplify_outputs.json` und ruft auf:

```ts
Amplify.configure(outputs)
```

Wenn `amplify_outputs.json` fehlt, führe zuerst Backend-Bereitstellung/-Pull aus. Die Datei ist absichtlich per Git ignoriert (`amplify_outputs*`), sodass jede Umgebung ihre eigenen Outputs erzeugt.

### Backend-Schema und Auth

- Auth ist E-Mail-basiert (`amplify/auth/resource.ts`).
- Datenmodelle enthalten aktuell:
  - `Idea { content }`
  - `Creator { name, bio }`
- Standard-Autorisierungsmodus ist Cognito User Pool (`userPool`) mit Owner-basierten Regeln.

### Umgebungsvariablen

Verwende `.env` mit `VITE_`-präfixierten Schlüsseln für Frontend-Konfiguration. Aktualisiere `env.d.ts`, wenn du neue typisierte Env-Variablen hinzufügst.

## Beispiele

### Aktuelles Standardverhalten der App

Der aktuelle App-Einstiegspunkt mountet `App.vue` direkt:

```ts
createApp(App).mount('#app')
```

Das bedeutet: Die Landing-Page-UI ist die Standard-Runtime-Experience.

### Nutzung vorhandener CRUD-Demo-Komponenten

Repository-Komponenten wie `src/components/Ideas.vue` und `src/components/Creators.vue` zeigen bereits die Nutzung von `generateClient<Schema>()` und `observeQuery()` für Amplify Data.

Wenn du zu router-basierten authentifizierten Seiten wechseln möchtest, kannst du `src/router/index.js` wieder in `src/main.ts` einbinden. Hinweis: Dieser Route-Guard nutzt einen älteren `Auth.currentAuthenticatedUser()`-Stil und benötigt ggf. Anpassungen an deine installierte Amplify-Version.

## Deployment auf AWS

Detaillierte Anleitungen für das Deployment deiner Anwendung findest du im [Deployment-Abschnitt](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) der Dokumentation.

Dieses Repo enthält außerdem eine Amplify-Hosting-Build-Konfiguration in `amplify.yml`.

## Entwicklungshinweise

- `docs/amplify-status.md` enthält einen operativen Snapshot der gehosteten App (`onlyideas.art`).
- Legacy-`.old`-Dateien existieren in Backend-/Frontend-Verzeichnissen; behalte sie als historische Referenz, sofern sie nicht absichtlich bereinigt werden.
- Derzeit ist kein automatisiertes Test-Harness in `package.json` konfiguriert.

Fokusbereiche für manuelle Verifikation:

1. Authentifizierungs-Flow (falls Auth-UI per Router aktiviert/eingebunden wird).
2. Idea- und Creator-CRUD-Verhalten gegen Amplify Data.
3. Integrität des Produktions-Builds über `npm run build`.

## Fehlerbehebung

- `Cannot find module '../amplify_outputs.json'`:
  - Erzeuge Backend-Outputs mit Amplify-Sandbox/-Pull, bevor du die App startest.
- Authentifizierte Routen funktionieren nicht:
  - `main.ts` mountet aktuell nicht `src/router/index.js`; das ist im derzeitigen Landing-Page-Setup erwartetes Verhalten.
- Build schlägt bei Typen fehl:
  - Führe `npm run type-check` aus und behebe TypeScript-Probleme vor dem Bundling.
- CI-Backend-Deploy-Probleme:
  - Prüfe, ob `AWS_BRANCH` und `AWS_APP_ID` in den Amplify-Hosting-Umgebungsvariablen verfügbar sind.

## Roadmap

1. Mehrsprachige README-Dateien unter `i18n/` finalisieren, sodass sie zu den Sprachlinks oben passen.
2. Entscheiden, ob der router-basierte authentifizierte App-Modus als primär wiederhergestellt oder als Legacy archiviert werden soll.
3. Automatisierte Tests ergänzen (empfohlen: Vitest + Vue Test Utils).
4. Übergangs-`.old`-Dateien und auskommentiertes Tutorial-Scaffolding bereinigen, sobald nicht mehr benötigt.
5. Zusätzliche Production-Hardening-Maßnahmen aus `docs/amplify-status.md` aktivieren (zum Beispiel AWS WAF).

## Mitwirken

Beiträge sind willkommen. Bitte lies [CONTRIBUTING.md](CONTRIBUTING.md), bevor du Issues oder Pull Requests eröffnest.

Dieses Projekt hat den [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct) übernommen. Weitere Informationen findest du in [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## Sicherheit

Siehe [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) für weitere Informationen.

## Lizenz

Diese Bibliothek ist unter der MIT-0-Lizenz lizenziert. Siehe die Datei LICENSE.
