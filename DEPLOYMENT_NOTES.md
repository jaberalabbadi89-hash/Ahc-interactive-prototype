# دليل التسليم والتشغيل التقني (Deployment & Handover Notes)

يحتوي هذا المستند على التعليمات التقنية اللازمة لدمج ونشر النموذج التجريبي لمشروع **Asociación Huella de Carbono** في بيئة الإنتاج الحية الخاصة بـ (WordPress / Divi) و (Moodle / Edwiser).

---

## 1. مواقع ومسارات حقن الأكواد (Code Injection Mapping)

### أ. ملف التنسيقات الرئيسي `custom-style.css`
* **الموقع الأول (WordPress / Divi):**
  * **المسار:** `Divi` ➔ `Theme Options` ➔ `General` ➔ `Custom CSS` (في أسفل الصفحة).
  * **أو بدلاً من ذلك:** قم بلصقه داخل ملف `style.css` الخاص بالقالب الفرعي (Child Theme) النشط.
* **الموقع الثاني (Moodle / Edwiser):**
  * **المسار:** لوحة تحكم Edwiser RemUI ➔ إعدادات القالب ➔ تبويب التنسيقات المخصصة (Custom CSS).
  * **الغرض:** تنظيف لوحة معلومات الطالب من الكتل غير الهامة وتوحيد شكل البطاقات مع الهوية البصرية.

### ب. ملف التفاعلات والمنطق `script.js`
* **المسار (WordPress / Divi):**
  * **الموقع:** `Divi` ➔ `Theme Options` ➔ `Integration` ➔ `Add code to the <body_of_page>` (داخل وسم `<script>`).
  * **تنبيه هام:** يجب تغليف الأكواد بحدث `DOMContentLoaded` لمنع أي تعارض مع الـ DOM أو تأخير في التحميل.
  * **المسار البديل:** إضافته كملف مستقر عبر ملف `functions.php` لقالب الابن باستخدام دالة `wp_enqueue_script()`.

### ج. أصول الوسائط وصورة المنزل `house_layout.png`
* **المسار:** يجب رفع صورة المنزل [house_layout.png](file:///Users/Alumne_tarda1/Desktop/h5p/house_layout.png) إلى مكتبة وسائط ووردبريس (WordPress Media Library)، ثم نسخ الرابط المباشر وتحديث حقل `src` الخاص بالصورة داخل دالة حقن النشاط الرابع (OA-04) في كود الـ JavaScript لتفادي روابط الـ Broken Images.

### د. تخصيص مظهر H5P العام `h5p-overrides.css`
* **المسار:** يتم رفع هذا الملف إما عبر إضافات WordPress التي تسمح بحقن CSS لـ H5P (مثل H5P CSS Editor)، أو عبر إضافته إلى مجلد القالب واستدعائه برمجياً لتعديل أنشطة H5P المضمنة في Moodle أو WordPress مباشرة.

---

## 2. دليل الصيانة والتعديل على الهوية البصرية

### أ. تعديل الألوان الرسمية للجمعية
تعتمد جميع الملفات على متغيرات CSS مخصصة (Custom Properties) لتسهيل تعديل الألوان بضغطة زر واحدة. لتغيير اللون الأخضر الرئيسي للجمعية، عدّل فقط القيم التالية في بداية ملف CSS:
```css
:root {
    --ahc-color-primary-green: #62a144;  /* اللون الأخضر الرئيسي */
    --ahc-color-green-hover: #4d8235;    /* تأثير التمرير (Hover) */
}
```

### ب. تعديل الخطوط
لتعديل الخط العام للمنصة دون تدمير التجاوبية، قم بتغيير المتغيرات المستهدفة أو إعلان الخط الجديد في `body` أو قواعد H5P المستهدفة:
```css
font-family: 'Outfit', 'Inter', 'Cairo', sans-serif !important;
```

---

## 3. ⚠️ تحذير تقني هام (Developer Warning)

> [!WARNING]
> **منع حذف كود الـ Intersection Observer:**
> يحتوي ملف [custom-interactions.js](file:///Users/Alumne_tarda1/Desktop/h5p/custom-interactions.js) على كود يراقب تمرير الصفحة ليقوم بإظهار بطاقات Bento بشكل تدريجي ناعم (`.ahc-reveal--visible`). 
> حذف هذا الكود أو تعديله بشكل خاطئ سيجعل جميع بطاقات Bento في الصفحة الرئيسية مخفية تماماً (`opacity: 0`) لأنها لن تتلقى الكلاس النشط، مما يؤدي إلى ظهور صفحة بيضاء فارغة أمام الطلاب.
