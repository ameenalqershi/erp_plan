# قسم المشتريات – تحليل جداول قاعدة البيانات

---

## جدول: purchase_requests (طلبات الشراء)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                         |
|-----------------------|--------------------|----------------------------------|-------------------------------------|
| request_id            | INT (PK)           | رقم الطلب                        | PRIMARY KEY, AUTO_INCREMENT         |
| request_number        | VARCHAR(30)        | رقم الطلب (تسلسلي/يدوي)          | UNIQUE, NOT NULL                    |
| requested_by          | INT (FK)           | المستخدم مقدم الطلب              | FK→users.user_id                    |
| department_id         | INT (FK)           | القسم طالب الشراء                 | FK→departments.department_id        |
| request_date          | DATE               | تاريخ الطلب                       | NOT NULL                            |
| priority              | ENUM               | أولوية (عالية/متوسطة/عادية)      |                                     |
| status                | ENUM               | (مسودة، تحت الموافقة، معتمد، ملغي)|                                     |
| note                  | TEXT               | ملاحظات                          |                                     |
| branch_id             | INT (FK)           | الفرع                            | FK→branches.branch_id               |
| created_at            | DATETIME           | تاريخ الإنشاء                     |                                     |
| updated_at            | DATETIME           | تاريخ آخر تحديث                   |                                     |

---

## جدول: purchase_request_lines (تفاصيل طلب الشراء)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                            |
|-----------------------|--------------------|----------------------------------|----------------------------------------|
| line_id               | INT (PK)           | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT            |
| request_id            | INT (FK)           | رقم الطلب                        | FK→purchase_requests.request_id        |
| item_id               | INT (FK)           | رقم الصنف                        | FK→items.item_id                       |
| description           | VARCHAR(300)       | وصف الصنف                        |                                         |
| quantity              | DECIMAL(18,4)      | الكمية المطلوبة                  |                                         |
| unit_id               | INT (FK)           | وحدة القياس                      | FK→units.unit_id                       |
| required_date         | DATE               | تاريخ الحاجة                      |                                         |
| note                  | TEXT               | ملاحظات                          |                                         |

---

## جدول: purchase_rfq (استفسار عروض الأسعار)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                              |
|-----------------------|--------------------|----------------------------------|------------------------------------------|
| rfq_id                | INT (PK)           | رقم الاستفسار                    | PRIMARY KEY, AUTO_INCREMENT              |
| rfq_number            | VARCHAR(30)        | رقم الاستفسار (تسلسلي/يدوي)      | UNIQUE, NOT NULL                         |
| request_id            | INT (FK)           | رقم طلب الشراء (إن وجد)           | FK→purchase_requests.request_id           |
| sent_date             | DATE               | تاريخ إرسال الاستفسار             |                                          |
| status                | ENUM               | (مسودة، أرسلت، مغلقة، ملغاة)      |                                          |
| note                  | TEXT               | ملاحظات                          |                                          |
| branch_id             | INT (FK)           | الفرع                            | FK→branches.branch_id                    |
| created_by            | INT (FK)           | المستخدم                          | FK→users.user_id                         |
| created_at            | DATETIME           | تاريخ الإنشاء                     |                                          |

---

## جدول: purchase_rfq_suppliers (موردو عروض الأسعار)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                 |
|-----------------------|--------------------|----------------------------------|---------------------------------------------|
| rfq_supplier_id       | INT (PK)           | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT                 |
| rfq_id                | INT (FK)           | رقم الاستفسار                    | FK→purchase_rfq.rfq_id                      |
| supplier_id           | INT (FK)           | رقم المورد                        | FK→suppliers.supplier_id                    |
| sent_date             | DATE               | تاريخ إرسال الاستفسار             |                                             |
| received_date         | DATE               | تاريخ استلام العرض                |                                             |
| status                | ENUM               | (بانتظار عرض، استلم العرض، تجاهل)|                                             |
| note                  | TEXT               | ملاحظات                          |                                             |

