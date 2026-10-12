# Vô Hạn Nhàm Chán

> Game được tạo ra từ 1 kẻ đang nhàm chán.
> Không thắng, không thua. Chỉ chọn 1 cách để tận hưởng sự nhàm chán.

Game chạm vô định cho điện thoại Android. Không quảng cáo, không tài khoản, chạy hoàn toàn offline.

| Chế độ | Cách chơi |
|---|---|
| **Vô Hạn Cào Bóc** | Cào từng lớp vật liệu dễ ngủ. Kéo từ mép để bóc mảng lớn. Âm nền Mưa / Sóng / Quạt. |
| **Gợn Sóng** | Mặt hồ mô phỏng bằng WebGL, có cá koi. Chạm đều tay để cá quen dần và bơi lại. |
| **Vô Hạn Loạn Âm** | Chạm hay vuốt đâu cũng ra nhạc. Ba tiếng: chuông gió, đàn tranh, kalimba. |
| **0.8%** | Bi tí hon, lỗ vừa khít bi, cách nhau ít nhất nửa bàn. Mỗi lượt một cú vuốt, phải chạm ít nhất 1 băng rồi vào lỗ mới tính. |

---

## Cấu trúc thư mục

```
vo-han-nham-chan/
├── www/                      ← TOÀN BỘ GAME (sửa game ở đây)
│   ├── index.html
│   ├── css/style.css         giao diện
│   ├── css/fonts.css         font chạy offline
│   ├── js/game.js            logic, đồ họa, âm thanh
│   └── fonts/                file font .woff2
├── android/                  project Android (Capacitor sinh ra, đã chỉnh icon, màn chờ, khoá dọc…)
│   └── app/shared-dev.keystore   khoá ký chung để các bản APK cài đè được lên nhau
├── .github/workflows/build-apk.yml   GitHub tự build APK
├── capacitor.config.json     tên app, mã app, màu nền
├── package.json / package-lock.json
├── .gitignore / .gitattributes
└── README.md
```

Không có bước build web: game là HTML, CSS, JS thuần nằm trong `www/`. Capacitor chép thư mục này vào app Android.

---

## 1. Đưa code lên GitHub

### Cách A: dùng dòng lệnh git (khuyên dùng)

1. Tạo repo mới trên github.com: bấm **New repository**, đặt tên `vo-han-nham-chan`, để **trống** (không tick README, .gitignore hay license).
2. Giải nén file zip, mở terminal ngay trong thư mục `vo-han-nham-chan` (thư mục có file `package.json`), rồi chạy:

```bash
git init
git add .
git commit -m "Vô Hạn Nhàm Chán: bản Android đầu tiên"
git branch -M main
git remote add origin https://github.com/<tên-tài-khoản>/vo-han-nham-chan.git
git push -u origin main
```

Nếu được hỏi mật khẩu khi `git push`, GitHub **không** nhận mật khẩu tài khoản. Bạn cần vào *Settings → Developer settings → Personal access tokens*, tạo token có quyền `repo` (và `workflow`), rồi dán token vào chỗ mật khẩu.

### Cách B: dùng GitHub Desktop (không cần gõ lệnh)

1. Cài GitHub Desktop, chọn **File → Add local repository**, trỏ tới thư mục `vo-han-nham-chan`. Nếu được hỏi tạo repository thì bấm **create a repository**.
2. Viết nội dung commit, bấm **Commit to main**, rồi bấm **Publish repository**. Nhớ bỏ tick *Keep this code private* nếu muốn để công khai.

> Không nên kéo thả file lên trang web GitHub: trình duyệt dễ bỏ sót thư mục ẩn `.github` (chứa workflow build APK).

---

## 2. Lấy file APK

Ngay sau khi push, GitHub tự build APK trong khoảng 4–8 phút.

### Cách nhanh: tải từ tab Actions

1. Vào repo trên GitHub, mở tab **Actions** rồi bấm vào lần chạy **Build APK** mới nhất (có dấu tích xanh).
2. Kéo xuống phần **Artifacts**, bấm vào `VoHanNhamChan-1.0.x.apk` để tải. File tải về là APK luôn, không phải zip.

Tab Actions yêu cầu đăng nhập GitHub. Artifact được giữ 30 ngày.

### Cách tiện cho điện thoại: tạo bản phát hành (Release)

Gắn tag phiên bản rồi push:

```bash
git tag v1.0.0
git push origin v1.0.0
```

Workflow sẽ build xong rồi tự tạo trang **Releases → v1.0.0** có file `VoHanNhamChan-1.0.0.apk`. Bạn mở trang đó bằng trình duyệt trên điện thoại và tải thẳng file APK về. Lần sau tăng số: `v1.0.1`, `v1.1.0`…

Muốn build lại mà không sửa gì: tab **Actions → Build APK → Run workflow**.

---

