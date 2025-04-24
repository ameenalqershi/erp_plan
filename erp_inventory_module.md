# قسم المخزون والمستودعات – تحليل جداول قاعدة البيانات

---

## جدول: items (الأصناف)
| الحقل                | النوع               | الوصف                                                      | قيود/علاقات                               |
|----------------------|---------------------|------------------------------------------------------------|-------------------------------------------|
| item_id              | INT (PK)            | رقم الصنف                                                  | PRIMARY KEY, AUTO_INCREMENT               |
| item_code            | VARCHAR(50)         | كود الصنف                                                  | UNIQUE, NOT NULL                          |
| barcode              | VARCHAR(50)         | باركود الصنف                                               | UNIQUE, NULLABLE                          |
| name_ar              | VARCHAR(200)        | اسم الصنف بالعربي                                          | NOT NULL                                  |
| name_en              | VARCHAR(200)        | اسم الصنف بالإنجليزي                                       |                                           |
| group_id             | INT (FK)            | مجموعة الأصناف                                             | FK→item_groups.group_id                   |
| type                 | ENUM                | نوع الصنف (بضاعة، خام، نهائي، خدمة، هدية...)              |                                           |
| brand_id             | INT (FK)            | الماركة/العلامة التجارية                                   | FK→brands.brand_id                        |
| model                | VARCHAR(100)        | الموديل                                                    |                                           |
| main_unit_id         | INT (FK)            | الوحدة الرئيسية                                            | FK→units.unit_id                          |
| purchase_unit_id     | INT (FK)            | وحدة الشراء الافتراضية                                     | FK→units.unit_id                          |
| sales_unit_id        | INT (FK)            | وحدة البيع الافتراضية                                      | FK→units.unit_id                          |
| conversion_rate      | DECIMAL(10,4)       | معدل التحويل بين الوحدات                                   |                                           |
| min_stock_level      | DECIMAL(18,4)       | الحد الأدنى للمخزون                                        |                                           |
| max_stock_level      | DECIMAL(18,4)       | الحد الأقصى للمخزون                                        |                                           |
| reorder_level        | DECIMAL(18,4)       | مستوى إعادة الطلب                                          |                                           |
| expiry_control       | BOOLEAN             | هل له صلاحية انتهاء                                        |                                           |
| serial_control       | BOOLEAN             | هل له تتبع أرقام تسلسلية                                   |                                           |
| batch_control        | BOOLEAN             | هل له تتبع دفعات                                           |                                           |
| shelf_life_days      | INT                 | العمر الافتراضي (بالأيام)                                  |                                           |
| cost_method          | ENUM                | طريقة احتساب التكلفة (متوسط، وارد أولاً، إلخ)              |                                           |
| default_supplier_id  | INT (FK)            | المورد الافتراضي                                           | FK→suppliers.supplier_id                  |
| is_active            | BOOLEAN             | مفعل/غير مفعل                                              |                                           |
| image_url            | VARCHAR(250)        | صورة الصنف                                                 |                                           |
| description          | TEXT                | وصف إضافي                                                  |                                           |
| created_by           | INT (FK)            | المستخدم المُنشئ                                            | FK→users.user_id                          |
| created_at           | DATETIME            | تاريخ الإنشاء                                               |                                           |
| updated_by           | INT (FK)            | المستخدم المعدّل                                            | FK→users.user_id                          |
| updated_at           | DATETIME            | تاريخ آخر تعديل                                             |                                           |

---

## جدول: item_groups (مجموعات الأصناف)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| group_id          | INT (PK)         | رقم المجموعة                 | PRIMARY KEY                  |
| group_code        | VARCHAR(30)      | كود المجموعة                 | UNIQUE, NOT NULL             |
| group_name_ar     | VARCHAR(100)     | اسم المجموعة بالعربي         | NOT NULL                     |
| group_name_en     | VARCHAR(100)     | اسم المجموعة بالإنجليزي      |                              |
| parent_id         | INT (FK)         | المجموعة الأعلى              | FK→item_groups.group_id      |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                              |
| note              | TEXT             | ملاحظات                      |                              |

---