---

## جدول: purchase_quotations (عروض الموردين)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                              |
|-----------------------|--------------------|----------------------------------|------------------------------------------|
| quotation_id          | INT (PK)           | رقم عرض المورد                    | PRIMARY KEY, AUTO_INCREMENT              |
| rfq_id                | INT (FK)           | رقم الاستفسار                    | FK→purchase_rfq.rfq_id                   |
| supplier_id           | INT (FK)           | رقم المورد                        | FK→suppliers.supplier_id                 |
| quotation_number      | VARCHAR(30)        | رقم العرض                        | UNIQUE, NOT NULL                         |
| quotation_date        | DATE               | تاريخ العرض                       |                                          |
| status                | ENUM               | (مسودة، معتمد، مرفوض، منتهي)     |                                          |
| valid_until           | DATE               | تاريخ نهاية الصلاحية              |                                          |
| note                  | TEXT               | ملاحظات                          |                                          |
| branch_id             | INT (FK)           | الفرع                            | FK→branches.branch_id                    |
| created_at            | DATETIME           | تاريخ الإنشاء                     |                                          |

---

## جدول: purchase_quotation_lines (تفاصيل عرض المورد)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------|--------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT                |
| quotation_id          | INT (FK)           | رقم عرض المورد                    | FK→purchase_quotations.quotation_id        |
| item_id               | INT (FK)           | رقم الصنف                        | FK→items.item_id                           |
| description           | VARCHAR(300)       | وصف إضافي                        |                                            |
| quantity              | DECIMAL(18,4)      | الكمية                            |                                            |
| unit_id               | INT (FK)           | وحدة القياس                      | FK→units.unit_id                           |
| unit_price            | DECIMAL(18,4)      | سعر الوحدة                        |                                            |
| discount_amount       | DECIMAL(18,2)      | الخصم                             |                                            |
| tax_amount            | DECIMAL(18,2)      | الضريبة                           |                                            |
| total_line            | DECIMAL(18,2)      | الإجمالي                          |                                            |

---

## جدول: purchase_orders (أوامر الشراء)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------|--------------------------------------------|
| order_id              | INT (PK)           | رقم أمر الشراء                    | PRIMARY KEY, AUTO_INCREMENT                |
| order_number          | VARCHAR(30)        | رقم الأمر (تسلسلي/يدوي)           | UNIQUE, NOT NULL                           |
| supplier_id           | INT (FK)           | رقم المورد                        | FK→suppliers.supplier_id                   |
| rfq_id                | INT (FK)           | رقم الاستفسار                    | FK→purchase_rfq.rfq_id                     |
| quotation_id          | INT (FK)           | رقم عرض المورد                    | FK→purchase_quotations.quotation_id        |
| order_date            | DATE               | تاريخ الأمر                       |                                            |
| expected_delivery     | DATE               | تاريخ التسليم المتوقع              |                                            |
| status                | ENUM               | (مسودة، معتمد، مكتمل، ملغي)       |                                            |
| total_amount          | DECIMAL(18,2)      | إجمالي الأمر                      |                                            |
| currency_id           | INT (FK)           | العملة                            | FK→currencies.currency_id                  |
| discount_amount       | DECIMAL(18,2)      | إجمالي الخصومات                   |                                            |
| tax_amount            | DECIMAL(18,2)      | إجمالي الضرائب                    |                                            |
| note                  | TEXT               | ملاحظات                          |                                            |
| branch_id             | INT (FK)           | الفرع                            | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                  | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                     |                                            |
| updated_by            | INT (FK)           | المستخدم المعدّل                  | FK→users.user_id                           |
| updated_at            | DATETIME           | تاريخ آخر تعديل                    |                                            |

---

