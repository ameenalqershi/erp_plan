# 🟣 Infrastructure Layer Structure (تفصيلي لكل المجلدات والموديولات حسب تحليل مخططات مشروعك)

---

## الهدف من الطبقة  
طبقة Infrastructure هي المسؤولة عن تنفيذ التفاصيل التقنية (Data Access, External Services, File System, Email, Logging, ...).  
تربط التطبيق مع مصادر البيانات وقواعد البيانات والخدمات الخارجية والبنية التحتية الفعلية.

---

## الهيكل العام  
```
src/
└── Infrastructure/
    ├─ Data/
    ├─ DependencyInjection/
    ├─ Identity/
    ├─ Interceptors/
    ├─ Logging/
    ├─ Mail/
    ├─ Migrations/
    ├─ Persistence/
    ├─ Repositories/
    ├─ Reporting/
    ├─ Services/
    ├─ Storage/
    ├─ ThirdParty/
    ├─ Files/
    ├─ Notifications/
    ├─ Configurations/
    ├─ Security/
    └─ Infrastructure.csproj
```

---

## 🟢 المجلدات العامة ومحتواها

### Data/
```
Data/
├─ ApplicationDbContext.cs          # السياق الرئيسي لقاعدة البيانات (EF Core)
├─ ApplicationDbContextFactory.cs   # لإنشاء السياق في الـMigrations أو عند الحاجة
├─ Seed/
│   ├─ DbSeeder.cs                  # تهيئة بيانات أولية
│   ├─ SeedData.sql                 # سكريبت بيانات ابتدائية
```

---

### DependencyInjection/
```
DependencyInjection/
├─ DependencyInjection.cs           # تسجيل جميع الخدمات في الـDI Container
├─ MediatRRegistration.cs           # تسجيل Handlers/Behaviors
├─ RepositoryRegistration.cs        # تسجيل الريبو
├─ ServiceRegistration.cs           # تسجيل الخدمات الخارجية
```

---

### Identity/
```
Identity/
├─ ApplicationUser.cs               # نموذج المستخدم (يرث IdentityUser)
├─ ApplicationRole.cs               # نموذج الدور
├─ ApplicationUserStore.cs          # تخزين المستخدمين
├─ IdentityDbContext.cs             # DbContext خاص بالهوية
├─ IdentityInitializer.cs           # تهيئة مستخدمين/أدوار افتراضية
```

---

### Interceptors/
```
Interceptors/
├─ AuditableEntitySaveChangesInterceptor.cs # تتبع الإنشاء والتعديل
├─ SoftDeleteInterceptor.cs                 # دعم الحذف الناعم
├─ QueryFilterInterceptor.cs                # تطبيق الفلاتر العامة
```

---

### Logging/
```
Logging/
├─ LoggerAdapter.cs                 # تكامل مع ILogger
├─ Log4NetLogger.cs                 # إذا كان Log4Net مستخدم
├─ SerilogLogger.cs                 # إذا كان Serilog مستخدم
├─ LoggingConfigurator.cs           # إعدادات اللوجينج
```

---

### Mail/
```
Mail/
├─ SmtpMailService.cs               # إرسال البريد عبر SMTP
├─ SendGridMailService.cs           # تكامل مع SendGrid
├─ MailTemplateRenderer.cs          # توليد قوالب البريد
├─ MailSettings.cs                  # إعدادات البريد الإلكتروني
```

---

### Migrations/
```
Migrations/
├─ 202401010001_InitialCreate.cs    # أول ترحيل للجدول
├─ ...                             # باقي ترحيلات EF Core
├─ README.md                       # شرح إدارة الترحيلات
```

---

### Persistence/
```
Persistence/
├─ UnitOfWork.cs                   # نمط وحدة العمل
├─ TransactionScopeManager.cs      # إدارة الترانزاكشن
├─ SqlConnectionFactory.cs         # مصنع اتصال SQL
```

---

### Repositories/
```
Repositories/
├─ GenericRepository.cs             # ريبو عام قابل لإعادة الاستخدام
├─ AccountRepository.cs
├─ JournalRepository.cs
├─ InvoiceRepository.cs
├─ WarehouseRepository.cs
├─ ...                             # بقية الريبو لكل كيان رئيسي
```

---

### Reporting/
```
Reporting/
├─ ReportBuilder.cs                 # منطق بناء التقارير
├─ PdfReportExporter.cs             # تصدير PDF
├─ ExcelReportExporter.cs           # تصدير Excel
├─ ReportTemplateLoader.cs          # تحميل قوالب التقارير
```

---

### Services/
```
Services/
├─ DateTimeService.cs               # مزود الوقت
├─ FileStorageService.cs            # تخزين الملفات
├─ NotificationService.cs           # إرسال الإشعارات
├─ SmsService.cs                    # إرسال SMS
├─ CurrencyExchangeService.cs       # جلب أسعار الصرف
├─ ...                             # أي خدمات بنية تحتية أخرى
```

---

### Storage/
```
Storage/
├─ LocalFileStorage.cs              # تخزين ملفات محلي
├─ AzureBlobStorage.cs              # تخزين سحابي (اختياري)
├─ FileStorageSettings.cs           # إعدادات التخزين
```

---

### ThirdParty/
```
ThirdParty/
├─ PaymentGatewayService.cs         # دفع إلكتروني
├─ ExternalCrmConnector.cs          # تكامل مع CRM خارجي
├─ ExternalErpConnector.cs          # تكامل مع ERP آخر
├─ GoogleDriveSyncService.cs        # مزامنة Google Drive
```

---

### Files/
```
Files/
├─ FileUploader.cs                  # رفع الملفات
├─ FileDownloader.cs                # تنزيل الملفات
├─ FileValidator.cs                 # التحقق من نوع/حجم الملفات
```

---

### Notifications/
```
Notifications/
├─ PushNotificationService.cs       # إشعارات دفع
├─ WebhookPublisher.cs              # إرسال Webhooks
├─ NotificationSettings.cs          # إعدادات الإشعارات
```

---

### Configurations/
```
Configurations/
├─ DbConfiguration.cs               # إعدادات قاعدة البيانات (ConnectionString)
├─ MailConfiguration.cs             # إعدادات البريد
├─ LoggingConfiguration.cs          # إعدادات اللوجينج
├─ IdentityConfiguration.cs         # إعدادات الهوية
├─ ThirdPartyConfiguration.cs       # إعدادات الخدمات الخارجية
```

---

### Security/
```
Security/
├─ EncryptionService.cs             # التشفير
├─ JwtTokenGenerator.cs             # توليد JWT
├─ TokenValidationService.cs        # تحقق من التوكن
├─ HashingService.cs                # تجزئة كلمات المرور
├─ SecuritySettings.cs              # إعدادات أمنية عامة
```

---

## 💡 ملاحظات هامة  
- يمكن إضافة أو تخصيص مجلدات أو ملفات إضافية حسب احتياج المشروع.
- غالبًا ما يضاف لكل كيان ريبو خاص به، أو ريبو عام مع ريبو تخصصي.
- غالبًا تربط هذه الطبقة مع الـApplication عبر الاعتماد على واجهات abstractions/Interfaces.

---

**إذا أردت تفصيل أي مجلد أو خدمة أو مثال كود لأي ملف، فقط أخبرني بذلك.**