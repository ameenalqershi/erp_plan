# قسم التصنيع والإنتاج – تحليل جداول قاعدة البيانات

---

## جدول: bom_headers (رؤوس قوائم المواد - Bill of Materials)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| bom_id                | INT (PK)           | رقم القائمة                                      | PRIMARY KEY, AUTO_INCREMENT            |
| product_item_id       | INT (FK)           | الصنف النهائي (المنتج)                           | FK→items.item_id                       |
| bom_code              | VARCHAR(30)        | كود القائمة                                      | UNIQUE, NOT NULL                       |
| version               | VARCHAR(20)        | رقم الإصدار                                      |                                        |
| description           | TEXT               | وصف إضافي للقائمة                                |                                        |
| is_active             | BOOLEAN            | مفعل/غير مفعل                                    |                                        |
| valid_from            | DATE               | تاريخ بداية الصلاحية                             |                                        |
| valid_to              | DATE               | تاريخ نهاية الصلاحية                             |                                        |
| created_by            | INT (FK)           | المستخدم المُنشئ                                 | FK→users.user_id                       |
| created_at            | DATETIME           | تاريخ الإنشاء                                    |                                        |
| updated_by            | INT (FK)           | المستخدم المعدّل                                 | FK→users.user_id                       |
| updated_at            | DATETIME           | تاريخ آخر تعديل                                   |                                        |

---

