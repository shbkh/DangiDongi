# دنگ — نسخهٔ اپ موبایل (اندروید و iOS)

## راه ۱ (سریع‌ترین، هم اندروید هم آیفون): نصب به‌صورت PWA
پوشهٔ `www` را روی هر هاستی که HTTPS دارد قرار دهید (GitHub Pages، Netlify، Cloudflare Pages و …).
- **اندروید (Chrome):** منوی ⋮ ← «Install app / نصب برنامه»
- **آیفون (Safari):** دکمهٔ Share ← «Add to Home Screen / افزودن به صفحهٔ اصلی»
بعد از نصب، مثل اپ معمولی و بدون نوار آدرس و آفلاین کار می‌کند.

## راه ۲: ساخت فایل APK (اندروید) با GitHub Actions — بدون نصب هیچ ابزاری
1. یک مخزن (Repository) جدید در GitHub بسازید و تمام محتویات این پوشه را در آن آپلود کنید.
2. به تب **Actions** بروید ← «Build Android APK» ← **Run workflow**.
3. پس از ~۵ دقیقه، از پایین صفحهٔ اجرا، فایل **dang-apk** (حاوی `app-debug.apk`) را دانلود کنید.
4. APK را روی گوشی بریزید و نصب کنید (اجازهٔ «نصب از منابع ناشناس» لازم است).

برای فعال‌شدن نسخهٔ وب (راه ۱) هم: Settings ← Pages ← Source: **GitHub Actions**.

## ساخت روی کامپیوتر خودتان
```bash
npm install
npx cap add android
npx capacitor-assets generate --android --iconBackgroundColor '#667eea' --iconBackgroundColorDark '#667eea'
npx cap sync android
cd android && ./gradlew assembleDebug      # خروجی: android/app/build/outputs/apk/debug/app-debug.apk
```
(نیازمند Node 20، JDK 21 و Android SDK؛ یا `npx cap open android` برای باز کردن در Android Studio.)

## iOS (فایل IPA / App Store)
ساخت اپ بومی iOS فقط روی **macOS با Xcode** ممکن است و برای نصب روی آیفون واقعی یا انتشار، **حساب Apple Developer** لازم است:
```bash
npm install
npx cap add ios
npx capacitor-assets generate --ios --iconBackgroundColor '#667eea' --iconBackgroundColorDark '#667eea'
npx cap sync ios
npx cap open ios      # در Xcode: Signing & Capabilities ← Team را انتخاب کنید ← Run / Archive
```
اگر حساب Apple Developer ندارید، راه ۱ (PWA) بهترین گزینه برای آیفون است.

## تغییرات اعمال‌شده روی `index.html`
- افزودن manifest، آیکون‌ها، متاتگ‌های iOS و Service Worker (آفلاین)
- رعایت ناحیهٔ امن (notch / نوار وضعیت) و جلوگیری از زوم خودکار iOS
- خروجی CSV: در اپ بومی و موبایل، دکمهٔ «ذخیره / ارسال فایل» از منوی اشتراک‌گذاری سیستم استفاده می‌کند (لینک blob داخل WebView کار نمی‌کند)؛ فایل با BOM ذخیره می‌شود تا فارسی در Excel درست نمایش داده شود
- منطق برنامه و ظاهر آن دست‌نخورده است
