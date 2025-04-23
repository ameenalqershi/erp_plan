# قسم الأصول الثابتة – تحليل جداول قاعدة البيانات

---

## جدول: fixed_assets (الأصول الثابتة)
| الحقل                | النوع               | الوصف                                       | قيود/علاقات                         |
|----------------------|---------------------|---------------------------------------------|-------------------------------------|
| asset_id             | INT (PK)            | رقم الأصل                                   | PRIMARY KEY, AUTO_INCREMENT         |
| asset_code           | VARCHAR(50)         | كود الأصل                                   | UNIQUE, NOT NULL                    |
| barcode              | VARCHAR(50)         | باركود الأصل                                | UNIQUE, NULLABLE                    |
| asset_name_ar        | VARCHAR(200)        | اسم الأصل بالعربي                           | NOT NULL                            |
| asset_name_en        | VARCHAR(200)        | اسم الأصل بالإنجليزي                        |                                     |
| group_id             | INT (FK)            | مجموعة الأصول                               | FK→fixed_asset_groups.group_id      |
| type                 | ENUM                | نوع الأصل (مبنى، سيارة، جهاز، معدات...)     |                                     |
| purchase_invoice_id  | INT (FK)            | فاتورة الشراء                               | FK→purchase_invoices.invoice_id     |
| supplier_id          | INT (FK)            | المورد                                      | FK→suppliers.supplier_id            |
| purchase_date        | DATE                | تاريخ الشراء                                |                                     |
| purchase_cost        | DECIMAL(18,2)       | تكلفة الشراء                                |                                     |
| currency_id          | INT (FK)            | العملة                                      | FK→currencies.currency_id           |
| useful_life_years    | INT                 | العمر الافتراضي (بالسنوات)                  |                                     |
| depreciation_method  | ENUM                | طريقة الإهلاك (خط مستقيم/متناقص/مخصص)       |                                     |
| depreciation_rate    | DECIMAL(6,3)        | نسبة الإهلاك السنوية (%)                    |                                     |
| salvage_value        | DECIMAL(18,2)       | قيمة التخريد/المتبقي                        |                                     |
| start_depreciation   | DATE                | بداية الإهلاك                               |                                     |
| status               | ENUM                | (نشط، متوقف، تحت الصيانة، موقوف، مباع)       |                                     |
| location_id          | INT (FK)            | الموقع الفعلي للأصل                          | FK→asset_locations.location_id      |
| department_id        | INT (FK)            | القسم/الوحدة المالكة للأصل                   | FK→departments.department_id        |
| cost_center_id       | INT (FK)            | مركز التكلفة                                | FK→cost_centers.cost_center_id      |
| branch_id            | INT (FK)            | الفرع                                        | FK→branches.branch_id               |
| warranty_expiry      | DATE                | نهاية الضمان                                 |                                     |
| asset_serial         | VARCHAR(100)        | الرقم التسلسلي للأصل                         |                                     |
| maintenance_cycle    | INT                 | دورة الصيانة (أيام)                           |                                     |
| notes                | TEXT                | ملاحظات                                      |                                     |
| image_url            | VARCHAR(255)        | صورة الأصل                                   |                                     |
| attachment_id        | INT (FK)            | مرفقات ومستندات                              | FK→attachments.attachment_id        |
| created_by           | INT (FK)            | المستخدم المُنشئ                              | FK→users.user_id                    |
| created_at           | DATETIME            | تاريخ الإنشاء                                 |                                     |
| updated_by           | INT (FK)            | المستخدم المعدّل                              | FK→users.user_id                    |
| updated_at           | DATETIME            | تاريخ آخر تعديل                               |                                     |

---

## جدول: fixed_asset_groups (مجموعات الأصول)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| group_id          | INT (PK)         | رقم المجموعة                 | PRIMARY KEY                  |
| group_code        | VARCHAR(30)      | كود المجموعة                 | UNIQUE, NOT NULL             |
| group_name_ar     | VARCHAR(100)     | اسم المجموعة بالعربي         | NOT NULL                     |
| group_name_en     | VARCHAR(100)     | اسم المجموعة بالإنجليزي      |                              |
| parent_id         | INT (FK)         | المجموعة الأعلى              | FK→fixed_asset_groups.group_id|
| depreciation_method| ENUM            | طريقة الإهلاك الافتراضية     |                              |
| depreciation_rate | DECIMAL(6,3)     | نسبة الإهلاك الافتراضية      |                              |
| useful_life_years | INT              | العمر الافتراضي الافتراضي    |                              |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                              |
| note              | TEXT             | ملاحظات                      |                              |

---