## جدول: purchase_order_lines (تفاصيل أمر الشراء)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                 |
|-----------------------|--------------------|----------------------------------|---------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT                 |
| order_id              | INT (FK)           | رقم أمر الشراء                   | FK→purchase_orders.order_id                  |
| item_id               | INT (FK)           | رقم الصنف                        | FK→items.item_id                            |
| description           | VARCHAR(300)       | وصف إضافي                        |                                             |
| quantity              | DECIMAL(18,4)      | الكمية                            |                                             |
| unit_id               | INT (FK)           | وحدة القياس                      | FK→units.unit_id                            |
| unit_price            | DECIMAL(18,4)      | سعر الوحدة                        |                                             |
| discount_amount       | DECIMAL(18,2)      | الخصم                             |                                             |
| tax_amount            | DECIMAL(18,2)      | الضريبة                           |                                             |
| total_line            | DECIMAL(18,2)      | الإجمالي بعد الخصم والضريبة        |                                             |
| delivery_status       | ENUM               | (بانتظار، تم التسليم، ملغي)        |                                             |
| warehouse_id          | INT (FK)           | المخزن المستلم                    | FK→warehouses.warehouse_id                  |

---

## جدول: purchase_receipts (إذن استلام أصناف)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------|--------------------------------------------|
| receipt_id            | INT (PK)           | رقم إذن الاستلام                  | PRIMARY KEY, AUTO_INCREMENT                |
| receipt_number        | VARCHAR(30)        | رقم الإذن (تسلسلي/يدوي)           | UNIQUE, NOT NULL                           |
| order_id              | INT (FK)           | رقم أمر الشراء                    | FK→purchase_orders.order_id                 |
| supplier_id           | INT (FK)           | رقم المورد                        | FK→suppliers.supplier_id                   |
| receipt_date          | DATE               | تاريخ الاستلام                    |                                            |
| received_by           | INT (FK)           | موظف الاستلام                     | FK→employees.employee_id                    |
| status                | ENUM               | (مسودة، مكتمل، ملغي)              |                                            |
| note                  | TEXT               | ملاحظات                          |                                            |
| branch_id             | INT (FK)           | الفرع                            | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                  | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                     |                                            |

---

## جدول: purchase_receipt_lines (تفاصيل إذن الاستلام)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                 |
|-----------------------|--------------------|----------------------------------|---------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT                 |
| receipt_id            | INT (FK)           | رقم إذن الاستلام                  | FK→purchase_receipts.receipt_id             |
| item_id               | INT (FK)           | رقم الصنف                        | FK→items.item_id                            |
| quantity              | DECIMAL(18,4)      | الكمية المستلمة                   |                                             |
| unit_id               | INT (FK)           | وحدة القياس                      | FK→units.unit_id                            |
| warehouse_id          | INT (FK)           | المخزن المستلم                    | FK→warehouses.warehouse_id                  |
| batch_id              | INT (FK)           | رقم الدفعة (إن وجد)               | FK→stock_batches.batch_id                   |
| quality_status        | ENUM               | (مطابق، جزئي، مرفوض)              |                                             |
| note                  | TEXT               | ملاحظات                          |                                             |

---

## جدول: purchase_invoices (فواتير المشتريات)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------|--------------------------------------------|
| invoice_id            | INT (PK)           | رقم الفاتورة                      | PRIMARY KEY, AUTO_INCREMENT                |
| invoice_number        | VARCHAR(30)        | رقم الفاتورة (تسلسلي/يدوي)        | UNIQUE, NOT NULL                           |
| order_id              | INT (FK)           | رقم أمر الشراء                    | FK→purchase_orders.order_id                 |
| supplier_id           | INT (FK)           | رقم المورد                        | FK→suppliers.supplier_id                   |
| invoice_date          | DATE               | تاريخ الفاتورة                    |                                            |
| due_date              | DATE               | تاريخ الاستحقاق                   |                                            |
| status                | ENUM               | (مسودة، معتمد، مدفوعة، ملغي)      |                                            |
| total_amount          | DECIMAL(18,2)      | إجمالي الفاتورة                   |                                            |
| currency_id           | INT (FK)           | العملة                            | FK→currencies.currency_id                  |
| discount_amount       | DECIMAL(18,2)      | إجمالي الخصومات                   |                                            |
| tax_amount            | DECIMAL(18,2)      | إجمالي الضرائب                    |                                            |
| paid_amount           | DECIMAL(18,2)      | إجمالي المدفوع                    |                                            |
| remaining_amount      | DECIMAL(18,2)      | المتبقي                            |                                            |
| note                  | TEXT               | ملاحظات                          |                                            |
| branch_id             | INT (FK)           | الفرع                            | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                  | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                     |                                            |
| updated_by            | INT (FK)           | المستخدم المعدّل                  | FK→users.user_id                           |
| updated_at            | DATETIME           | تاريخ آخر تعديل                    |                                            |

