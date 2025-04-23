# قسم المالية (المحاسبة العامة) – تحليل جداول قاعدة البيانات

---

## جدول: accounts (شجرة الحسابات)
| الحقل                | النوع            | الوصف                                                          | قيود/علاقات/ملاحظات                                   |
|----------------------|------------------|----------------------------------------------------------------|-------------------------------------------------------|
| account_id           | INT (PK)         | رقم تسلسلي للحساب                                              | PRIMARY KEY, AUTO_INCREMENT                           |
| account_code         | VARCHAR(30)      | كود الحساب المالي                                              | UNIQUE, NOT NULL                                      |
| account_name_ar      | VARCHAR(200)     | اسم الحساب بالعربي                                             | NOT NULL                                              |
| account_name_en      | VARCHAR(200)     | اسم الحساب بالإنجليزي                                          |                                                       |
| parent_account_id    | INT (FK)         | الحساب الأب (شجرة متفرعة)                                      | FK→accounts.account_id, NULL=حساب رئيسي               |
| group_id             | INT (FK)         | تصنيف الحساب (أصل، التزام...)                                  | FK→account_groups.group_id                            |
| is_leaf              | BOOLEAN          | هل هو حساب تفصيلي (لا يقبل أبناء)                              |                                                       |
| account_type         | ENUM             | نوع الحساب (رئيسي/فرعي/تفصيلي/مخصص)                           |                                                       |
| nature               | ENUM             | طبيعة الحساب (مدين/دائن/كلاهما)                               |                                                       |
| level                | INT              | مستوى العمق في الشجرة                                          |                                                       |
| currency_id          | INT (FK)         | العملة الافتراضية للحساب                                       | FK→currencies.currency_id                             |
| cost_center_id       | INT (FK)         | مركز التكلفة الافتراضي                                         | FK→cost_centers.cost_center_id                        |
| branch_id            | INT (FK)         | الفرع الافتراضي                                                | FK→branches.branch_id                                 |
| opening_balance      | DECIMAL(18,2)    | الرصيد الافتتاحي                                               |                                                       |
| opening_balance_type | ENUM             | نوع الرصيد (مدين/دائن)                                         |                                                       |
| allow_manual_entry   | BOOLEAN          | هل يقبل إدخال يدوي في القيود                                    |                                                       |
| is_control_account   | BOOLEAN          | حساب تحكم (يستخدم للربط مع الأنظمة الفرعية)                   |                                                       |
| is_active            | BOOLEAN          | مفعل/غير مفعل                                                  |                                                       |
| notes                | TEXT             | ملاحظات إضافية                                                 |                                                       |
| created_by           | INT (FK)         | المستخدم المُنشئ                                                | FK→users.user_id                                      |
| created_at           | DATETIME         | تاريخ الإنشاء                                                   |                                                       |
| updated_by           | INT (FK)         | المستخدم المعدّل                                                | FK→users.user_id                                      |
| updated_at           | DATETIME         | تاريخ آخر تعديل                                                 |                                                       |

---

## جدول: account_groups (تصنيفات الحسابات)
| الحقل           | النوع            | الوصف                              | قيود/علاقات               |
|-----------------|------------------|------------------------------------|---------------------------|
| group_id        | INT (PK)         | رقم تسلسلي                         | PRIMARY KEY               |
| group_code      | VARCHAR(20)      | كود التصنيف                        | UNIQUE, NOT NULL          |
| group_name_ar   | VARCHAR(100)     | اسم التصنيف بالعربي                |                           |
| group_name_en   | VARCHAR(100)     | اسم التصنيف بالإنجليزي             |                           |
| parent_id       | INT (FK)         | التصنيف الأعلى                     | FK→account_groups.group_id|
| type            | ENUM             | نوع (أصول - التزامات - إيرادات ...) |                           |
| level           | INT              | المستوى في الشجرة                   |                           |
| is_active       | BOOLEAN          | مفعل/غير مفعل                      |                           |

---

