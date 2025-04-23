# 🏗️ **مخطط هيكلة مشروع ERP متكامل بمعمارية CQRS (تفصيلي واحترافي مع الموديولات)**

---

## الجذر Root
- [ ] **src/**  
    يحتوي جميع المشاريع الفرعية (Projects) بالنظام
- [ ] **.editorconfig**, **.gitignore**  
    إعدادات المشروع العامة

---

## **1. Domain Layer (النطاق/المجال)**
**الغرض:** منطق الأعمال المجرد (Core Business Logic)، الكيانات، القيم، واجهات المستودعات.

```
src/
└── Domain/
    ├─ Common/
    │   ├─ Entities/
    │   ├─ ValueObjects/
    │   ├─ Primitives/
    │   ├─ Aggregates/
    │   ├─ Events/
    │   ├─ Enums/
    │   ├─ Exceptions/
    │   └─ Interfaces/
    ├─ ERP/
    │   ├─ Accounts/       # 🟢 Module: شجرة الحسابات والدليل المحاسبي
    │   ├─ Journals/       # 🟢 Module: القيود اليومية
    │   ├─ Invoices/       # 🟢 Module: الفواتير (مبيعات/مشتريات)
    │   ├─ Customers/      # 🟢 Module: العملاء
    │   ├─ Suppliers/      # 🟢 Module: الموردين
    │   ├─ Products/       # 🟢 Module: الأصناف
    │   ├─ Warehouses/     # 🟢 Module: المخازن
    │   ├─ Sales/          # 🟢 Module: المبيعات
    │   ├─ Purchases/      # 🟢 Module: المشتريات
    │   ├─ HR/             # 🟢 Module: الموارد البشرية
    │   ├─ Users/          # 🟢 Module: المستخدمون والصلاحيات
    │   ├─ Settings/       # 🟢 Module: الإعدادات العامة
    │   ├─ Notifications/  # 🟢 Module: الإشعارات
    │   ├─ Reports/        # 🟢 Module: التقارير
    │   ├─ Audit/          # 🟢 Module: التدقيق والسجلات
    │   └─ ...             # أضف أي وحدات تخصصية أخرى هنا
    └─ Domain.csproj
```

---

## **2. Application Layer (تطبيق الأعمال)**
**الغرض:** منطق التطبيق، أوامر واستعلامات النظام (CQRS)، الخدمات، واجهات الاستخدام.

```
src/
└── Application/
    ├─ Abstractions/
    │   ├─ Interfaces/
    │   └─ Services/
    ├─ Behaviors/
    ├─ Common/
    │   ├─ Configurations/
    │   ├─ Extensions/
    │   ├─ Mappings/
    │   ├─ Responses/         # 🟡 فئات الاستجابة الموحدة مثل Result, ApiResponse, ErrorResponse
    │   ├─ Serialization/
    │   ├─ Specifications/
    │   └─ Validators/
    ├─ ContextValidators/
    ├─ Enums/
    ├─ Exceptions/
    ├─ Features/
    │   ├─ Accounts/         # 🟢 Module: شجرة الحسابات
    │   ├─ Journals/         # 🟢 Module: القيود اليومية
    │   ├─ Invoices/         # 🟢 Module: الفواتير
    │   ├─ Customers/        # 🟢 Module: العملاء
    │   ├─ Suppliers/        # 🟢 Module: الموردين
    │   ├─ Products/         # 🟢 Module: الأصناف
    │   ├─ Warehouses/       # 🟢 Module: المخازن
    │   ├─ Sales/            # 🟢 Module: المبيعات
    │   ├─ Purchases/        # 🟢 Module: المشتريات
    │   ├─ HR/               # 🟢 Module: الموارد البشرية
    │   ├─ Users/            # 🟢 Module: المستخدمون والصلاحيات
    │   ├─ Settings/         # 🟢 Module: الإعدادات العامة
    │   ├─ Notifications/    # 🟢 Module: الإشعارات
    │   ├─ Reports/          # 🟢 Module: التقارير
    │   ├─ Audit/            # 🟢 Module: التدقيق والسجلات
    │   └─ ...               # أضف أي وحدات تخصصية أخرى هنا
    ├─ Helper/
    └─ Application.csproj
```

---

## **3. Infrastructure Layer (البنية التحتية)**
**الغرض:** التواصل مع قواعد البيانات، الخدمات الخارجية (Email, SMS, Files, Cache).

```
src/
└── Infrastructure/
    ├─ Data/
    │   ├─ Contexts/
    │   ├─ Migrations/
    │   ├─ Repositories/
    │   │   ├─ AccountRepository.cs         # 🟢 لكل Module Repository خاص
    │   │   ├─ JournalRepository.cs
    │   │   ├─ InvoiceRepository.cs
    │   │   └─ ...
    │   └─ Seed/
    ├─ Identity/
    ├─ Services/
    │   ├─ Email/
    │   ├─ Sms/
    │   └─ Files/
    ├─ External/
    │   ├─ PaymentGateways/
    │   ├─ ERPIntegrations/
    │   └─ ...
    ├─ Configurations/
    ├─ DependencyInjection/
    ├─ Extensions/
    └─ Infrastructure.csproj
```

---

## **4. Infrastructure.Shared Layer (مشتركة بنية تحتية)**
**الغرض:** أدوات مشتركة بين مشاريع البنية التحتية (Extensions, Constants, Utilities).

```
src/
└── Infrastructure.Shared/
    ├─ Constants/
    ├─ Extensions/
    ├─ Utilities/
    ├─ Resources/
    └─ Infrastructure.Shared.csproj
```

---

## **5. Shared Layer**
**الغرض:** الأدوات/المكتبات/الأنماط الشائعة بين كل الطبقات (Validation, Results, Localization, رسائل النظام الموحدة).

```
src/
└── Shared/
    ├─ Localization/
    │   ├─ SystemMessages.ar.resx    # 🟡 رسائل النظام الموحدة (نجاح/خطأ/تحذير...) متعددة اللغات
    │   └─ SystemMessages.en.resx
    ├─ Results/
    │   ├─ Result.cs                 # 🟡 نتيجة العملية الموحدة (نجاح/فشل/بيانات/أخطاء)
    │   ├─ ApiResponse.cs            # 🟡 استجابة الـAPI الموحدة
    │   └─ ErrorResponse.cs          # 🟡 نموذج الخطأ الموحد
    ├─ DTOs/
    │   ├─ PagedResultDto.cs         # 🟡 نتيجة التصفح الموحدة (Paging)
    │   └─ ...
    ├─ Exceptions/
    ├─ Extensions/
    ├─ Resources/
    ├─ Utilities/
    └─ Shared.csproj
```

---

## **6. Client Layer (واجهة المستخدم)**
**الغرض:** تطبيق العميل (Blazor, Avalonia, Angular, إلخ).

```
src/
└── Client/
    ├─ Pages/
    │   ├─ Accounts/       # 🟢 Module: شجرة الحسابات
    │   ├─ Journals/       # 🟢 Module: القيود اليومية
    │   ├─ Invoices/       # 🟢 Module: الفواتير
    │   ├─ Customers/
    │   ├─ Suppliers/
    │   ├─ Products/
    │   ├─ Warehouses/
    │   ├─ Sales/
    │   ├─ Purchases/
    │   ├─ HR/
    │   ├─ Users/
    │   ├─ Settings/
    │   ├─ Notifications/
    │   ├─ Reports/
    │   ├─ Audit/
    │   └─ ...
    ├─ Components/
    │   ├─ Accounts/
    │   ├─ Journals/
    │   └─ ...             # لكل Module مكونات خاصة
    │   └─ SystemMessage.razor  # 🟡 مكون عرض الرسائل الموحدة
    ├─ Services/
    ├─ Extensions/
    ├─ Mappings/
    ├─ Resources/
    ├─ Styles/
    ├─ Helpers/
    ├─ Models/
    ├─ ViewModels/
    └─ Client.csproj
```

---

## **7. Client.Infrastructure Layer (بنية واجهة المستخدم)**
**الغرض:** الربط مع API، إدارة الجلسات، التخزين المحلي، خدمات مساعدة.

```
src/
└── Client.Infrastructure/
    ├─ Api/
    ├─ Auth/
    ├─ Services/
    │   ├─ MessageService.cs         # 🟡 خدمة مركزية لإدارة رسائل النظام في الواجهة
    ├─ Storage/
    ├─ DependencyInjection/
    └─ Client.Infrastructure.csproj
```

---

## **8. Server Layer (واجهة الـAPI)**
**الغرض:** نقطة دخول الـAPI/الواجهة الخلفية، إعدادات الربط، الفلاتر، التحكم بالصلاحيات.

```
src/
└── Server/
    ├─ Controllers/
    │   ├─ AccountsController.cs         # 🟢 لكل Module Controller خاص
    │   ├─ JournalsController.cs
    │   ├─ InvoicesController.cs
    │   ├─ CustomersController.cs
    │   ├─ SuppliersController.cs
    │   ├─ ProductsController.cs
    │   ├─ WarehousesController.cs
    │   ├─ SalesController.cs
    │   ├─ PurchasesController.cs
    │   ├─ HRController.cs
    │   ├─ UsersController.cs
    │   ├─ SettingsController.cs
    │   ├─ NotificationsController.cs
    │   ├─ ReportsController.cs
    │   ├─ AuditController.cs
    │   └─ ...
    ├─ Extensions/
    ├─ Filters/
    │   ├─ ExceptionFilter.cs        # 🟡 فلتر موحد لمعالجة الأخطاء وإرجاع ErrorResponse
    ├─ Middlewares/
    ├─ DependencyInjection/
    ├─ Settings/
    ├─ Mappings/
    ├─ Startup.cs / Program.cs
    └─ Server.csproj
```

---

## **9. Infrastructure.ML (ذكاء اصطناعي/تعلم آلي - إذا كان مطلوباً)**
**الغرض:** نماذج تعلم آلي أو ذكاء اصطناعي مرتبطة بالنظام.

```
src/
└── Infrastructure.ML/
    ├─ Models/
    ├─ Services/
    ├─ Data/
    └─ Infrastructure.ML.csproj
```

---

# 🔗 **العناصر المشتركة والأدوات العامة (Reusable/Shared Tools)**
- **BaseEntity, AuditableEntity, ValueObject, AggregateRoot** (Domain/Common/Primitives)
- **Result<T>, PaginatedResult<T>, ApiResponse, ErrorResponse** (Shared/Results, Application/Common/Responses)
- **IRepository<T>, IUnitOfWork** (Domain/Interfaces, Infrastructure/Repositories)
- **BaseRequest, BaseResponse, PaginationRequest, PaginationResponse** (Application/Common)
- **Specifications** (Application/Common/Specifications)
- **Localization/SystemMessages.[lang].resx** (Shared/Localization) 🟡 رسائل النظام الموحدة
- **ValidationAttributes, Validators** (Application/Common/Validators)
- **Mapping Profiles** (Application/Common/Mappings)
- **Behaviors: Logging, Caching, Validation, Performance** (Application/Behaviors)
- **Exception Handling, Error Codes, ExceptionFilter** (Shared/Exceptions, Application/Exceptions, Server/Filters)
- **Dependency Injection Extensions** (Infrastructure/DependencyInjection, Server/DependencyInjection)
- **Utilities (DateTime, Number, File, ...)** (Shared/Utilities)
- **Extensions (LINQ, String, Enum, ...)** (Shared/Extensions, Infrastructure.Shared/Extensions)
- **Resources (Images, Icons, Strings, ...)** (Shared/Resources, Client/Resources)
- **SystemMessage Component/Service** (Client/Components/SystemMessage, Client.Infrastructure/Services/MessageService)

---

# 🗂️ **قائمة جميع الموديولات (Modules):**
- **Accounts** (الحسابات والدليل المحاسبي)
- **Journals** (القيود اليومية)
- **Invoices** (الفواتير)
- **Customers** (العملاء)
- **Suppliers** (الموردين)
- **Products** (الأصناف)
- **Warehouses** (المخازن)
- **Sales** (المبيعات)
- **Purchases** (المشتريات)
- **HR** (الموارد البشرية)
- **Users & Roles** (المستخدمون والصلاحيات)
- **Settings** (الإعدادات العامة)
- **Notifications** (الإشعارات)
- **Reports** (التقارير)
- **Audit & Logs** (التدقيق والسجلات)
- *(أضف أي Module تخصصي آخر لاحقاً)*

---

# 📌 **نصائح هيكلية لنجاح المشروع (Best Practices)**
- **كل Module مستقل عبر كافة الطبقات (Vertically Sliced Modules)**
- **كل ما هو مشترك أو موحد (نتائج، رسائل، استجابات، DTOs) في Shared أو Application/Common/Responses**
- **لا تكرر الكود أبداً**: ضع كل أداة أو وظيفة مشتركة في المكان الأنسب لها (Shared/، Infrastructure.Shared/، Common/)
- **CQRS واضح:** كل ميزة (Feature) تقسم إلى أوامر (Commands) واستعلامات (Queries) ومعالجات (Handlers)
- **الفرونت اند يجب أن يستهلك نفس نماذج الاستجابة والرسائل الموحدة من الباك اند**

---

**هذا المخطط يضمن لك مشروع ERP قابل للتوسع، منظم بوضوح، يدعم أي إضافة أو صيانة مستقبلية باحترافية عالية.**