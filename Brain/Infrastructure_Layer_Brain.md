# 🧠 الصورة الذهنية الكاملة (العميقة والمفصلة لأقصى مدى) لطبقة البنية التحتية (Infrastructure Layer) في مشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء طبقة البنية التحتية**

- [جميع روابط مخططات المشروع ERP-Pro](https://github.com/ameenalqershi/erp_plan)
  - ملفات إعدادات البنية التحتية (ORM, Database, Messaging, External APIs, File Storage, ...)
  - ملفات هيكلية الـInfrastructure وتوزيع المجلدات والخدمات
  - ملفات الـDependency Injection وتهيئة الـServices
  - ملفات الأمن والربط بالأنظمة الخارجية
  - ملفات الـMigrations
  - ملفات التعامل مع الـDomain Events (Outbox/Inbox, EventBus)

---

## 1. **مبادئ حاكمة عظمى لطبقة البنية التحتية**

- البنية التحتية هي المسؤولة عن **توفير وتنفيذ كل ما هو خارجي عن منطق الأعمال**: قواعد البيانات، الربط الخارجي، الملفات، الرسائل، البريد، SMS، هوية المستخدم، الـCache، إلخ.
- كل كود هنا يجب أن ينفذ فقط الخدمات عبر الـAbstractions الموجودة في Application/Domain.
- يجب أن تتبع مبدأ Dependency Inversion:  
  - التطبيق والدومين لا يعرفان تفاصيل التنفيذ هنا، بل فقط الـInterfaces.
- يمنع منعا باتا وضع أي منطق أعمال هنا.
- يمنع منعا باتا استدعاء أي كود من الطبقات الأعلى (Application/Domain) إلا عبر الـInterfaces.
- يجب أن تكون كل خدمة قابلة للاختبار (Testable) وقابلة للاستبدال (Swappable).
- كل إعداد أو سر (Connection String, API Key, ...) يجب أن يدار من إعدادات مشفرة (Configuration/Secrets).

---

## 2. **هيكلية المجلدات والملفات بالتفصيل**

### Infrastructure Root
- **Persistence**
  - DbContext (EF Core أو ما يعادله)
  - Migrations
  - Repositories (تنفيذ IRepository لكل كيان)
  - Seeders (بيانات أولية)
  - TransactionManager/UnitOfWork
- **Identity**
  - UserStore, RoleStore, Claims, JWT, OAuth, SSO, PasswordHasher, ...  
    - كل ما يخص المصادقة والصلاحيات
- **Messaging**
  - EventBus, Outbox/Inbox, RabbitMQ/Kafka/ServiceBus
  - IntegrationEvents (تنفيذ إرسال/استقبال)
- **FileStorage**
  - LocalFileStorage, CloudStorage (S3, Azure, ...)
- **Notification**
  - EmailSender, SmsSender, PushNotification
- **Configuration**
  - AppSettings, Secrets, KeyVault, ...  
    - كل الإعدادات السرية
- **Integration**
  - ExternalApis (ERPLink, Tax, Payment, ...), Webhooks, Adapters
- **Cache**
  - Redis, MemoryCache, DistributedCache
- **DI (DependencyInjection)**
  - RegisterInfrastructureServices (جميع الخدمات والإعدادات)

---

## 3. **دور كل ملف/مجلد وعلاقته ببقية الطبقات (تفصيل شديد)**

- **Persistence:**  
  - ينفذ DbContext حسب الجداول من الدومين.
  - يدير الـMigrations بدقة.
  - ينفذ كل Repository Interface في Application/Domain.
  - لا يضيف أي كود أعمال، فقط CRUD/Queries.
  - يدير الـTransactions وUnitOfWork.
- **Identity:**  
  - ينفذ ICurrentUserService وواجهات المصادقة والتفويض.
  - يدير JWT/OAuth/SSO/API Keys حسب المخطط.
- **Messaging:**  
  - ينفذ EventBus/Outbox/Inbox لمعالجة الأحداث عبر الأنظمة.
  - يلتقط DomainEvents من الدومين ويرسلها للـBus.
- **FileStorage:**  
  - ينفذ IFileStorage لكل مصدر (محلي/سحابي).
- **Notification:**  
  - ينفذ IEmailSender، ISmsSender، إلخ.
- **Integration:**  
  - ينفذ الربط مع الأنظمة الخارجية عبر Abstractions فقط.
- **Configuration:**  
  - يدير كل الإعدادات والسرية.
- **Cache:**  
  - ينفذ ICacheService لأي نوع Cache.
- **DI:**  
  - يسجّل جميع الخدمات للـDI Container.

---

## 4. **خطوات التنفيذ خطوة بخطوة**

1. لكل خدمة أو تقنية خارجية، ابدأ بتعريف Interface/Abstraction في Application/Domain.
2. نفذ الـImplementation هنا في Infrastructure.
3. اربط كل Implementation بالـDI Container.
4. راعِ الفصل التام بين التنفيذ والواجهة.
5. لا تكتب أي منطق أعمال أو Validation هنا.
6. اختبر كل خدمة منفصلة (Unit Test/Integration Test).
7. أدخل كل إعداد في Configuration/Secrets.
8. حدث Migrations عند كل تعديل في الدومين.
9. لا تخرج عن أي قاعدة أو بنية دون تحديث الصورة الذهنية أولا.

---

## 5. **تعليمات صارمة لاستخدام الصورة الذهنية**

- لا تكتب أي كود تنفيذ إلا لما هو معرف كـInterface في الطبقات العليا.
- لا تكتب أي منطق أعمال أو Validation أو Mapping هنا.
- لا تضع أي بيانات سرية في الكود مباشرة.
- لا تكرر الكود أو تربط الخدمات ببعضها إلا عبر DI.
- اختبر كل خدمة على حدة.

---

## 6. **روابط المخططات الأساسية لهذا التحليل (للمراجعة السريعة):**

- [هيكلية البنية التحتية](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Infrastructure.md)
- [ملفات الإعداد والربط](https://github.com/ameenalqershi/erp_plan/tree/main/Infrastructure)
- [تحليل الجداول والمعطيات](https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_جداول_ERP_واقعي_موسع.md)
- [ملفات الـIntegration](https://github.com/ameenalqershi/erp_plan/tree/main/Integrations)

---

**هذه الصورة الذهنية مرجع نهائي لبناء طبقة البنية التحتية، ولا يجوز الخروج عنها إلا بتحديثها رسميا من المستخدم أو بتعديل المخططات.**