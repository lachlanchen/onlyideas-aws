[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Modèle de démarrage AWS Amplify Vue.js

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

Ce dépôt fournit un modèle de démarrage pour créer des applications avec Vue.js et AWS Amplify, en mettant l’accent sur une configuration simple de l’authentification, de l’API et des capacités de base de données.

## Vue d’ensemble

Ce modèle vous fournit une application Vue de base intégrée à AWS Amplify, optimisée pour la scalabilité et les performances. Il est idéal pour les développeurs qui veulent lancer rapidement leur projet avec des services AWS préconfigurés comme Cognito, AppSync et DynamoDB.

Dans l’état actuel du dépôt, l’application inclut deux axes pratiques :

1. Une expérience marketing/landing `OnlyIdeas` active en page unique, rendue par `src/App.vue`.
2. Des composants hérités/de type template pour l’authentification et le CRUD (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue`, etc.), présents pour extension mais non connectés actuellement dans `src/main.ts`.

### En un coup d’œil

| Zone | État actuel |
| --- | --- |
| Point d’entrée runtime frontend | `src/main.ts` monte directement `App.vue` |
| Stack backend | Amplify Gen 2 (`auth` + `data`) |
| Modèles de données | `Idea` et `Creator` |
| Mode d’authentification | Connexion par e-mail + autorisation Cognito User Pool |
| CI/CD | Pipeline Amplify Hosting via `amplify.yml` |
| Fichiers README i18n | Liens de langue présents ; fichiers sous `i18n/` en attente de remplissage |

## Fonctionnalités

- 🔐 **Authentification** : configuration avec Amazon Cognito pour une authentification utilisateur sécurisée.
- ⚡ **API** : endpoint GraphQL prêt à l’emploi avec AWS AppSync.
- 🗄️ **Base de données** : base de données temps réel propulsée par Amazon DynamoDB.
- 🧩 **Définitions backend Amplify Gen 2** :
  - `amplify/auth/resource.ts` définit la connexion par e-mail.
  - `amplify/data/resource.ts` définit les modèles `Idea` et `Creator` avec autorisation par propriétaire.
- 🛠️ **Chaîne d’outils frontend prête pour la production** :
  - Vue 3 + TypeScript + Vite.
  - Scripts de vérification de types et de build (`vue-tsc` + build Vite).
- 🚀 **Configuration CI/CD Amplify Hosting** via `amplify.yml`.

## Structure du projet

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

## Prérequis

- Node.js 20+ recommandé (le projet utilise `@tsconfig/node20` et des outils Vite modernes).
- npm 9+.
- Un accès à un compte AWS si vous exécutez des workflows de sandbox/déploiement backend.

## Installation

```bash
npm install
```

Si vous prévoyez d’exécuter l’application avec des fonctionnalités connectées au backend, provisionnez ou récupérez d’abord les sorties backend Amplify afin que `amplify_outputs.json` soit disponible.

Workflow backend local courant :

```bash
npx ampx sandbox
```

Ou, pour les déploiements du pipeline CI (déjà configuré dans `amplify.yml`) :

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## Utilisation

| Tâche | Commande |
| --- | --- |
| Démarrer le développement local | `npm run dev` |
| Générer le bundle de production (inclut le type-check) | `npm run build` |
| Prévisualiser le build de production | `npm run preview` |
| Exécuter uniquement le type-check | `npm run type-check` |

## Configuration

### Sorties runtime Amplify

`src/main.ts` importe `../amplify_outputs.json` et appelle :

```ts
Amplify.configure(outputs)
```

Si `amplify_outputs.json` est manquant, exécutez d’abord le provisionnement/la récupération backend. Le fichier est volontairement ignoré par Git (`amplify_outputs*`) afin que chaque environnement génère ses propres sorties.

### Schéma backend et auth

- L’authentification est basée sur l’e-mail (`amplify/auth/resource.ts`).
- Les modèles de données incluent actuellement :
  - `Idea { content }`
  - `Creator { name, bio }`
- Le mode d’autorisation par défaut est le user pool Cognito (`userPool`) avec des règles basées sur le propriétaire.

### Variables d’environnement

Utilisez `.env` avec des clés préfixées par `VITE_` pour la configuration frontend. Mettez à jour `env.d.ts` lors de l’ajout de nouvelles variables d’environnement typées.

## Exemples

### Comportement actuel de l’application par défaut

Le point d’entrée actuel de l’application monte directement `App.vue` :

```ts
createApp(App).mount('#app')
```

Cela signifie que l’interface landing-page est l’expérience runtime par défaut.

### Utilisation des composants de démonstration CRUD existants

Des composants du dépôt comme `src/components/Ideas.vue` et `src/components/Creators.vue` montrent déjà l’utilisation de `generateClient<Schema>()` et `observeQuery()` pour Amplify Data.

Si vous souhaitez revenir à des pages authentifiées basées sur le routeur, vous pouvez reconnecter `src/router/index.js` dans `src/main.ts`. Remarque : ce garde de route utilise un style plus ancien `Auth.currentAuthenticatedUser()` et peut nécessiter des mises à jour selon la version d’Amplify installée.

## Déploiement sur AWS

Pour des instructions détaillées sur le déploiement de votre application, consultez la [section de déploiement](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) de notre documentation.

Ce dépôt inclut également la configuration de build Amplify Hosting dans `amplify.yml`.

## Notes de développement

- `docs/amplify-status.md` contient un état opérationnel de l’application hébergée (`onlyideas.art`).
- Des fichiers `.old` hérités existent dans les répertoires backend/frontend ; conservez-les comme référence historique sauf nettoyage volontaire.
- Il n’existe actuellement aucun harnais de tests automatisés configuré dans `package.json`.

Points de vérification manuelle prioritaires :

1. Flux d’authentification (si vous activez/reconnectez l’UI d’auth via le routeur).
2. Comportement CRUD des idées et des créateurs avec Amplify Data.
3. Intégrité du build de production via `npm run build`.

## Dépannage

- `Cannot find module '../amplify_outputs.json'` :
  - Générez les sorties backend via sandbox/pull Amplify avant de démarrer l’application.
- Les routes authentifiées ne fonctionnent pas :
  - `main.ts` ne monte actuellement pas `src/router/index.js` ; c’est attendu dans la configuration landing-page actuelle.
- Le build échoue sur les types :
  - Exécutez `npm run type-check` et corrigez les problèmes TypeScript avant le bundling.
- Problèmes de déploiement backend CI :
  - Vérifiez que `AWS_BRANCH` et `AWS_APP_ID` sont disponibles dans les variables d’environnement Amplify Hosting.

## Feuille de route

1. Finaliser les fichiers README multilingues sous `i18n/` afin de correspondre aux liens de navigation de langue ci-dessus.
2. Décider si le mode application authentifiée basé sur le routeur doit être restauré comme principal, ou archivé comme héritage.
3. Ajouter des tests automatisés (recommandé : Vitest + Vue Test Utils).
4. Nettoyer les fichiers `.old` transitoires et le scaffolding tutoriel commenté lorsqu’ils ne seront plus nécessaires.
5. Activer les renforcements de production supplémentaires notés dans `docs/amplify-status.md` (par exemple AWS WAF).

## Contribution

Les contributions sont les bienvenues. Veuillez lire [CONTRIBUTING.md](CONTRIBUTING.md) avant d’ouvrir des issues ou des pull requests.

Ce projet a adopté le [Code de conduite Amazon Open Source](https://aws.github.io/code-of-conduct). Pour plus d’informations, voir [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## Sécurité

Consultez [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) pour plus d’informations.

## Licence

Cette bibliothèque est sous licence MIT-0. Consultez le fichier LICENSE.
