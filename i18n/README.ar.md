[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# قالب بداية AWS Amplify لـ Vue.js

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

يوفّر هذا المستودع قالبًا ابتدائيًا لإنشاء تطبيقات باستخدام Vue.js وAWS Amplify، مع التركيز على سهولة الإعداد لميزات المصادقة وواجهات API وقواعد البيانات.

## نظرة عامة

يمنحك هذا القالب تطبيق Vue أساسيًا مدمجًا مع AWS Amplify، ومهيأً بشكل مبسّط للتوسع والأداء. وهو مناسب للمطورين الذين يريدون بدء مشاريعهم بسرعة مع خدمات AWS مضبوطة مسبقًا مثل Cognito وAppSync وDynamoDB.

في الحالة الحالية للمستودع، يتضمن التطبيق مسارين عمليين:

1. تجربة تسويقية/صفحة هبوط `OnlyIdeas` فعّالة من صفحة واحدة، ويتم عرضها عبر `src/App.vue`.
2. مكونات قديمة/بنمط القالب للمصادقة وعمليات CRUD (`src/router/index.js` و`src/components/Ideas.vue` و`src/components/Creators.vue` وغيرها) موجودة للتوسعة، لكنها غير موصولة حاليًا في `src/main.ts`.

### لمحة سريعة

| المجال | الحالة الحالية |
| --- | --- |
| نقطة تشغيل الواجهة الأمامية | `src/main.ts` يقوم بتركيب `App.vue` مباشرة |
| مكدس الخلفية | Amplify Gen 2 (`auth` + `data`) |
| نماذج البيانات | `Idea` و`Creator` |
| وضع المصادقة | تسجيل دخول بالبريد الإلكتروني + تفويض Cognito User Pool |
| CI/CD | خط Amplify Hosting عبر `amplify.yml` |
| ملفات README متعددة اللغات | روابط اللغات موجودة؛ والملفات تحت `i18n/` بانتظار الاستكمال |

## الميزات

- 🔐 **المصادقة**: إعداد باستخدام Amazon Cognito لمصادقة مستخدمين آمنة.
- ⚡ **API**: نقطة نهاية GraphQL جاهزة للاستخدام عبر AWS AppSync.
- 🗄️ **قاعدة البيانات**: قاعدة بيانات لحظية مدعومة بـ Amazon DynamoDB.
- 🧩 **تعريفات الخلفية في Amplify Gen 2**:
  - `amplify/auth/resource.ts` يعرّف تسجيل الدخول بالبريد الإلكتروني.
  - `amplify/data/resource.ts` يعرّف نماذج `Idea` و`Creator` مع تفويض المالك.
- 🛠️ **سلسلة أدوات واجهة أمامية جاهزة للإنتاج**:
  - Vue 3 + TypeScript + Vite.
  - سكربتات فحص الأنواع والبناء (`vue-tsc` + Vite build).
- 🚀 **إعداد CI/CD لـ Amplify Hosting** عبر `amplify.yml`.

## هيكل المشروع

```text
onlyideas-aws/
├─ amplify/                    # تعريفات الخلفية في Amplify Gen 2
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # لقطة حالة النشر
├─ i18n/                       # أهداف README متعددة اللغات (حاليًا عناصر نائبة)
├─ public/
├─ src/
│  ├─ App.vue                  # واجهة صفحة الهبوط النشطة
│  ├─ main.ts                  # تهيئة التطبيق + Amplify.configure(outputs)
│  ├─ router/index.js          # تدفق router قديم (غير مركّب حاليًا)
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # خط بناء/نشر Amplify Hosting
├─ package.json
└─ README.md
```

## المتطلبات المسبقة

- يُنصح باستخدام Node.js 20+ (المشروع يستخدم `@tsconfig/node20` وأدوات Vite حديثة).
- npm 9+.
- صلاحية الوصول إلى حساب AWS إذا كنت ستشغّل سير عمل sandbox/deploy للخلفية.

## التثبيت

```bash
npm install
```

إذا كنت تخطط لتشغيل التطبيق مع وظائف متصلة بالخلفية، فقم أولًا بتهيئة أو سحب مخرجات Amplify الخلفية كي يصبح `amplify_outputs.json` متاحًا.

سير العمل المحلي الشائع للخلفية:

```bash
npx ampx sandbox
```

أو لنشرات خط CI (مهيأة مسبقًا في `amplify.yml`):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## الاستخدام

| المهمة | الأمر |
| --- | --- |
| بدء التطوير المحلي | `npm run dev` |
| بناء حزمة الإنتاج (يشمل فحص الأنواع) | `npm run build` |
| معاينة بناء الإنتاج | `npm run preview` |
| تشغيل فحص الأنواع فقط | `npm run type-check` |

## الإعداد

### مخرجات تشغيل Amplify

يقوم `src/main.ts` باستيراد `../amplify_outputs.json` ويستدعي:

```ts
Amplify.configure(outputs)
```

إذا كان `amplify_outputs.json` مفقودًا، شغّل تهيئة/سحب الخلفية أولًا. هذا الملف متجاهل عمدًا في git (`amplify_outputs*`) بحيث تُنشئ كل بيئة مخرجاتها الخاصة.

### مخطط الخلفية والمصادقة

- المصادقة تعتمد على البريد الإلكتروني (`amplify/auth/resource.ts`).
- تشمل نماذج البيانات حاليًا:
  - `Idea { content }`
  - `Creator { name, bio }`
- وضع التفويض الافتراضي هو Cognito user pool (`userPool`) مع قواعد تعتمد على المالك.

### متغيرات البيئة

استخدم `.env` مع مفاتيح مسبوقة بـ `VITE_` لإعدادات الواجهة الأمامية. حدّث `env.d.ts` عند إضافة متغيرات بيئة جديدة ذات أنواع معرّفة.

## أمثلة

### السلوك الافتراضي الحالي للتطبيق

نقطة دخول التطبيق الحالية تركّب `App.vue` مباشرة:

```ts
createApp(App).mount('#app')
```

وهذا يعني أن واجهة صفحة الهبوط هي تجربة التشغيل الافتراضية.

### استخدام مكونات CRUD الموجودة

مكونات المستودع مثل `src/components/Ideas.vue` و`src/components/Creators.vue` تعرض مسبقًا استخدام `generateClient<Schema>()` و`observeQuery()` في Amplify Data.

إذا أردت التحويل إلى صفحات مصادقة معتمدة على router، يمكنك إعادة توصيل `src/router/index.js` داخل `src/main.ts`. ملاحظة: حارس المسارات هذا يستخدم نمطًا أقدم من `Auth.currentAuthenticatedUser()` وقد يحتاج تحديثًا ليتوافق مع إصدار Amplify المثبّت لديك.

## النشر إلى AWS

للحصول على تعليمات مفصلة حول نشر التطبيق، راجع [قسم النشر](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) في التوثيق.

يتضمن هذا المستودع أيضًا إعداد بناء Amplify Hosting في `amplify.yml`.

## ملاحظات التطوير

- يحتوي `docs/amplify-status.md` على لقطة تشغيلية للتطبيق المستضاف (`onlyideas.art`).
- توجد ملفات `.old` قديمة في مجلدات الخلفية والواجهة الأمامية؛ احتفظ بها كمرجع تاريخي ما لم يتم تنظيفها عمدًا.
- لا توجد حاليًا بيئة اختبارات آلية مهيأة في `package.json`.

مجالات التحقق اليدوي التي يُنصح بالتركيز عليها:

1. تدفق المصادقة (عند تمكين/إعادة توصيل واجهة المصادقة المعتمدة على router).
2. سلوك CRUD للأفكار والمنشئين مقابل Amplify Data.
3. سلامة بناء الإنتاج عبر `npm run build`.

## استكشاف الأخطاء وإصلاحها

- `Cannot find module '../amplify_outputs.json'`:
  - أنشئ مخرجات الخلفية عبر Amplify sandbox/pull قبل بدء التطبيق.
- المسارات التي تتطلب مصادقة لا تعمل:
  - `main.ts` لا يقوم حاليًا بتركيب `src/router/index.js`؛ وهذا متوقع في إعداد صفحة الهبوط الحالي.
- فشل البناء بسبب الأنواع:
  - شغّل `npm run type-check` وأصلح مشكلات TypeScript قبل التجميع.
- مشكلات نشر الخلفية في CI:
  - تحقق من توفر `AWS_BRANCH` و`AWS_APP_ID` في متغيرات بيئة Amplify Hosting.

## خارطة الطريق

1. استكمال ملفات README متعددة اللغات تحت `i18n/` بما يتوافق مع روابط اللغات أعلاه.
2. تحديد ما إذا كان يجب إعادة وضع التطبيق المعتمد على router والمصادقة كخيار أساسي، أو أرشفته كمسار قديم.
3. إضافة اختبارات آلية (موصى به: Vitest + Vue Test Utils).
4. تنظيف ملفات `.old` الانتقالية وهيكل الشرح المعلّق عندما لا يعود مطلوبًا.
5. تفعيل المزيد من تعزيزات الإنتاج المذكورة في `docs/amplify-status.md` (مثل AWS WAF).

## المساهمة

المساهمات مرحب بها. يُرجى قراءة [CONTRIBUTING.md](../CONTRIBUTING.md) قبل فتح القضايا أو طلبات السحب.

يعتمد هذا المشروع [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct). لمزيد من المعلومات راجع [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md).

## الأمان

راجع [CONTRIBUTING](../CONTRIBUTING.md#security-issue-notifications) لمزيد من المعلومات.

## الترخيص

هذه المكتبة مرخّصة بموجب ترخيص MIT-0. راجع ملف LICENSE.
