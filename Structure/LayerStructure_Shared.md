# 🟢 Shared Layer Structure (تفصيلي حسب احتياج مشروع ERP متكامل)

---

## الهدف من الطبقة  
طبقة Shared هي طبقة الأدوات العامة والمشتركة التي يمكن لأي طبقة أو مشروع (Application, Infrastructure, API, ...إلخ) أن يعتمد عليها مباشرة.  
تشمل أنواع بيانات عامة، نماذج، ثوابت، أدوات مساعدات (Helpers/Utils)، إعدادات، موارد (موحدة/متعددة اللغات)، وقيم كائنية (ValueObjects)، وكل ما لا يرتبط بسياق أعمال محدد.

---

## الهيكل العام  
```
src/
└── Shared/
    ├─ Constants/
    ├─ DTOs/
    ├─ Enums/
    ├─ Events/
    ├─ Exceptions/
    ├─ Extensions/
    ├─ Helpers/
    ├─ Interfaces/
    ├─ Localization/
    ├─ Models/
    ├─ Resources/
    ├─ Settings/
    ├─ Utilities/
    ├─ ValueObjects/
    ├─ Validation/
    └─ Shared.csproj
```

---

## 🟢 تفصيل أهم المجلدات ومحتوياتها

### Constants/
```
Constants/
├─ AppConstants.cs                # ثوابت النظام العامة (تسميات، مفاتيح، ...إلخ)
├─ RegexConstants.cs              # تعبيرات نمطية شائعة
├─ ErrorMessages.cs               # رسائل أخطاء قياسية
├─ ClaimTypes.cs                  # ثوابت Claims (JWT وغيرها)
```

---

### DTOs/
```
DTOs/
├─ LookupDto.cs                   # نموذج بيانات عام للقوائم المنسدلة أو الـ Lookup
├─ KeyValueDto.cs                 # نموذج زوج (مفتاح/قيمة)
├─ PaginationDto.cs               # بيانات التقسيم
├─ SharedResultDto.cs             # نتيجة موحدة للعمليات العامة
```

---

### Enums/
```
Enums/
├─ UserRole.cs
├─ Gender.cs
├─ StatusEnum.cs                  # حالة عامة (نشط/غير نشط/...إلخ)
├─ LanguageEnum.cs
```

---

### Events/
```
Events/
├─ DomainEvent.cs                 # حدث خاص بالدومين (أساسي)
├─ IntegrationEvent.cs            # حدث تكامل خارجي
├─ NotificationEvent.cs           # حدث إشعار عام
```

---

### Exceptions/
```
Exceptions/
├─ BusinessException.cs           # خطأ أعمال عام
├─ NotFoundException.cs
├─ UnauthorizedException.cs
├─ ValidationException.cs
├─ ForbiddenException.cs
```

---

### Extensions/
```
Extensions/
├─ StringExtensions.cs
├─ DateTimeExtensions.cs
├─ EnumExtensions.cs
├─ CollectionExtensions.cs
```

---

### Helpers/
```
Helpers/
├─ EmailHelper.cs
├─ EncryptionHelper.cs
├─ FileHelper.cs
├─ UrlHelper.cs
├─ MathHelper.cs
```

---

### Interfaces/
```
Interfaces/
├─ ILocalizable.cs
├─ IAuditable.cs
├─ IHasCreationTime.cs
├─ IHasModificationTime.cs
```

---

### Localization/
```
Localization/
├─ SharedResources.cs
├─ cultures.json
├─ ar.json
├─ en.json
```

---

### Models/
```
Models/
├─ ApiResponseModel.cs
├─ ExceptionDetailsModel.cs
├─ PaginatedListModel.cs
├─ NotificationModel.cs
```

---

### Resources/
```
Resources/
├─ SharedResources.resx
├─ ValidationMessages.resx
├─ EmailTemplates/
│   ├─ WelcomeEmail.html
│   ├─ ResetPasswordEmail.html
```

---

### Settings/
```
Settings/
├─ AppSettings.cs
├─ JwtSettings.cs
├─ MailSettings.cs
├─ StorageSettings.cs
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
├─ Money.cs
```

---

### Validation/
```
Validation/
├─ ValidationMessages.cs
├─ EmailValidator.cs
├─ PhoneNumberValidator.cs
├─ NameValidator.cs
```

---

## 💡 ملاحظات ختامية  
- هذه الطبقة لا تعتمد على أي طبقة في المشروع، بل تعتمد عليها جميع الطبقات الأخرى.
- جميع الملفات هنا يجب أن تكون بدون أي منطق أعمال خاص أو تكامل مع مصادر بيانات أو خدمات خارجية.
- كل كود في Shared يجب أن يكون عامًا وقابلاً لإعادة الاستخدام في أي جزء من النظام أو حتى في مشاريع مستقبلية.

---

**إذا أردت تفصيل أي مجلد أو مثال كود/شرح لأي ملف من ملفات هذه الطبقة، أخبرني بذلك!**