---

## جدول: purchase_invoice_lines (تفاصيل فاتورة المشتريات)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                 |
|-----------------------|--------------------|----------------------------------|---------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT                 |
| invoice_id            | INT (FK)           | رقم الفاتورة                      | FK→purchase_invoices.invoice_id             |
| item_id               | INT (FK)           | رقم الصنف                        | FK→items.item_id                            |
| description           | VARCHAR(300)       | وصف إضافي                        |                                             |
| quantity              | DECIMAL(18,4)      | الكمية                            |                                             |
| unit_id               | INT (FK)           | وحدة القياس                      | FK→units.unit_id                            |
| unit_price            | DECIMAL(18,4)      | سعر الوحدة                        |                                             |
| discount_amount       | DECIMAL(18,2)      | الخصم                             |                                             |
| tax_amount            | DECIMAL(18,2)      | الضريبة                           |                                             |
| total_line            | DECIMAL(18,2)      | الإجمالي بعد الخصم والضريبة        |                                             |
| warehouse_id          | INT (FK)           | المخزن                            | FK→warehouses.warehouse_id                  |
| batch_id              | INT (FK)           | رقم الدفعة (إن وجد)               | FK→stock_batches.batch_id                   |

---

## جدول: purchase_returns (مرتجعات المشتريات)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------|--------------------------------------------|
| return_id             | INT (PK)           | رقم المرتجع                       | PRIMARY KEY, AUTO_INCREMENT                |
| return_number         | VARCHAR(30)        | رقم المرتجع (تسلسلي/يدوي)         | UNIQUE, NOT NULL                           |
| invoice_id            | INT (FK)           | رقم الفاتورة                      | FK→purchase_invoices.invoice_id            |
| supplier_id           | INT (FK)           | رقم المورد                        | FK→suppliers.supplier_id                   |
| return_date           | DATE               | تاريخ المرتجع                      |                                            |
| status                | ENUM               | (مسودة، معتمد، ملغي)              |                                            |
| total_amount          | DECIMAL(18,2)      | إجمالي المرتجع                     |                                            |
| currency_id           | INT (FK)           | العملة                            | FK→currencies.currency_id                  |
| note                  | TEXT               | ملاحظات                          |                                            |
| branch_id             | INT (FK)           | الفرع                            | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                  | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                     |                                            |

---

## جدول: purchase_return_lines (تفاصيل مرتجع المشتريات)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                 |
|-----------------------|--------------------|----------------------------------|---------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                        | PRIMARY KEY, AUTO_INCREMENT                 |
| return_id             | INT (FK)           | رقم المرتجع                       | FK→purchase_returns.return_id               |
| item_id               | INT (FK)           | رقم الصنف                        | FK→items.item_id                            |
| quantity              | DECIMAL(18,4)      | الكمية المرتجعة                   |                                             |
| unit_id               | INT (FK)           | وحدة القياس                      | FK→units.unit_id                            |
| unit_price            | DECIMAL(18,4)      | سعر الوحدة                        |                                             |
| warehouse_id          | INT (FK)           | المخزن                            | FK→warehouses.warehouse_id                  |
| batch_id              | INT (FK)           | رقم الدفعة (إن وجد)               | FK→stock_batches.batch_id                   |
| note                  | TEXT               | ملاحظات                          |                                             |