## جدول: journal_entries (القيود اليومية)
| الحقل           | النوع            | الوصف                            | قيود/علاقات                       |
|-----------------|------------------|----------------------------------|-----------------------------------|
| entry_id        | INT (PK)         | رقم القيد                        | PRIMARY KEY, AUTO_INCREMENT       |
| entry_number    | VARCHAR(30)      | رقم القيد (تسلسلي/يدوي)          | UNIQUE, NOT NULL                  |
| entry_date      | DATE             | تاريخ القيد                      | NOT NULL                          |
| entry_type      | ENUM             | نوع القيد (عادي، تسوية، ترحيل، آلي، إقفال...) |                                  |
| description     | TEXT             | وصف عام للقيد                    |                                   |
| reference_type  | VARCHAR(50)      | نوع المرجع (فاتورة، سند، نظام خارجي...) |                                 |
| reference_id    | VARCHAR(50)      | رقم المرجع                       |                                   |
| status          | ENUM             | (مسودة، معتمد، مرحّل، مقفل، ملغي) |                                   |
| period_id       | INT (FK)         | الفترة المالية                   | FK→fiscal_periods.period_id       |
| created_by      | INT (FK)         | المستخدم المنشئ                   | FK→users.user_id                  |
| created_at      | DATETIME         | تاريخ الإنشاء                     |                                   |
| posted_by       | INT (FK)         | المستخدم المرحّل                  | FK→users.user_id                  |
| posted_at       | DATETIME         | تاريخ الترحيل                     |                                   |
| approved_by     | INT (FK)         | المستخدم الموافق                  | FK→users.user_id                  |
| approved_at     | DATETIME         | تاريخ الموافقة                    |                                   |
| branch_id       | INT (FK)         | رقم الفرع                         | FK→branches.branch_id             |
| notes           | TEXT             | ملاحظات إدارية                   |                                   |
| attachment_id   | INT (FK)         | مرفق القيد (مستند/صورة)           | FK→attachments.attachment_id      |

---

## جدول: journal_entry_lines (تفاصيل القيد)
| الحقل              | النوع            | الوصف                            | قيود/علاقات                     |
|--------------------|------------------|----------------------------------|---------------------------------|
| line_id            | INT (PK)         | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT     |
| entry_id           | INT (FK)         | رقم القيد                        | FK→journal_entries.entry_id     |
| line_number        | INT              | رقم السطر داخل القيد             |                                 |
| account_id         | INT (FK)         | رقم الحساب                       | FK→accounts.account_id          |
| description        | VARCHAR(300)     | وصف السطر                        |                                 |
| cost_center_id     | INT (FK)         | مركز التكلفة                     | FK→cost_centers.cost_center_id  |
| project_id         | INT (FK)         | المشروع                          | FK→projects.project_id          |
| department_id      | INT (FK)         | قسم إداري (اختياري)              | FK→departments.department_id    |
| branch_id          | INT (FK)         | رقم الفرع                         | FK→branches.branch_id           |
| debit              | DECIMAL(18,2)    | مبلغ مدين                        |                                 |
| credit             | DECIMAL(18,2)    | مبلغ دائن                        |                                 |
| currency_id        | INT (FK)         | العملة                            | FK→currencies.currency_id       |
| rate               | DECIMAL(10,4)    | سعر الصرف (إذا عملة أجنبية)      |                                 |
| reference_type     | VARCHAR(50)      | نوع المرجع (فاتورة، سند...)      |                                 |
| reference_id       | VARCHAR(50)      | رقم المرجع                       |                                 |
| is_auto            | BOOLEAN          | هل السطر آلي أم يدوي              |                                 |

---

## جدول: cost_centers (مراكز التكلفة)
| الحقل               | النوع            | الوصف                            | قيود/علاقات                     |
|---------------------|------------------|----------------------------------|---------------------------------|
| cost_center_id      | INT (PK)         | رقم المركز                       | PRIMARY KEY                     |
| cost_center_code    | VARCHAR(20)      | كود المركز                       | UNIQUE, NOT NULL                |
| name_ar             | VARCHAR(100)     | اسم المركز بالعربي               |                                 |
| name_en             | VARCHAR(100)     | اسم المركز بالإنجليزي            |                                 |
| type                | ENUM             | نوع (مشروع/قسم/وحدة/إجمالي)     |                                 |
| parent_id           | INT (FK)         | المركز الأعلى                    | FK→cost_centers.cost_center_id  |
| level               | INT              | المستوى في الشجرة                 |                                 |
| branch_id           | INT (FK)         | الفرع                            | FK→branches.branch_id           |
| is_active           | BOOLEAN          | مفعل/غير مفعل                    |                                 |
| notes               | TEXT             | ملاحظات                          |                                 |

---

## جدول: fiscal_periods (الفترات المالية)
| الحقل           | النوع            | الوصف                            | قيود/علاقات                     |
|-----------------|------------------|----------------------------------|---------------------------------|
| period_id       | INT (PK)         | رقم الفترة                       | PRIMARY KEY                     |
| year            | INT              | السنة المالية                     | NOT NULL                        |
| month           | INT              | رقم الشهر (1-12)                  |                                 |
| quarter         | INT              | ربع السنة (1-4)                   |                                 |
| status          | ENUM             | مفتوحة، جارية، مقفلة             |                                 |
| start_date      | DATE             | بداية الفترة                      |                                 |
| end_date        | DATE             | نهاية الفترة                      |                                 |
| closed_by       | INT (FK)         | من أغلق الفترة                    | FK→users.user_id                |
| closed_at       | DATETIME         | تاريخ الإغلاق                      |                                 |
| notes           | TEXT             | ملاحظات                          |                                 |