## جدول: units (وحدات القياس)
| الحقل         | النوع            | الوصف                        | قيود/علاقات                   |
|---------------|------------------|------------------------------|-------------------------------|
| unit_id       | INT (PK)         | رقم الوحدة                   | PRIMARY KEY                   |
| unit_code     | VARCHAR(20)      | كود الوحدة                   | UNIQUE, NOT NULL              |
| unit_name_ar  | VARCHAR(50)      | اسم الوحدة بالعربي           |                               |
| unit_name_en  | VARCHAR(50)      | اسم الوحدة بالإنجليزي        |                               |
| is_active     | BOOLEAN          | مفعل/غير مفعل                |                               |

---

## جدول: item_unit_conversions (تحويلات الوحدات للصنف)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                            |
|-------------------|------------------|------------------------------|----------------------------------------|
| conversion_id     | INT (PK)         | رقم التحويل                  | PRIMARY KEY, AUTO_INCREMENT            |
| item_id           | INT (FK)         | رقم الصنف                    | FK→items.item_id                       |
| from_unit_id      | INT (FK)         | الوحدة من                    | FK→units.unit_id                       |
| to_unit_id        | INT (FK)         | الوحدة إلى                   | FK→units.unit_id                       |
| conversion_rate   | DECIMAL(18,6)    | معدل التحويل                 |                                        |
| is_default        | BOOLEAN          | تحويل افتراضي                |                                        |

---

## جدول: brands (العلامات التجارية)
| الحقل         | النوع            | الوصف                        | قيود/علاقات                   |
|---------------|------------------|------------------------------|-------------------------------|
| brand_id      | INT (PK)         | رقم العلامة                  | PRIMARY KEY                   |
| name_ar       | VARCHAR(100)     | اسم العلامة بالعربي          |                               |
| name_en       | VARCHAR(100)     | اسم العلامة بالإنجليزي       |                               |
| is_active     | BOOLEAN          | مفعل/غير مفعل                |                               |

---

## جدول: warehouses (المخازن)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                       |
|-------------------|------------------|------------------------------|-----------------------------------|
| warehouse_id      | INT (PK)         | رقم المخزن                   | PRIMARY KEY                       |
| warehouse_code    | VARCHAR(30)      | كود المخزن                   | UNIQUE, NOT NULL                  |
| warehouse_name_ar | VARCHAR(100)     | اسم المخزن بالعربي           | NOT NULL                          |
| warehouse_name_en | VARCHAR(100)     | اسم المخزن بالإنجليزي        |                                   |
| type              | ENUM             | نوع المخزن (رئيسي/فرعي/تالف/هدايا...) |                              |
| branch_id         | INT (FK)         | الفرع                        | FK→branches.branch_id             |
| manager_id        | INT (FK)         | مسؤول المخزن                 | FK→employees.employee_id          |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                                   |
| address           | VARCHAR(200)     | العنوان                      |                                   |
| phone             | VARCHAR(30)      | رقم الهاتف                   |                                   |
| note              | TEXT             | ملاحظات                      |                                   |

---

## جدول: warehouse_locations (مواقع التخزين الداخلية)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                       |
|-------------------|------------------|------------------------------|-----------------------------------|
| location_id       | INT (PK)         | رقم الموقع                   | PRIMARY KEY                       |
| warehouse_id      | INT (FK)         | رقم المخزن                   | FK→warehouses.warehouse_id        |
| code              | VARCHAR(30)      | كود الموقع                   | UNIQUE, NOT NULL                  |
| name_ar           | VARCHAR(100)     | اسم الموقع بالعربي           |                                   |
| name_en           | VARCHAR(100)     | اسم الموقع بالإنجليزي        |                                   |
| parent_id         | INT (FK)         | الموقع الأعلى                | FK→warehouse_locations.location_id|
| level             | INT              | مستوى الموقع في الشجرة        |                                   |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                                   |
| note              | TEXT             | ملاحظات                      |                                   |

---

