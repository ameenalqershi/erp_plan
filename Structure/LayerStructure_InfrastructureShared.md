# 🟣 Infrastructure.Shared Layer Structure (تفصيلي حسب احتياج مشروع ERP متكامل)

---

## الهدف من الطبقة  
طبقة Infrastructure.Shared هي مستودع "المشتركات" التقنية بين مشاريع/طبقات البنية التحتية (Infrastructure, API, إلخ).  
توفر أدوات عامة، إعدادات مركزية، خدمات مساعدة، نماذج مشتركة، تكاملات، وأيضاً Constants وUtilities وكل ما يمكن مشاركته بين أكثر من مشروع أو طبقة.

---

## الهيكل العام  
```
src/
└── Infrastructure.Shared/
    ├─ Constants/
    ├─ Extensions/
    ├─ Helpers/
    ├─ Localization/
    ├─ Models/
    ├─ Resources/
    ├─ Services/
    ├─ Settings/
    ├─ Specifications/
    ├─ Utilities/
    ├─ ValueObjects/
    ├─ Validation/
    ├─ Integration/
    ├─ Middlewares/
    ├─ DTOs/
    └─ Infrastructure.Shared.csproj
```

---

## 🟢 تفصيل أهم المجلدات ومحتوياتها

### Constants/
```
Constants/
├─ GlobalConstants.cs           # ثوابت عامة على مستوى النظام
├─ RegexConstants.cs            # تعبيرات نمطية مشتركة
├─ ErrorMessages.cs             # رسائل الأخطاء القياسية
├─ ClaimTypesConstants.cs       # أسماء Claims موحدة
```

---

### Extensions/
```
Extensions/
├─ StringExtensions.cs
├─ DateTimeExtensions.cs
├─ EnumExtensions.cs
├─ ClaimsPrincipalExtensions.cs
├─ QueryableExtensions.cs
```

---

### Helpers/
```
Helpers/
├─ EncryptionHelper.cs
├─ HashingHelper.cs
├─ EmailHelper.cs
├─ FileHelper.cs
├─ UrlHelper.cs
```

---

### Localization/
```
Localization/
├─ SharedResources.cs
├─ LocalizationManager.cs
├─ cultures.json
├─ ar.json
├─ en.json
```

---

### Models/
```
Models/
├─ ApiErrorModel.cs             # نموذج خطأ موحد
├─ PaginatedList.cs
├─ FileUploadModel.cs
├─ NotificationModel.cs
```

---

### Resources/
```
Resources/
├─ SharedResources.resx         # نصوص الرسائل المشتركة متعدد اللغات
├─ EmailTemplates/
│   ├─ WelcomeTemplate.html
│   ├─ ResetPasswordTemplate.html
```

---

### Services/
```
Services/
├─ EncryptionService.cs
├─ LocalizationService.cs
├─ SharedMailService.cs
├─ FileValidationService.cs
```

---

### Settings/
```
Settings/
├─ AppSettings.cs               # إعدادات عامة مشتركة
├─ JwtSettings.cs
├─ MailSettings.cs
├─ StorageSettings.cs
```

---

### Specifications/
```
Specifications/
├─ ISpecification.cs
├─ SpecificationEvaluator.cs
├─ BaseSpecification.cs
```

---

### Utilities/
```
Utilities/
├─ RandomGenerator.cs
├─ BarcodeGenerator.cs
├─ PasswordGenerator.cs
├─ QrCodeGenerator.cs
```

---

### ValueObjects/
```
ValueObjects/
├─ Email.cs
├─ PhoneNumber.cs
├─ Address.cs
```

---

### Validation/
```
Validation/
├─ ValidationMessages.cs
├─ ValidationHelper.cs
├─ EmailValidator.cs
├─ PhoneValidator.cs
```

---

### Integration/
```
Integration/
├─ ExternalApiClient.cs         # عميل API خارجي مشترك
├─ SharedWebhookPublisher.cs    # نشر Webhook مشترك
```

---

### Middlewares/
```
Middlewares/
├─ ExceptionMiddleware.cs
├─ LocalizationMiddleware.cs
├─ RequestLoggingMiddleware.cs
```

---

### DTOs/
```
DTOs/
├─ SharedResultDto.cs
├─ LookupDto.cs
├─ KeyValueDto.cs
```

---

## 💡 ملاحظات ختامية  
- هذه الطبقة لا تعتمد على أي طبقة في المشروع، بل تعتمد عليها باقي الطبقات (بنية تحتية، تطبيق، واجهات).
- كل ملف أو خدمة أو ثابت هنا يُستخدم في أكثر من طبقة أو مشروع فرعي.
- يفضل فصل الأكواد التي لا تخص سياقاً محدداً في طبقة الـShared لتسهيل إعادة الاستخدام والصيانة.

---

**إذا أردت تفصيل أو مثال كود لأي ملف أو إضافة خاصة لمشروعك، أخبرني بذلك!**