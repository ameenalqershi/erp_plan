# 🧠 الصورة الذهنية الكاملة (العميقة والمفصلة لأقصى مدى) لطبقة Server في مشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء طبقة Server**

- [جميع روابط مخططات المشروع ERP-Pro](https://github.com/ameenalqershi/erp_plan)
  - ملفات إعداد الخوادم (Hosting, Deployment, CI/CD, Docker, K8s, ...)
  - ملفات الـAPI Controllers, Middlewares, Routing, Exception Handling
  - ملفات الـSwagger/OpenAPI
  - ملفات الأمن (Security, AuthN/AuthZ, RateLimit, Cors)
  - ملفات إعدادات البيئة (Environment, Config)

---

## 1. **مبادئ حاكمة عظمى لطبقة Server**

- طبقة Server هي واجهة التوزيع (API Gateway, Web API, SignalR, ...)
- يجب أن تكون كل Controller أو Endpoint منفصل طبقًا للـUseCase
- يجب أن تعتمد فقط على Application Layer (Services, DTOs, ...)
- يجب ألا تحتوي أي منطق أعمال أو دوميني في الكود
- يجب أن تدير كل الأخطاء عبر Middlewares/ExceptionHandlers
- يجب أن تدير الأمن والصلاحيات بدقة (AuthN/AuthZ, Policy, Claims)
- يجب أن تدير كل إعداد من Config/Environment

---

## 2. **هيكلية المجلدات والملفات بالتفصيل**

### Server Root
- **Controllers**
  - لكل UseCase Controller/Endpoint مستقل
- **Middlewares**
  - ExceptionHandler, Logging, Validation, Security, RateLimit, ...
- **Routing**
  - API Routes/Maps
- **Swagger**
  - OpenAPI/Swagger Setup
- **Security**
  - AuthN/AuthZ, Policies, Claims, Cors, RateLimit
- **Config**
  - Environment, AppSettings, ...
- **DI**
  - RegisterServerServices
- **Tests**
  - Integration/E2E Tests

---

## 3. **دور كل ملف/مجلد (تفصيل شديد)**

- **Controllers:**  
  - يستقبل الطلبات من العميل، يحولها لـDTOs/Commands/Queries، يستدعي Application Layer، يعيد النتائج
- **Middlewares:**  
  - تدير الأخطاء، التحقق، التوثيق، تسجيل الدخول، معدل الطلبات
- **Routing:**  
  - إدارة مسارات الـAPI
- **Swagger:**  
  - توثيق الـAPI وإتاحة الفحص
- **Security:**  
  - إدارة المصادقة والتفويض والسياسات
- **Config:**  
  - إعدادات البيئة والاتصال والسرية
- **DI:**  
  - تسجيل الخدمات

---

## 4. **خطوات التنفيذ خطوة بخطوة**

1. لكل UseCase، أنشئ Controller/Endpoint مستقل
2. مرر كل الطلبات للـApplication Layer فقط
3. أدخل كل إعداد في Config/Environment
4. فعّل جميع Middlewares (Exception, Logging, Security, ...)
5. فعّل Swagger/OpenAPI
6. لا تكتب أي منطق أعمال هنا
7. اختبر كل Endpoint/Controller منفصلا

---

## 5. **تعليمات صارمة لاستخدام الصورة الذهنية**

- لا تكتب أي منطق أعمال هنا
- لا تكتب أي كود دوميني هنا
- لا تخرج عن الطبقات بالتبعية
- اختبر كل Endpoint/Controller منفصلا

---

## 6. **روابط المخططات الأساسية لهذا التحليل (للمراجعة السريعة):**

- [هيكلية Server](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Server.md)
- [ملفات Controllers/Middlewares](https://github.com/ameenalqershi/erp_plan/tree/main/Server)
- [ملفات Swagger](https://github.com/ameenalqershi/erp_plan/tree/main/Server/Swagger)

---

**هذه الصورة الذهنية مرجع نهائي لبناء طبقة Server في مشروع ERP-Pro، ولا يجوز الخروج عنها إلا بتحديث رسمي من المستخدم أو تعديل المخططات.**