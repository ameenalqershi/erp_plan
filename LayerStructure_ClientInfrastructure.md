# 🟢 Client.Infrastructure Layer Structure (تفصيلي حسب احتياج مشروع ERP متكامل - WPF)

---

## الهدف من الطبقة  
طبقة Client.Infrastructure هي طبقة البنية التحتية الداعمة لتطبيق العميل (WPF)،  
تحتوي على جميع الخدمات والتكاملات التقنية التي يحتاجها العميل للوصول للبيانات عن بعد، التخزين المحلي، الإعدادات، إدارة الهوية والمصادقة، الشبكات، الربط مع الخدمات الخارجية، إلخ.  
تجعل تطبيق الـClient (WPF) نظيفًا ومنفصلًا عن التفاصيل التقنية.

---

## الهيكل العام  
```
src/
└── Client.Infrastructure/
    ├─ Api/
    │   ├─ Clients/
    │   ├─ Endpoints/
    │   ├─ Models/
    │   ├─ Mappers/
    │   ├─ Responses/
    │   ├─ Requests/
    ├─ Authentication/
    ├─ Authorization/
    ├─ Caching/
    ├─ Connectivity/
    ├─ DependencyInjection/
    ├─ Exceptions/
    ├─ Extensions/
    ├─ Identity/
    ├─ Localization/
    ├─ Logging/
    ├─ Notifications/
    ├─ Persistence/
    ├─ Resources/
    ├─ Security/
    ├─ Services/
    ├─ Settings/
    ├─ Storage/
    ├─ Validation/
    ├─ Helpers/
    ├─ Utilities/
    └─ Client.Infrastructure.csproj
```

---

## 🟢 تفصيل أهم المجلدات ومحتوياتها

### Api/
- كل ما يتعلق بالتعامل مع الـAPI (Endpoints, HttpClient, إدارة الطلبات والاستجابة، إلخ).
```
Api/
├─ Clients/
│   ├─ BaseApiClient.cs
│   ├─ AccountsApiClient.cs
│   ├─ InvoicesApiClient.cs
│   ├─ ...
├─ Endpoints/
│   ├─ ApiRoutes.cs
│   ├─ EndpointConstants.cs
├─ Models/
│   ├─ ApiResponseModel.cs
│   ├─ PaginationResponse.cs
│   ├─ ...
├─ Mappers/
│   ├─ ApiToDomainMapper.cs
├─ Requests/
│   ├─ CreateAccountRequest.cs
│   ├─ LoginRequest.cs
│   ├─ ...
├─ Responses/
│   ├─ AccountResponse.cs
│   ├─ LoginResponse.cs
│   ├─ ...
```

---

### Authentication/Authorization/
- إدارة المصادقة (JWT, Tokens, Login/Logout)، صلاحيات المستخدم.
```
Authentication/
├─ AuthService.cs
├─ TokenProvider.cs
├─ LoginManager.cs

Authorization/
├─ AuthorizationService.cs
├─ PermissionChecker.cs
```

---

### Caching/
- تخزين البيانات مؤقتًا (In-Memory/Local).
```
Caching/
├─ MemoryCacheService.cs
├─ LocalCacheService.cs
```

---

### Connectivity/
- إدارة التحقق من اتصال الإنترنت، مراقبة الشبكة.
```
Connectivity/
├─ NetworkMonitor.cs
├─ ConnectivityService.cs
```

---

### DependencyInjection/
- تسجيل الخدمات والبنية التحتية في DI Container.
```
DependencyInjection/
├─ DependencyInjection.cs
```

---

### Exceptions/
- معالجة الاستثناءات الخاصة بالبنية التحتية (API/Network/Deserialization).
```
Exceptions/
├─ ApiException.cs
├─ NetworkException.cs
├─ UnauthorizedException.cs
├─ ExceptionHandler.cs
```

---

### Extensions/
- دوال امتداد تسهل التعامل مع الـHttpResponse, Serialization, إلخ.
```
Extensions/
├─ HttpResponseExtensions.cs
├─ SerializationExtensions.cs
```

---

### Identity/
- بيانات المستخدم الحالي، إدارة الجلسات.
```
Identity/
├─ CurrentUserProvider.cs
├─ SessionManager.cs
```

---

### Localization/
- دعم التعدد اللغوي للعناصر التي تُدار من جانب العميل.
```
Localization/
├─ LocalizationProvider.cs
├─ LanguageResources/
│   ├─ ar.json
│   ├─ en.json
```

---

### Logging/
- تسجيل الأحداث والأخطاء محليًا أو عن بعد.
```
Logging/
├─ Logger.cs
├─ FileLogger.cs
├─ RemoteLogger.cs
```

---

### Notifications/
- خدمات إشعار المستخدم (Toast, Popup, إلخ).
```
Notifications/
├─ NotificationService.cs
├─ ToastNotifier.cs
```

---

### Persistence/
- إدارة التخزين الدائم للبيانات (ملفات، SQLite، إلخ).
```
Persistence/
├─ LocalDbContext.cs
├─ Repository/
│   ├─ LocalAccountRepository.cs
│   ├─ LocalInvoiceRepository.cs
```

---

### Resources/
- موارد ثابتة أو ملفات تُستخدم في طبقة البنية التحتية.
```
Resources/
├─ Certificates/
├─ Icons/
```

---

### Security/
- حماية البيانات، تشفير، إدارة كلمات المرور.
```
Security/
├─ EncryptionService.cs
├─ HashingService.cs
```

---

### Services/
- خدمات مساعدة (تحميل ملفات، تصدير، إلخ).
```
Services/
├─ FileDownloadService.cs
├─ FileUploadService.cs
├─ ExportService.cs
├─ PrintService.cs
```

---

### Settings/
- إدارة إعدادات التطبيق (API URLs، مفاتيح، إلخ).
```
Settings/
├─ AppSettings.cs
├─ ApiSettings.cs
├─ UserSettings.cs
```

---

### Storage/
- تخزين الملفات محليًا (Documents, Temp, إلخ).
```
Storage/
├─ FileStorageService.cs
├─ TempStorageService.cs
```

---

### Validation/
- التحقق من صحة المدخلات قبل إرسالها للـAPI.
```
Validation/
├─ ApiRequestValidator.cs
├─ AuthValidator.cs
```

---

### Helpers/Utilities/
- أدوات مساعدة عامة (Serialization، بناء الروابط، إلخ).
```
Helpers/
├─ SerializationHelper.cs
├─ UrlBuilder.cs
├─ JsonHelper.cs

Utilities/
├─ RetryPolicy.cs
├─ NetworkUtils.cs
```

---

## 💡 ملاحظات ختامية  
- طبقة Client.Infrastructure تفصل التفاصيل التقنية عن طبقة العرض (Client/WPF).
- جميع الخدمات هنا يتم حقنها في ViewModels/Services الخاصة بالواجهة عبر DI.
- كل ما يخص التعامل مع API أو الشبكة أو التخزين أو المصادقة يكون هنا وليس في الـViewModels.
- يمكن إضافة أو تخصيص أي مجلدات أو خدمات حسب الحاجة أو التكامل مع تقنيات أخرى (Bluetooth, Local Printing, ...إلخ).

---

**إذا أردت مثال كود لأي خدمة أو شرح عملي لأي جزء من هذه الطبقة، أخبرني بذلك!**