## 3. Cài APK lên điện thoại Android

1. Mở file APK vừa tải.
2. Android hỏi về *cài ứng dụng không rõ nguồn gốc*: bấm **Cài đặt**, rồi bật **Cho phép từ nguồn này** cho trình duyệt hoặc trình quản lý file đang mở APK.
3. Bấm **Cài đặt** lần nữa. Nếu Play Protect cảnh báo, chọn **Vẫn cài đặt** (app tự build nên Google chưa biết đến).

**Cập nhật:** mọi bản build đều ký bằng cùng một khoá (`android/app/shared-dev.keystore`) và số phiên bản luôn tăng, nên bản mới cài đè lên bản cũ mà không mất tiến trình chơi.

---

## 4. Sửa game

1. Sửa file trong `www/` (thường là `www/js/game.js` hoặc `www/css/style.css`).
2. Thử trên máy tính: mở `www/index.html` bằng Chrome và bật chế độ điện thoại (F12 → biểu tượng điện thoại). Hoặc chạy `npm run serve` rồi mở địa chỉ hiện ra.
3. Đẩy lên GitHub để có APK mới:

```bash
git add .
git commit -m "Sửa ..."
git push
```

Không cần chạy `npx cap sync` trước khi push, vì GitHub tự chạy.

---

## 5. (Tuỳ chọn) Ký bằng khoá riêng

Khoá chung `shared-dev.keystore` nằm công khai trong repo, nên chỉ phù hợp để tự cài và chia sẻ APK cho bạn bè. Muốn đưa lên Google Play hoặc muốn không ai ký giả được app của bạn thì dùng khoá riêng:

1. Tạo khoá. Cần Java, có sẵn khi cài Android Studio hoặc JDK 17 trở lên:

```bash
keytool -genkeypair -v -keystore release.keystore -storetype PKCS12 \
  -alias vohannhamchan -keyalg RSA -keysize 2048 -validity 10000
```

2. Chuyển khoá thành chuỗi base64:
   - macOS / Linux: `base64 -i release.keystore -o keystore.base64.txt` (Linux: `base64 -w0 release.keystore > keystore.base64.txt`)
   - Windows PowerShell: `[Convert]::ToBase64String([IO.File]::ReadAllBytes("release.keystore")) > keystore.base64.txt`
3. Trên GitHub, vào **Settings → Secrets and variables → Actions → New repository secret** và tạo 4 secret:

| Tên secret | Giá trị |
|---|---|
| `ANDROID_KEYSTORE_BASE64` | toàn bộ nội dung `keystore.base64.txt` |
| `ANDROID_KEYSTORE_PASSWORD` | mật khẩu keystore |
| `ANDROID_KEY_ALIAS` | `vohannhamchan` (hoặc alias bạn đặt) |
| `ANDROID_KEY_PASSWORD` | mật khẩu khoá (với PKCS12 thường trùng mật khẩu keystore) |

4. Push lại. Từ đó workflow ký APK bằng khoá riêng.

**Cất giữ `release.keystore` và mật khẩu cẩn thận. Không commit file này** (đã có trong `.gitignore`). Mất khoá là không cập nhật được app trên Google Play.

Lưu ý: APK ký bằng khoá riêng không cài đè được lên APK ký bằng khoá chung. Lần đầu đổi khoá, bạn phải gỡ app cũ trước.

---

## 6. (Tuỳ chọn) Build APK trên máy tính

Cần Node.js 22+, JDK 21 và Android Studio (kèm Android SDK).

```bash
npm ci
npx cap sync android
npx cap open android        # mở bằng Android Studio, bấm Run để chạy trên điện thoại/giả lập
# hoặc build bằng dòng lệnh:
cd android && ./gradlew assembleRelease
# file ra: android/app/build/outputs/apk/release/app-release.apk
```

---

## Thông tin kỹ thuật

- **Capacitor 8.5.3** bọc game web thành app Android. Android tối thiểu 7.0 (API 24), target API 36.
- Game viết bằng HTML/CSS/JavaScript thuần: Canvas 2D, WebGL (mặt nước), Web Audio (toàn bộ âm thanh được tổng hợp, không có file âm thanh).
- Font Bricolage Grotesque, Be Vietnam Pro, JetBrains Mono được đóng gói sẵn (SIL Open Font License), không cần mạng.
- App khoá màn hình dọc. Có quyền rung (`VIBRATE`). Nút Back của Android quay về màn hình chính. Âm thanh tự tạm dừng khi app chạy nền.
- Tiến trình chơi lưu bằng `localStorage` trên máy. Gỡ app là mất tiến trình.
- Mã ứng dụng: `com.vohannhamchan.app`. Muốn đổi thì sửa `appId` trong `capacitor.config.json`, cùng với `namespace`/`applicationId` trong `android/app/build.gradle` và đường dẫn package của `MainActivity.java`.
