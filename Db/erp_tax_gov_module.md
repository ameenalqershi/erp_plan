# قسم الضرائب والمعاملات الحكومية – تحليل جداول قاعدة البيانات

---

## جدول: tax_types (أنواع الضرائب)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                  |
|-----------------|------------------|------------------------------|------------------------------|
| tax_type_id     | INT (PK)         | رقم نوع الضريبة              | PRIMARY KEY                  |
| code            | VARCHAR(30)      | كود الضريبة                  | UNIQUE, NOT NULL             |
| name_ar         | VARCHAR(100)     | الاسم بالعربي                |                              |
| name_en         | VARCHAR(100)     | الاسم بالإنجليزي             |                              |
| tax_category    | ENUM             | (قيمة مضافة، جمارك، دخل...) |                              |
| rate            | DECIMAL(7,4)     | نسبة الضريبة                 |                              |
| start_date      | DATE             | بداية التطبيق                |                              |
| end_date        | DATE             | نهاية التطبيق                |                              |
| is_active       | BOOLEAN          | مفعل/غير مفعل                |                              |
| description     | TEXT             | وصف إضافي                    |                              |

---

## جدول: taxes (ضرائب العمليات)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                       |
|-----------------|------------------|------------------------------|-----------------------------------|
| tax_id          | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT       |
| reference_type  | VARCHAR(30)      | نوع المرجع (فاتورة، سند، ...) |                                   |
| reference_id    | INT              | رقم المرجع                   |                                   |
| tax_type_id     | INT (FK)         | نوع الضريبة                  | FK→tax_types.tax_type_id          |
| amount          | DECIMAL(18,2)    | مبلغ الضريبة                 |                                   |
| base_amount     | DECIMAL(18,2)    | أساس حساب الضريبة            |                                   |
| rate            | DECIMAL(7,4)     | نسبة الضريبة                 |                                   |
| currency_id     | INT (FK)         | العملة                       | FK→currencies.currency_id         |
| tax_date        | DATE             | تاريخ احتساب الضريبة         |                                   |
| note            | TEXT             | ملاحظات                      |                                   |

---

## جدول: gov_transactions (المعاملات الحكومية)
| الحقل              | النوع            | الوصف                                | قيود/علاقات                  |
|--------------------|------------------|--------------------------------------|------------------------------|
| gov_txn_id         | INT (PK)         | رقم المعاملة                         | PRIMARY KEY, AUTO_INCREMENT  |
| txn_type           | ENUM             | نوع المعاملة (ضريبة، زكاة، جمارك، ...) |                              |
| reference_id       | VARCHAR(50)      | رقم المستند أو المرجع الحكومي         |                              |
| period_id          | INT (FK)         | الفترة المالية                        | FK→fiscal_periods.period_id  |
| start_date         | DATE             | بداية الفترة                          |                              |
| end_date           | DATE             | نهاية الفترة                          |                              |
| amount             | DECIMAL(18,2)    | مبلغ المعاملة                         |                              |
| status             | ENUM             | (مفتوح، مدفوع، جزئي، ملغي)            |                              |
| gov_entity_id      | INT (FK)         | الجهة الحكومية                        | FK→gov_entities.gov_entity_id|
| notes              | TEXT             | ملاحظات                               |                              |
| attachment_id      | INT (FK)         | مرفقات                                 | FK→attachments.attachment_id |
| created_by         | INT (FK)         | من أنشأ                               | FK→users.user_id             |
| created_at         | DATETIME         | تاريخ الإنشاء                         |                              |

---

## جدول: gov_entities (الجهات الحكومية)
| الحقل              | النوع            | الوصف                                | قيود/علاقات                  |
|--------------------|------------------|--------------------------------------|------------------------------|
| gov_entity_id      | INT (PK)         | رقم الجهة                            | PRIMARY KEY, AUTO_INCREMENT  |
| code               | VARCHAR(20)      | كود الجهة                            | UNIQUE, NOT NULL             |
| name_ar            | VARCHAR(100)     | اسم الجهة بالعربي                    |                              |
| name_en            | VARCHAR(100)     | اسم الجهة بالإنجليزي                 |                              |
| contact_person     | VARCHAR(100)     | جهة الاتصال                          |                              |
| phone              | VARCHAR(30)      | رقم الهاتف                           |                              |
| email              | VARCHAR(100)     | البريد الإلكتروني                     |                              |
| address            | VARCHAR(200)     | العنوان                              |                              |
| is_active          | BOOLEAN          | مفعل/غير مفعل                        |                              |
| notes              | TEXT             | ملاحظات                              |                              |

---