---

## جدول: purchase_payments (سندات الصرف)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------|--------------------------------------------|
| payment_id            | INT (PK)           | رقم السند                         | PRIMARY KEY, AUTO_INCREMENT                |
| payment_number        | VARCHAR(30)        | رقم السند (تسلسلي/يدوي)           | UNIQUE, NOT NULL                           |
| supplier_id           | INT (FK)           | رقم المورد                        | FK→suppliers.supplier_id                   |
| invoice_id            | INT (FK)           | رقم الفاتورة                      | FK→purchase_invoices.invoice_id            |
| payment_date          | DATE               | تاريخ الصرف                        |                                            |
| payment_method        | ENUM               | (نقدي، شيك، تحويل، بطاقة...)      |                                            |
| amount                | DECIMAL(18,2)      | المبلغ                             |                                            |
| currency_id           | INT (FK)           | العملة                            | FK→currencies.currency_id                  |
| bank_account_id       | INT (FK)           | الحساب البنكي الدافع (إن وجد)      | FK→bank_accounts.bank_account_id           |
| cash_box_id           | INT (FK)           | الصندوق الدافع (إن وجد)           | FK→cash_boxes.cash_box_id                  |
| status                | ENUM               | (مسودة، معتمد، ملغي)              |                                            |
| note                  | TEXT               | ملاحظات                          |                                            |
| branch_id             | INT (FK)           | الفرع                            | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                  | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                     |                                            |

---

## جدول: purchase_discounts (خصومات الموردين)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------|--------------------------------------------|
| discount_id           | INT (PK)           | رقم الخصم/العرض                   | PRIMARY KEY, AUTO_INCREMENT                |
| name_ar               | VARCHAR(100)       | اسم العرض بالعربي                 |                                            |
| name_en               | VARCHAR(100)       | اسم العرض بالإنجليزي              |                                            |
| discount_type         | ENUM               | (نسبة، مبلغ ثابت، شريحة كمية ...) |                                            |
| value                 | DECIMAL(18,4)      | القيمة أو النسبة                  |                                            |
| start_date            | DATE               | بداية العرض                       |                                            |
| end_date              | DATE               | نهاية العرض                       |                                            |
| is_active             | BOOLEAN            | مفعل/غير مفعل                     |                                            |
| apply_on              | ENUM               | (فاتورة، صنف، مورد، مجموعة ...)   |                                            |
| note                  | TEXT               | ملاحظات                          |                                            |

---

## جدول: purchase_taxes (الضرائب على المشتريات)
| الحقل                 | النوع              | الوصف                            | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------|--------------------------------------------|
| tax_id                | INT (PK)           | رقم الضريبة                       | PRIMARY KEY, AUTO_INCREMENT                |
| name_ar               | VARCHAR(100)       | اسم الضريبة بالعربي               |                                            |
| name_en               | VARCHAR(100)       | اسم الضريبة بالإنجليزي            |                                            |
| tax_type              | ENUM               | (قيمة مضافة، جمارك، رسم خدمة ...) |                                            |
| percentage            | DECIMAL(5,2)       | النسبة                            |                                            |
| fixed_amount          | DECIMAL(18,2)      | مبلغ ثابت                         |                                            |
| start_date            | DATE               | بداية التطبيق                     |                                            |
| end_date              | DATE               | نهاية التطبيق                     |                                            |
| is_active             | BOOLEAN            | مفعل/غير مفعل                     |                                            |
| apply_on              | ENUM               | (فاتورة، صنف، مورد، مجموعة ...)   |                                            |
| note                  | TEXT               | ملاحظات                          |                                            |

---

## العلاقات الرئيسية في قسم المشتريات