## جدول: bom_lines (تفاصيل قوائم المواد)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| bom_line_id           | INT (PK)           | رقم السطر                                        | PRIMARY KEY, AUTO_INCREMENT            |
| bom_id                | INT (FK)           | رقم رأس القائمة                                  | FK→bom_headers.bom_id                  |
| item_id               | INT (FK)           | رقم المادة الخام/المكون                          | FK→items.item_id                       |
| quantity              | DECIMAL(18,4)      | الكمية المطلوبة                                  |                                        |
| unit_id               | INT (FK)           | وحدة القياس                                      | FK→units.unit_id                       |
| scrap_percentage      | DECIMAL(6,3)       | نسبة الهالك الافتراضية (%)                       |                                        |
| sequence              | INT                | التسلسل                                          |                                        |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: manufacturing_orders (أوامر التشغيل/الإنتاج)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| mo_id                 | INT (PK)           | رقم أمر التشغيل                                  | PRIMARY KEY, AUTO_INCREMENT            |
| mo_number             | VARCHAR(30)        | رقم الأمر                                        | UNIQUE, NOT NULL                       |
| product_item_id       | INT (FK)           | الصنف النهائي                                    | FK→items.item_id                       |
| bom_id                | INT (FK)           | رقم BOM المستخدم                                 | FK→bom_headers.bom_id                  |
| planned_quantity      | DECIMAL(18,4)      | الكمية المخطط إنتاجها                            |                                        |
| produced_quantity     | DECIMAL(18,4)      | الكمية النهائية المنتجة                           |                                        |
| unit_id               | INT (FK)           | وحدة المنتج النهائي                              | FK→units.unit_id                       |
| start_date            | DATE               | تاريخ بدء الإنتاج                                 |                                        |
| end_date              | DATE               | تاريخ الانتهاء                                    |                                        |
| status                | ENUM               | (مسودة، معتمد، جاري، مكتمل، ملغي، مؤجل)           |                                        |
| production_line_id    | INT (FK)           | خط الإنتاج                                        | FK→production_lines.line_id             |
| cost_center_id        | INT (FK)           | مركز التكلفة                                     | FK→cost_centers.cost_center_id          |
| project_id            | INT (FK)           | رقم المشروع (إن وجد)                             | FK→projects.project_id                  |
| branch_id             | INT (FK)           | الفرع                                            | FK→branches.branch_id                   |
| created_by            | INT (FK)           | المستخدم المُنشئ                                 | FK→users.user_id                        |
| created_at            | DATETIME           | تاريخ الإنشاء                                    |                                        |
| updated_by            | INT (FK)           | المستخدم المعدّل                                 | FK→users.user_id                        |
| updated_at            | DATETIME           | تاريخ آخر تعديل                                   |                                        |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: manufacturing_order_steps (المراحل التشغيلية لأمر التصنيع)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| step_id               | INT (PK)           | رقم المرحلة                                      | PRIMARY KEY, AUTO_INCREMENT            |
| mo_id                 | INT (FK)           | رقم أمر التشغيل                                  | FK→manufacturing_orders.mo_id          |
| step_code             | VARCHAR(30)        | كود المرحلة                                      |                                        |
| step_name_ar          | VARCHAR(100)       | اسم المرحلة بالعربي                              |                                        |
| step_name_en          | VARCHAR(100)       | اسم المرحلة بالإنجليزي                           |                                        |
| planned_start         | DATETIME           | بداية المرحلة المخططة                            |                                        |
| planned_end           | DATETIME           | نهاية المرحلة المخططة                            |                                        |
| actual_start          | DATETIME           | بداية المرحلة الفعلية                            |                                        |
| actual_end            | DATETIME           | نهاية المرحلة الفعلية                            |                                        |
| status                | ENUM               | (جاري، مكتمل، ملغي)                              |                                        |
| operator_id           | INT (FK)           | منفذ المرحلة (موظف/عامل)                         | FK→employees.employee_id               |
| resource_id           | INT (FK)           | مورد التشغيل (آلة/معدات)                         | FK→resources.resource_id                |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: manufacturing_consumptions (صرف المواد الخام - استهلاك)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| consumption_id        | INT (PK)           | رقم السطر                                        | PRIMARY KEY, AUTO_INCREMENT            |
| mo_id                 | INT (FK)           | رقم أمر التشغيل                                  | FK→manufacturing_orders.mo_id          |
| item_id               | INT (FK)           | المادة الخام/المكون                              | FK→items.item_id                       |
| quantity_planned      | DECIMAL(18,4)      | الكمية المخططة                                   |                                        |
| quantity_actual       | DECIMAL(18,4)      | الكمية الفعلية المستهلكة                         |                                        |
| unit_id               | INT (FK)           | وحدة القياس                                      | FK→units.unit_id                       |
| warehouse_id          | INT (FK)           | المخزن المسحوب منه                               | FK→warehouses.warehouse_id             |
| batch_id              | INT (FK)           | رقم الدفعة (إن وجد)                              | FK→stock_batches.batch_id               |
| cost_price            | DECIMAL(18,4)      | تكلفة الوحدة                                     |                                        |
| total_cost            | DECIMAL(18,4)      | إجمالي التكلفة                                   |                                        |
| consumption_date      | DATETIME           | تاريخ الصرف                                      |                                        |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: manufacturing_productions (إنتاج المنتج النهائي/تحت التشغيل)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| production_id         | INT (PK)           | رقم السطر                                        | PRIMARY KEY, AUTO_INCREMENT            |
| mo_id                 | INT (FK)           | رقم أمر التشغيل                                  | FK→manufacturing_orders.mo_id          |
| item_id               | INT (FK)           | المنتج النهائي/شبه النهائي                       | FK→items.item_id                       |
| quantity_planned      | DECIMAL(18,4)      | الكمية المخططة                                   |                                        |
| quantity_actual       | DECIMAL(18,4)      | الكمية الفعلية المنتجة                           |                                        |
| unit_id               | INT (FK)           | وحدة القياس                                      | FK→units.unit_id                       |
| warehouse_id          | INT (FK)           | المخزن المستقبل                                  | FK→warehouses.warehouse_id             |
| batch_id              | INT (FK)           | رقم الدفعة (إن وجد)                              | FK→stock_batches.batch_id               |
| cost_price            | DECIMAL(18,4)      | تكلفة الوحدة المنتجة                             |                                        |
| total_cost            | DECIMAL(18,4)      | إجمالي التكلفة                                   |                                        |
| production_date       | DATETIME           | تاريخ الإنتاج                                     |                                        |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: manufacturing_losses (الهالك والفاقد في التصنيع)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| loss_id               | INT (PK)           | رقم السطر                                        | PRIMARY KEY, AUTO_INCREMENT            |
| mo_id                 | INT (FK)           | رقم أمر التشغيل                                  | FK→manufacturing_orders.mo_id          |
| item_id               | INT (FK)           | المادة الفاقدة                                   | FK→items.item_id                       |
| quantity              | DECIMAL(18,4)      | الكمية المفقودة                                  |                                        |
| unit_id               | INT (FK)           | وحدة القياس                                      | FK→units.unit_id                       |
| warehouse_id          | INT (FK)           | المخزن                                            | FK→warehouses.warehouse_id             |
| reason                | VARCHAR(200)       | سبب الهالك/الفاقد                                |                                        |
| loss_date             | DATETIME           | تاريخ الفقد/الهالك                               |                                        |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: production_lines (خطوط الإنتاج)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| line_id               | INT (PK)           | رقم الخط                                         | PRIMARY KEY, AUTO_INCREMENT            |
| line_code             | VARCHAR(30)        | كود الخط                                         | UNIQUE, NOT NULL                       |
| name_ar               | VARCHAR(100)       | اسم الخط بالعربي                                 |                                        |
| name_en               | VARCHAR(100)       | اسم الخط بالإنجليزي                              |                                        |
| is_active             | BOOLEAN            | مفعل/غير مفعل                                    |                                        |
| branch_id             | INT (FK)           | الفرع                                            | FK→branches.branch_id                  |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: resources (الآلات والمعدات)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| resource_id           | INT (PK)           | رقم المورد التشغيلي                              | PRIMARY KEY, AUTO_INCREMENT            |
| resource_code         | VARCHAR(30)        | كود المورد التشغيلي                              | UNIQUE, NOT NULL                       |
| name_ar               | VARCHAR(100)       | الاسم بالعربي                                    |                                        |
| name_en               | VARCHAR(100)       | الاسم بالإنجليزي                                 |                                        |
| type                  | ENUM               | نوع المورد (آلة، عامل، معدات...)                 |                                        |
| is_active             | BOOLEAN            | مفعل/غير مفعل                                    |                                        |
| branch_id             | INT (FK)           | الفرع                                            | FK→branches.branch_id                  |
| capacity              | DECIMAL(18,4)      | الطاقة/القدرة                                    |                                        |
| unit_id               | INT (FK)           | وحدة الطاقة                                      | FK→units.unit_id                       |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: manufacturing_costs (تكاليف الإنتاج غير المباشرة)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| cost_id               | INT (PK)           | رقم السطر                                        | PRIMARY KEY, AUTO_INCREMENT            |
| mo_id                 | INT (FK)           | رقم أمر التشغيل                                  | FK→manufacturing_orders.mo_id          |
| cost_type             | ENUM               | نوع التكلفة (مواد مساعدة، عمالة، كهرباء، صيانة...)|                                        |
| amount                | DECIMAL(18,4)      | المبلغ                                           |                                        |
| description           | TEXT               | وصف إضافي                                        |                                        |
| cost_center_id        | INT (FK)           | مركز التكلفة                                     | FK→cost_centers.cost_center_id         |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## جدول: manufacturing_quality_checks (الفحص والجودة)
| الحقل                 | النوع              | الوصف                                            | قيود/علاقات                            |
|-----------------------|--------------------|--------------------------------------------------|----------------------------------------|
| qc_id                 | INT (PK)           | رقم الفحص                                        | PRIMARY KEY, AUTO_INCREMENT            |
| mo_id                 | INT (FK)           | رقم أمر التشغيل                                  | FK→manufacturing_orders.mo_id          |
| item_id               | INT (FK)           | الصنف المفحوص                                    | FK→items.item_id                       |
| sample_size           | DECIMAL(18,4)      | حجم العينة المفحوصة                              |                                        |
| passed_qty            | DECIMAL(18,4)      | كمية المقبول                                     |                                        |
| rejected_qty          | DECIMAL(18,4)      | كمية المرفوض                                     |                                        |
| qc_result             | ENUM               | النتيجة (مقبول/مرفوض/إعادة فحص)                 |                                        |
| qc_date               | DATETIME           | تاريخ الفحص                                       |                                        |
| inspector_id          | INT (FK)           | المفتش/المسؤول                                   | FK→employees.employee_id               |
| note                  | TEXT               | ملاحظات                                          |                                        |