## جدول: stock_transactions (حركات المخزون)
| الحقل               | النوع               | الوصف                               | قيود/علاقات                          |
|---------------------|---------------------|-------------------------------------|--------------------------------------|
| transaction_id      | INT (PK)            | رقم الحركة                          | PRIMARY KEY, AUTO_INCREMENT          |
| transaction_type    | ENUM                | نوع الحركة (إدخال، إخراج، تحويل، جرد، تسوية...) |                              |
| reference_type      | VARCHAR(50)         | نوع المرجع (أمر شراء، فاتورة بيع، إلخ)         |                              |
| reference_id        | VARCHAR(50)         | رقم المرجع                          |                                      |
| transaction_date    | DATETIME            | تاريخ الحركة                         |                                      |
| warehouse_id        | INT (FK)            | رقم المخزن                           | FK→warehouses.warehouse_id           |
| location_id         | INT (FK)            | رقم الموقع الداخلي                   | FK→warehouse_locations.location_id    |
| item_id             | INT (FK)            | رقم الصنف                            | FK→items.item_id                     |
| unit_id             | INT (FK)            | وحدة القياس                          | FK→units.unit_id                     |
| quantity            | DECIMAL(18,4)       | الكمية                               |                                      |
| cost_price          | DECIMAL(18,4)       | تكلفة الوحدة                         |                                      |
| total_cost          | DECIMAL(18,4)       | إجمالي التكلفة                       |                                      |
| batch_id            | INT (FK)            | رقم الدفعة (إن وجد)                  | FK→stock_batches.batch_id             |
| serial_number       | VARCHAR(100)        | الرقم التسلسلي (إن وجد)              |                                      |
| expiry_date         | DATE                | تاريخ الانتهاء (إن وجد)               |                                      |
| from_warehouse_id   | INT (FK)            | المخزن المصدر (للتحويل)               | FK→warehouses.warehouse_id           |
| to_warehouse_id     | INT (FK)            | المخزن المستلم (للتحويل)              | FK→warehouses.warehouse_id           |
| status              | ENUM                | (مسودة، معتمد، ملغي، مكتمل)           |                                      |
| created_by          | INT (FK)            | المستخدم المُنشئ                      | FK→users.user_id                     |
| created_at          | DATETIME            | تاريخ الإنشاء                         |                                      |
| note                | TEXT                | ملاحظات                               |                                      |
| branch_id           | INT (FK)            | الفرع                                 | FK→branches.branch_id                |

---

## جدول: stock_batches (دفعات الأصناف)
| الحقل               | النوع            | الوصف                         | قيود/علاقات                          |
|---------------------|------------------|-------------------------------|--------------------------------------|
| batch_id            | INT (PK)         | رقم الدفعة                    | PRIMARY KEY, AUTO_INCREMENT          |
| item_id             | INT (FK)         | رقم الصنف                     | FK→items.item_id                     |
| batch_number        | VARCHAR(100)     | رقم الدفعة الخارجي            | UNIQUE                               |
| supplier_id         | INT (FK)         | المورد المصدر                  | FK→suppliers.supplier_id             |
| production_date     | DATE             | تاريخ الإنتاج                  |                                      |
| expiry_date         | DATE             | تاريخ الانتهاء                 |                                      |
| quantity_received   | DECIMAL(18,4)    | الكمية المستلمة                |                                      |
| quantity_remaining  | DECIMAL(18,4)    | الكمية المتبقية                |                                      |
| warehouse_id        | INT (FK)         | المخزن                         | FK→warehouses.warehouse_id           |
| note                | TEXT             | ملاحظات                        |                                      |

---

## جدول: stock_reservations (حجوزات المخزون)
| الحقل               | النوع            | الوصف                         | قيود/علاقات                          |
|---------------------|------------------|-------------------------------|--------------------------------------|
| reservation_id      | INT (PK)         | رقم الحجز                     | PRIMARY KEY, AUTO_INCREMENT          |
| item_id             | INT (FK)         | رقم الصنف                     | FK→items.item_id                     |
| warehouse_id        | INT (FK)         | المخزن                         | FK→warehouses.warehouse_id           |
| order_type          | ENUM             | نوع أمر الحجز (بيع/تصنيع/مشروع)|                                      |
| order_id            | INT              | رقم الأمر                      |                                      |
| quantity            | DECIMAL(18,4)    | الكمية المحجوزة                |                                      |
| reserved_by         | INT (FK)         | المستخدم                       | FK→users.user_id                     |
| reserved_at         | DATETIME         | تاريخ الحجز                     |                                      |
| expiry_at           | DATETIME         | انتهاء صلاحية الحجز             |                                      |
| status              | ENUM             | (نشط، منتهي، ملغي)              |                                      |
| note                | TEXT             | ملاحظات                        |                                      |