## جدول: asset_locations (مواقع الأصول)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                       |
|-------------------|------------------|------------------------------|-----------------------------------|
| location_id       | INT (PK)         | رقم الموقع                   | PRIMARY KEY                       |
| location_code     | VARCHAR(30)      | كود الموقع                   | UNIQUE, NOT NULL                  |
| location_name_ar  | VARCHAR(100)     | اسم الموقع بالعربي           |                                   |
| location_name_en  | VARCHAR(100)     | اسم الموقع بالإنجليزي        |                                   |
| parent_id         | INT (FK)         | الموقع الأعلى                | FK→asset_locations.location_id    |
| branch_id         | INT (FK)         | الفرع                        | FK→branches.branch_id             |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                                   |
| note              | TEXT             | ملاحظات                      |                                   |

---

## جدول: asset_assignments (تخصيصات الأصول)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                              |
|-------------------|------------------|------------------------------|------------------------------------------|
| assignment_id     | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT              |
| asset_id          | INT (FK)         | رقم الأصل                    | FK→fixed_assets.asset_id                 |
| assigned_to_type  | ENUM             | نوع المخصص له (موظف/قسم)      |                                          |
| assigned_to_id    | INT              | رقم الموظف أو القسم           | FK→employees.employee_id أو departments.department_id|
| assigned_at       | DATETIME         | تاريخ التخصيص                 |                                          |
| return_date       | DATETIME         | تاريخ الإرجاع (إن وجد)        |                                          |
| status            | ENUM             | (نشط، مُعاد، تالف، فاقد)      |                                          |
| note              | TEXT             | ملاحظات                      |                                          |

---

## جدول: asset_depreciations (إهلاكات الأصول)
| الحقل                | النوع            | الوصف                        | قيود/علاقات                              |
|----------------------|------------------|------------------------------|------------------------------------------|
| depreciation_id      | INT (PK)         | رقم الإهلاك                  | PRIMARY KEY, AUTO_INCREMENT              |
| asset_id             | INT (FK)         | رقم الأصل                    | FK→fixed_assets.asset_id                 |
| period_id            | INT (FK)         | الفترة المالية               | FK→fiscal_periods.period_id              |
| depreciation_date    | DATE             | تاريخ الإهلاك                |                                          |
| amount               | DECIMAL(18,2)    | مبلغ الإهلاك                 |                                          |
| method               | ENUM             | طريقة الإهلاك                |                                          |
| status               | ENUM             | (محسوب، مرحّل، ملغي)         |                                          |
| note                 | TEXT             | ملاحظات                      |                                          |
| journal_entry_id     | INT (FK)         | رقم قيد اليومية المحاسبي      | FK→journal_entries.entry_id              |

---

## جدول: asset_transfers (تحويلات الأصول)
| الحقل                | النوع            | الوصف                        | قيود/علاقات                              |
|----------------------|------------------|------------------------------|------------------------------------------|
| transfer_id          | INT (PK)         | رقم التحويل                  | PRIMARY KEY, AUTO_INCREMENT              |
| asset_id             | INT (FK)         | رقم الأصل                    | FK→fixed_assets.asset_id                 |
| from_location_id     | INT (FK)         | الموقع المنقول منه            | FK→asset_locations.location_id           |
| to_location_id       | INT (FK)         | الموقع المنقول إليه           | FK→asset_locations.location_id           |
| transfer_date        | DATE             | تاريخ التحويل                 |                                          |
| transferred_by       | INT (FK)         | من قام بالتحويل               | FK→users.user_id                         |
| note                 | TEXT             | ملاحظات                      |                                          |

---

## جدول: asset_maintenances (صيانة الأصول)
| الحقل                | النوع            | الوصف                        | قيود/علاقات                              |
|----------------------|------------------|------------------------------|------------------------------------------|
| maintenance_id       | INT (PK)         | رقم عملية الصيانة            | PRIMARY KEY, AUTO_INCREMENT              |
| asset_id             | INT (FK)         | رقم الأصل                    | FK→fixed_assets.asset_id                 |
| maintenance_type     | ENUM             | (دورية، طارئة، تصحيحية)      |                                          |
| description          | TEXT             | وصف العملية                  |                                          |
| maintenance_date     | DATE             | تاريخ العملية                |                                          |
| performed_by         | VARCHAR(100)     | الجهة المنفذة (فريق/مقاول)    |                                          |
| cost                 | DECIMAL(18,2)    | تكلفة الصيانة                |                                          |
| status               | ENUM             | (مجدولة، مكتملة، ملغاة)       |                                          |
| note                 | TEXT             | ملاحظات                      |                                          |
| attachment_id        | INT (FK)         | مستندات الصيانة              | FK→attachments.attachment_id             |

---

