# إعداد إشعارات WEZTY — مشروع Android

## ما يتضمنه المشروع
- إضافة `@capacitor/push-notifications` المتوافق مع Capacitor 7.
- طلب إذن إشعارات Android وتسجيل FCM token لكل حساب معتمد في `wezty_push_tokens/{uid}`.
- قواعد Firestore تقصر قراءة/تحديث كل token على الحساب صاحب UID نفسه.
- إعداد GitHub Actions لنسخ `google-services.json` إلى `android/app/` وتفعيل Google Services Gradle plugin أثناء البناء.
- إبقاء نسخة الموقع في الجذر ونسخة التطبيق المحلية في `www/index.html` متطابقتين.

## قرار معماري مهم
تمت إزالة `server.url` من `capacitor.config.json` عمداً. التطبيق كان يفتح صفحة GitHub Pages خارج حزمة التطبيق؛ هذا يجعل توافر جسر Capacitor والإضافات الأصلية غير مضمون. الآن سيستخدم APK ملفات `www/` المرفقة داخله، بينما يظل موقع GitHub Pages متاحاً من ملف الجذر `index.html`. أي تحديث لاحق لواجهة APK يحتاج إعادة بناء APK.

## قبل التثبيت
1. انشر قواعد `firestore.rules` في Firebase Console > Firestore Database > Rules بعد مراجعتها.
2. تأكد أن `google-services.json` يعود إلى مشروع `zozo-41d24` وحزمة `com.wezty.app`.
3. شغّل GitHub Actions workflow باسم `Build WEZTY APK`.
4. لا تثبّت APK إلا بعد نجاح البناء ومراجعة سجل GitHub Actions.
5. ثبّت النسخة الجديدة على الجهازين، وافتح التطبيق وسجّل الدخول واسمح بالإشعارات.
6. بعد تسجيل الجهازين، اختبر إرسال إشعار تجريبي قبل الاعتماد على إشعارات الرسائل واللعبة والمنتصف الليلي.

## ملاحظات
- لم يتم تأكيد نجاح البناء أو وصول الإشعارات بعد؛ ذلك يتطلب تشغيل GitHub Actions والاختبار على الجهازين.
- لا ترفع أي Service Account private key إلى المستودع. `google-services.json` هو إعداد عميل Android وليس مفتاح Service Account.
- Apps Script يفحص Firestore كل دقيقة، لذلك قد تتأخر إشعارات الرسائل واللعبة حتى دقيقة أو أكثر قليلاً.
