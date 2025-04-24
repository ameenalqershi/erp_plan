# قسم المبيعات – تحليل جداول قاعدة البيانات

---

## جدول: sales_quotations (عروض الأسعار)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| quotation_id          | INT (PK)           | رقم عرض السعر                                      | PRIMARY KEY, AUTO_INCREMENT                |
| quotation_number      | VARCHAR(30)        | رقم العرض (تسلسلي/يدوي)                           | UNIQUE, NOT NULL                           |
| customer_id           | INT (FK)           | رقم العميل                                         | FK→customers.customer_id                   |
| date_created          | DATE               | تاريخ إنشاء العرض                                  |                                            |
| valid_until           | DATE               | تاريخ انتهاء الصلاحية                              |                                            |
| status                | ENUM               | (مسودة، معتمد، ملغي، منتهٍ)                        |                                            |
| total_amount          | DECIMAL(18,2)      | إجمالي العرض                                       |                                            |
| currency_id           | INT (FK)           | العملة                                             | FK→currencies.currency_id                  |
| discount_amount       | DECIMAL(18,2)      | إجمالي الخصومات                                    |                                            |
| tax_amount            | DECIMAL(18,2)      | إجمالي الضرائب                                     |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |
| branch_id             | INT (FK)           | الفرع                                              | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                                   | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                                      |                                            |
| updated_by            | INT (FK)           | المستخدم المعدّل                                   | FK→users.user_id                           |
| updated_at            | DATETIME           | تاريخ آخر تعديل                                     |                                            |

---

## جدول: sales_quotation_lines (تفاصيل عرض السعر)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                                          | PRIMARY KEY, AUTO_INCREMENT                |
| quotation_id          | INT (FK)           | رقم عرض السعر                                      | FK→sales_quotations.quotation_id           |
| item_id               | INT (FK)           | رقم الصنف                                          | FK→items.item_id                           |
| description           | VARCHAR(300)       | وصف إضافي                                          |                                            |
| quantity              | DECIMAL(18,4)      | الكمية                                             |                                            |
| unit_id               | INT (FK)           | وحدة القياس                                        | FK→units.unit_id                           |
| unit_price            | DECIMAL(18,4)      | سعر الوحدة                                         |                                            |
| discount_amount       | DECIMAL(18,2)      | الخصم على السطر                                    |                                            |
| tax_amount            | DECIMAL(18,2)      | ضريبة السطر                                        |                                            |
| total_line            | DECIMAL(18,2)      | الإجمالي بعد الخصم والضريبة                        |                                            |

---

## جدول: sales_orders (أوامر البيع)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| order_id              | INT (PK)           | رقم أمر البيع                                      | PRIMARY KEY, AUTO_INCREMENT                |
| order_number          | VARCHAR(30)        | رقم الأمر (تسلسلي/يدوي)                            | UNIQUE, NOT NULL                           |
| quotation_id          | INT (FK)           | رقم عرض السعر المصدر                               | FK→sales_quotations.quotation_id           |
| customer_id           | INT (FK)           | رقم العميل                                         | FK→customers.customer_id                   |
| order_date            | DATE               | تاريخ الأمر                                        |                                            |
| delivery_date         | DATE               | تاريخ التسليم المتوقع                              |                                            |
| status                | ENUM               | (مسودة، معتمد، ملغي، مكتمل جزئياً/كلياً)           |                                            |
| total_amount          | DECIMAL(18,2)      | إجمالي الأمر                                       |                                            |
| currency_id           | INT (FK)           | العملة                                             | FK→currencies.currency_id                  |
| discount_amount       | DECIMAL(18,2)      | إجمالي الخصومات                                    |                                            |
| tax_amount            | DECIMAL(18,2)      | إجمالي الضرائب                                     |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |
| branch_id             | INT (FK)           | الفرع                                              | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                                   | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                                      |                                            |
| updated_by            | INT (FK)           | المستخدم المعدّل                                   | FK→users.user_id                           |
| updated_at            | DATETIME           | تاريخ آخر تعديل                                     |                                            |

---

