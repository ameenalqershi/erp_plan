# قسم العملاء والموردين والعلاقات التجارية – تحليل جداول قاعدة البيانات

---

## جدول: customers (العملاء)
| الحقل                | النوع             | الوصف                                               | قيود/علاقات                              |
|----------------------|-------------------|-----------------------------------------------------|------------------------------------------|
| customer_id          | INT (PK)          | رقم العميل التسلسلي                                | PRIMARY KEY, AUTO_INCREMENT              |
| customer_code        | VARCHAR(30)       | كود العميل                                         | UNIQUE, NOT NULL                         |
| name_ar              | VARCHAR(200)      | اسم العميل بالعربي                                 | NOT NULL                                 |
| name_en              | VARCHAR(200)      | اسم العميل بالإنجليزي                              |                                          |
| group_id             | INT (FK)          | مجموعة العملاء                                     | FK→customer_groups.group_id              |
| commercial_reg_no    | VARCHAR(50)       | رقم السجل التجاري                                  |                                          |
| tax_id               | VARCHAR(50)       | رقم البطاقة الضريبية                               |                                          |
| national_id          | VARCHAR(50)       | رقم الهوية الوطنية                                 |                                          |
| phone1               | VARCHAR(30)       | رقم هاتف رئيسي                                     |                                          |
| phone2               | VARCHAR(30)       | رقم هاتف إضافي                                     |                                          |
| email                | VARCHAR(100)      | بريد إلكتروني                                      |                                          |
| website              | VARCHAR(100)      | موقع ويب                                           |                                          |
| address              | VARCHAR(300)      | العنوان الرئيسي                                    |                                          |
| city                 | VARCHAR(100)      | المدينة                                            |                                          |
| country              | VARCHAR(100)      | الدولة                                             |                                          |
| branch_id            | INT (FK)          | الفرع الافتراضي                                    | FK→branches.branch_id                    |
| is_active            | BOOLEAN           | مفعل/غير مفعل                                      |                                          |
| credit_limit         | DECIMAL(18,2)     | حد الائتمان                                        |                                          |
| credit_days          | INT               | عدد أيام السماح                                    |                                          |
| opening_balance      | DECIMAL(18,2)     | الرصيد الافتتاحي                                   |                                          |
| opening_balance_type | ENUM              | نوع الرصيد (مدين/دائن)                             |                                          |
| note                 | TEXT              | ملاحظات                                            |                                          |
| created_by           | INT (FK)          | المستخدم المُنشئ                                   | FK→users.user_id                         |
| created_at           | DATETIME          | تاريخ الإنشاء                                      |                                          |
| updated_by           | INT (FK)          | المستخدم المعدّل                                   | FK→users.user_id                         |
| updated_at           | DATETIME          | تاريخ آخر تعديل                                     |                                          |

---

## جدول: customer_groups (مجموعات العملاء)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                  |
|-----------------|------------------|------------------------------|------------------------------|
| group_id        | INT (PK)         | رقم المجموعة                 | PRIMARY KEY                  |
| group_code      | VARCHAR(20)      | كود المجموعة                 | UNIQUE, NOT NULL             |
| group_name_ar   | VARCHAR(100)     | اسم المجموعة بالعربي         | NOT NULL                     |
| group_name_en   | VARCHAR(100)     | اسم المجموعة بالإنجليزي      |                              |
| parent_id       | INT (FK)         | المجموعة الأعلى              | FK→customer_groups.group_id  |
| is_active       | BOOLEAN          | مفعل/غير مفعل                |                              |
| note            | TEXT             | ملاحظات                      |                              |

---