---

## جدول: stock_adjustments (تسويات وجرد المخزون)
| الحقل                | النوع            | الوصف                          | قيود/علاقات                         |
|----------------------|------------------|--------------------------------|-------------------------------------|
| adjustment_id        | INT (PK)         | رقم التسوية                    | PRIMARY KEY, AUTO_INCREMENT         |
| adjustment_number    | VARCHAR(30)      | رقم التسوية (تسلسلي/يدوي)      | UNIQUE, NOT NULL                    |
| adjustment_type      | ENUM             | نوع التسوية (زيادة، نقصان، جرد)|                                     |
| reference_type       | VARCHAR(50)      | نوع المرجع (جرد، خسارة، إلخ)   |                                     |
| reference_id         | VARCHAR(50)      | رقم المرجع                     |                                     |
| warehouse_id         | INT (FK)         | رقم المخزن                     | FK→warehouses.warehouse_id          |
| item_id              | INT (FK)         | رقم الصنف                      | FK→items.item_id                    |
| unit_id              | INT (FK)         | وحدة القياس                    | FK→units.unit_id                    |
| batch_id             | INT (FK)         | رقم الدفعة (إن وجد)            | FK→stock_batches.batch_id           |
| counted_quantity     | DECIMAL(18,4)    | الكمية الفعلية                 |                                     |
| system_quantity      | DECIMAL(18,4)    | الكمية بالنظام                 |                                     |
| difference           | DECIMAL(18,4)    | الفرق                          |                                     |
| cost_price           | DECIMAL(18,4)    | تكلفة الوحدة                   |                                     |
| total_cost           | DECIMAL(18,4)    | إجمالي تكلفة التسوية           |                                     |
| adjustment_date      | DATETIME         | تاريخ التسوية                  |                                     |
| approved_by          | INT (FK)         | من اعتمد التسوية               | FK→users.user_id                    |
| approved_at          | DATETIME         | تاريخ الاعتماد                  |                                     |
| status               | ENUM             | (مسودة، معتمد، ملغي)           |                                     |
| note                 | TEXT             | ملاحظات                        |                                     |

---

## جدول: stock_aging (أعمار المخزون)
| الحقل                | النوع            | الوصف                          | قيود/علاقات                         |
|----------------------|------------------|--------------------------------|-------------------------------------|
| aging_id             | INT (PK)         | رقم السطر                      | PRIMARY KEY, AUTO_INCREMENT         |
| item_id              | INT (FK)         | رقم الصنف                      | FK→items.item_id                    |
| warehouse_id         | INT (FK)         | رقم المخزن                     | FK→warehouses.warehouse_id          |
| batch_id             | INT (FK)         | رقم الدفعة (إن وجد)            | FK→stock_batches.batch_id           |
| date_in              | DATE             | تاريخ الإدخال                   |                                     |
| quantity             | DECIMAL(18,4)    | الكمية                          |                                     |
| days_in_stock        | INT              | عدد الأيام في المخزون           |                                     |
| expiry_date          | DATE             | تاريخ الانتهاء (إن وجد)         |                                     |

---

## جدول: stock_transfer_orders (أوامر تحويل المخزون)
| الحقل                | النوع            | الوصف                          | قيود/علاقات                         |
|----------------------|------------------|--------------------------------|-------------------------------------|
| transfer_id          | INT (PK)         | رقم أمر التحويل                | PRIMARY KEY, AUTO_INCREMENT         |
| transfer_number      | VARCHAR(30)      | رقم التحويل (تسلسلي/يدوي)      | UNIQUE, NOT NULL                    |
| from_warehouse_id    | INT (FK)         | المخزن المصدر                   | FK→warehouses.warehouse_id          |
| to_warehouse_id      | INT (FK)         | المخزن المستلم                  | FK→warehouses.warehouse_id          |
| transfer_date        | DATE             | تاريخ التحويل                   |                                     |
| status               | ENUM             | (مسودة، مكتمل، ملغي)           |                                     |
| created_by           | INT (FK)         | المستخدم مُنشئ الأمر            | FK→users.user_id                    |
| created_at           | DATETIME         | تاريخ الإنشاء                   |                                     |
| note                 | TEXT             | ملاحظات                        |                                     |

---

