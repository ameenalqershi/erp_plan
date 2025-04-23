# 🧠 الصورة الذهنية الكاملة (العميقة والمفصلة لأقصى مدى) لطبقة العميل (Client Layer) في مشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء طبقة العميل**

- [جميع روابط مخططات المشروع ERP-Pro](https://github.com/ameenalqershi/erp_plan)
  - ملفات هيكلية واجهات المستخدم (UI Modules, Pages, Components)
  - ملفات تصميم الـUX/UI (Figma, XD, ... أو مخططات صفحات)
  - ملفات الـAPI Contracts (Swagger, OpenAPI, Postman, ...)
  - ملفات الـDTOs/Requests/Responses
  - ملفات الـState Management, Routing, Authorization
  - ملفات الـLocalization, Theming, Permissions

---

## 1. **مبادئ حاكمة عظمى لطبقة العميل**

- العميل هو من يستهلك خدمات التطبيق عبر API أو SignalR أو WebSocket أو غيره.
- يجب أن يكون كل موديول أو صفحة أو مكون (Component) مستقل وقابل لإعادة الاستخدام.
- يجب أن تعكس كل صفحة/Component السيناريو الحقيقي حسب المخطط وتصميم UX/UI.
- يجب أن يكون كل اتصال مع الـAPI عبر Contracts واضحة (Request/Response/DTO).
- يجب ألا تتكرر أي منطق أعمال في العميل (فقط عرض، إرسال، استقبال).
- يجب أن تدار كل حالة (State) مركزيا (State Management).
- يجب دعم التوطين (Localization) والثيمات (Themes).
- يجب مراعاة الصلاحيات (Permissions) في كل صفحة أو عنصر.

---

## 2. **هيكلية المجلدات والملفات بالتفصيل**

### Client Root
- **Modules**
  - لكل موديول (مالية، عملاء، موردين، مخزون، أصول...):  
    - Pages (Add, Edit, List, Details, ... إلخ)
    - Components (Form, Table, Card, Modal, ... إلخ)
    - Services (API Service, State Service)
    - Models (DTOs/Request/Response)
    - Store (State Management)
    - Routing (Routes/Guards)
    - Localization (i18n, ar, en, ...)
- **Common**
  - Components (Button, Input, Modal, Loader, ...)
  - Layouts (Sidebar, Header, Footer, ...)
  - Utilities (Helpers, Formatters, Validators)
  - Themes/Styles
  - Permissions/Guards
- **API**
  - ApiService (Base), HttpClient, Interceptors
  - Contracts (OpenAPI/Swagger types)
- **State**
  - Global State (Auth, User, Notifications, Settings, ...)
- **Config**
  - Environment, AppSettings, ...  
- **Assets**
  - Images, Icons, Fonts
- **Tests**
  - Unit/Integration/E2E Tests

---

## 3. **دور كل ملف/مجلد وعلاقته ببقية الطبقات (تفصيل شديد)**

- **Modules:**  
  - كل صفحة أو مكون يستهلك API عبر Service خاص به (لا يوجد طلب مباشر في Components)
  - كل صفحة/Component يحصل على بياناته من Store/Service
  - كل عملية CRUD أو Action مرتبطة بUse Case في التطبيق
- **Common:**  
  - جميع المكونات المشتركة لتوحيد التصميم وتجربة المستخدم
- **API:**  
  - كل اتصال API عبر ApiService فقط، مع دعم Interceptors للأخطاء/التحقق/التوكن
- **State:**  
  - كل حالة مركزية (مثل المستخدم الحالي) تدار هنا
- **Localization:**  
  - كل نص أو رسالة أو عنوان يجب أن يكون قابل للتوطين
- **Permissions:**  
  - كل عنصر أو عملية يتحقق من صلاحية المستخدم قبل العرض أو التنفيذ

---

## 4. **خطوات التنفيذ خطوة بخطوة**

1. لكل موديول، أنشئ مجلد فيه Pages/Components/Services/Models/Store/Routing
2. لكل صفحة/Use Case، أنشئ Component خاص
3. أنشئ Service لكل موديول للتعامل مع الـAPI فقط
4. أنشئ Store لإدارة الحالة (CRUD, Actions, State)
5. اربط كل Component بالـStore وليس بالـAPI مباشرة
6. نفذ التوطين في كل نص
7. فعّل الصلاحيات على كل صفحة/عنصر
8. فعّل الثيمات والتصميم حسب ملف التصميم
9. اختبر كل Page/Component/Service منفصلا (Unit/E2E)
10. لا تكتب أي منطق أعمال هنا، فقط عرض وتحكم بالواجهة

---

## 5. **تعليمات صارمة لاستخدام الصورة الذهنية**

- لا تكتب أي اتصال API إلا عبر Service مخصص
- لا تربط أي Component بالـAPI مباشرة
- لا تكرر الكود، استخدم Common Components
- لا تترك نصوصًا ثابتة، استخدم Localization
- لا تعرض أي صفحة أو زر أو عملية بدون تحقق صلاحية المستخدم
- لا تكتب أي منطق أعمال، فقط عرض وتهيئة بيانات

---

## 6. **روابط المخططات الأساسية لهذا التحليل (للمراجعة السريعة):**

- [هيكلية العميل](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Client.md)
- [تصاميم UX/UI](https://github.com/ameenalqershi/erp_plan/tree/main/UX)
- [ملفات الـAPI Contracts](https://github.com/ameenalqershi/erp_plan/tree/main/API)
- [مخططات الموديولات](https://github.com/ameenalqershi/erp_plan/tree/main/Modules)

---

**هذه الصورة الذهنية مرجع نهائي لبناء طبقة العميل في مشروع ERP-Pro، ولا يجوز الخروج عنها إلا بتحديث رسمي من المستخدم أو تعديل المخططات.**