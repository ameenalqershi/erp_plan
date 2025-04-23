# 🟠 Server Layer Structure (واجهة الـAPI - تفصيلي حسب احتياج مشروع ERP متكامل)

---

## الهدف من الطبقة  
طبقة Server (غالباً مشروع WebAPI) هي واجهة الدخول إلى النظام، تستقبل طلبات العملاء (Clients)، وتتعامل مع طبقة الـApplication لتنفيذ العمليات، وتعيد النتائج كـ JSON أو صيغ API أخرى.  
تشمل: Controllers، Endpoints، Middlewares، إعدادات التحقق والتوثيق، التوثيق (Swagger)، الـFilters، وأدوات إدارة دورة حياة الطلب.

---

## الهيكل العام  
```
src/
└── Server/
    ├─ Controllers/
    ├─ Endpoints/
    ├─ Extensions/
    ├─ Filters/
    ├─ Middlewares/
    ├─ Models/
    ├─ OpenApi/
    ├─ Properties/
    ├─ Resources/
    ├─ Services/
    ├─ Settings/
    ├─ Validation/
    ├─ Program.cs
    ├─ Startup.cs (إن وجد)
    └─ Server.csproj
```

---

## 🟢 تفصيل أهم المجلدات ومحتوياتها

### Controllers/
```
Controllers/
├─ AccountsController.cs
├─ JournalsController.cs
├─ InvoicesController.cs
├─ CustomersController.cs
├─ SuppliersController.cs
├─ ProductsController.cs
├─ WarehousesController.cs
├─ SalesController.cs
├─ PurchasesController.cs
├─ HRController.cs
├─ UsersController.cs
├─ SettingsController.cs
├─ NotificationsController.cs
├─ ReportsController.cs
├─ AuditController.cs
├─ InventoryController.cs
├─ FixedAssetsController.cs
├─ ProjectsController.cs
├─ ContractsController.cs
├─ LoansController.cs
├─ PayrollController.cs
├─ TaxesController.cs
├─ BankingController.cs
├─ POSController.cs
├─ CRMController.cs
├─ ECommerceController.cs
├─ ManufacturingController.cs
```
> يمكن وجود Controllers تخصصية أصغر لوحدات فرعية أو عمليات خاصة.

---

### Endpoints/
```
Endpoints/
├─ Accounts/
│   ├─ CreateAccountEndpoint.cs
│   ├─ UpdateAccountEndpoint.cs
│   ├─ DeleteAccountEndpoint.cs
│   └─ ...
├─ Invoices/
│   ├─ CreateInvoiceEndpoint.cs
│   └─ ...
├─ ... (نفس التنظيم للموديولات الأخرى)
```
> في حال استخدام Minimal APIs أو أسلوب Endpoints المنفصلة.

---

### Extensions/
```
Extensions/
├─ ServiceCollectionExtensions.cs
├─ ApplicationBuilderExtensions.cs
├─ SwaggerExtensions.cs
├─ MiddlewareExtensions.cs
├─ LocalizationExtensions.cs
```

---

### Filters/
```
Filters/
├─ ApiExceptionFilter.cs
├─ ValidationFilter.cs
├─ AuthorizationFilter.cs
├─ AuditActionFilter.cs
```

---

### Middlewares/
```
Middlewares/
├─ ExceptionMiddleware.cs
├─ RequestLoggingMiddleware.cs
├─ LocalizationMiddleware.cs
├─ JwtTokenMiddleware.cs
```

---

### Models/
```
Models/
├─ ApiResponseModel.cs
├─ ErrorModel.cs
├─ PaginationParameters.cs
├─ LoginRequestModel.cs
├─ RegisterRequestModel.cs
├─ ChangePasswordModel.cs
├─ FileUploadModel.cs
├─ NotificationRequestModel.cs
```
> نماذج خاصة بطبقة API (وليس Dto طبقة Application)

---

### OpenApi/
```
OpenApi/
├─ SwaggerConfig.cs
├─ SwaggerDoc.xml
├─ SecuritySchemeConfig.cs
```
> كل ما يخص توثيق الـAPI (Swagger/OpenAPI)

---

### Properties/
```
Properties/
├─ launchSettings.json
```

---

### Resources/
```
Resources/
├─ ar.json
├─ en.json
├─ SharedResources.resx
```

---

### Services/
```
Services/
├─ JwtTokenService.cs
├─ FileService.cs
├─ LocalizationService.cs
├─ ApiUserContextService.cs
```
> خدمات مساعدة خاصة بالواجهة (API)، مثل استخراج بيانات المستخدم الحالي من JWT إلخ.

---

### Settings/
```
Settings/
├─ SwaggerSettings.cs
├─ CorsSettings.cs
├─ ApiSettings.cs
├─ LocalizationSettings.cs
```

---

### Validation/
```
Validation/
├─ ModelValidationExtensions.cs
├─ ApiValidationResponseFactory.cs
├─ ApiValidationMessages.cs
```

---

### Program.cs و Startup.cs
- **Program.cs**: نقطة البداية لتشغيل الـAPI (WebHost).
- **Startup.cs**: إعداد الخدمات، الـDI، الميدلوير، الربط مع البنية التحتية، إلخ (في بعض المشاريع الحديثة الإعداد كله في Program.cs).

---

## 💡 ملاحظات ختامية  
- كل Controller أو Endpoint يعتمد فقط على Application Layer (وليس Infrastructure مباشرة).
- يمكن إضافة مجلدات فرعية بحسب الحاجة (مثلاً Notifications/Endpoints خاصة، أو ملفات Auth منفصلة).
- جميع الأدوات العامة (Logging، Localization، Exception Handling، CORS، Security) تجهز وتفعل هنا.
- يجب أن تكون كل ملفات هذه الطبقة محايدة من منطق الأعمال، وتقوم فقط بالتوجيه (Routing)، التحقق، تهيئة الطلب/الاستجابة، والاتصال بالـApplication.

---

**إذا أردت مثال كود لأي Controller أو Endpoint أو Middleware أو إعدادات خاصة بالـAPI، أخبرني بذلك!**