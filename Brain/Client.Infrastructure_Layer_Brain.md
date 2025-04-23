# 🧠 الصورة الذهنية الكاملة (العميقة والمفصلة لأقصى مدى) لطبقة Client.Infrastructure في مشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء طبقة Client.Infrastructure**

- [جميع روابط مخططات المشروع ERP-Pro](https://github.com/ameenalqershi/erp_plan)
  - ملفات خدمات البنية التحتية الخاصة بالعميل (API, Auth, Storage, Cache, ...)
  - ملفات Settings, Config, Shared Abstractions
  - ملفات الربط مع السيرفر (API, SignalR, WebSocket, ...)
  - ملفات الـInterceptors, Http, ErrorHandling

---

## 1. **مبادئ حاكمة عظمى لطبقة Client.Infrastructure**

- هذه الطبقة تنفذ كل ما هو خاص ببنية العميل التقنية (API, Auth, LocalStorage, Cache, ...)
- يجب ألا تحتوي أي منطق أعمال أو عرض أو تفاعل مع المستخدم مباشرة.
- يجب أن يكون كل Service قابل للاختبار والتبديل (Testable, Swappable)
- يجب أن تعتمد فقط على الـInterfaces/Abstractions المشتركة مع Client وShared
- كل إعداد يجب أن يكون في Config/Settings

---

## 2. **هيكلية المجلدات والملفات بالتفصيل**

### Client.Infrastructure Root
- **API**
  - ApiService (Base), HttpClient, Interceptors, ErrorHandler
  - Contracts (OpenAPI/Swagger types)
- **Auth**
  - AuthService, TokenService, JWT, OAuth, ...
- **Storage**
  - LocalStorageService, SessionStorageService, CookieService
- **Cache**
  - MemoryCacheService, IndexedDBService
- **Config**
  - AppConfig, Environment, Settings
- **DI**
  - RegisterClientInfrastructureServices
- **Abstractions**
  - Interfaces/Contracts مشتركة مع Client/Shared

---

## 3. **دور كل ملف/مجلد (تفصيل شديد)**

- **API:**  
  - كل تنفيذ للاتصال بالـAPI (GET/POST/PUT/DELETE)
  - Interceptors للأخطاء والتوكن والتوقيت
- **Auth:**  
  - كل ما يخص توثيق المستخدم (JWT, OAuth, Refresh, ...)
- **Storage:**  
  - تخزين بيانات المستخدم/التوكن محليا
- **Cache:**  
  - إدارة الكاش محليا (Memory/IndexedDB)
- **Config:**  
  - كل إعداد خاص بالعميل
- **DI:**  
  - تسجيل الخدمات
- **Abstractions:**  
  - أي عقد مشترك

---

## 4. **خطوات التنفيذ خطوة بخطوة**

1. لكل خدمة (API/Auth/Storage/Cache)، أنشئ Interface في Abstractions
2. نفذ كل خدمة هنا فقط
3. سجل كل خدمة في DI
4. لا تكتب أي منطق أعمال أو تفاعل مع المستخدم هنا
5. اختبر كل خدمة منفصلة

---

## 5. **تعليمات صارمة لاستخدام الصورة الذهنية**

- لا تكتب أي منطق أعمال هنا
- لا تكتب أي تفاعل واجهة مستخدم
- لا تكرر الكود بين هذه الطبقة وغيرها
- لا تضع أي إعداد خاص أو حساس في الكود

---

## 6. **روابط المخططات الأساسية لهذا التحليل (للمراجعة السريعة):**

- [هيكلية Client.Infrastructure](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Client.Infrastructure.md)
- [ملفات API/Auth/Config](https://github.com/ameenalqershi/erp_plan/tree/main/Client.Infrastructure)

---

**هذه الصورة الذهنية مرجع نهائي لبناء طبقة Client.Infrastructure في مشروع ERP-Pro، ولا يجوز الخروج عنها إلا بتحديث رسمي من المستخدم أو تعديل المخططات.**