- **purchase_requests.department_id → departments.department_id**
- **purchase_requests.branch_id → branches.branch_id**
- **purchase_request_lines.request_id → purchase_requests.request_id**
- **purchase_request_lines.item_id → items.item_id**
- **purchase_request_lines.unit_id → units.unit_id**
- **purchase_rfq.request_id → purchase_requests.request_id**
- **purchase_rfq.branch_id → branches.branch_id**
- **purchase_rfq_suppliers.rfq_id → purchase_rfq.rfq_id**
- **purchase_rfq_suppliers.supplier_id → suppliers.supplier_id**
- **purchase_quotations.rfq_id → purchase_rfq.rfq_id**
- **purchase_quotations.supplier_id → suppliers.supplier_id**
- **purchase_quotation_lines.quotation_id → purchase_quotations.quotation_id**
- **purchase_quotation_lines.item_id → items.item_id**
- **purchase_quotation_lines.unit_id → units.unit_id**
- **purchase_orders.supplier_id → suppliers.supplier_id**
- **purchase_orders.rfq_id → purchase_rfq.rfq_id**
- **purchase_orders.quotation_id → purchase_quotations.quotation_id**
- **purchase_orders.branch_id → branches.branch_id**
- **purchase_order_lines.order_id → purchase_orders.order_id**
- **purchase_order_lines.item_id → items.item_id**
- **purchase_order_lines.unit_id → units.unit_id**
- **purchase_order_lines.warehouse_id → warehouses.warehouse_id**
- **purchase_receipts.order_id → purchase_orders.order_id**
- **purchase_receipts.supplier_id → suppliers.supplier_id**
- **purchase_receipts.received_by → employees.employee_id**
- **purchase_receipts.branch_id → branches.branch_id**
- **purchase_receipt_lines.receipt_id → purchase_receipts.receipt_id**
- **purchase_receipt_lines.item_id → items.item_id**
- **purchase_receipt_lines.unit_id → units.unit_id**
- **purchase_receipt_lines.warehouse_id → warehouses.warehouse_id**
- **purchase_receipt_lines.batch_id → stock_batches.batch_id**
- **purchase_invoices.order_id → purchase_orders.order_id**
- **purchase_invoices.supplier_id → suppliers.supplier_id**
- **purchase_invoices.branch_id → branches.branch_id**
- **purchase_invoice_lines.invoice_id → purchase_invoices.invoice_id**
- **purchase_invoice_lines.item_id → items.item_id**
- **purchase_invoice_lines.unit_id → units.unit_id**
- **purchase_invoice_lines.warehouse_id → warehouses.warehouse_id**
- **purchase_invoice_lines.batch_id → stock_batches.batch_id**
- **purchase_returns.invoice_id → purchase_invoices.invoice_id**
- **purchase_returns.supplier_id → suppliers.supplier_id**
- **purchase_returns.branch_id → branches.branch_id**
- **purchase_return_lines.return_id → purchase_returns.return_id**
- **purchase_return_lines.item_id → items.item_id**
- **purchase_return_lines.unit_id → units.unit_id**
- **purchase_return_lines.warehouse_id → warehouses.warehouse_id**
- **purchase_return_lines.batch_id → stock_batches.batch_id**
- **purchase_payments.supplier_id → suppliers.supplier_id**
- **purchase_payments.invoice_id → purchase_invoices.invoice_id**
- **purchase_payments.bank_account_id → bank_accounts.bank_account_id**
- **purchase_payments.cash_box_id → cash_boxes.cash_box_id**

---

## ملاحظات عامة

- تدعم جميع جداول المشتريات التدقيق (تاريخ الإنشاء/التعديل، المستخدم)، الفروع، العملات، الربط مع الموردين والمخازن.
- دورة المشتريات تدعم: طلبات الشراء، استفسار عروض أسعار، عروض الموردين، أوامر الشراء، الاستلام، الفواتير، المرتجعات، سندات الصرف، الخصومات، الضرائب.
- يدعم النظام التعددية في الموردين، الأصناف، الوحدات، العروض، الدُفعات، الربط مع المالية والمخزون.
- جميع العمليات قابلة للربط بنظام الموافقات وسجلات التدقيق والمرفقات.

---