## جدول: sales_order_lines (تفاصيل أمر البيع)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                                          | PRIMARY KEY, AUTO_INCREMENT                |
| order_id              | INT (FK)           | رقم أمر البيع                                      | FK→sales_orders.order_id                   |
| item_id               | INT (FK)           | رقم الصنف                                          | FK→items.item_id                           |
| description           | VARCHAR(300)       | وصف إضافي                                          |                                            |
| quantity              | DECIMAL(18,4)      | الكمية                                             |                                            |
| unit_id               | INT (FK)           | وحدة القياس                                        | FK→units.unit_id                           |
| unit_price            | DECIMAL(18,4)      | سعر الوحدة                                         |                                            |
| discount_amount       | DECIMAL(18,2)      | الخصم على السطر                                    |                                            |
| tax_amount            | DECIMAL(18,2)      | ضريبة السطر                                        |                                            |
| total_line            | DECIMAL(18,2)      | الإجمالي بعد الخصم والضريبة                        |                                            |
| delivery_status       | ENUM               | (قيد التحضير، جاهز، تم التسليم، ملغي)              |                                            |
| warehouse_id          | INT (FK)           | المخزن المصدر                                      | FK→warehouses.warehouse_id                 |

---

## جدول: sales_deliveries (إذن صرف/تسليم البضاعة)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| delivery_id           | INT (PK)           | رقم إذن التسليم                                    | PRIMARY KEY, AUTO_INCREMENT                |
| delivery_number       | VARCHAR(30)        | رقم الإذن (تسلسلي/يدوي)                            | UNIQUE, NOT NULL                           |
| order_id              | INT (FK)           | رقم أمر البيع                                      | FK→sales_orders.order_id                   |
| delivery_date         | DATE               | تاريخ التسليم                                      |                                            |
| delivered_by          | INT (FK)           | المسؤول عن التسليم (موظف)                          | FK→employees.employee_id                   |
| status                | ENUM               | (جاري، مكتمل، ملغي)                                |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |
| branch_id             | INT (FK)           | الفرع                                              | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                                   | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                                      |                                            |

---

## جدول: sales_delivery_lines (تفاصيل إذن التسليم)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                                          | PRIMARY KEY, AUTO_INCREMENT                |
| delivery_id           | INT (FK)           | رقم الإذن                                          | FK→sales_deliveries.delivery_id            |
| item_id               | INT (FK)           | رقم الصنف                                          | FK→items.item_id                           |
| quantity              | DECIMAL(18,4)      | الكمية المسلمة                                     |                                            |
| unit_id               | INT (FK)           | وحدة القياس                                        | FK→units.unit_id                           |
| warehouse_id          | INT (FK)           | المخزن المصدر                                      | FK→warehouses.warehouse_id                 |
| batch_id              | INT (FK)           | رقم الدفعة (إن وجد)                                | FK→stock_batches.batch_id                  |
| note                  | TEXT               | ملاحظات                                            |                                            |

---

## جدول: sales_invoices (فواتير البيع)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| invoice_id            | INT (PK)           | رقم الفاتورة                                       | PRIMARY KEY, AUTO_INCREMENT                |
| invoice_number        | VARCHAR(30)        | رقم الفاتورة (تسلسلي/يدوي)                         | UNIQUE, NOT NULL                           |
| order_id              | INT (FK)           | رقم أمر البيع المصدر                                | FK→sales_orders.order_id                   |
| customer_id           | INT (FK)           | رقم العميل                                         | FK→customers.customer_id                   |
| invoice_date          | DATE               | تاريخ الفاتورة                                     |                                            |
| due_date              | DATE               | تاريخ الاستحقاق                                    |                                            |
| status                | ENUM               | (مسودة، معتمد، مدفوعة، جزئي، ملغي)                 |                                            |
| total_amount          | DECIMAL(18,2)      | إجمالي الفاتورة                                    |                                            |
| currency_id           | INT (FK)           | العملة                                             | FK→currencies.currency_id                  |
| discount_amount       | DECIMAL(18,2)      | إجمالي الخصومات                                    |                                            |
| tax_amount            | DECIMAL(18,2)      | إجمالي الضرائب                                     |                                            |
| paid_amount           | DECIMAL(18,2)      | إجمالي المدفوع                                     |                                            |
| remaining_amount      | DECIMAL(18,2)      | المتبقي                                            |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |
| branch_id             | INT (FK)           | الفرع                                              | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                                   | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                                      |                                            |
| updated_by            | INT (FK)           | المستخدم المعدّل                                   | FK→users.user_id                           |
| updated_at            | DATETIME           | تاريخ آخر تعديل                                     |                                            |
| reference_type        | VARCHAR(50)        | نوع المرجع (POS، تقسيط، إلكترونية...)              |                                            |
| reference_id          | VARCHAR(50)        | رقم المرجع أو الإشعار                              |                                            |

