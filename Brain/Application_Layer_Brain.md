# 🧠 الصورة الذهنية الكاملة (العميقة والمفصلة لأقصى مدى) لطبقة التطبيق (Application Layer) في مشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء طبقة التطبيق**

- [جميع روابط مخططات المشروع ERP-Pro](https://github.com/ameenalqershi/erp_plan)
  - ملفات تحليل العمليات وتدفقات الأعمال (Business Processes)
  - ملفات الـCQRS وشرح الـUse Cases
  - ملفات الـDTOs وطلبات الأوامر (Commands) والاستعلامات (Queries)
  - ملفات الـApplication Services، الـMediatR، الـWorkflows، الـPolicies، إلخ
  - ملفات الـValidation ونتائج العمليات (OperationResult/Result Pattern)
  - هيكلية المشروع البرمجية:  
    https://github.com/ameenalqershi/erp_plan/blob/main/Structure/ProjectStructure_CQRS_Detailed.md  
    https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Application.md

---

## 1. **مبادئ حاكمة عظمى لطبقة التطبيق**

- طبقة التطبيق هي المسؤولة عن **تنسيق تدفق الأعمال** بين الدومين والبنية التحتية وتقديم واجهة منطقية للاستخدام (API, UI, ...).
- لا تحتوي أي منطق دوميني عميق، بل تستدعي منطق الدومين عبر Aggregates/Domain Services/Repositories.
- تطبق نمط CQRS بدقة:  
  - أوامر (Commands) للتغييرات  
  - استعلامات (Queries) للقراءة  
  - كل منهما مع Handlers مستقلين  
- كل عملية أو سيناريو أعمال (Use Case) يجب أن يعرف كـCommand/Query + Handler.
- كل Command أو Query له DTO مخصص (Request/Response).
- كل عملية يجب أن تقبل وتعيد Result/OperationResult يعطي حالة التنفيذ والأخطاء.
- كل عملية تحقق Validation وAuthorization قبل التنفيذ.
- لا يجوز استدعاء أي خدمة من البنية التحتية مباشرة، بل عبر Abstractions/Interfaces فقط.
- لا يجوز الوصول إلى الدومين إلا عبر Repositories/Services.
- كل حدث دوميني (Domain Event) يجب التقاطه وتمريره للـEvent Handlers/Outbox.

---

## 2. **هيكلية المجلدات والملفات بالتفصيل**

### 2.1 **Application Root**
- **Abstractions**
  - IUnitOfWork
  - IRepository<T, TId>
  - ICurrentUserService
  - IDateTimeProvider
  - IEmailSender, ISmsSender, IFileStorage
  - IPermissionService, IAppPolicy
  - كل Interface يحتاجه التطبيق للتواصل مع باقي الطبقات
- **Common/Models**
  - Result, OperationResult, Error
  - Pagination, Sorting, Filtering models
- **Validation**
  - كل ملفات Validators (FluentValidation أو غيره)  
    - لكل Command/Query Validator خاص به  
    - CommonValidators إن احتجت
- **Behaviors**
  - Pipeline behaviors (Logging, Validation, Authorization, Performance, Transaction, ...)

### 2.2 **CQRS**
- **Commands**
  - لكل Use Case أمر خاص (AddCustomerCommand, UpdateCustomerCommand, PostJournalEntryCommand, ...)
  - كل Command يحتوي فقط بيانات التنفيذ (DTO)
  - كل Command له Handler مستقل (ينفذ المنطق ويحدث الدومين)
  - كل Command له Validator خاص
- **Queries**
  - لكل استعلام Query خاص (GetCustomerByIdQuery, ListAccountsQuery, ...)
  - كل Query له Handler مستقل (ينفذ القراءة فقط)
  - كل Query له Validator خاص إذا لزم الأمر
- **Events**
  - Application Events (عند الحاجة فقط، مثلاً لمعالجة Outbox أو إشعارات)

### 2.3 **Services**
- **Application Services**
  - لكل موديول Service مستقل (ICustomerAppService, IAccountAppService, ...)
  - كل Service ينسق الاستدعاءات بين Handlers/Repositories/Services/Events
- **Mapping**
  - ملفات Mapping بين الدومين وDTOs (AutoMapper Profiles أو ما شابه)

### 2.4 **DTOs**
- Request/Response لكل Use Case أو استعلام.
- كل DTO منفصل عن كائنات الدومين.

### 2.5 **Workflows/Policies**
- إذا وجدت عمليات متسلسلة معقدة أو سياسات أعمال تخص التطبيق، تكتب هنا (Workflows, Approval Policies, ...)

### 2.6 **Exceptions**
- ApplicationException, ValidationException, ForbiddenException, NotFoundException, ...  
  (للاستخدام في التطبيق دون تجاوز للدومين)

---

## 3. **دور كل ملف/مجلد وعلاقته ببقية الطبقات (تفصيل شديد)**

- **Commands/Handlers:**  
  - يقبل بيانات من الـAPI/Client
  - يتحقق من الصلاحيات
  - يمرر للـValidator
  - ينفذ عبر Aggregate/DomainService من الدومين
  - يحفظ التغييرات عبر الـUnitOfWork/Repositories
  - يطلق Domain Events إذا لزم
  - يعيد Result/OperationResult/DTO
- **Queries/Handlers:**  
  - يقبل بيانات الاستعلام
  - ينفذ عبر Query Services أو DbContext/ReadModel (لا يعدل الدومين!)
  - يعيد بيانات DTO/PagedList/Result فقط
- **Services:**  
  - تنسق عمليات معقدة تتطلب أكثر من خطوة أو أكثر من Handler
  - لا تحتوي منطق أعمال، بل تنسق بين Handlers/Validators/Repositories
- **Mapping:**  
  - تفصل دومين عن DTOs بدقة
  - تمنع التسريب العرضي لأي خاصية دومينية غير مصرح بها
- **Validation:**  
  - كل عملية لها Validator خاص بها
  - التحقق يتم قبل Handler
  - كل خطأ Validation يعاد في صورة مفهومة للـClient
- **Behaviors:**  
  - Logging, Exception Handling, Performance, Authorization, Transaction
  - تطبق عبر MediatR Pipeline أو ما شابه
- **Exceptions:**  
  - تستخدم لإخراج الأخطاء الخاصة بالتطبيق دون كشف تفاصيل الدومين أو البنية التحتية

---

## 4. **التكامل مع باقي الطبقات (ترابط لا يقبل الثغرات)**

- **مع الدومين:**  
  - فقط عبر Aggregates/Repositories/DomainServices/ValueObjects/Enumerations
  - لا يجوز تعديل الدومين إلا عبر Command Handler مع UnitOfWork
  - التقاط DomainEvents وتمريرها للـHandlers المناسبين
- **مع البنية التحتية:**  
  - فقط عبر Abstractions/Interfaces (لا يوجد استخدام مباشر لأي ORM أو API خارجي)
  - كل تنفيذ للبنية التحتية يتم حقنه (Dependency Injection)
- **مع الـClient/API:**  
  - فقط عبر DTOs وOperationResult
  - لا يجوز كشف أي كيان دوميني للعميل مباشرة

---

## 5. **قواعد التنفيذ خطوة بخطوة**

1. لكل Use Case أو عملية، ابدأ بتعريف Command/Query + DTO (Request/Response)
2. اكتب Validator خاص للعملية (FluentValidation أو ما شابه)
3. اكتب Handler للعملية (CommandHandler/QueryHandler)
4. Handler يستدعي الدومين عبر الـRepositories/DomainServices
5. Handler يتعامل مع UnitOfWork لحفظ التغييرات
6. أضف Mapping بين الدومين والـDTO للمدخلات والمخرجات
7. أضف Behaviors/Interceptors للـLogging/Validation/Authorization/Transaction
8. جميع الأخطاء تخرج في صورة OperationResult واضحة
9. كل DomainEvent يلتقط ويرسل للـEventHandlers
10. لا تخرج عن هذه القواعد لأي سبب إلا بتحديث الصورة الذهنية

---

## 6. **تعليمات صارمة لاستخدام الصورة الذهنية**

- لا تكتب أي أمر أو استعلام أو Service أو Mapping أو Validator إلا إذا كان مذكورًا في مخططات العمليات أو Use Cases أو طلب المستخدم
- لا تكتب أي منطق أعمال في التطبيق، بل في الدومين فقط
- لا تكتب أي كود بنية تحتية هنا، فقط استدعاء عبر Abstractions
- لا تكرر الكود أو تخلط بين Command وQuery
- لا تخلط بين DTO والدومين

---

## 7. **روابط المخططات الأساسية لهذا التحليل (للمراجعة السريعة):**

- [تحليل العمليات والـUseCases](https://github.com/ameenalqershi/erp_plan/tree/main/Business%20rules)
- [هيكلية التطبيق](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Application.md)
- [تحليل الجداول والمعطيات](https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_جداول_ERP_واقعي_موسع.md)
- [ملفات الـCQRS](https://github.com/ameenalqershi/erp_plan/tree/main/CQRS)
- [هيكلية المشروع](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/ProjectStructure_CQRS_Detailed.md)

---

**هذه الصورة الذهنية مرجع نهائي لبناء طبقة التطبيق في مشروع ERP-Pro، ولا يجوز الخروج عنها إلا بتحديثها صراحة من المستخدم أو تعديل المخططات الرسمية.**