## جدول: customer_contacts (بيانات اتصال العملاء)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                     |
|-----------------|------------------|------------------------------|---------------------------------|
| contact_id      | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT     |
| customer_id     | INT (FK)         | رقم العميل                   | FK→customers.customer_id        |
| contact_name    | VARCHAR(100)     | اسم جهة الاتصال              |                                 |
| position        | VARCHAR(100)     | المنصب/الوظيفة               |                                 |
| mobile          | VARCHAR(30)      | رقم الجوال                   |                                 |
| email           | VARCHAR(100)     | بريد إلكتروني                |                                 |
| phone           | VARCHAR(30)      | رقم هاتف مكتبي               |                                 |
| fax             | VARCHAR(30)      | فاكس                         |                                 |
| note            | TEXT             | ملاحظات                      |                                 |
| is_main         | BOOLEAN          | جهة الاتصال الرئيسية؟         |                                 |

---

## جدول: customer_addresses (عناوين العملاء)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                    |
|-----------------|------------------|------------------------------|--------------------------------|
| address_id      | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT    |
| customer_id     | INT (FK)         | رقم العميل                   | FK→customers.customer_id       |
| address_type    | ENUM             | نوع العنوان (رئيسي/فرعي/شحن/فاتورة) |                          |
| address         | VARCHAR(300)     | العنوان التفصيلي             |                                |
| city            | VARCHAR(100)     | المدينة                      |                                |
| country         | VARCHAR(100)     | الدولة                       |                                |
| postal_code     | VARCHAR(20)      | الرمز البريدي                |                                |
| is_main         | BOOLEAN          | عنوان رئيسي                  |                                |
| note            | TEXT             | ملاحظات                      |                                |

---

## جدول: suppliers (الموردون)
| الحقل                | النوع             | الوصف                                               | قيود/علاقات                              |
|----------------------|-------------------|-----------------------------------------------------|------------------------------------------|
| supplier_id          | INT (PK)          | رقم المورد التسلسلي                                 | PRIMARY KEY, AUTO_INCREMENT              |
| supplier_code        | VARCHAR(30)       | كود المورد                                          | UNIQUE, NOT NULL                         |
| name_ar              | VARCHAR(200)      | اسم المورد بالعربي                                  | NOT NULL                                 |
| name_en              | VARCHAR(200)      | اسم المورد بالإنجليزي                               |                                          |
| group_id             | INT (FK)          | مجموعة الموردين                                     | FK→supplier_groups.group_id              |
| commercial_reg_no    | VARCHAR(50)       | رقم السجل التجاري                                  |                                          |
| tax_id               | VARCHAR(50)       | رقم البطاقة الضريبية                               |                                          |
| national_id          | VARCHAR(50)       | رقم الهوية الوطنية                                 |                                          |
| phone1               | VARCHAR(30)       | رقم هاتف رئيسي                                     |                                          |
| phone2               | VARCHAR(30)       | رقم هاتف إضافي                                     |                                          |
| email                | VARCHAR(100)      | بريد إلكتروني                                      |                                          |
| website              | VARCHAR(100)      | موقع ويب                                           |                                          |
| address              | VARCHAR(300)      | العنوان الرئيسي                                    |                                          |
| city                 | VARCHAR(100)      | المدينة                                            |                                          |
| country              | VARCHAR(100)      | الدولة                                             |                                          |
| branch_id            | INT (FK)          | الفرع الافتراضي                                    | FK→branches.branch_id                    |
| is_active            | BOOLEAN           | مفعل/غير مفعل                                      |                                          |
| credit_limit         | DECIMAL(18,2)     | حد الائتمان                                        |                                          |
| credit_days          | INT               | عدد أيام السماح                                    |                                          |
| opening_balance      | DECIMAL(18,2)     | الرصيد الافتتاحي                                   |                                          |
| opening_balance_type | ENUM              | نوع الرصيد (مدين/دائن)                             |                                          |
| note                 | TEXT              | ملاحظات                                            |                                          |
| created_by           | INT (FK)          | المستخدم المُنشئ                                   | FK→users.user_id                         |
| created_at           | DATETIME          | تاريخ الإنشاء                                      |                                          |
| updated_by           | INT (FK)          | المستخدم المعدّل                                   | FK→users.user_id                         |
| updated_at           | DATETIME          | تاريخ آخر تعديل                                     |                                          |

