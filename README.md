# موقع مناسك الثابت (GitHub Pages)

موقع static بدون أي أسرار أو مفاتيح، رابطه النهائي: **https://manasik-app.github.io**

| الملف | الغرض |
|---|---|
| `index.html` | صفحة تعريفية + زر Google Play |
| `privacy.html` / `terms.html` | سياسة الخصوصية والشروط (مولَّدة من `LegalTexts` في التطبيق) |
| `p.html` | رابط مشاركة البرنامج: `p.html?id=<uuid>` يحاول فتح التطبيق (`intent://program/<id>…`) أو يوجّه للمتجر |
| `og.png` | صورة المعاينة (1200×630) لواتساب وغيره، وتُشار إليها وسوم Open Graph برابط مطلق `https://manasik-app.github.io/og.png` |
| `style.css` + `fonts/` | الألوان وخط Cairo (مستضاف محليًا، بدون Google Fonts) |

## النشر على GitHub Pages

الموقع يُنشر كـ **User/Organization site**، لذلك اسم المستودع لازم يكون بالضبط **`manasik-app.github.io`** تحت المنظمة **`manasik-app`** (فيكون الرابط بلا مسار فرعي).

1. أنشئ منظمة GitHub باسم **`manasik-app`** (إن لم تكن موجودة)، ثم مستودعًا **عامًا** داخلها باسم **`manasik-app.github.io`** (بدون README أو .gitignore).
2. انسخ **محتويات** مجلد `site/` (وليس المجلد نفسه) إلى مجلد جديد خارج مشروع التطبيق، وانشرها:

   ```bash
   mkdir manasik-app.github.io && cp -r site/. manasik-app.github.io/ && cd manasik-app.github.io
   git init -b main
   git add -A && git commit -m "site: initial"
   git remote add origin https://github.com/manasik-app/manasik-app.github.io.git
   git push -u origin main
   ```

   (أو ارفع الملفات من واجهة GitHub: Add file ← Upload files. لا تنسخ `site/README.md` إن لم ترد ظهوره في المستودع؛ لا يضر وجوده.)
3. في المستودع: **Settings ← Pages ← Build and deployment**: Source = *Deploy from a branch*، Branch = `main` / `(root)` ← Save.
4. بعد دقيقة يظهر الموقع على `https://manasik-app.github.io/`.

## الاختبار والاستخدام

1. افتح `https://manasik-app.github.io/p.html?id=<uuid-برنامج>` من أندرويد (يفتح التطبيق إن كان مثبتًا، وإلا يذهب للمتجر)، وأرسل الرابط في واتساب لترى المعاينة.
2. روابط المشاركة داخل التطبيق تُبنى من `kShareBaseUrl` في `lib/core/config/app_config.dart` (`https://manasik-app.github.io`) على الشكل `…/p.html?id=<id>`.
3. استخدم `https://manasik-app.github.io/privacy.html` كرابط سياسة الخصوصية في Google Play Console.

## ملاحظات

- صفحة `p.html` لا تقرأ أي بيانات من السيرفر: المعاينة عامة (اسم مناسك + الوصف + الشعار) لأن زواحف واتساب لا تنفّذ JavaScript. المعاينة باسم/سعر/صورة كل برنامج تحتاج خادمًا (دالة `supabase/functions/share` مؤجّلة لحين وجود دومين مخصص).
- لتحديث نصوص الخصوصية/الشروط: عدّلها في `lib/core/constants/legal_texts.dart` ثم أعد توليد `privacy.html` و`terms.html` (أو عدّلهما يدويًا بنفس النص).
- مفتاح Supabase (publishable) لا يوجد في هذا الموقع إطلاقًا.