---

## جدول: sales_invoice_lines (تفاصيل الفاتورة)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                                          | PRIMARY KEY, AUTO_INCREMENT                |
| invoice_id            | INT (FK)           | رقم الفاتورة                                      | FK→sales_invoices.invoice_id               |
| item_id               | INT (FK)           | رقم الصنف                                          | FK→items.item_id                           |
| description           | VARCHAR(300)       | وصف إضافي                                          |                                            |
| quantity              | DECIMAL(18,4)      | الكمية                                             |                                            |
| unit_id               | INT (FK)           | وحدة القياس                                        | FK→units.unit_id                           |
| unit_price            | DECIMAL(18,4)      | سعر الوحدة                                         |                                            |
| discount_amount       | DECIMAL(18,2)      | الخصم على السطر                                    |                                            |
| tax_amount            | DECIMAL(18,2)      | ضريبة السطر                                        |                                            |
| total_line            | DECIMAL(18,2)      | الإجمالي بعد الخصم والضريبة                        |                                            |
| warehouse_id          | INT (FK)           | المخزن المصدر                                      | FK→warehouses.warehouse_id                 |
| batch_id              | INT (FK)           | رقم الدفعة (إن وجد)                                | FK→stock_batches.batch_id                  |
| delivery_status       | ENUM               | (لم تُسلّم، قيد التسليم، تم التسليم)               |                                            |

---

## جدول: sales_returns (مرتجعات المبيعات)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| return_id             | INT (PK)           | رقم المرتجع                                        | PRIMARY KEY, AUTO_INCREMENT                |
| return_number         | VARCHAR(30)        | رقم المرتجع (تسلسلي/يدوي)                          | UNIQUE, NOT NULL                           |
| invoice_id            | INT (FK)           | رقم الفاتورة المصدر                                | FK→sales_invoices.invoice_id               |
| customer_id           | INT (FK)           | رقم العميل                                         | FK→customers.customer_id                   |
| return_date           | DATE               | تاريخ المرتجع                                      |                                            |
| status                | ENUM               | (مسودة، معتمد، ملغي)                               |                                            |
| total_amount          | DECIMAL(18,2)      | إجمالي المرتجع                                     |                                            |
| currency_id           | INT (FK)           | العملة                                             | FK→currencies.currency_id                  |
| note                  | TEXT               | ملاحظات                                            |                                            |
| branch_id             | INT (FK)           | الفرع                                              | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                                   | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                                      |                                            |

---

## جدول: sales_return_lines (تفاصيل مرتجع المبيعات)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| line_id               | INT (PK)           | رقم السطر                                          | PRIMARY KEY, AUTO_INCREMENT                |
| return_id             | INT (FK)           | رقم المرتجع                                        | FK→sales_returns.return_id                 |
| item_id               | INT (FK)           | رقم الصنف                                          | FK→items.item_id                           |
| quantity              | DECIMAL(18,4)      | الكمية المرتجعة                                    |                                            |
| unit_id               | INT (FK)           | وحدة القياس                                        | FK→units.unit_id                           |
| unit_price            | DECIMAL(18,4)      | سعر الوحدة                                         |                                            |
| warehouse_id          | INT (FK)           | المخزن المستلم                                     | FK→warehouses.warehouse_id                 |
| batch_id              | INT (FK)           | رقم الدفعة (إن وجد)                                | FK→stock_batches.batch_id                  |
| note                  | TEXT               | ملاحظات                                            |                                            |

---

