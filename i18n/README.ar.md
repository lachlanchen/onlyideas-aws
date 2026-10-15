[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner — apps and four buddies](https://lazying.art/media/brand/lazyingart-intro.gif)](https://lazying.art)

# قالب بداية AWS Amplify لـ Vue.js

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![Node 20+](https://img.shields.io/badge/Node-20%2B-5FA04E?logo=nodedotjs&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-6F42C1.svg)

---

يوفّر هذا المستودع قالبًا مبدئيًا لإنشاء تطبيقات باستخدام Vue.js وAWS Amplify، مع التركيز على إعداد سريع للمصادقة وواجهات API وخصائص قاعدة البيانات.

## 🎯 نظرة عامة

يوفر هذا القالب تطبيق Vue أساسيًا مدمجًا مع AWS Amplify، ومهيأً بشكل مبسط للتوسع والأداء. وهو مناسب للمطورين الذين يرغبون في إطلاق مشروعهم بسرعة باستخدام خدمات AWS المكوّنة مسبقًا مثل Cognito وAppSync وDynamoDB.

في حالته الحالية، يتضمن المستودع مسارين عمليين:

1. تجربة تسويقية/صفحة هبوط `OnlyIdeas` فعالة من صفحة واحدة تُعرض عبر `src/App.vue`.
2. مكونات مصادقة وCRUD على نمط القالب القديم (`src/router/index.js`، `src/components/Ideas.vue`، `src/components/Creators.vue` وغيرها) موجودة للتوسعة لكنها غير مفعّلة حاليًا ضمن `src/main.ts`.

### 🧭 لمحة سريعة

| المجال | الحالة الحالية |
| --- | --- |
| نقطة تشغيل الواجهة الأمامية | `src/main.ts` يقوم بتحميل `App.vue` مباشرة |
| طبقة الخلفية | Amplify Gen 2 (`auth` + `data`) |
| نماذج البيانات | `Idea` و`Creator` |
| وضع المصادقة | تسجيل دخول بالبريد الإلكتروني + تفويض `Cognito User Pool` |
| CI/CD | خط نشر Amplify Hosting عبر `amplify.yml` |
| ملفات README متعددة اللغات | روابط اللغات موجودة؛ الملفات داخل `i18n/` ما زالت تحتاج إلى استكمال |

## ✨ الميزات

- 🔐 **المصادقة**: إعداد باستخدام Amazon Cognito لمصادقة آمنة للمستخدمين.
- ⚡ **API**: نقطة نهاية GraphQL جاهزة للاستخدام مع AWS AppSync.
- 🗄️ **قاعدة البيانات**: قاعدة بيانات آنية مدعومة بواسطة Amazon DynamoDB.
- 🧩 **تعريفات الخلفية في Amplify Gen 2**:
  - `amplify/auth/resource.ts` يعرّف تسجيل الدخول بالبريد الإلكتروني.
  - `amplify/data/resource.ts` يعرّف نماذج `Idea` و`Creator` مع تفويض المالك.
- 🛠️ **سلسلة أدوات واجهة أمامية جاهزة للإنتاج**:
  - Vue 3 + TypeScript + Vite.
  - سكربتات فحص الأنواع والبناء (`vue-tsc` + Vite build).
- 🚀 **إعداد CI/CD لـ Amplify Hosting** عبر `amplify.yml`.

## 🧱 هيكل المشروع

```text
onlyideas-aws/
├─ amplify/                    # تعريفات الخلفية في Amplify Gen 2
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # لقطة حالة النشر
├─ i18n/                       # أهداف README متعددة اللغات
├─ public/
├─ src/
│  ├─ App.vue                  # واجهة صفحة الهبوط النشطة
│  ├─ main.ts                  # تهيئة التطبيق + Amplify.configure(outputs)
│  ├─ router/index.js          # تدفق router قديم (غير مركب حاليًا)
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

## 🧰 المتطلبات المسبقة

- يوصى بـ Node.js 20+ (المشروع يستخدم `@tsconfig/node20` وأدوات Vite الحديثة).
- npm 9+.
- الوصول إلى حساب AWS في حال تشغيل تدفقات sandbox/deploy للواجهة الخلفية.

## 🧪 التثبيت

```bash
npm install
```

إذا كنت تخطط لتشغيل التطبيق بوظائف مرتبطة بالخلفية، جهز أولًا مخرجات Amplify أو اسحبها حتى يتوفر `amplify_outputs.json`.

سير عمل الخلفية الشائع محليًا:

```bash
npx ampx sandbox
```

أو لنشر خط CI (مُعدّ مسبقًا في `amplify.yml`):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## ▶️ الاستخدام

| المهمة | الأمر |
| --- | --- |
| بدء التطوير محليًا | `npm run dev` |
| بناء نسخة الإنتاج (يشمل فحص الأنواع) | `npm run build` |
| معاينة نسخة الإنتاج | `npm run preview` |
| تشغيل فحص الأنواع فقط | `npm run type-check` |

## 🔧 الإعداد

### مخرجات تشغيل Amplify

`src/main.ts` يستورد `../amplify_outputs.json` وينفذ:

```ts
Amplify.configure(outputs)
```

إذا كان `amplify_outputs.json` مفقودًا، شغّل أولًا تجهيز أو سحب الخلفية. هذا الملف متجاهَل في Git عمداً (`amplify_outputs*`) حتى تُنتج كل بيئة مخرجاتها الخاصة.

### مخطط الخلفية والمصادقة

- المصادقة معتمدة على البريد الإلكتروني (`amplify/auth/resource.ts`).
- نماذج البيانات الحالية تتضمن:
  - `Idea { content }`
  - `Creator { name, bio }`
- وضع التفويض الافتراضي هو `Cognito user pool` (`userPool`) مع قواعد تعتمد على المالك.

### ⚙️ متغيرات البيئة

استخدم ملف `.env` بمفاتيح ذات بادئة `VITE_` لإعدادات الواجهة الأمامية. حدّث `env.d.ts` عند إضافة متغيرات بيئية ذات أنواع جديدة.

## 📦 الأمثلة

### سلوك التطبيق الافتراضي الحالي

نقطة إدخال التطبيق الحالية تُحمّل `App.vue` مباشرة:

```ts
createApp(App).mount('#app')
```

هذا يعني أن واجهة صفحة الهبوط هي تجربة التشغيل الافتراضية.

### استخدام مكونات عرض CRUD الموجودة

المكوّنات الموجودة في المستودع مثل `src/components/Ideas.vue` و`src/components/Creators.vue` تعرض مسبقًا استخدامات `generateClient<Schema>()` و`observeQuery()` في Amplify Data.

إذا أردت التحويل إلى صفحات مصادقة تعتمد على الـ router، يمكنك إعادة ربط `src/router/index.js` داخل `src/main.ts`. ملاحظة: حارس المسار هنا يستخدم نمطًا قديمًا من `Auth.currentAuthenticatedUser()` وقد يحتاج تحديثًا ليتوافق مع إصدار Amplify المثبّت لديك.

## 🚀 النشر إلى AWS

للحصول على تعليمات تفصيلية حول نشر تطبيقك، راجع [قسم النشر](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) في توثيقنا.

يتضمن هذا المستودع أيضًا إعداد بناء Amplify Hosting في `amplify.yml`.

## 🧑‍💻 ملاحظات التطوير

- `docs/amplify-status.md` يحتوي على لقطة تشغيلية للتطبيق المستضاف (`onlyideas.art`).
- توجد ملفات `.old` قديمة في مجلدات الخلفية/الواجهة الأمامية؛ احتفظ بها كمرجع تاريخي ما لم تقم بإزالتها عمدًا.
- لا يوجد حاليًا سير تحقق آلي مضبوط في `package.json`.

نقاط التحقق اليدوي المقترحة:

1. تدفق المصادقة (إذا تم تفعيل/توصيل واجهة المصادقة المعتمدة على router).
2. سلوك CRUD للأفكار والمبدعين ضد Amplify Data.
3. سلامة بناء الإنتاج عبر `npm run build`.

## 🐞 استكشاف الأخطاء وإصلاحها

- `Cannot find module '../amplify_outputs.json'`:
  - أنشئ مخرجات الخلفية عبر `amplify sandbox/pull` قبل تشغيل التطبيق.
- لا تعمل المسارات التي تتطلب مصادقة:
  - `main.ts` لا يحمّل `src/router/index.js` حاليًا؛ هذا متوقّع ضمن إعداد صفحة الهبوط الحالي.
- فشل البناء بسبب الأنواع:
  - شغّل `npm run type-check` وأصلح أخطاء TypeScript قبل التجميع.
- مشاكل نشر الخلفية في CI:
  - تأكد من وجود المتغيرين `AWS_BRANCH` و`AWS_APP_ID` ضمن متغيرات البيئة في Amplify Hosting.

## 🗺️ خارطة الطريق

1. استكمال ملفات README متعددة اللغات داخل `i18n/` لتطابق روابط اللغات المذكورة أعلاه.
2. تحديد ما إذا كان وضع التطبيق المعتمد على router والمصادقة سيعود كمسار أساسي أم سيُؤرشف كمسار قديم.
3. إضافة اختبارات آلية (موصى به: Vitest + Vue Test Utils).
4. تنظيف ملفات `.old` الانتقالية وهيكل الشرح التعليقي بعد انتهاء الحاجة إليها.
5. تفعيل مزيد من تقويات الإنتاج المذكورة في `docs/amplify-status.md` (مثل AWS WAF).

## 🤝 المساهمة

المساهمات مرحّبة. يرجى قراءة [CONTRIBUTING.md](../CONTRIBUTING.md) قبل فتح قضايا أو طلبات دمج.

اعتمد هذا المشروع [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct). لمزيد من المعلومات، راجع [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md).

## 🛡️ الأمان

راجع [CONTRIBUTING](../CONTRIBUTING.md#security-issue-notifications) للمزيد من التفاصيل.

## الترخيص

هذه المكتبة مرخصة ضمن رخصة MIT-0. راجع ملف LICENSE.


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
