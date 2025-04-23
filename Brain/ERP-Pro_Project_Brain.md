# 🧠 الصورة الذهنية العظمى الكاملة (العميقة والمفصلة لأقصى مدى) لمشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء مشروع ERP-Pro**

- جميع المخططات، النماذج، وثائق التحليل، ملفات الـMD، والروابط الرسمية للمشروع على:  
  [erp_plan GitHub Repo](https://github.com/ameenalqershi/erp_plan)
  - تحليل الجداول والبيانات الواقعية (تحليل_جداول_ERP_واقعي_موسع.md)
  - تحليل هيكلية الأقسام والموديولات (تحليل_هيكلية_الأقسام_والموديولات_ERP_موسع_تفصيلي.md)
  - تدفقات العمليات وقواعد الأعمال (تدفقات_عمليات_ERP_كاملة_وأنواع_القيود.md)
  - ملفات الموديولات (Modules)
  - ملفات قواعد الأعمال (Business rules)
  - هيكلية وتقسيم المشروع البرمجي (Structure/ProjectStructure_CQRS_Detailed.md)
  - نماذج الـCQRS وملفات الـUseCases
  - ملفات الـAPI وواجهات الـClient
  - ملفات إعدادات البيئة، البنية التحتية، التوزيع، الأمان، الخ...

**ولا يجوز الخروج عن هذه المصادر أو تفسيرها إلا بتوجيه المستخدم أو تحديث رسمي للمخططات.**

---

## 1. **المبادئ العظمى الحاكمة للمشروع ككل**

- المشروع مبني على منهجية DDD (Design + Domain-Driven) وCQRS وClean Architecture.
- كل طبقة/موديول/ملف/كلاس في المشروع له مرجعية واضحة في المخططات.
- لا يُسمح بأي كود أو خاصية أو علاقة أو واجهة أو خدمة إلا إذا كان لها أصل واضح في التحليل أو المخطط أو بتوجيه المستخدم.
- كل طبقة مستقلة ولها مسؤوليات واضحة وتتكامل مع الطبقات الأخرى عبر Abstractions فقط.
- كل تغيير أو إضافة يجب أن ينعكس على كل الطبقات المرتبطة ويتم تحديث الصورة الذهنية فورًا.
- كل كود يجب أن يكون قابلًا للتخزين في SQL وقابلًا للاختبار والأتمتة والتوزيع.
- كل واجهة أو عملية أو خدمة أو حدث يجب أن يكون له اختبار واضح وموثق.

---

## 2. **تفصيل هيكلية الطبقات والمجلدات (صورة كلية عظمى)**

### 2.1 **Domain**
- منطق الأعمال الأساسي، الكيانات، القيم، الأحداث، التعدادات، القواعد.
- مستقل عن أي تقنية أو بنية تحتية.
- كل موديول (مالية، عملاء، موارد بشرية...) له مجلده وكياناته وقيمه وأحداثه الخاصة.
- Common/SharedKernel لقيم وأنواع مشتركة.

### 2.2 **Application**
- طبقة تنسيق الأعمال (CQRS: Commands, Queries, Handlers, DTOs, Services, Validation, Authorization).
- مسؤولة عن تنفيذ تدفقات العمليات واستهلاك منطق الدومين عبر Abstractions فقط.
- لا تحتوي منطق أعمال عميق، بل فقط orchestration.

### 2.3 **Infrastructure**
- تنفيذ كل ما هو خارجي (Persistence, Messaging, FileStorage, Integration, Identity, ...).
- تنفيذ كل Abstraction يحتاجه Application/Domain.
- إدارة الإعدادات والأمان والتكامل مع الأنظمة الخارجية.

### 2.4 **Client**
- واجهات المستخدم (Web, Desktop, Mobile, ...).
- Modules/Pages/Components/Services/State/Store/Localization.
- استهلاك الـAPI فقط عبر DTOs/Contracts/OperationResult.
- لا منطق أعمال، عرض وتحكم فقط.

### 2.5 **Shared/Infrastructure.Shared**
- كل ما هو مشترك بين أكثر من طبقة (DTOs, Constants, Enums, Localization, Helpers, Extensions).
- لا منطق أعمال أو كود موديولي.

### 2.6 **Client.Infrastructure**
- خدمات البنية التحتية الخاصة بالعميل (API, Auth, Storage, Cache, Config).
- تنفيذ كل Abstraction يحتاجه الـClient.

### 2.7 **Server**
- الطبقة المستضيفة (API Controllers, Middlewares, Routing, Security, Config).
- استدعاء Application Layer فقط، لا منطق أعمال أو دومين.

---

## 3. **العلاقات والترابط بين الطبقات (صورة ذهنية ترابطية)**

- **Domain** لا يعرف شيئًا عن Application/Infrastructure/Client/Server.
- **Application** يعرف Domain فقط عبر Aggregates/Repositories/DTOs/Events، لا يعرف Infrastructure أو Client أو Server مباشرة.
- **Infrastructure** ينفذ الـAbstractions من Application/Domain فقط، لا يعرف Client أو Server.
- **Client** يستهلك API من Server فقط، لا يعرف Domain/Application/Infrastructure مباشرة.
- **Client.Infrastructure** ينفذ Abstractions للـClient فقط.
- **Shared/Infrastructure.Shared** متاحة للاستهلاك من أي طبقة حسب الحاجة.
- **Server** يستدعي Application Layer، ويعرض بيانات للـClient.

---

## 4. **قواعد التنفيذ والتكامل الشامل**

- لكل موديول (مالية، عملاء، ...):  
  1. يبدأ التحليل من المخطط (جداول، عمليات، علاقات، قواعد أعمال)
  2. يبنى الكود دومينيًا أولًا (كيانات، قيم، أحداث، واجهات)
  3. تبنى خدمات التطبيق (UseCases, Commands, Queries, Handlers, Services)
  4. تبنى Repositories/Interfaces في Application/Domain
  5. تنفذ في Infrastructure (Persistence, Messaging, Integration)
  6. تبنى واجهات العميل لكل UseCase (Pages, Components, Services)
  7. تبنى اختبارات لكل طبقة (Unit, Integration, E2E)
  8. توثق كل خطوة وتربط باختبارها
  9. كل تغيير في المخطط ينعكس تلقائيًا في كل الطبقات المرتبطة

---

## 5. **مبادئ صارمة لا يجوز الخروج عنها**

- لا تكتب أي كود أو خاصية أو علاقة أو خدمة أو عملية إلا إذا كان لها أصل في المخطط أو ملف التحليل أو بتوجيه المستخدم.
- كل طبقة تلتزم بمسؤولياتها فقط.
- لا تضع أي منطق أعمال في غير الدومين.
- لا تكرر الكود أو تضعه في أكثر من طبقة.
- لا تكتب أي استثناء أو خاصية أو نوع أو واجهة أو خدمة إلا إذا كان لها استخدام حقيقي وموثق.
- كل كود يجب أن يكون قابلًا للاختبار والتوسعة والصيانة.

---

## 6. **خطوات بناء المشروع الكامل (تفصيل لا يقبل الثغرات)**

1. تحليل المخططات (جداول، عمليات، علاقات، قواعد أعمال)
2. توليد ملفات Domain (كيانات، قيم، أحداث، واجهات)
3. بناء Application Layer (CQRS, Services, DTOs, Validation, Behaviors)
4. تنفيذ Abstractions في Infrastructure (Persistence, Messaging, Integration, Identity, FileStorage)
5. بناء Shared/Infrastructure.Shared (DTOs, Enums, Constants, Helpers, Localization)
6. بناء Client Layer (Pages, Components, Services, State, Routing, Localization)
7. بناء Client.Infrastructure (API, Auth, Storage, Cache, Config)
8. بناء Server Layer (Controllers, Middlewares, Routing, Security, Config)
9. بناء اختبارات لكل طبقة (Unit, Integration, E2E)
10. توثيق كل طبقة وملف وواجهة واختبار
11. مراجعة الترابط الكامل بين الطبقات
12. تحديث الصورة الذهنية فور كل تعديل أو توسعة

---

## 7. **تعليمات شاملة لاستخدام الصورة الذهنية العظمى**

- كل صورة ذهنية فرعية (Domain/Application/Infrastructure/Client/...) هي ملحق تفصيلي لهذه الصورة العظمى.
- لا تكتب أي كود أو تضيف أو تحذف أو تعدل في المشروع إلا بعد الرجوع للصورة الذهنية العظمى والفرعية.
- إذا تعارض أي كود مع هذه الصورة، يجب تحديث الصورة الذهنية أولًا.
- كل تحديث في المخطط يجب أن ينعكس في الصورة الذهنية العظمى.
- كل عملية تطوير أو إضافة أو توسعة يجب أن تراعي أثرها على كل الطبقات والموديولات.

---

## 8. **روابط المخططات الأساسية للتحليل (للمراجعة السريعة):**

- [تحليل الجداول الموسع](https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_جداول_ERP_واقعي_موسع.md)
- [تحليل الأقسام والموديولات](https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_هيكلية_الأقسام_والموديولات_ERP_موسع_تفصيلي.md)
- [تدفقات العمليات وقواعد الأعمال](https://github.com/ameenalqershi/erp_plan/blob/main/تدفقات_عمليات_ERP_كاملة_وأنواع_القيود.md)
- [هيكلية المشروع](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/ProjectStructure_CQRS_Detailed.md)
- [ملفات الموديولات](https://github.com/ameenalqershi/erp_plan/tree/main/Modules)
- [كل ملفات الـDTOs/API/UseCases](https://github.com/ameenalqershi/erp_plan/tree/main/API)
- [ملفات Shared/Infrastructure.Shared](https://github.com/ameenalqershi/erp_plan/tree/main/Shared)

---

**هذه الصورة الذهنية العظمى هي المرجع النهائي والوحيد لبناء مشروع ERP-Pro  
ولا يجوز الخروج عنها أو تعديلها إلا من خلال المستخدم أو تحديث المخططات الرسمية، وأي تطبيق أو كود أو إضافة أو اختبار يجب أن يلتزم بها التزامًا كاملًا.**