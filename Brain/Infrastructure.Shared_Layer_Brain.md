# 🧠 الصورة الذهنية الكاملة (العميقة والمفصلة لأقصى مدى) لطبقة Infrastructure.Shared في مشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء طبقة Infrastructure.Shared**

- [جميع روابط مخططات المشروع ERP-Pro](https://github.com/ameenalqershi/erp_plan)
  - ملفات Shared Utilities, Helpers, Common Services
  - ملفات الأنواع والبنى المشتركة (DTOs, Constants, Enums, Models, Settings)
  - ملفات التهيئة والإعدادات العامة
  - ملفات Shared Abstractions

---

## 1. **مبادئ حاكمة عظمى لطبقة Infrastructure.Shared**

- هذه الطبقة تجمع كل ما هو مشترك وقابل لإعادة الاستخدام في أكثر من طبقة أو في أي جزء من البنية التحتية.
- يجب أن لا تحتوي أي منطق أعمال أو منطق خاص بموديول أو طبقة بعينها.
- يجب أن تكون كل خدمة/كلاس/Enum/Constant هنا مستقلة وقابلة للاستدعاء من أي جزء آخر.
- يجب ألا تعتمد على أي كود من طبقات أدنى (Domain, Application, Infrastructure)، بل فقط على .NET/Framework/Core.
- كل تحديث في هذه الطبقة يجب أن يراعي عدم كسر أي تكامل مع بقية الطبقات.

---

## 2. **هيكلية المجلدات والملفات بالتفصيل**

### Infrastructure.Shared Root
- **Helpers**
  - StringHelper, DateTimeHelper, MathHelper, ValidationHelper, ...  
    - دوال مساعدة عامة فقط
- **Extensions**
  - Extension Methods على الأنواع الأساسية (Collection, String, DateTime, ...)
- **Constants**
  - SystemConstants, ErrorCodes, RegexPatterns, ...  
    - أي ثابت يستخدم في أكثر من طبقة
- **Enums**
  - Shared Enumerations (Status, Gender, Language, ...)
- **Models**
  - Shared models (KeyValuePair, PagingInfo, ...)
- **Settings**
  - SharedSettings, AppConfig, FeatureFlags
- **Abstractions**
  - Interfaces/Contracts مشتركة

---

## 3. **دور كل ملف/مجلد (تفصيل شديد)**

- **Helpers/Extensions:**  
  - دوال/امتدادات متكررة ومُعممة فقط
- **Constants:**  
  - ثوابت مركزية تُستخدم في أكثر من طبقة
- **Enums:**  
  - تعدادات مشتركة (Status, Gender, ...)
- **Models:**  
  - نماذج بسيطة مشتركة (KeyValue, Paging, ...)
- **Settings:**  
  - إعدادات مشتركة (AppConfig, FeatureFlags)
- **Abstractions:**  
  - أي عقدة أو واجهة مشتركة بين عدة مشاريع

---

## 4. **خطوات التنفيذ خطوة بخطوة**

1. لكل خدمة/ثابت/Enum/Model متكرر، أنشئ ملفه هنا فقط إذا احتاجه أكثر من طبقة
2. لا تكتب أي منطق أعمال أو أي كود موديولي هنا
3. لا تضع أي شيء خاص بتقنية أو طبقة معينة
4. اختبر كل Helper/Extension منفصلا
5. لا تضع أي إعداد حساس، فقط إعدادات عامة

---

## 5. **تعليمات صارمة لاستخدام الصورة الذهنية**

- لا تكرر أي كود بين هذه الطبقة وباقي الطبقات
- لا تكتب أي منطق أعمال هنا
- لا تكتب أي كود مخصص بموديول
- لا تضع أي إعداد خاص أو حساس هنا

---

## 6. **روابط المخططات الأساسية لهذا التحليل (للمراجعة السريعة):**

- [هيكلية Shared](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Infrastructure.Shared.md)
- [ملفات Constants/Helpers/Settings](https://github.com/ameenalqershi/erp_plan/tree/main/Shared)

---

**هذه الصورة الذهنية مرجع نهائي لبناء طبقة Infrastructure.Shared في مشروع ERP-Pro، ولا يجوز الخروج عنها إلا بتحديث رسمي من المستخدم أو تعديل المخططات.**