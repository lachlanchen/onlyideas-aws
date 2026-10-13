[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner — apps and four buddies](https://github.com/lachlanchen/LazyingArtWebsite/raw/main/media/brand/lazyingart-intro.gif)](https://lazying.art)

# AWS Amplify Vue.js Starter Template

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

Dieses Repository stellt ein Starter-Template für die Erstellung von Anwendungen mit Vue.js und AWS Amplify bereit und legt den Fokus auf eine einfache Einrichtung für Authentifizierung, API und Datenbank-Funktionen.

## 🎯 Overview

Diese Vorlage liefert eine grundlegende Vue-Anwendung mit integrierter AWS-Amplify-Anbindung, optimiert für Skalierbarkeit und Performance. Sie ist ideal für Entwicklerinnen und Entwickler, die ihr Projekt mit vorkonfigurierten AWS-Services wie Cognito, AppSync und DynamoDB schnell starten möchten.

In der aktuellen Repository-Version enthält die App zwei praxisnahe Pfade:

1. Eine aktive, einseitige `OnlyIdeas`-Marketing-/Landing-Experience, die durch `src/App.vue` gerendert wird.
2. Authentifizierte und CRUD-orientierte Legacy-/Vorlagen-Komponenten (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue` usw.), die für Erweiterungen vorhanden sind, jedoch derzeit nicht in `src/main.ts` eingebunden sind.

### 🧭 At a glance

| Bereich | Aktueller Stand |
| --- | --- |
| Frontend runtime entry | `src/main.ts` mountet `App.vue` direkt |
| Backend-Stack | Amplify Gen 2 (`auth` + `data`) |
| Datenmodelle | `Idea` und `Creator` |
| Authentifizierungsmodus | E-Mail-Login + Cognito User Pool-Autorisierung |
| CI/CD | Amplify Hosting-Pipeline über `amplify.yml` |
| i18n README-Dateien | Sprachlinks vorhanden; Dateien unter `i18n/` werden ergänzt |

## ✨ Features

- 🔐 **Authentifizierung**: Einrichtung mit Amazon Cognito für sichere Nutzer-Authentifizierung.
- ⚡ **API**: Einsatzbereiter GraphQL-Endpunkt mit AWS AppSync.
- 🗄️ **Datenbank**: Echtzeitdatenbank auf Basis von Amazon DynamoDB.
- 🧩 **Amplify Gen 2 Backend-Definitionen**:
  - `amplify/auth/resource.ts` definiert E-Mail-Login.
  - `amplify/data/resource.ts` definiert `Idea` und `Creator` Modelle mit Owner-Berechtigungen.
- 🛠️ **Produktionsreife Frontend-Toolchain**:
  - Vue 3 + TypeScript + Vite.
  - Type-Check- und Build-Skripte (`vue-tsc` + Vite build).
- 🚀 **Amplify Hosting CI/CD-Konfiguration** über `amplify.yml`.

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

## 🧰 Voraussetzungen

- Node.js 20+ empfohlen (das Projekt nutzt `@tsconfig/node20` und moderne Vite-Tooling-Standards).
- npm 9+.
- AWS-Konto-Zugriff, falls Sie Backend-Sandbox-/Deployment-Workflows ausführen.

## 🧪 Installation

```bash
npm install
```

Wenn Sie die App mit backendgebundener Funktionalität ausführen möchten, stellen Sie zuerst die Amplify-Backend-Outputs bereit oder ziehen diese, damit `amplify_outputs.json` verfügbar ist.

Typischer lokaler Backend-Workflow:

```bash
npx ampx sandbox
```

Oder für CI-Pipeline-Deployments (bereits in `amplify.yml` konfiguriert):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ Verwendung

| Aufgabe | Befehl |
| --- | --- |
| Lokale Entwicklung starten | `npm run dev` |
| Produktions-Bundle bauen (inkl. Type-Check) | `npm run build` |
| Produktions-Build prüfen | `npm run preview` |
| Nur Type-Check ausführen | `npm run type-check` |

## 🔧 Konfiguration

### Amplify Runtime Outputs

`src/main.ts` importiert `../amplify_outputs.json` und ruft auf:

```ts
Amplify.configure(outputs)
```

Wenn `amplify_outputs.json` fehlt, führen Sie zuerst das Bereitstellen oder Pullen des Backends aus. Die Datei ist absichtlich in Git ignoriert (`amplify_outputs*`), sodass jede Umgebung eigene Outputs erzeugt.

### Backend-Schema und Auth

- Authentifizierung ist e-mail-basiert (`amplify/auth/resource.ts`).
- Datenmodelle enthalten derzeit:
  - `Idea { content }`
  - `Creator { name, bio }`
- Der Standard-Autorisierungsmodus ist Cognito User Pool (`userPool`) mit Owner-basierten Regeln.

### ⚙️ Umgebungsvariablen

Verwenden Sie `.env` mit `VITE_`-präfigisierten Schlüsseln für die Frontend-Konfiguration. Aktualisieren Sie `env.d.ts`, wenn neue typisierte Umgebungsvariablen hinzugefügt werden.

## 📦 Beispiele

### Aktuelles Standard-App-Verhalten

Der aktuelle App-Einstiegspunkt mountet `App.vue` direkt:

```ts
createApp(App).mount('#app')
```

Das bedeutet, dass die Landing-Page-UI die Standardlaufzeit-Experience ist.

### Verwendung vorhandener CRUD-Demo-Komponenten

Repository-Komponenten wie `src/components/Ideas.vue` und `src/components/Creators.vue` zeigen bereits die Nutzung von `generateClient<Schema>()` und `observeQuery()` für Amplify Data.

Wenn Sie auf router-basierte authentifizierte Seiten wechseln möchten, können Sie `src/router/index.js` wieder in `src/main.ts` einbinden. Hinweis: Dieser Route Guard nutzt einen älteren `Auth.currentAuthenticatedUser()`-Stil und kann je nach installierter Amplify-Version eine Aktualisierung benötigen.

## 🚀 Deployment auf AWS

Für ausführliche Anweisungen zum Deployment Ihrer Anwendung siehe den [Deployment-Abschnitt](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) in unserer Dokumentation.

Dieses Repository enthält außerdem die Amplify Hosting Build-Konfiguration in `amplify.yml`.

## 🧑‍💻 Entwicklungshinweise

- `docs/amplify-status.md` enthält einen operativen Snapshot der gehosteten Anwendung (`onlyideas.art`).
- Legacy `.old` Dateien existieren in Backend-/Frontend-Verzeichnissen; behalten Sie sie als historische Referenz bei, solange sie nicht gezielt aufgeräumt werden.
- Aktuell ist kein automatisierter Test-Workflow in `package.json` konfiguriert.

Schwerpunkte für manuelle Verifikation:

1. Authentifizierungsfluss (falls Auth-UI per Router eingebunden wird).
2. Idea- und Creator-CRUD-Verhalten gegen Amplify Data.
3. Integrität des Produktions-Builds über `npm run build`.

## 🐞 Fehlerbehebung

- `Cannot find module '../amplify_outputs.json'`:
  - Erzeugen Sie Backend-Outputs mit Amplify Sandbox/Pull, bevor Sie die App starten.
- Authentifizierte Routen funktionieren nicht:
  - `main.ts` mountet aktuell nicht `src/router/index.js`; das ist im aktuellen Landing-Page-Setup so vorgesehen.
- Build scheitert bei Typen:
  - Führen Sie `npm run type-check` aus und beheben Sie TypeScript-Probleme vor dem Bundle.
- Probleme beim CI-Backend-Deployment:
  - Stellen Sie sicher, dass `AWS_BRANCH` und `AWS_APP_ID` in den Amplify Hosting Umgebungsvariablen vorhanden sind.

## 🗺️ Roadmap

1. Mehrsprachige README-Dateien unter `i18n/` finalisieren, sodass sie zu den Sprachlinks oben passen.
2. Entscheiden, ob der router-basierte authentifizierte App-Modus als Primärmodus wiederhergestellt oder als Legacy archiviert werden soll.
3. Automatisierte Tests hinzufügen (empfohlen: Vitest + Vue Test Utils).
4. Übergangs-`.old`-Dateien und auskommentiertes Tutorial-Scaffolding bereinigen, sobald sie nicht mehr benötigt werden.
5. Zusätzliche Produktions-Härtung laut `docs/amplify-status.md` aktivieren (zum Beispiel AWS WAF).

## 🤝 Mitwirken

Beiträge sind willkommen. Bitte lesen Sie [CONTRIBUTING.md](CONTRIBUTING.md), bevor Sie Issues oder Pull Requests eröffnen.

Dieses Projekt folgt dem [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct). Weitere Informationen finden Sie in [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## 🛡️ Sicherheit

Weitere Details finden Sie unter [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications).

## Lizenz

Diese Bibliothek ist unter der MIT-0-Lizenz lizenziert. Siehe die Datei LICENSE.


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
