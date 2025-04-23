# 🟢 Client Layer Structure (WPF - واجهة المستخدم) (تفصيلي حسب احتياج مشروع ERP متكامل)

---

## الهدف من الطبقة  
طبقة Client (مشروع WPF) هي واجهة المستخدم الرسومية (Desktop GUI)،  
تتفاعل مع طبقة Server (API) لجلب/إرسال البيانات، تعرض النماذج والشاشات، وتدير تجربة المستخدم وتدفق النوافذ.  
تشمل: نوافذ (Windows)، صفحات (Pages)، عناصر تحكم (UserControls)، ViewModels، إدارة الطلبات (Services)، موارد، إعدادات، توطين، قوالب، إلخ.

---

## الهيكل العام  
```
src/
└── Client/
    ├─ App.xaml
    ├─ App.xaml.cs
    ├─ MainWindow.xaml
    ├─ MainWindow.xaml.cs
    ├─ Views/
    │   ├─ Accounts/
    │   ├─ Journals/
    │   ├─ Invoices/
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
    │   ├─ Inventory/
    │   ├─ FixedAssets/
    │   ├─ Projects/
    │   ├─ Contracts/
    │   ├─ Loans/
    │   ├─ Payroll/
    │   ├─ Taxes/
    │   ├─ Banking/
    │   ├─ POS/
    │   ├─ CRM/
    │   ├─ ECommerce/
    │   ├─ Manufacturing/
    │   └─ ...
    ├─ ViewModels/
    │   ├─ Accounts/
    │   ├─ Journals/
    │   ├─ Invoices/
    │   ├─ Customers/
    │   ├─ Suppliers/
    │   ├─ ... (مطابق لـ Views)
    ├─ Models/
    ├─ Services/
    │   ├─ Api/
    │   ├─ Navigation/
    │   ├─ Dialog/
    │   ├─ Authentication/
    │   ├─ Notification/
    │   └─ ...
    ├─ Commands/
    ├─ Converters/
    ├─ Behaviors/
    ├─ Templates/
    ├─ Styles/
    ├─ Resources/
    ├─ Themes/
    ├─ Helpers/
    ├─ Extensions/
    ├─ Localization/
    ├─ Validation/
    ├─ Assets/
    │   ├─ Images/
    │   ├─ Icons/
    │   ├─ Fonts/
    ├─ App.config
    └─ Client.csproj
```

---

## 🟢 تفصيل أهم المجلدات ومحتوياتها

### Views/
- يحتوي على جميع نوافذ وصفحات الواجهة (XAML)، كل موديول له مجلد فرعي.
```
Views/Accounts/AccountsView.xaml
Views/Invoices/InvoiceDetailsView.xaml
Views/Sales/SalesOrderView.xaml
...
```

### ViewModels/
- منطق العرض والتفاعل (MVVM)، لكل View أو نافذة ViewModel خاص بها.
```
ViewModels/Accounts/AccountsViewModel.cs
ViewModels/Invoices/InvoiceDetailsViewModel.cs
...
```

### Models/
- نماذج البيانات (Data Models)، غالباً مطابقة أو مكملة لـ DTOs الطبقات الأخرى.
```
Models/AccountModel.cs
Models/InvoiceModel.cs
Models/UserModel.cs
...
```

### Services/
- خدمات الطبقة مثل:
    - **Api/**: التعامل مع REST API
    - **Navigation/**: إدارة التنقل بين الصفحات
    - **Dialog/**: إدارة الحوارات والتنبيهات
    - **Authentication/**: مصادقة المستخدم
    - **Notification/**: عرض الإشعارات للمستخدم
```
Services/Api/AccountApiService.cs
Services/Api/InvoiceApiService.cs
Services/Navigation/NavigationService.cs
Services/Dialog/DialogService.cs
Services/Authentication/AuthService.cs
Services/Notification/ToastService.cs
...
```

### Commands/
- أوامر WPF (RelayCommand, DelegateCommand) لتنفيذ الأحداث من الواجهة.
```
Commands/RelayCommand.cs
Commands/SaveAccountCommand.cs
...
```

### Converters/
- محولات القيم (ValueConverters) لـ XAML (مثلاً: تحويل الحالة إلى لون).
```
Converters/StatusToColorConverter.cs
Converters/BooleanToVisibilityConverter.cs
...
```

### Behaviors/
- سلوكيات WPF مخصصة (مثلاً: Drag & Drop, Input Masking).
```
Behaviors/DragDropBehavior.cs
Behaviors/NumericInputBehavior.cs
```

### Templates/Styles/Themes/
- قوالب تحكم، أنماط، وثيمات (XAML) لتوحيد واجهة المستخدم.
```
Templates/CustomButtonTemplate.xaml
Styles/GlobalStyles.xaml
Themes/LightTheme.xaml
Themes/DarkTheme.xaml
```

### Resources/
- موارد عامة، صور، نصوص، قواميس موارد.
```
Resources/Images/
Resources/Icons/
Resources/Strings.ar.xaml
Resources/Strings.en.xaml
```

### Helpers/
- أدوات مساعدة (Utilities) مثل عمليات التنسيق، الحسابات البسيطة، إلخ.
```
Helpers/DateHelper.cs
Helpers/ValidationHelper.cs
Helpers/FileHelper.cs
```

### Extensions/
- دوال امتداد للكائنات المكررة الاستخدام.
```
Extensions/StringExtensions.cs
Extensions/DateExtensions.cs
```

### Localization/
- إدارة التعدد اللغوي للواجهة.
```
Localization/LocalizationManager.cs
Localization/ar.xaml
Localization/en.xaml
```

### Validation/
- منطق التحقق من صحة الإدخال والحقول.
```
Validation/EmailValidator.cs
Validation/InvoiceValidator.cs
...
```

### Assets/
- ملفات الصور والأيقونات والخطوط.
```
Assets/Images/logo.png
Assets/Icons/add.png
Assets/Fonts/CustomFont.ttf
```

---

### ملفات الجذر
- **App.xaml / App.xaml.cs**: نقطة البدء للواجهة وتسجيل الموارد.
- **MainWindow.xaml / MainWindow.xaml.cs**: النافذة الرئيسية (قد يتغير الاسم حسب التصميم).
- **App.config**: إعدادات الربط (API URLs، مفاتيح، إلخ).

---

## 💡 ملاحظات ختامية  
- كل View له ViewModel مطابق (MVVM).
- جميع استدعاءات البيانات تتم عبر Services (يفضل ApiService).
- كل موديول رئيسي (ERP) له مجلد في Views و ViewModels لتقسيم الشاشات والمنطق.
- يمكن إضافة أو تخصيص أي مجلدات حسب الحاجة أو التخصيصات (ثيمات، لغات، ...).
- جميع الطبقات الأخرى (Shared, Infrastructure.Shared, ...إلخ) يمكن استيرادها هنا عند الحاجة.

---

**إذا أردت مثال كود لأي نافذة أو خدمة أو ViewModel، أو شرح عملي لأي جزء من هيكلية WPF، أخبرني بذلك!**