## جدول: sales_payments (سندات القبض)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| payment_id            | INT (PK)           | رقم السند                                          | PRIMARY KEY, AUTO_INCREMENT                |
| payment_number        | VARCHAR(30)        | رقم السند (تسلسلي/يدوي)                            | UNIQUE, NOT NULL                           |
| customer_id           | INT (FK)           | رقم العميل                                         | FK→customers.customer_id                   |
| invoice_id            | INT (FK)           | رقم الفاتورة                                       | FK→sales_invoices.invoice_id               |
| payment_date          | DATE               | تاريخ السداد                                       |                                            |
| payment_method        | ENUM               | (نقدي، شيك، تحويل، بطاقة...)                      |                                            |
| amount                | DECIMAL(18,2)      | المبلغ                                             |                                            |
| currency_id           | INT (FK)           | العملة                                             | FK→currencies.currency_id                  |
| bank_account_id       | INT (FK)           | الحساب البنكي المستقبل (إن وجد)                   | FK→bank_accounts.bank_account_id           |
| cash_box_id           | INT (FK)           | الصندوق المستقبل (إن وجد)                         | FK→cash_boxes.cash_box_id                  |
| status                | ENUM               | (مسودة، معتمد، ملغي)                              |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |
| branch_id             | INT (FK)           | الفرع                                              | FK→branches.branch_id                      |
| created_by            | INT (FK)           | المستخدم المُنشئ                                   | FK→users.user_id                           |
| created_at            | DATETIME           | تاريخ الإنشاء                                      |                                            |

---

## جدول: sales_discounts (الخصومات والعروض)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| discount_id           | INT (PK)           | رقم الخصم/العرض                                    | PRIMARY KEY, AUTO_INCREMENT                |
| name_ar               | VARCHAR(100)       | اسم العرض بالعربي                                  |                                            |
| name_en               | VARCHAR(100)       | اسم العرض بالإنجليزي                               |                                            |
| discount_type         | ENUM               | (نسبة، مبلغ ثابت، كوبون، شريحة كمية...)            |                                            |
| value                 | DECIMAL(18,4)      | القيمة أو النسبة                                   |                                            |
| start_date            | DATE               | بداية العرض                                        |                                            |
| end_date              | DATE               | نهاية العرض                                        |                                            |
| is_active             | BOOLEAN            | مفعل/غير مفعل                                      |                                            |
| apply_on              | ENUM               | (فاتورة، صنف، عميل مجموعة، فترة...)                |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |

---

## جدول: sales_taxes (الضرائب على المبيعات)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| tax_id                | INT (PK)           | رقم الضريبة                                        | PRIMARY KEY, AUTO_INCREMENT                |
| name_ar               | VARCHAR(100)       | اسم الضريبة بالعربي                                |                                            |
| name_en               | VARCHAR(100)       | اسم الضريبة بالإنجليزي                             |                                            |
| tax_type              | ENUM               | (قيمة مضافة، جمارك، رسم خدمة...)                  |                                            |
| percentage            | DECIMAL(5,2)       | النسبة                                             |                                            |
| fixed_amount          | DECIMAL(18,2)      | مبلغ ثابت                                          |                                            |
| start_date            | DATE               | بداية التطبيق                                      |                                            |
| end_date              | DATE               | نهاية التطبيق                                      |                                            |
| is_active             | BOOLEAN            | مفعل/غير مفعل                                      |                                            |
| apply_on              | ENUM               | (فاتورة، صنف، مجموعة، منطقة...)                    |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |

---

## جدول: sales_installments (تقسيط العملاء)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| installment_id        | INT (PK)           | رقم خطة التقسيط                                    | PRIMARY KEY, AUTO_INCREMENT                |
| invoice_id            | INT (FK)           | رقم الفاتورة                                       | FK→sales_invoices.invoice_id               |
| customer_id           | INT (FK)           | رقم العميل                                         | FK→customers.customer_id                   |
| total_amount          | DECIMAL(18,2)      | إجمالي المبلغ                                      |                                            |
| start_date            | DATE               | بداية التقسيط                                      |                                            |
| end_date              | DATE               | نهاية التقسيط                                      |                                            |
| installment_count     | INT                | عدد الأقساط                                        |                                            |
| installment_amount    | DECIMAL(18,2)      | قيمة القسط الواحد                                   |                                            |
| interest_rate         | DECIMAL(5,2)       | نسبة الفائدة (إن وجدت)                             |                                            |
| status                | ENUM               | (جاري، مكتمل، متأخر، ملغي)                         |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |

---