---

## جدول: supplier_groups (مجموعات الموردين)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                  |
|-----------------|------------------|------------------------------|------------------------------|
| group_id        | INT (PK)         | رقم المجموعة                 | PRIMARY KEY                  |
| group_code      | VARCHAR(20)      | كود المجموعة                 | UNIQUE, NOT NULL             |
| group_name_ar   | VARCHAR(100)     | اسم المجموعة بالعربي         | NOT NULL                     |
| group_name_en   | VARCHAR(100)     | اسم المجموعة بالإنجليزي      |                              |
| parent_id       | INT (FK)         | المجموعة الأعلى              | FK→supplier_groups.group_id  |
| is_active       | BOOLEAN          | مفعل/غير مفعل                |                              |
| note            | TEXT             | ملاحظات                      |                              |

---

## جدول: supplier_contacts (بيانات اتصال الموردين)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                      |
|-----------------|------------------|------------------------------|----------------------------------|
| contact_id      | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT      |
| supplier_id     | INT (FK)         | رقم المورد                   | FK→suppliers.supplier_id         |
| contact_name    | VARCHAR(100)     | اسم جهة الاتصال              |                                  |
| position        | VARCHAR(100)     | المنصب/الوظيفة               |                                  |
| mobile          | VARCHAR(30)      | رقم الجوال                   |                                  |
| email           | VARCHAR(100)     | بريد إلكتروني                |                                  |
| phone           | VARCHAR(30)      | رقم هاتف مكتبي               |                                  |
| fax             | VARCHAR(30)      | فاكس                         |                                  |
| note            | TEXT             | ملاحظات                      |                                  |
| is_main         | BOOLEAN          | جهة الاتصال الرئيسية؟         |                                  |

---

## جدول: supplier_addresses (عناوين الموردين)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                    |
|-----------------|------------------|------------------------------|--------------------------------|
| address_id      | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT    |
| supplier_id     | INT (FK)         | رقم المورد                   | FK→suppliers.supplier_id       |
| address_type    | ENUM             | نوع العنوان (رئيسي/فرعي/شحن/فاتورة) |                          |
| address         | VARCHAR(300)     | العنوان التفصيلي             |                                |
| city            | VARCHAR(100)     | المدينة                      |                                |
| country         | VARCHAR(100)     | الدولة                       |                                |
| postal_code     | VARCHAR(20)      | الرمز البريدي                |                                |
| is_main         | BOOLEAN          | عنوان رئيسي                  |                                |
| note            | TEXT             | ملاحظات                      |                                |

---

## جدول: ar_transactions (حركات الذمم المدينة)
| الحقل               | النوع            | الوصف                            | قيود/علاقات                         |
|---------------------|------------------|----------------------------------|-------------------------------------|
| ar_txn_id           | INT (PK)         | رقم الحركة                       | PRIMARY KEY, AUTO_INCREMENT         |
| customer_id         | INT (FK)         | رقم العميل                       | FK→customers.customer_id            |
| txn_type            | ENUM             | نوع الحركة (فاتورة، سند قبض، إشعار ...) |                               |
| reference_type      | VARCHAR(50)      | نوع المرجع (فاتورة، سند ...)     |                                     |
| reference_id        | VARCHAR(50)      | رقم المرجع                       |                                     |
| txn_date            | DATE             | تاريخ الحركة                      |                                     |
| due_date            | DATE             | تاريخ الاستحقاق                   |                                     |
| amount              | DECIMAL(18,2)    | المبلغ                            |                                     |
| currency_id         | INT (FK)         | العملة                            | FK→currencies.currency_id           |
| status              | ENUM             | (مفتوح، مدفوع، جزئي، ملغي)        |                                     |
| note                | TEXT             | ملاحظات                          |                                     |
| branch_id           | INT (FK)         | الفرع                             | FK→branches.branch_id               |
| created_by          | INT (FK)         | المستخدم المُنشئ                   | FK→users.user_id                    |
| created_at          | DATETIME         | تاريخ الإنشاء                      |                                     |

