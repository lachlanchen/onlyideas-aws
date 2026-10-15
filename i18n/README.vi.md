[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner — apps and four buddies](https://lazying.art/media/brand/lazyingart-intro.gif)](https://lazying.art)


# Mẫu Khởi Đầu AWS Amplify Vue.js

![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vue.js&logoColor=white)
![Vite 5](https://img.shields.io/badge/Vite-5-646cff?logo=vite&logoColor=white)
![Amplify Gen 2](https://img.shields.io/badge/AWS_Amplify-Gen_2-ff9900?logo=amazonaws&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178c6?logo=typescript&logoColor=white)
![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue.svg)

Kho này cung cấp một mẫu khởi đầu để xây dựng ứng dụng bằng Vue.js và AWS Amplify, tập trung vào việc thiết lập dễ dàng cho xác thực, API và cơ sở dữ liệu.

## Tổng quan

Mẫu này trang bị cho bạn một ứng dụng Vue nền tảng đã tích hợp AWS Amplify, được tinh gọn cho khả năng mở rộng và hiệu năng. Đây là lựa chọn phù hợp cho nhà phát triển muốn khởi động dự án nhanh với các dịch vụ AWS đã cấu hình sẵn như Cognito, AppSync và DynamoDB.

Ở trạng thái hiện tại của kho, ứng dụng gồm hai hướng thực tế:

1. Một trải nghiệm marketing/landing một trang `OnlyIdeas` đang hoạt động, được render bởi `src/App.vue`.
2. Các thành phần xác thực và CRUD kiểu legacy/template (`src/router/index.js`, `src/components/Ideas.vue`, `src/components/Creators.vue`, v.v.) có sẵn để mở rộng nhưng hiện chưa được nối trong `src/main.ts`.

### Tóm tắt nhanh

| Khu vực | Trạng thái hiện tại |
| --- | --- |
| Điểm vào runtime frontend | `src/main.ts` mount trực tiếp `App.vue` |
| Backend stack | Amplify Gen 2 (`auth` + `data`) |
| Mô hình dữ liệu | `Idea` và `Creator` |
| Chế độ xác thực | Đăng nhập email + phân quyền Cognito User Pool |
| CI/CD | Pipeline Amplify Hosting qua `amplify.yml` |
| Tệp README i18n | Đã có liên kết ngôn ngữ; các tệp trong `i18n/` đang chờ hoàn thiện |

## Tính năng

- 🔐 **Xác thực**: Thiết lập với Amazon Cognito cho xác thực người dùng an toàn.
- ⚡ **API**: Endpoint GraphQL sẵn dùng với AWS AppSync.
- 🗄️ **Cơ sở dữ liệu**: Cơ sở dữ liệu thời gian thực vận hành bởi Amazon DynamoDB.
- 🧩 **Định nghĩa backend Amplify Gen 2**:
  - `amplify/auth/resource.ts` định nghĩa đăng nhập email.
  - `amplify/data/resource.ts` định nghĩa model `Idea` và `Creator` với phân quyền owner.
- 🛠️ **Bộ công cụ frontend sẵn sàng production**:
  - Vue 3 + TypeScript + Vite.
  - Script type-check và build (`vue-tsc` + Vite build).
- 🚀 **Cấu hình CI/CD Amplify Hosting** qua `amplify.yml`.

## Cấu trúc dự án

```text
onlyideas-aws/
├─ amplify/                    # định nghĩa backend Amplify Gen 2
│  ├─ backend.ts
│  ├─ auth/resource.ts
│  └─ data/resource.ts
├─ docs/
│  └─ amplify-status.md        # ảnh chụp trạng thái triển khai
├─ i18n/                       # đích README đa ngôn ngữ (hiện là placeholder)
├─ public/
├─ src/
│  ├─ App.vue                  # UI trang landing đang hoạt động
│  ├─ main.ts                  # bootstrap app + Amplify.configure(outputs)
│  ├─ router/index.js          # luồng router legacy (hiện chưa được mount)
│  ├─ views/Home.vue
│  └─ components/
│     ├─ Auth.vue
│     ├─ Ideas.vue
│     ├─ Creators.vue
│     └─ Todos.vue
├─ amplify.yml                 # pipeline build/deploy Amplify Hosting
├─ package.json
└─ README.md
```

## Điều kiện tiên quyết

- Khuyến nghị Node.js 20+ (dự án dùng `@tsconfig/node20` và toolchain Vite hiện đại).
- npm 9+.
- Quyền truy cập tài khoản AWS nếu chạy sandbox backend/quy trình deploy.

## Cài đặt

```bash
npm install
```

Nếu bạn dự định chạy ứng dụng với chức năng kết nối backend, hãy provision hoặc pull output backend Amplify trước để `amplify_outputs.json` khả dụng.

Quy trình backend local phổ biến:

```bash
npx ampx sandbox
```

Hoặc, cho triển khai pipeline CI (đã cấu hình trong `amplify.yml`):

```bash
npx ampx pipeline-deploy --branch <branch> --app-id <app-id>
```

## Cách dùng

| Tác vụ | Lệnh |
| --- | --- |
| Bắt đầu phát triển local | `npm run dev` |
| Build bundle production (bao gồm type-check) | `npm run build` |
| Preview bản build production | `npm run preview` |
| Chỉ chạy type-check | `npm run type-check` |

## Cấu hình

### Output runtime Amplify

`src/main.ts` import `../amplify_outputs.json` và gọi:

```ts
Amplify.configure(outputs)
```

Nếu thiếu `amplify_outputs.json`, hãy chạy provision/pull backend trước. Tệp này được đưa vào gitignore có chủ đích (`amplify_outputs*`) để mỗi môi trường tự tạo output riêng.

### Schema backend và auth

- Auth dựa trên email (`amplify/auth/resource.ts`).
- Các model dữ liệu hiện gồm:
  - `Idea { content }`
  - `Creator { name, bio }`
- Chế độ phân quyền mặc định là Cognito user pool (`userPool`) với rule dựa trên owner.

### Biến môi trường

Dùng `.env` với các key tiền tố `VITE_` cho cấu hình frontend. Cập nhật `env.d.ts` khi thêm biến môi trường có kiểu mới.

## Ví dụ

### Hành vi mặc định của ứng dụng hiện tại

Điểm vào app hiện tại mount trực tiếp `App.vue`:

```ts
createApp(App).mount('#app')
```

Điều này có nghĩa UI landing-page là trải nghiệm runtime mặc định.

### Sử dụng các component demo CRUD hiện có

Các component trong kho như `src/components/Ideas.vue` và `src/components/Creators.vue` đã thể hiện cách dùng `generateClient<Schema>()` và `observeQuery()` cho Amplify Data.

Nếu bạn muốn chuyển sang các trang xác thực dựa trên router, bạn có thể nối lại `src/router/index.js` vào `src/main.ts`. Lưu ý: route guard này dùng kiểu cũ `Auth.currentAuthenticatedUser()` và có thể cần cập nhật theo phiên bản Amplify đã cài.

## Triển khai lên AWS

Để biết hướng dẫn chi tiết khi triển khai ứng dụng, tham khảo [phần triển khai](https://docs.amplify.aws/vue/start/quickstart/#deploy-a-fullstack-app-to-aws) trong tài liệu.

Kho này cũng bao gồm cấu hình build Amplify Hosting trong `amplify.yml`.

## Ghi chú phát triển

- `docs/amplify-status.md` chứa ảnh chụp vận hành của ứng dụng đã host (`onlyideas.art`).
- Các tệp `.old` legacy tồn tại trong thư mục backend/frontend; giữ lại như tham chiếu lịch sử trừ khi chủ động dọn dẹp.
- Hiện chưa có test harness tự động được cấu hình trong `package.json`.

Các khu vực cần kiểm tra thủ công:

1. Luồng xác thực (nếu bật/nối auth UI theo router).
2. Hành vi CRUD idea và creator với Amplify Data.
3. Tính toàn vẹn bản build production qua `npm run build`.

## Khắc phục sự cố

- `Cannot find module '../amplify_outputs.json'`:
  - Tạo output backend bằng Amplify sandbox/pull trước khi chạy ứng dụng.
- Các route yêu cầu xác thực không hoạt động:
  - `main.ts` hiện không mount `src/router/index.js`; đây là hành vi dự kiến trong cấu hình landing-page hiện tại.
- Build lỗi kiểu dữ liệu:
  - Chạy `npm run type-check` và sửa lỗi TypeScript trước khi bundling.
- Lỗi deploy backend trên CI:
  - Xác thực `AWS_BRANCH` và `AWS_APP_ID` có sẵn trong biến môi trường Amplify Hosting.

## Lộ trình

1. Hoàn thiện các tệp README đa ngôn ngữ trong `i18n/` để khớp với liên kết điều hướng ngôn ngữ ở trên.
2. Quyết định có nên khôi phục chế độ app xác thực dựa trên router làm chính, hay lưu trữ như legacy.
3. Thêm test tự động (khuyến nghị: Vitest + Vue Test Utils).
4. Dọn dẹp các tệp chuyển tiếp `.old` và phần khung tutorial đã comment khi không còn cần thiết.
5. Bật thêm các biện pháp tăng cường production được ghi trong `docs/amplify-status.md` (ví dụ AWS WAF).

## Đóng góp

Hoan nghênh đóng góp. Vui lòng đọc [CONTRIBUTING.md](CONTRIBUTING.md) trước khi mở issue hoặc pull request.

Dự án này áp dụng [Amazon Open Source Code of Conduct](https://aws.github.io/code-of-conduct). Xem thêm thông tin tại [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## Bảo mật

Xem [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) để biết thêm thông tin.

## Giấy phép

Thư viện này được cấp phép theo MIT-0 License. Xem tệp LICENSE.


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
