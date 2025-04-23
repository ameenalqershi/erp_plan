# الجداول المتخصصة – المشاريع والعقود وغيرها

---

## جدول: projects (المشاريع)
| الحقل              | النوع            | الوصف                                 | قيود/علاقات                          |
|--------------------|------------------|---------------------------------------|--------------------------------------|
| project_id         | INT (PK)         | رقم المشروع                           | PRIMARY KEY, AUTO_INCREMENT          |
| project_code       | VARCHAR(30)      | كود المشروع                           | UNIQUE, NOT NULL                     |
| project_name_ar    | VARCHAR(200)     | اسم المشروع بالعربي                   | NOT NULL                             |
| project_name_en    | VARCHAR(200)     | اسم المشروع بالإنجليزي                |                                      |
| customer_id        | INT (FK)         | العميل/الجهة المالكة                  | FK→customers.customer_id             |
| manager_id         | INT (FK)         | مدير المشروع                          | FK→employees.employee_id             |
| status             | ENUM             | (جاري، منتهي، متوقف، ملغي)            |                                      |
| start_date         | DATE             | تاريخ البدء                            |                                      |
| end_date           | DATE             | تاريخ الانتهاء                         |                                      |
| budget             | DECIMAL(18,2)    | الميزانية                              |                                      |
| description        | TEXT             | وصف المشروع                            |                                      |
| branch_id          | INT (FK)         | الفرع                                 | FK→branches.branch_id                |
| parent_project_id  | INT (FK)         | المشروع الأعلى                        | FK→projects.project_id               |
| cost_center_id     | INT (FK)         | مركز التكلفة                          | FK→cost_centers.cost_center_id       |
| notes              | TEXT             | ملاحظات                               |                                      |
| created_by         | INT (FK)         | من أنشأ                               | FK→users.user_id                     |
| created_at         | DATETIME         | تاريخ الإنشاء                         |                                      |
| updated_by         | INT (FK)         | من عدّل                               | FK→users.user_id                     |
| updated_at         | DATETIME         | تاريخ التعديل الأخير                  |                                      |

---

## جدول: project_tasks (مهام المشروع)
| الحقل              | النوع            | الوصف                                 | قيود/علاقات                          |
|--------------------|------------------|---------------------------------------|--------------------------------------|
| task_id            | INT (PK)         | رقم المهمة                            | PRIMARY KEY, AUTO_INCREMENT          |
| project_id         | INT (FK)         | رقم المشروع                           | FK→projects.project_id               |
| task_code          | VARCHAR(30)      | كود المهمة                             | UNIQUE, NOT NULL                     |
| task_name_ar       | VARCHAR(200)     | اسم المهمة بالعربي                     | NOT NULL                             |
| task_name_en       | VARCHAR(200)     | اسم المهمة بالإنجليزي                  |                                      |
| assigned_to        | INT (FK)         | الموظف/الفريق المسؤول                 | FK→employees.employee_id             |
| start_date         | DATE             | تاريخ البدء                            |                                      |
| end_date           | DATE             | تاريخ الانتهاء                         |                                      |
| planned_hours      | DECIMAL(10,2)    | الساعات المخططة                        |                                      |
| actual_hours       | DECIMAL(10,2)    | الساعات الفعلية                        |                                      |
| status             | ENUM             | (جاري، مكتمل، متوقف، ملغي)            |                                      |
| parent_task_id     | INT (FK)         | المهمة الأعلى                          | FK→project_tasks.task_id             |
| priority           | ENUM             | (عالية، متوسطة، منخفضة)               |                                      |
| progress           | DECIMAL(5,2)     | نسبة الإنجاز (%)                       |                                      |
| notes              | TEXT             | ملاحظات                               |                                      |

---

## جدول: contracts (العقود)
| الحقل              | النوع            | الوصف                                 | قيود/علاقات                          |
|--------------------|------------------|---------------------------------------|--------------------------------------|
| contract_id        | INT (PK)         | رقم العقد                             | PRIMARY KEY, AUTO_INCREMENT          |
| contract_code      | VARCHAR(30)      | كود العقد                             | UNIQUE, NOT NULL                     |
| contract_type      | ENUM             | (توريد، تنفيذ، صيانة، استشارات ...)  |                                      |
| subject            | VARCHAR(200)     | موضوع العقد                           |                                      |
| customer_id        | INT (FK)         | العميل/الجهة المتعاقدة                | FK→customers.customer_id             |
| supplier_id        | INT (FK)         | المورد/الطرف الثاني                   | FK→suppliers.supplier_id             |
| project_id         | INT (FK)         | المشروع التابع له العقد                | FK→projects.project_id               |
| start_date         | DATE             | تاريخ بداية العقد                     |                                      |
| end_date           | DATE             | تاريخ نهاية العقد                     |                                      |
| total_value        | DECIMAL(18,2)    | قيمة العقد                            |                                      |
| currency_id        | INT (FK)         | العملة                                | FK→currencies.currency_id            |
| status             | ENUM             | (ساري، مكتمل، ملغي، موقوف)           |                                      |
| signed_date        | DATE             | تاريخ التوقيع                         |                                      |
| attachment_id      | INT (FK)         | مرفق العقد                            | FK→attachments.attachment_id         |
| notes              | TEXT             | ملاحظات                               |                                      |
| created_by         | INT (FK)         | من أنشأ                               | FK→users.user_id                     |
| created_at         | DATETIME         | تاريخ الإنشاء                         |                                      |

---