## جدول: sales_pos_receipts (نقاط البيع)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| pos_receipt_id        | INT (PK)           | رقم إيصال نقطة البيع                               | PRIMARY KEY, AUTO_INCREMENT                |
| receipt_number        | VARCHAR(30)        | رقم الإيصال (تسلسلي/يدوي)                          | UNIQUE, NOT NULL                           |
| invoice_id            | INT (FK)           | رقم الفاتورة                                       | FK→sales_invoices.invoice_id               |
| pos_terminal_id       | INT (FK)           | جهاز نقطة البيع                                    | FK→pos_terminals.pos_terminal_id           |
| payment_method        | ENUM               | (نقدي، بطاقة، شيك، إلخ)                            |                                            |
| amount                | DECIMAL(18,2)      | المبلغ                                              |                                            |
| payment_date          | DATE               | تاريخ الحركة                                        |                                            |
| cashier_id            | INT (FK)           | كاشير                                              | FK→users.user_id                           |
| status                | ENUM               | (مسودة، معتمد، ملغي)                               |                                            |
| branch_id             | INT (FK)           | الفرع                                               | FK→branches.branch_id                      |
| created_at            | DATETIME           | تاريخ الإنشاء                                       |                                            |

---

## جدول: pos_terminals (أجهزة نقاط البيع)
| الحقل                 | النوع              | الوصف                                              | قيود/علاقات                                |
|-----------------------|--------------------|----------------------------------------------------|--------------------------------------------|
| pos_terminal_id       | INT (PK)           | رقم الجهاز                                         | PRIMARY KEY, AUTO_INCREMENT                |
| terminal_code         | VARCHAR(30)        | كود الجهاز                                         | UNIQUE, NOT NULL                           |
| location              | VARCHAR(100)       | الموقع                                             |                                            |
| branch_id             | INT (FK)           | الفرع                                              | FK→branches.branch_id                      |
| is_active             | BOOLEAN            | مفعل/غير مفعل                                      |                                            |
| note                  | TEXT               | ملاحظات                                            |                                            |

---

## العلاقات الرئيسية في قسم المبيعات

- **sales_quotations.customer_id → customers.customer_id**
- **sales_quotation_lines.quotation_id → sales_quotations.quotation_id**
- **sales_orders.quotation_id → sales_quotations.quotation_id**
- **sales_orders.customer_id → customers.customer_id**
- **sales_order_lines.order_id → sales_orders.order_id**
- **sales_order_lines.item_id → items.item_id**
- **sales_order_lines.unit_id → units.unit_id**
- **sales_order_lines.warehouse_id → warehouses.warehouse_id**
- **sales_deliveries.order_id → sales_orders.order_id**
- **sales_delivery_lines.delivery_id → sales_deliveries.delivery_id**
- **sales_delivery_lines.item_id → items.item_id**
- **sales_delivery_lines.warehouse_id → warehouses.warehouse_id**
- **sales_invoices.order_id → sales_orders.order_id**
- **sales_invoices.customer_id → customers.customer_id**
- **sales_invoice_lines.invoice_id → sales_invoices.invoice_id**
- **sales_invoice_lines.item_id → items.item_id**
- **sales_invoice_lines.unit_id → units.unit_id**
- **sales_invoice_lines.warehouse_id → warehouses.warehouse_id**
- **sales_invoice_lines.batch_id → stock_batches.batch_id**
- **sales_returns.invoice_id → sales_invoices.invoice_id**
- **sales_return_lines.return_id → sales_returns.return_id**
- **sales_payments.customer_id → customers.customer_id**
- **sales_payments.invoice_id → sales_invoices.invoice_id**
- **sales_payments.bank_account_id → bank_accounts.bank_account_id**
- **sales_payments.cash_box_id → cash_boxes.cash_box_id**
- **sales_installments.invoice_id → sales_invoices.invoice_id**
- **sales_pos_receipts.invoice_id → sales_invoices.invoice_id**
- **sales_pos_receipts.pos_terminal_id → pos_terminals.pos_terminal_id**

---

## ملاحظات عامة

- تدعم جميع جداول المبيعات الفروع، العملات، المستخدمين، تدقيق العمليات.
- تدعم كافة العمليات البيعية: عروض أسعار، أوامر بيع، تسليم، فواتير، مرتجعات، سندات قبض، تقسيط، POS، العروض والضرائب.
- تدعم تعدد المخازن، الوحدات، الدُفعات، أجهزة نقاط البيع.
- جاهزة للربط مع الأقسام الأخرى (العملاء، المخزون، المالية، التقارير).

---