## جدول: asset_disposals (استبعاد/بيع الأصول)
| الحقل                | النوع            | الوصف                        | قيود/علاقات                              |
|----------------------|------------------|------------------------------|------------------------------------------|
| disposal_id          | INT (PK)         | رقم العملية                  | PRIMARY KEY, AUTO_INCREMENT              |
| asset_id             | INT (FK)         | رقم الأصل                    | FK→fixed_assets.asset_id                 |
| disposal_type        | ENUM             | (بيع، تخريد، تبرع، هالك، فقدان)|                                         |
| disposal_date        | DATE             | تاريخ الاستبعاد              |                                          |
| disposal_value       | DECIMAL(18,2)    | قيمة البيع/التخريد           |                                          |
| buyer_receiver       | VARCHAR(100)     | المشتري/المستلم              |                                          |
| reason               | TEXT             | سبب الاستبعاد                |                                          |
| status               | ENUM             | (مسودة، مكتمل، ملغي)         |                                          |
| note                 | TEXT             | ملاحظات                      |                                          |
| journal_entry_id     | INT (FK)         | قيد اليومية المحاسبي         | FK→journal_entries.entry_id              |
| attachment_id        | INT (FK)         | مستندات العملية              | FK→attachments.attachment_id             |

---

## جدول: asset_warranties (ضمانات الأصول)
| الحقل                | النوع            | الوصف                        | قيود/علاقات                              |
|----------------------|------------------|------------------------------|------------------------------------------|
| warranty_id          | INT (PK)         | رقم الضمان                   | PRIMARY KEY, AUTO_INCREMENT              |
| asset_id             | INT (FK)         | رقم الأصل                    | FK→fixed_assets.asset_id                 |
| warranty_provider    | VARCHAR(100)     | مقدم الضمان                   |                                          |
| warranty_number      | VARCHAR(50)      | رقم الضمان                   |                                          |
| start_date           | DATE             | بداية الضمان                  |                                          |
| end_date             | DATE             | نهاية الضمان                  |                                          |
| terms                | TEXT             | الشروط                        |                                          |
| status               | ENUM             | (ساري، منتهي، ملغي)           |                                          |
| note                 | TEXT             | ملاحظات                      |                                          |
| attachment_id        | INT (FK)         | مستندات الضمان               | FK→attachments.attachment_id             |

---

## العلاقات الرئيسية في قسم الأصول الثابتة

- **fixed_assets.group_id → fixed_asset_groups.group_id**
- **fixed_assets.purchase_invoice_id → purchase_invoices.invoice_id**
- **fixed_assets.supplier_id → suppliers.supplier_id**
- **fixed_assets.currency_id → currencies.currency_id**
- **fixed_assets.location_id → asset_locations.location_id**
- **fixed_assets.department_id → departments.department_id**
- **fixed_assets.cost_center_id → cost_centers.cost_center_id**
- **fixed_assets.branch_id → branches.branch_id**
- **fixed_assets.attachment_id → attachments.attachment_id**
- **fixed_asset_groups.parent_id → fixed_asset_groups.group_id**
- **asset_assignments.asset_id → fixed_assets.asset_id**
- **asset_assignments.assigned_to_id → employees.employee_id أو departments.department_id (حسب assigned_to_type)**
- **asset_depreciations.asset_id → fixed_assets.asset_id**
- **asset_depreciations.period_id → fiscal_periods.period_id**
- **asset_depreciations.journal_entry_id → journal_entries.entry_id**
- **asset_transfers.asset_id → fixed_assets.asset_id**
- **asset_transfers.from_location_id/to_location_id → asset_locations.location_id**
- **asset_transfers.transferred_by → users.user_id**
- **asset_maintenances.asset_id → fixed_assets.asset_id**
- **asset_maintenances.attachment_id → attachments.attachment_id**
- **asset_disposals.asset_id → fixed_assets.asset_id**
- **asset_disposals.journal_entry_id → journal_entries.entry_id**
- **asset_disposals.attachment_id → attachments.attachment_id**
- **asset_warranties.asset_id → fixed_assets.asset_id**
- **asset_warranties.attachment_id → attachments.attachment_id**

---

## ملاحظات عملية

- كل أصل مرتبط بكامل بياناته: الشراء، المورد، الموقع، المجموعة، حالة التشغيل، الضمان، الإهلاك، التخصيص، الصيانة، الاستبعاد.
- كل العمليات تدعم الربط بالمرفقات، سجلات التدقيق، والتوثيق المالي.
- النظام يدعم إدارة الإهلاك الدوري آليًا مع الترحيل المحاسبي التلقائي.
- يدعم النظام كل عمليات الصيانة (دورية/طارئة)، التحويل بين المواقع/الأقسام، البيع/الاستبعاد، الضمانات، الأعطال.
- موائم للربط مع المالية (القيود)، المخزون (أصول مخزنية)، المشاريع، مراكز التكلفة، الفروع.

---