---

## جدول: ap_transactions (حركات الذمم الدائنة)
| الحقل               | النوع            | الوصف                            | قيود/علاقات                         |
|---------------------|------------------|----------------------------------|-------------------------------------|
| ap_txn_id           | INT (PK)         | رقم الحركة                       | PRIMARY KEY, AUTO_INCREMENT         |
| supplier_id         | INT (FK)         | رقم المورد                       | FK→suppliers.supplier_id            |
| txn_type            | ENUM             | نوع الحركة (فاتورة، سند صرف، إشعار ...) |                              |
| reference_type      | VARCHAR(50)      | نوع المرجع (فاتورة، سند ...)     |                                     |
| reference_id        | VARCHAR(50)      | رقم المرجع                       |                                     |
| txn_date            | DATE             | تاريخ الحركة                      |                                     |
| due_date            | DATE             | تاريخ الاستحقاق                   |                                     |
| amount              | DECIMAL(18,2)    | المبلغ                            |                                     |
| currency_id         | INT (FK)         | العملة                            | FK→currencies.currency_id           |
| status              | ENUM             | (مفتوح، مدفوع، جزئي، ملغي)        |                                     |
| note                | TEXT             | ملاحظات                          |                                     |
| branch_id           | INT (FK)         | الفرع                             | FK→branches.branch_id               |
| created_by          | INT (FK)         | المستخدم المُنشئ                   | FK→users.user_id                    |
| created_at          | DATETIME         | تاريخ الإنشاء                      |                                     |

---

## جدول: credit_limits (حدود الائتمان)
| الحقل             | النوع            | الوصف                            | قيود/علاقات                         |
|-------------------|------------------|----------------------------------|-------------------------------------|
| id                | INT (PK)         | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT         |
| party_type        | ENUM             | نوع الطرف (عميل/مورد)           |                                     |
| party_id          | INT              | رقم العميل أو المورد             | FK→customers.customer_id أو suppliers.supplier_id حسب party_type |
| credit_limit      | DECIMAL(18,2)    | الحد الائتماني                   |                                     |
| currency_id       | INT (FK)         | العملة                            | FK→currencies.currency_id           |
| valid_from        | DATE             | بداية المدة                       |                                     |
| valid_to          | DATE             | نهاية المدة                       |                                     |
| note              | TEXT             | ملاحظات                          |                                     |

---

## العلاقات الرئيسية في قسم العملاء والموردين

- **customers.group_id → customer_groups.group_id**
- **customers.branch_id → branches.branch_id**
- **customer_contacts.customer_id → customers.customer_id**
- **customer_addresses.customer_id → customers.customer_id**
- **suppliers.group_id → supplier_groups.group_id**
- **suppliers.branch_id → branches.branch_id**
- **supplier_contacts.supplier_id → suppliers.supplier_id**
- **supplier_addresses.supplier_id → suppliers.supplier_id**
- **ar_transactions.customer_id → customers.customer_id**
- **ar_transactions.currency_id → currencies.currency_id**
- **ap_transactions.supplier_id → suppliers.supplier_id**
- **ap_transactions.currency_id → currencies.currency_id**
- **credit_limits.party_id → customers.customer_id أو suppliers.supplier_id حسب party_type**
- **credit_limits.currency_id → currencies.currency_id**

---

## ملاحظات عامة

- جميع الجداول تدعم التدقيق (تاريخ الإنشاء/التعديل، المستخدم).
- بإمكان العملاء والموردين الانتماء لمجموعات متفرعة (شجرة).
- جميع الحركات المالية مرتبطة بالجداول المالية والمحاسبية.
- بيانات الاتصال والعناوين تدعم التعددية (أكثر من رقم/عنوان/شخص اتصال).
- يمكن توسيع الجداول لتدعم الموافقات الإلكترونية، العقود، برامج الولاء، إلخ.

---