## جدول: contract_amendments (ملحقات وتعديلات العقود)
| الحقل                | النوع            | الوصف                                 | قيود/علاقات                          |
|----------------------|------------------|---------------------------------------|--------------------------------------|
| amendment_id         | INT (PK)         | رقم الملحق/التعديل                    | PRIMARY KEY, AUTO_INCREMENT          |
| contract_id          | INT (FK)         | رقم العقد                              | FK→contracts.contract_id             |
| amendment_type       | ENUM             | (تمديد، زيادة قيمة، أخرى ...)         |                                      |
| description          | TEXT             | وصف التعديل                            |                                      |
| value_change         | DECIMAL(18,2)    | قيمة الزيادة/النقصان                  |                                      |
| new_end_date         | DATE             | تاريخ نهاية جديد (إن وجد)              |                                      |
| signed_date          | DATE             | تاريخ التوقيع                          |                                      |
| attachment_id        | INT (FK)         | مرفق الملحق                            | FK→attachments.attachment_id         |
| notes                | TEXT             | ملاحظات                                |                                      |
| created_by           | INT (FK)         | من أنشأ                                | FK→users.user_id                     |
| created_at           | DATETIME         | تاريخ الإنشاء                          |                                      |

---

## جدول: cost_centers (مراكز التكلفة)
| الحقل           | النوع            | الوصف                                 | قيود/علاقات                          |
|-----------------|------------------|---------------------------------------|--------------------------------------|
| cost_center_id  | INT (PK)         | رقم المركز                            | PRIMARY KEY, AUTO_INCREMENT          |
| center_code     | VARCHAR(30)      | كود المركز                            | UNIQUE, NOT NULL                     |
| name_ar         | VARCHAR(100)     | الاسم بالعربي                         | NOT NULL                             |
| name_en         | VARCHAR(100)     | الاسم بالإنجليزي                      |                                      |
| parent_id       | INT (FK)         | المركز الأعلى                         | FK→cost_centers.cost_center_id       |
| branch_id       | INT (FK)         | الفرع                                 | FK→branches.branch_id                |
| is_active       | BOOLEAN          | مفعل/غير مفعل                         |                                      |
| notes           | TEXT             | ملاحظات                               |                                      |

---

## جدول: milestones (مراحل الإنجاز للمشاريع/العقود)
| الحقل            | النوع            | الوصف                                 | قيود/علاقات                          |
|------------------|------------------|---------------------------------------|--------------------------------------|
| milestone_id     | INT (PK)         | رقم المرحلة                           | PRIMARY KEY, AUTO_INCREMENT          |
| project_id       | INT (FK)         | رقم المشروع                           | FK→projects.project_id               |
| contract_id      | INT (FK)         | رقم العقد (إن وجد)                    | FK→contracts.contract_id             |
| milestone_name   | VARCHAR(200)     | اسم المرحلة                           |                                      |
| start_date       | DATE             | بداية المرحلة                         |                                      |
| end_date         | DATE             | نهاية المرحلة                         |                                      |
| planned_value    | DECIMAL(18,2)    | القيمة المخططة                        |                                      |
| actual_value     | DECIMAL(18,2)    | القيمة الفعلية                        |                                      |
| status           | ENUM             | (جاري، مكتمل، ملغي)                   |                                      |
| completion_pct   | DECIMAL(5,2)     | نسبة الإنجاز (%)                       |                                      |
| notes            | TEXT             | ملاحظات                               |                                      |

---

## العلاقات الرئيسية في الجداول المتخصصة

- **projects.customer_id → customers.customer_id**
- **projects.manager_id → employees.employee_id**
- **projects.parent_project_id → projects.project_id**
- **projects.cost_center_id → cost_centers.cost_center_id**
- **projects.branch_id → branches.branch_id**
- **projects.created_by/updated_by → users.user_id**
- **project_tasks.project_id → projects.project_id**
- **project_tasks.parent_task_id → project_tasks.task_id**
- **project_tasks.assigned_to → employees.employee_id**
- **contracts.customer_id → customers.customer_id**
- **contracts.supplier_id → suppliers.supplier_id**
- **contracts.project_id → projects.project_id**
- **contracts.currency_id → currencies.currency_id**
- **contracts.attachment_id → attachments.attachment_id**
- **contracts.created_by → users.user_id**
- **contract_amendments.contract_id → contracts.contract_id**
- **contract_amendments.attachment_id → attachments.attachment_id**
- **contract_amendments.created_by → users.user_id**
- **cost_centers.parent_id → cost_centers.cost_center_id**
- **cost_centers.branch_id → branches.branch_id**
- **milestones.project_id → projects.project_id**
- **milestones.contract_id → contracts.contract_id**

---

## ملاحظات عملية

- تدعم الجداول دورة حياة المشاريع الكاملة (بيانات، مهام، موازنات، مراحل، مسؤولية، ربط مع العقود والعميل والتكلفة والفرع).
- العقود تدعم كل الأنواع، والربط بالمشاريع والعملاء والموردين والعملات والملاحق والمرفقات.
- ملاحق العقود تدعم التعديلات المهيكلة والتوثيق الكامل.
- المهام والمراحل تدعم التتبع الزمني والإنجاز، والربط بالفريق أو الموظفين وتدفق العمل.
- مراكز التكلفة تدعم هيكلة متعددة المستويات، الربط بالمشاريع، الفروع، التقارير التحليلية.
- جميع الجداول تدعم الربط مع السجلات المالية، المستندات، الإشعارات، التدقيق، المرفقات، الموافقات.

---