---

## جدول: currencies (العملات)
| الحقل           | النوع            | الوصف                            | قيود/علاقات                     |
|-----------------|------------------|----------------------------------|---------------------------------|
| currency_id     | INT (PK)         | رقم العملة                       | PRIMARY KEY                     |
| code            | VARCHAR(10)      | كود العملة (YER, SAR, USD...)    | UNIQUE, NOT NULL                |
| name_ar         | VARCHAR(50)      | اسم العملة بالعربي                |                                 |
| name_en         | VARCHAR(50)      | اسم العملة بالإنجليزي             |                                 |
| symbol          | VARCHAR(5)       | رمز العملة                        |                                 |
| decimal_places  | INT              | عدد الأرقام العشرية               |                                 |
| is_default      | BOOLEAN          | افتراضية/غير افتراضية            |                                 |
| is_active       | BOOLEAN          | مفعل/غير مفعل                    |                                 |

---

## جدول: exchange_rates (أسعار الصرف)
| الحقل           | النوع            | الوصف                            | قيود/علاقات                     |
|-----------------|------------------|----------------------------------|---------------------------------|
| rate_id         | INT (PK)         | رقم السجل                        | PRIMARY KEY                     |
| currency_id     | INT (FK)         | رقم العملة                       | FK→currencies.currency_id       |
| rate_date       | DATE             | تاريخ السعر                       |                                 |
| rate            | DECIMAL(18,6)    | سعر الصرف                        |                                 |

---

## جدول: bank_accounts (الحسابات البنكية)
| الحقل               | النوع            | الوصف                        | قيود/علاقات                      |
|---------------------|------------------|------------------------------|----------------------------------|
| bank_account_id     | INT (PK)         | رقم الحساب البنكي            | PRIMARY KEY                      |
| bank_name           | VARCHAR(100)     | اسم البنك                    |                                  |
| branch_name         | VARCHAR(100)     | اسم الفرع البنكي             |                                  |
| account_number      | VARCHAR(50)      | رقم الحساب                   | UNIQUE                           |
| iban                | VARCHAR(50)      | رقم IBAN                     | UNIQUE, NULLABLE                 |
| swift_code          | VARCHAR(20)      | كود سويفت                    |                                  |
| currency_id         | INT (FK)         | العملة                        | FK→currencies.currency_id        |
| opening_balance     | DECIMAL(18,2)    | الرصيد الافتتاحي             |                                  |
| status              | ENUM             | نشط/مغلق/مجمّد               |                                  |
| branch_id           | INT (FK)         | رقم الفرع                     | FK→branches.branch_id            |
| notes               | TEXT             | ملاحظات                      |                                  |
| created_by          | INT (FK)         | المستخدم المُنشئ               | FK→users.user_id                 |
| created_at          | DATETIME         | تاريخ الإنشاء                 |                                  |

---

## جدول: cash_boxes (الصناديق النقدية)
| الحقل           | النوع            | الوصف                            | قيود/علاقات                     |
|-----------------|------------------|----------------------------------|---------------------------------|
| cash_box_id     | INT (PK)         | رقم الصندوق                      | PRIMARY KEY                     |
| name_ar         | VARCHAR(100)     | اسم الصندوق بالعربي              |                                 |
| name_en         | VARCHAR(100)     | اسم الصندوق بالإنجليزي           |                                 |
| currency_id     | INT (FK)         | العملة                           | FK→currencies.currency_id       |
| branch_id       | INT (FK)         | رقم الفرع                        | FK→branches.branch_id           |
| opening_balance | DECIMAL(18,2)    | رصيد افتتاحي                     |                                 |
| is_active       | BOOLEAN          | مفعل/غير مفعل                    |                                 |
| notes           | TEXT             | ملاحظات                          |                                 |

---

