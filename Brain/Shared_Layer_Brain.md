# 🧠 الصورة الذهنية الكاملة (العميقة والمفصلة لأقصى مدى) لطبقة Shared في مشروع ERP-Pro

---

## 0. **المصدر الأساسي والنهائي لبناء طبقة Shared**

- [جميع روابط مخططات المشروع ERP-Pro](https://github.com/ameenalqershi/erp_plan)
  - ملفات Shared Models, Common Enums, Constants, Localization Resources, ...
  - ملفات الـDTOs المشتركة بين أكثر من طبقة
  - ملفات الـLocalization والـResources
  - ملفات الـGlobal Types, Settings, Configurations

---

## 1. **مبادئ حاكمة عظمى لطبقة Shared**

- طبقة Shared تجمع كل ما هو مشترك بين أكثر من طبقة (Domain, Application, Client, ...)
- يجب أن لا تحتوي أي منطق أعمال أو منطق يخص موديول معين أو طبقة بعينها.
- يجب أن تكون مثالية لأي مشاركة (Cross-Layer/Cross-Project)
- كل تحديث يجب أن يراعي أثره على جميع الطبقات التي تعتمد على Shared.

---

## 2. **هيكلية المجلدات والملفات بالتفصيل**

### Shared Root
- **Models**
  - Shared DTOs/Models (UserInfo, LookupDto, StatusDto, ...)
- **Enums**
  - Shared Enumerations (Status, Locale, ...)
- **Constants**
  - Global Constants (AppFeatures, Modules, ErrorCodes, ...)
- **Localization**
  - Resources/ar.json, Resources/en.json, ...  
    - كل نصوص التوطين
- **Settings**
  - SharedSettings, GlobalConfig, ...
- **Extensions**
  - Methods مفيدة لكل الطبقات
- **Abstractions**
  - Shared Interfaces/Contracts

---

## 3. **دور كل ملف/مجلد (تفصيل شديد)**

- **Models:**  
  - نماذج بيانات مشتركة بين الطبقات (مثال: UserInfo)
- **Enums:**  
  - كل تعداد مشترك (مثال: StatusEnum)
- **Constants:**  
  - ثوابت عامة (مثال: AppFeatures)
- **Localization:**  
  - نصوص متعددة اللغات
- **Settings:**  
  - إعدادات عامة مشتركة
- **Extensions:**  
  - دوال عامة
- **Abstractions:**  
  - واجهات مشتركة

---

## 4. **خطوات التنفيذ خطوة بخطوة**

1. لكل نوع/ثابت/تعداد/نموذج مشترك بين أكثر من طبقة ضعه هنا فقط
2. لا تكتب منطق أعمال أو كود خاص بأي موديول
3. لا تضع أي إعداد خاص أو حساس
4. اختبر كل Model/Enum/Constant منفصلا

---

## 5. **تعليمات صارمة لاستخدام الصورة الذهنية**

- لا تكرر أي كود بين هذه الطبقة وباقي الطبقات
- لا تكتب أي منطق أعمال هنا
- لا تضع أي إعداد خاص أو حساس هنا

---

## 6. **روابط المخططات الأساسية لهذا التحليل (للمراجعة السريعة):**

- [هيكلية Shared](https://github.com/ameenalqershi/erp_plan/blob/main/Structure/LayerStructure_Shared.md)
- [ملفات Localization/DTOs](https://github.com/ameenalqershi/erp_plan/tree/main/Shared)

---

**هذه الصورة الذهنية مرجع نهائي لبناء طبقة Shared في مشروع ERP-Pro، ولا يجوز الخروج عنها إلا بتحديث رسمي من المستخدم أو تعديل المخططات.**