---

## العلاقات الرئيسية في قسم التصنيع والإنتاج

- **bom_headers.product_item_id → items.item_id**
- **bom_lines.bom_id → bom_headers.bom_id**
- **bom_lines.item_id → items.item_id**
- **bom_lines.unit_id → units.unit_id**
- **manufacturing_orders.product_item_id → items.item_id**
- **manufacturing_orders.bom_id → bom_headers.bom_id**
- **manufacturing_orders.unit_id → units.unit_id**
- **manufacturing_orders.production_line_id → production_lines.line_id**
- **manufacturing_orders.cost_center_id → cost_centers.cost_center_id**
- **manufacturing_orders.project_id → projects.project_id**
- **manufacturing_orders.branch_id → branches.branch_id**
- **manufacturing_order_steps.mo_id → manufacturing_orders.mo_id**
- **manufacturing_order_steps.operator_id → employees.employee_id**
- **manufacturing_order_steps.resource_id → resources.resource_id**
- **manufacturing_consumptions.mo_id → manufacturing_orders.mo_id**
- **manufacturing_consumptions.item_id → items.item_id**
- **manufacturing_consumptions.unit_id → units.unit_id**
- **manufacturing_consumptions.warehouse_id → warehouses.warehouse_id**
- **manufacturing_consumptions.batch_id → stock_batches.batch_id**
- **manufacturing_productions.mo_id → manufacturing_orders.mo_id**
- **manufacturing_productions.item_id → items.item_id**
- **manufacturing_productions.unit_id → units.unit_id**
- **manufacturing_productions.warehouse_id → warehouses.warehouse_id**
- **manufacturing_productions.batch_id → stock_batches.batch_id**
- **manufacturing_losses.mo_id → manufacturing_orders.mo_id**
- **manufacturing_losses.item_id → items.item_id**
- **manufacturing_losses.unit_id → units.unit_id**
- **manufacturing_losses.warehouse_id → warehouses.warehouse_id**
- **production_lines.branch_id → branches.branch_id**
- **resources.branch_id → branches.branch_id**
- **resources.unit_id → units.unit_id**
- **manufacturing_costs.mo_id → manufacturing_orders.mo_id**
- **manufacturing_costs.cost_center_id → cost_centers.cost_center_id**
- **manufacturing_quality_checks.mo_id → manufacturing_orders.mo_id**
- **manufacturing_quality_checks.item_id → items.item_id**
- **manufacturing_quality_checks.inspector_id → employees.employee_id**

---

## ملاحظات عملية

- كل أمر تصنيع مرتبط بقائمة مواد (BOM) وإمكانية التعدد للإصدارات.
- تدعم أوامر التصنيع المراحل التشغيلية، خطوط الإنتاج، المشاريع، التكاليف المباشرة وغير المباشرة.
- يدعم النظام تتبع كامل للمواد الخام، المنتج النهائي، الهالك، الدُفعات، الجودة.
- كل العمليات قابلة للربط بالمخزون (صرف خامات، استلام منتج نهائي)، المالية (قيود تلقائية)، المشاريع، مراكز التكلفة.
- جداول الجودة والهالك تدعم التحليل التفصيلي للفاقد وتحسين العمليات الإنتاجية.

---