## جدول: bank_statements (كشوفات البنك)
| الحقل           | النوع            | الوصف                            | قيود/علاقات                     |
|-----------------|------------------|----------------------------------|---------------------------------|
| statement_id    | INT (PK)         | رقم الكشف                        | PRIMARY KEY                     |
| bank_account_id | INT (FK)         | رقم الحساب البنكي                | FK→bank_accounts.bank_account_id|
| period_id       | INT (FK)         | الفترة المالية                   | FK→fiscal_periods.period_id     |
| start_balance   | DECIMAL(18,2)    | رصيد بداية الفترة                |                                 |
| end_balance     | DECIMAL(18,2)    | رصيد نهاية الفترة                |                                 |
| statement_date  | DATE             | تاريخ الكشف                      |                                 |
| uploaded_file   | VARCHAR(255)     | اسم ملف الكشف (PDF/Excel)        |                                 |
| notes           | TEXT             | ملاحظات                          |                                 |

---

## جدول: bank_statement_lines (تفاصيل كشف البنك)
| الحقل              | النوع            | الوصف                            | قيود/علاقات                   |
|--------------------|------------------|----------------------------------|-------------------------------|
| line_id            | INT (PK)         | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT   |
| statement_id       | INT (FK)         | رقم كشف البنك                    | FK→bank_statements.statement_id|
| txn_date           | DATE             | تاريخ الحركة                      |                               |
| description        | VARCHAR(300)     | وصف الحركة                        |                               |
| reference_number   | VARCHAR(100)     | رقم المرجع (شيك/سويفت/حوالة...)  |                               |
| debit              | DECIMAL(18,2)    | مبلغ مدين                        |                               |
| credit             | DECIMAL(18,2)    | مبلغ دائن                        |                               |
| balance            | DECIMAL(18,2)    | الرصيد بعد الحركة                 |                               |
| matched_entry_id   | INT (FK)         | القيد المالي المطابق              | FK→journal_entries.entry_id   |

---

## جدول: financial_settings (إعدادات محاسبية عامة)
| الحقل           | النوع            | الوصف                            | قيود/علاقات                     |
|-----------------|------------------|----------------------------------|---------------------------------|
| setting_id      | INT (PK)         | رقم السجل                        | PRIMARY KEY                     |
| setting_key     | VARCHAR(100)     | اسم الإعداد                      | UNIQUE                          |
| setting_value   | VARCHAR(500)     | قيمة الإعداد                     |                                 |
| description     | TEXT             | وصف الإعداد                      |                                 |
| updated_by      | INT (FK)         | من قام بالتعديل                   | FK→users.user_id                |
| updated_at      | DATETIME         | تاريخ آخر تعديل                   |                                 |

---

## العلاقات الرئيسية في قسم المالية

- **accounts.parent_account_id → accounts.account_id** (شجرة متفرعة)
- **accounts.group_id → account_groups.group_id**
- **accounts.currency_id → currencies.currency_id**
- **accounts.branch_id → branches.branch_id**
- **accounts.cost_center_id → cost_centers.cost_center_id**
- **journal_entries.period_id → fiscal_periods.period_id**
- **journal_entries.created_by/posted_by/approved_by → users.user_id**
- **journal_entries.branch_id → branches.branch_id**
- **journal_entries.attachment_id → attachments.attachment_id**
- **journal_entry_lines.entry_id → journal_entries.entry_id**
- **journal_entry_lines.account_id → accounts.account_id**
- **journal_entry_lines.cost_center_id → cost_centers.cost_center_id**
- **journal_entry_lines.project_id → projects.project_id**
- **journal_entry_lines.department_id → departments.department_id**
- **journal_entry_lines.branch_id → branches.branch_id**
- **journal_entry_lines.currency_id → currencies.currency_id**
- **cost_centers.parent_id → cost_centers.cost_center_id**
- **cost_centers.branch_id → branches.branch_id**
- **bank_accounts.currency_id → currencies.currency_id**
- **bank_accounts.branch_id → branches.branch_id**
- **bank_statements.bank_account_id → bank_accounts.bank_account_id**
- **bank_statements.period_id → fiscal_periods.period_id**
- **bank_statement_lines.statement_id → bank_statements.statement_id**
- **bank_statement_lines.matched_entry_id → journal_entries.entry_id**

---

## ملاحظات عامة
- جميع الجداول تحتوي على حقول التدقيق (تاريخ الإنشاء/التعديل، المستخدم).
- كل العمليات المالية (قيد يدوي/آلي/إقفال/تسوية/ترحيل) مرتبطة بهذه الجداول.
- تدعم جداول المالية الربط مع كل الأقسام الأخرى (مبيعات، مشتريات، رواتب، أصول، إلخ).
- يمكن توسيع financial_settings لإضافة سياسات الترحيل التلقائي، معايير الإقفال، إعدادات مراكز التكلفة، وغيرها.
- تدعم الجداول الربط مع المرفقات (attachments) وسجلات التدقيق (audit_trails).

---
