# 🧠 الصورة الذهنية الكاملة (العميقة والمفصلة لأقصى مدى) لطبقة الدومين في مشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء طبقة الدومين وجميع موديلاتها**

جميع ما سيكتب هنا مستمد حصراً من:
- [روابط المخططات الأصلية للمشروع ERP-Pro](https://github.com/ameenalqershi/erp_plan)  
  وبالذات:
  - تحليل الجداول الواقعي الموسع:  
    https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_جداول_ERP_واقعي_موسع.md  
    https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_جداول_ERP_واقعي_موسع(1).md
  - تحليل هيكلية الأقسام والموديولات:  
    https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_هيكلية_الأقسام_والموديولات_ERP_موسع_تفصيلي.md
  - تدفقات عمليات ERP وأنواع القيود:  
    https://github.com/ameenalqershi/erp_plan/blob/main/تدفقات_عمليات_ERP_كاملة_وأنواع_القيود.md
  - جميع ملفات الـModules (مالية، عملاء، موردين، مخزون، أصول، ...):  
    https://github.com/ameenalqershi/erp_plan/tree/main/Modules
  - جميع ملفات قواعد الأعمال (Business Rules):  
    https://github.com/ameenalqershi/erp_plan/tree/main/Business%20rules
  - هيكلية وتقسيم المشروع البرمجي:  
    https://github.com/ameenalqershi/erp_plan/blob/main/Structure/ProjectStructure_CQRS_Detailed.md  
    https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Domain.md
  - وغيرها من المستندات ذات الصلة.

**ولا يجوز الخروج عن هذا المصدر إلا بتعديل صريح من المستخدم.**

---

## 1. **مبادئ حاكمة عظمى لا يجوز الإخلال بها**

- كل ما يُكتب هنا هو انعكاس مباشر ودقيق لكل جداول وقواعد وعلاقات المخططات، ولا يُسمح بإضافة أو حذف أي كيان أو خاصية أو علاقة إلا في حالة وجودها في المخطط أو بطلب المستخدم.
- يجب أن تكون كل الكيانات والقيم الدومينية قابلة للتخزين في SQL (أي كل خاصية تعرف نوعها وحدودها وقيمها الافتراضية).
- يجب تطبيق DDD بحذافيره:  
  - Aggregates/Entities/ValueObjects/Enumerations/DomainEvents/Repositories Interfaces  
  - كل موديول مستقل دومينيًا (Bounded Context) لكن يسمح بالربط وفق المخططات.
- أي ارتباط مع طبقة أخرى يكون عبر abstractions/events فقط.
- لا يجوز كتابة أي سطر كود يخص الـInfrastructure أو الـApplication أو الـPresentation في الدومين.
- مراعاة الـAuditing في الكيانات التي تتطلب تدقيقاً.
- مراعاة الـStronglyTypedId لكل كيان رئيسي (CustomerId, AccountId, ...).
- مراعاة كل علاقة (1:1, 1:n, n:n) بنفس تفاصيلها من المخطط (بما في ذلك القيود، القيم الافتراضية، الـCascade، الخ...).

---

## 2. **تفصيل هيكلية المجلدات والملفات (مستوى أدنى/أقصى تفصيل)**

### 2.1 **Common**
- **Primitives**
  - Entity<TId>: أصل كل الكيانات الدومينية
  - ValueObject: أصل كل القيم الدومينية (غير قابلة للتغيير)
  - IAggregateRoot: علامة لجذر التجميع
  - StronglyTypedId<T>: أصل كل المعرّفات القوية
  - AuditableEntity<TId>: أصل الكيانات التي تحتاج تدقيقاً
  - Enumeration: قاعدة للتعدادات الغنية
- **Events**
  - IDomainEvent: واجهة أساسية
  - DomainEvent: كلاس أساسي للأحداث
  - EventDispatcher: واجهة فقط (التنفيذ في الـInfrastructure)
- **ValueObjects**
  - Email, PhoneNumber, Money, Address  
    (ويضاف عليها حسب الحاجة من المخططات، مثل: TaxNumber, CommercialRegister, PeriodRange, ...إلخ)
- **Exceptions**
  - DomainException, NotFoundException, BusinessRuleValidationException  
    (يجب تعريفها إذا لزم الأمر في القواعد أو المخططات)
- **Abstractions**
  - IRepository<T, TId>, IHasDomainEvents, IEntityWithAudit  
    (تضاف إذا احتاجها أي موديول)
- **SharedKernel**
  - Currency, Country, UnitOfMeasure, TaxType, PeriodType, PaymentMethod, AccountType, ...  
    (كل Enumeration أو قيمة تتكرر في أكثر من موديول)

### 2.2 **ERP**
- **ERP/Customers**
  - Customer.cs (AggregateRoot)
  - CustomerId.cs (StronglyTypedId)
  - CustomerType.cs (Enumeration)
  - CustomerAddress.cs (ValueObject)
  - CustomerContactPerson.cs (ValueObject إذا كانت العلاقة 1:n)
  - CustomerRegisteredEvent.cs (DomainEvent)
  - ICustomerRepository.cs (Interface)
  - [أي كيانات أو قيم أو أحداث أخرى حسب المخططات]
- **ERP/Suppliers**
  - Supplier.cs, SupplierId.cs, SupplierType.cs, ... [نفس الأسلوب والتفصيل]
- **ERP/Financial**
  - Account.cs, AccountId.cs, AccountType.cs, JournalEntry.cs, JournalEntryId.cs, EntryLine.cs, ...  
    - كل جدول في المخطط له كيان إما Entity أو AggregateRoot أو ValueObject أو Enumeration أو DomainEvent حسب الحاجة.
    - يجب مراعاة كل علاقة وكل خاصية بالضبط كما في المخطط.
- **ERP/Inventory**
  - Item.cs, ItemId.cs, ItemType.cs, ItemUnit.cs, StockTransaction.cs, Warehouse.cs, ...  
    - مع مراعاة كل حقول المخطط، العلاقات، القيود، القيم الافتراضية.
- **ERP/FixedAssets**
  - Asset.cs, AssetId.cs, AssetGroup.cs, AssetDepreciation.cs, AssetTransfer.cs, ...  
    - مع تطبيق قواعد الأعمال الخاصة بالإهلاك، النقل، ...الخ.
- **ERP/HRPayroll**
  - Employee.cs, EmployeeId.cs, PayrollPeriod.cs, PayrollEntry.cs, ...  
    - مع دعم العلاقات مع المالية، المشاريع، ...الخ.
- **ERP/ProjectsContracts**
  - Project.cs, Contract.cs, ProjectTask.cs, ...  
    - مع دعم العلاقات مع العملاء، المالية، الموارد البشرية، ...الخ.
- **ERP/Manufacturing**
  - ProductionOrder.cs, Bom.cs, Routing.cs, ...  
    - مع تطبيق تدفقات العمليات الخاصة بالإنتاج.
- **ERP/TaxGov**
  - TaxDeclaration.cs, TaxType.cs, ...  
    - مع مراعاة كل قواعد الأعمال الحكومية.
- **ERP/SettingsIntegration**
  - IntegrationConfig.cs, ExternalSystem.cs, ...  
    - كل ما هو خاص بالتكامل والإعدادات المركزية.

---

## 3. **التحليل التفصيلي (لكل نوع ملف/كائن/علاقة) مع أمثلة فعلية**

### 3.1 **Entity/AggregateRoot**
- تعريف كل كيان رئيسي في أي جدول رئيسي (باستثناء جداول Lookup أو Many-to-Many الوسطية).
- يجب أن يُعرّف كل خاصية بدقة (نوعها، هل هي مطلوبة، الطول، القيم الافتراضية، ...).
- يدعم العلاقات (Navigation Properties) عبر Id فقط، ولا يحمّل الكيانات المرتبطة إلا إذا كانت جزء من Aggregate.
- يطبق قواعد الأعمال الطولية، القيم الفريدة، الـAuditing، ...الخ.
- مثال:  
  - `Customer`  
    - Id (CustomerId StronglyTypedId), Name, Type (Enumeration), Contacts (List<CustomerContactPerson>), Address (CustomerAddress ValueObject), CreatedAt, UpdatedAt, ...
  - `Account`
    - Id (AccountId StronglyTypedId), Code, Name, Type (Enumeration), ParentId, Status, ...الخ

### 3.2 **ValueObject**
- يمثل كل قيمة مركبة لا تحتاج معرف خاص (مثل Email, Address, TaxNumber, PeriodRange, ...).
- Immutable بالكامل.
- يجب أن يحدد كل خاصية بدقة (نوعها، حدودها، التحقق منها، ...).
- يُستخدم في الكيانات في صورة خاصية أو مجموعة.
- مثال:  
  - CustomerAddress: Country, City, Area, Street, ZipCode
  - Money: Amount, Currency

### 3.3 **Enumeration**
- لكل قائمة ثابتة أو كود Lookup صغير (مثل CustomerType, AccountType, EntryStatus, ...).
- يجب أن تكون غنية (Rich Enumeration) وتدعم الامتداد مستقبلاً.
- توضع في SharedKernel لو تكررت في أكثر من موديول.

### 3.4 **DomainEvent**
- لكل حدث دوميني رئيسي في تدفقات العمليات (تسجيل عميل، ترحيل قيد، إصدار فاتورة...).
- يرث من DomainEvent الأساسي.
- يستخدم لنقل الأحداث للطبقات الأخرى (Application/Integration).

### 3.5 **StronglyTypedId**
- لكل كيان رئيسي معرف خاص StronglyTypedId (CustomerId, AccountId, ...).
- يدعم التحقق والمقارنة والطباعة.

### 3.6 **AuditableEntity**
- لكل كيان يحتاج متابعة من أنشأه ومتى عُدّل.
- خصائص: CreatedAt, CreatedBy, UpdatedAt, UpdatedBy (أنواع دقيقة حسب المخططات).

### 3.7 **Repository Interfaces**
- لكل AggregateRoot واجهة Repository خاصة به فقط داخل موديوله.
- لا تكتب أي منطق داخل الواجهة، فقط التواقيع الأساسية (Add, Update, Remove, FindById, Find, Exists, ...).

### 3.8 **SharedKernel**
- لكل قيمة أو نوع يتكرر عبر أكثر من موديول (Currency, Country, PeriodType, ...).
- يجب أن يكون مفصولاً عن موديولات الـERP.

### 3.9 **Exceptions**
- إذا حددت المخططات أو قواعد الأعمال استثناءات خاصة، يجب تعريفها (DomainException, BusinessRuleValidationException, ...).
- لا يُستخدم أي Exception من البنية التحتية مباشرة.

---

## 4. **تفصيل العلاقات بين الكيانات (مع تطبيق المخططات حرفيًا)**

- كل علاقة (1:1, 1:n, n:n) يجب أن تطبق كما في المخطط:
  - 1:1 = خاصية واحدة من نوع Id أو ValueObject.
  - 1:n = خاصية Collection (List<T> أو HashSet<T>).
  - n:n = كيان وسيط (مثال: ProjectEmployee أو ItemWarehouse).
- لا يجوز تضمين كيان كامل من موديول آخر، فقط Id أو ValueObject.
- كل علاقة معرفة في المخطط يجب أن توضح في الكلاس مع تعليق يشرح مصدرها من المخطط.
- يجب مراعاة الـCascade/Restrict/Delete حسب ما هو محدد في المخطط أو الواقع.

---

## 5. **تفصيل قواعد الأعمال في الدومين**

- لكل قاعدة أعمال (Business Rule) في المخططات يجب كتابة كودها كـ:
  - دوال تحقق داخل AggregateRoot (IsValid, CanPost, CanDelete, ...).
  - استثناءات خاصة عند مخالفة القواعد.
  - تعليقات توضح مرجع القاعدة من ملف قواعد الأعمال (مثال: // قاعدة: لا يمكن حذف حساب مرتبط بحركات - راجع business_rules_financial.md).
- لا يجوز تطبيق قاعدة أعمال غير موجودة في المخططات إلا بتوجيه المستخدم.

---

## 6. **خطوات بناء طبقة الدومين (بتسلسل التنفيذ الدقيق)**

1. كتابة Primitives في Common (لن تُكرر في أي موديول).
2. كتابة Events الأساسية (IDomainEvent, DomainEvent).
3. كتابة ValueObjects المشتركة (مع التحقق من الحاجة لكل واحدة من المخططات).
4. كتابة Enumerations/SharedKernel لكل قيمة متكررة.
5. لكل موديول:
   - دراسة جميع الجداول فيه (من المخطط).
   - استخراج كل كيان رئيسي:  
     - تعريفه (Entity/AggregateRoot) بالخصائص والعلاقات.
     - StronglyTypedId لكل كيان.
     - ValueObjects حسب الحاجة (عنوان، مبلغ، رقم ضريبة...).
     - Enumerations حسب المخطط.
     - الأحداث الدومينية لكل عملية رئيسية.
   - كتابة كل Repository Interface.
   - كتابة اختبارات تحقق للكيانات والقيم (في مشروع منفصل للاختبارات).
6. مراجعة العلاقات بين الكيانات والموديولات وربطها عبر Id فقط أو SharedKernel.
7. التأكد من عدم وجود أي منطق غير دوميني أو تكرار أو نقص أو زيادة.
8. مراجعة الصورة الذهنية وتحديثها عند كل إضافة أو تعديل.

---

## 7. **التكامل والترابط مع الطبقات الأخرى**

- لا توجد أي إشارة مباشرة لأي طبقة أخرى في الدومين.
- كل ما يخص الـPersistence (ORM, SQL) يتم فقط عبر Abstractions (Repositories).
- كل الأحداث (DomainEvents) يتم معالجتها خارج الدومين (Application/Integration).
- إذا احتاج كيان دوميني بيانات من طبقة أخرى، يتم تمريرها عبر Constructor أو Method Parameter فقط.

---

## 8. **تعليمات لاستخدام هذه الصورة الذهنية**

- أي تغيير أو إضافة في الكود يجب أن يكون مطابقًا لما في هذا الملف حرفيًا.
- إذا وجدت خاصية أو علاقة أو كيان غير مذكور هنا، لا تضفه إلا بعد تحديث الصورة الذهنية.
- إذا احتجت تحديثًا للصورة، أعد التحليل من جديد وحدث هذا الملف أولًا.
- عند أي توليد كود جديد، راجع هذا الملف أولًا وطبقه خطوة بخطوة.

---

## 9. **روابط المخططات الأساسية لهذا التحليل (للمراجعة السريعة):**

- [تحليل الجداول الموسع](https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_جداول_ERP_واقعي_موسع.md)
- [تحليل الموديولات والأقسام](https://github.com/ameenalqershi/erp_plan/blob/main/تحليل_هيكلية_الأقسام_والموديولات_ERP_موسع_تفصيلي.md)
- [تدفقات العمليات](https://github.com/ameenalqershi/erp_plan/blob/main/تدفقات_عمليات_ERP_كاملة_وأنواع_القيود.md)
- [كل ملفات الموديولات](https://github.com/ameenalqershi/erp_plan/tree/main/Modules)
- [كل ملفات قواعد الأعمال](https://github.com/ameenalqershi/erp_plan/tree/main/Business%20rules)
- [هيكلية المشروع](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/ProjectStructure_CQRS_Detailed.md)

---

**هذه الصورة الذهنية هي المرجع النهائي لبناء طبقة الدومين لكامل مشروع ERP-Pro  
ولا يجوز الخروج عنها أو تعديلها إلا عبر المستخدم أو بتحديث المخططات المعتمدة.**