## جدول: tax_returns (الإقرارات الضريبية)
| الحقل              | النوع            | الوصف                                | قيود/علاقات                  |
|--------------------|------------------|--------------------------------------|------------------------------|
| return_id          | INT (PK)         | رقم الإقرار                          | PRIMARY KEY, AUTO_INCREMENT  |
| tax_type_id        | INT (FK)         | نوع الضريبة                          | FK→tax_types.tax_type_id     |
| period_id          | INT (FK)         | الفترة المالية                       | FK→fiscal_periods.period_id  |
| start_date         | DATE             | بداية الفترة                          |                              |
| end_date           | DATE             | نهاية الفترة                          |                              |
| amount             | DECIMAL(18,2)    | إجمالي مبلغ الإقرار                   |                              |
| status             | ENUM             | (معد، مقدم، مدفوع، ملغي)              |                              |
| submission_date    | DATE             | تاريخ التقديم                          |                              |
| paid_date          | DATE             | تاريخ السداد                           |                              |
| gov_txn_id         | INT (FK)         | معاملة حكومية مرتبطة                  | FK→gov_transactions.gov_txn_id|
| notes              | TEXT             | ملاحظات                               |                              |
| attachment_id      | INT (FK)         | مرفقات                                 | FK→attachments.attachment_id |
| created_by         | INT (FK)         | من أنشأ                               | FK→users.user_id             |
| created_at         | DATETIME         | تاريخ الإنشاء                         |                              |

---

## جدول: e_invoice_integrations (تكامل الفوترة الإلكترونية)
| الحقل              | النوع            | الوصف                                | قيود/علاقات                  |
|--------------------|------------------|--------------------------------------|------------------------------|
| einv_id            | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| invoice_id         | INT              | رقم الفاتورة (داخلي)                 |                              |
| invoice_type       | ENUM             | (مبيعات، مشتريات، مردودات ...)        |                              |
| gov_ref_no         | VARCHAR(100)     | رقم الفاتورة الحكومي                  |                              |
| status             | ENUM             | (معد، مرسل، معتمد، مرفوض، ملغي)       |                              |
| submission_date    | DATETIME         | تاريخ الإرسال                         |                              |
| response_code      | VARCHAR(20)      | كود الاستجابة من الجهة                |                              |
| response_message   | TEXT             | رسالة الرد                            |                              |
| attachment_id      | INT (FK)         | المرفق                                | FK→attachments.attachment_id |
| created_by         | INT (FK)         | من أنشأ                               | FK→users.user_id             |
| created_at         | DATETIME         | تاريخ الإنشاء                         |                              |

---

## جدول: tax_audit_logs (سجلات تدقيق الضرائب)
| الحقل              | النوع            | الوصف                                | قيود/علاقات                  |
|--------------------|------------------|--------------------------------------|------------------------------|
| audit_log_id       | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| reference_type     | VARCHAR(30)      | نوع العملية (فاتورة، إقرار ...)      |                              |
| reference_id       | INT              | رقم المرجع                           |                              |
| action             | VARCHAR(50)      | نوع الحدث (إنشاء، تعديل، حذف ...)    |                              |
| user_id            | INT (FK)         | المستخدم                             | FK→users.user_id             |
| event_time         | DATETIME         | وقت الحدث                            |                              |
| old_value          | TEXT             | القيم قبل التغيير                    |                              |
| new_value          | TEXT             | القيم بعد التغيير                    |                              |
| note               | TEXT             | ملاحظات                              |                              |

---

## العلاقات الرئيسية في قسم الضرائب والمعاملات الحكومية

- **taxes.tax_type_id → tax_types.tax_type_id**
- **taxes.currency_id → currencies.currency_id**
- **gov_transactions.gov_entity_id → gov_entities.gov_entity_id**
- **gov_transactions.period_id → fiscal_periods.period_id**
- **gov_transactions.attachment_id → attachments.attachment_id**
- **gov_transactions.created_by → users.user_id**
- **tax_returns.tax_type_id → tax_types.tax_type_id**
- **tax_returns.period_id → fiscal_periods.period_id**
- **tax_returns.gov_txn_id → gov_transactions.gov_txn_id**
- **tax_returns.attachment_id → attachments.attachment_id**
- **tax_returns.created_by → users.user_id**
- **e_invoice_integrations.attachment_id → attachments.attachment_id**
- **e_invoice_integrations.created_by → users.user_id**
- **tax_audit_logs.user_id → users.user_id**

---

## ملاحظات عملية

- دعم كامل لإدارة أنواع الضرائب (قيمة مضافة، دخل، جمارك...) مع تواريخ صلاحية ونسب متعددة.
- كل عملية تجارية يمكن ربطها بضريبة واحدة أو أكثر عبر جدول taxes.
- معاملات الجهات الحكومية تدعم الربط مع الفترات المالية، الجهات، المرفقات، المستخدمين.
- الإقرارات الضريبية تدعم إعداد، تقديم، دفع، ربط حكومي، أرشفة.
- دعم تكامل الفوترة الإلكترونية مع الجهات الحكومية (ZATCA، E-Invoice...) مع سجل متكامل للردود والحالة.
- جميع العمليات تدعم الربط بالمرفقات، التدقيق، التوثيق، وإجراءات الاعتماد.
- تدقيق ضريبي متكامل عبر tax_audit_logs لأي تغيير أو تحديث في بيانات الضرائب أو الإقرارات.

---