## جدول: stock_transfer_lines (تفاصيل أوامر التحويل)
| الحقل                | النوع            | الوصف                          | قيود/علاقات                         |
|----------------------|------------------|--------------------------------|-------------------------------------|
| line_id              | INT (PK)         | رقم السطر                      | PRIMARY KEY, AUTO_INCREMENT         |
| transfer_id          | INT (FK)         | رقم أمر التحويل                | FK→stock_transfer_orders.transfer_id|
| item_id              | INT (FK)         | رقم الصنف                      | FK→items.item_id                    |
| quantity             | DECIMAL(18,4)    | الكمية المحولة                  |                                     |
| unit_id              | INT (FK)         | وحدة القياس                    | FK→units.unit_id                    |
| batch_id             | INT (FK)         | رقم الدفعة (إن وجد)            | FK→stock_batches.batch_id           |
| note                 | TEXT             | ملاحظات                        |                                     |

---

## العلاقات الرئيسية في قسم المخزون

- **items.group_id → item_groups.group_id**
- **items.main_unit_id/purchase_unit_id/sales_unit_id → units.unit_id**
- **items.brand_id → brands.brand_id**
- **items.default_supplier_id → suppliers.supplier_id**
- **item_unit_conversions.item_id → items.item_id**
- **item_unit_conversions.from_unit_id/to_unit_id → units.unit_id**
- **warehouses.branch_id → branches.branch_id**
- **warehouses.manager_id → employees.employee_id**
- **warehouse_locations.warehouse_id → warehouses.warehouse_id**
- **warehouse_locations.parent_id → warehouse_locations.location_id**
- **stock_transactions.warehouse_id → warehouses.warehouse_id**
- **stock_transactions.location_id → warehouse_locations.location_id**
- **stock_transactions.item_id → items.item_id**
- **stock_transactions.unit_id → units.unit_id**
- **stock_transactions.batch_id → stock_batches.batch_id**
- **stock_transactions.from_warehouse_id/to_warehouse_id → warehouses.warehouse_id**
- **stock_batches.item_id → items.item_id**
- **stock_batches.supplier_id → suppliers.supplier_id**
- **stock_batches.warehouse_id → warehouses.warehouse_id**
- **stock_reservations.item_id → items.item_id**
- **stock_reservations.warehouse_id → warehouses.warehouse_id**
- **stock_reservations.reserved_by → users.user_id**
- **stock_adjustments.warehouse_id → warehouses.warehouse_id**
- **stock_adjustments.item_id → items.item_id**
- **stock_adjustments.unit_id → units.unit_id**
- **stock_adjustments.batch_id → stock_batches.batch_id**
- **stock_adjustments.approved_by → users.user_id**
- **stock_aging.item_id → items.item_id**
- **stock_aging.warehouse_id → warehouses.warehouse_id**
- **stock_aging.batch_id → stock_batches.batch_id**
- **stock_transfer_orders.from_warehouse_id/to_warehouse_id → warehouses.warehouse_id**
- **stock_transfer_orders.created_by → users.user_id**
- **stock_transfer_lines.transfer_id → stock_transfer_orders.transfer_id**
- **stock_transfer_lines.item_id → items.item_id**
- **stock_transfer_lines.unit_id → units.unit_id**
- **stock_transfer_lines.batch_id → stock_batches.batch_id**

---

## ملاحظات عامة

- تدعم جميع الجداول الربط بالمخازن، المواقع الداخلية، الدُفعات، الوحدات، المستخدمين، الفروع، الموردين.
- تدعم تتبع كامل للأصناف: كود، باركود، وحدات متعددة، دُفعات، أرقام تسلسلية، صلاحية، تكلفة.
- المخزون يدعم التحويل، الحجز، الجرد، التسويات، أوامر التحويل، أعمار المخزون.
- كل عملية مخزنية قابلة للربط بالعمليات التجارية (مبيعات، مشتريات، تصنيع).
- تتيح جداول المواقع والمخازن بناء هياكل متشعبة (مخزن رئيسي/فرعي/رفوف/مناطق...).
- تدعم الربط مع جداول المرفقات والإشعارات والتدقيق.
- جاهزة للربط مع التقارير التحليلية (أعمار المخزون، مراقبة الحد الأدنى، تحليل الحركة...).

---