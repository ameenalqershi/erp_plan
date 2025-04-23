# قسم الإعدادات والتكامل – تحليل جداول قاعدة البيانات

---

## جدول: system_settings (الإعدادات العامة للنظام)
| الحقل           | النوع            | الوصف                                | قيود/علاقات                  |
|-----------------|------------------|--------------------------------------|------------------------------|
| setting_id      | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| setting_key     | VARCHAR(100)     | اسم الإعداد (system.default_currency)| UNIQUE                      |
| setting_value   | VARCHAR(500)     | قيمة الإعداد                         |                              |
| category        | VARCHAR(50)      | تصنيف الإعداد (مالية، عامة، أمان ...) |                              |
| is_editable     | BOOLEAN          | قابل للتعديل                         |                              |
| is_active       | BOOLEAN          | مفعل/غير مفعل                        |                              |
| description     | TEXT             | وصف الإعداد                          |                              |
| updated_by      | INT (FK)         | المستخدم المعدّل                     | FK→users.user_id             |
| updated_at      | DATETIME         | تاريخ آخر تعديل                      |                              |

---

## جدول: module_settings (إعدادات الوحدات/الأقسام)
| الحقل           | النوع            | الوصف                                | قيود/علاقات                  |
|-----------------|------------------|--------------------------------------|------------------------------|
| module_setting_id| INT (PK)        | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| module          | VARCHAR(50)      | اسم الوحدة (مبيعات، مشتريات...)      |                              |
| setting_key     | VARCHAR(100)     | اسم الإعداد                          |                              |
| setting_value   | VARCHAR(500)     | قيمة الإعداد                         |                              |
| is_active       | BOOLEAN          | مفعل/غير مفعل                        |                              |
| description     | TEXT             | وصف الإعداد                          |                              |
| updated_by      | INT (FK)         | المستخدم المعدّل                     | FK→users.user_id             |
| updated_at      | DATETIME         | تاريخ آخر تعديل                      |                              |

---

## جدول: fiscal_years (السنوات المالية)
| الحقل           | النوع            | الوصف                                | قيود/علاقات                  |
|-----------------|------------------|--------------------------------------|------------------------------|
| fiscal_year_id  | INT (PK)         | رقم السنة المالية                    | PRIMARY KEY, AUTO_INCREMENT  |
| year            | INT              | السنة (مثلاً 2025)                   | UNIQUE                      |
| start_date      | DATE             | بداية السنة المالية                  |                              |
| end_date        | DATE             | نهاية السنة المالية                  |                              |
| status          | ENUM             | (نشطة، مغلقة، أرشيف)                 |                              |
| created_by      | INT (FK)         | من أنشأ                               | FK→users.user_id             |
| created_at      | DATETIME         | تاريخ الإنشاء                         |                              |
| closed_by       | INT (FK)         | من أغلق السنة                         | FK→users.user_id             |
| closed_at       | DATETIME         | تاريخ الإغلاق                         |                              |
| note            | TEXT             | ملاحظات                              |                              |

---

## جدول: integration_services (أنظمة التكامل الخارجي)
| الحقل                 | النوع            | الوصف                                | قيود/علاقات                  |
|-----------------------|------------------|--------------------------------------|------------------------------|
| service_id            | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| service_code          | VARCHAR(30)      | كود الخدمة (SAP, TAX, E-Invoice ...) | UNIQUE, NOT NULL             |
| service_name          | VARCHAR(100)     | اسم الخدمة                           |                              |
| service_type          | ENUM             | نوع (ERP، بوابة دفع، حكومي ...)      |                              |
| api_url               | VARCHAR(255)     | عنوان التكامل (API Endpoint)         |                              |
| api_key               | VARCHAR(255)     | مفتاح الربط (API Key)                 |                              |
| username              | VARCHAR(100)     | اسم مستخدم التكامل                   |                              |
| password              | VARCHAR(255)     | كلمة مرور مشفرة                      |                              |
| is_active             | BOOLEAN          | مفعل/غير مفعل                        |                              |
| last_synced_at        | DATETIME         | آخر مزامنة                            |                              |
| description           | TEXT             | شرح أو ملاحظات                        |                              |
| created_by            | INT (FK)         | المستخدم المُنشئ                       | FK→users.user_id             |
| created_at            | DATETIME         | تاريخ الإنشاء                         |                              |

---

## جدول: integration_logs (سجلات التكامل)
| الحقل            | النوع            | الوصف                                | قيود/علاقات                  |
|------------------|------------------|--------------------------------------|------------------------------|
| log_id           | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| service_id       | INT (FK)         | رقم خدمة التكامل                     | FK→integration_services.service_id|
| integration_time | DATETIME         | وقت التكامل                          |                              |
| request_type     | VARCHAR(20)      | نوع الطلب (إرسال/استقبال)           |                              |
| request_payload  | TEXT             | نص الطلب (Request Body)              |                              |
| response_code    | VARCHAR(10)      | كود الاستجابة (200, 401, ...)        |                              |
| response_body    | TEXT             | نص الاستجابة                         |                              |
| status           | ENUM             | (ناجح، فشل، معلق)                    |                              |
| error_message    | TEXT             | رسالة الخطأ (إن وجد)                 |                              |
| created_by       | INT (FK)         | المستخدم أو النظام                    | FK→users.user_id             |
| created_at       | DATETIME         | تاريخ السجل                           |                              |

---

## جدول: scheduled_tasks (المهام المجدولة/الأوتوماتيكية)
| الحقل            | النوع            | الوصف                                | قيود/علاقات                  |
|------------------|------------------|--------------------------------------|------------------------------|
| task_id          | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| task_code        | VARCHAR(30)      | كود المهمة (backup, sync, ...)       | UNIQUE, NOT NULL             |
| description      | TEXT             | وصف المهمة                           |                              |
| schedule_cron    | VARCHAR(100)     | توقيت التنفيذ (CRON)                 |                              |
| is_active        | BOOLEAN          | مفعل/غير مفعل                        |                              |
| last_run_at      | DATETIME         | آخر تنفيذ                            |                              |
| next_run_at      | DATETIME         | الموعد القادم                        |                              |
| status           | ENUM             | (نشطة، موقوفة، فاشلة)                |                              |
| last_run_status  | ENUM             | (ناجح، فشل، جزئي)                     |                              |
| created_by       | INT (FK)         | من أنشأ                               | FK→users.user_id             |
| created_at       | DATETIME         | تاريخ الإنشاء                         |                              |

---

## جدول: scheduled_task_logs (سجلات تنفيذ المهام المجدولة)
| الحقل            | النوع            | الوصف                                | قيود/علاقات                  |
|------------------|------------------|--------------------------------------|------------------------------|
| log_id           | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| task_id          | INT (FK)         | رقم المهمة                           | FK→scheduled_tasks.task_id   |
| run_time         | DATETIME         | وقت التنفيذ                          |                              |
| run_status       | ENUM             | (ناجح، فشل، جزئي)                     |                              |
| log_details      | TEXT             | تفاصيل التنفيذ                       |                              |

---

## جدول: email_settings (إعدادات البريد الإلكتروني)
| الحقل            | النوع            | الوصف                                | قيود/علاقات                  |
|------------------|------------------|--------------------------------------|------------------------------|
| email_setting_id | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| smtp_server      | VARCHAR(100)     | اسم السيرفر                          |                              |
| smtp_port        | INT              | رقم المنفذ                            |                              |
| use_ssl          | BOOLEAN          | استخدام SSL                          |                              |
| username         | VARCHAR(100)     | اسم مستخدم البريد                     |                              |
| password         | VARCHAR(255)     | كلمة مرور مشفرة                      |                              |
| sender_email     | VARCHAR(100)     | البريد المرسل                         |                              |
| sender_name      | VARCHAR(100)     | اسم المرسل                            |                              |
| is_active        | BOOLEAN          | مفعل/غير مفعل                        |                              |
| updated_by       | INT (FK)         | آخر معدّل                            | FK→users.user_id             |
| updated_at       | DATETIME         | تاريخ التعديل                         |                              |

---

## جدول: sms_settings (إعدادات الرسائل النصية)
| الحقل            | النوع            | الوصف                                | قيود/علاقات                  |
|------------------|------------------|--------------------------------------|------------------------------|
| sms_setting_id   | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| api_url          | VARCHAR(255)     | بوابة إرسال الرسائل                  |                              |
| api_key          | VARCHAR(255)     | مفتاح الربط                           |                              |
| sender_id        | VARCHAR(50)      | اسم المرسل الافتراضي                  |                              |
| is_active        | BOOLEAN          | مفعل/غير مفعل                        |                              |
| updated_by       | INT (FK)         | آخر معدّل                            | FK→users.user_id             |
| updated_at       | DATETIME         | تاريخ التعديل                         |                              |

---

## جدول: api_tokens (مفاتيح الربط API)
| الحقل            | النوع            | الوصف                                | قيود/علاقات                  |
|------------------|------------------|--------------------------------------|------------------------------|
| token_id         | INT (PK)         | رقم السطر                            | PRIMARY KEY, AUTO_INCREMENT  |
| token_code       | VARCHAR(50)      | كود المفتاح                          | UNIQUE, NOT NULL             |
| user_id          | INT (FK)         | المخصص له (مستخدم/خدمة)              | FK→users.user_id             |
| service_id       | INT (FK)         | خدمة التكامل (إن وجد)                | FK→integration_services.service_id|
| token_value      | VARCHAR(255)     | المفتاح المشفر                        |                              |
| expiry_date      | DATE             | انتهاء الصلاحية                       |                              |
| is_active        | BOOLEAN          | مفعل/غير مفعل                        |                              |
| created_at       | DATETIME         | تاريخ الإنشاء                         |                              |

---

## العلاقات الرئيسية في قسم الإعدادات والتكامل

- **system_settings.updated_by → users.user_id**
- **module_settings.updated_by → users.user_id**
- **fiscal_years.created_by/closed_by → users.user_id**
- **integration_services.created_by → users.user_id**
- **integration_logs.service_id → integration_services.service_id**
- **integration_logs.created_by → users.user_id**
- **scheduled_tasks.created_by → users.user_id**
- **scheduled_task_logs.task_id → scheduled_tasks.task_id**
- **email_settings.updated_by → users.user_id**
- **sms_settings.updated_by → users.user_id**
- **api_tokens.user_id → users.user_id**
- **api_tokens.service_id → integration_services.service_id**

---

## ملاحظات عملية

- يدعم النظام إعدادات عامة ومرنة لكل الأقسام، مع دعم التخصيص لكل وحدة.
- يدعم الإدارة الكاملة للسنوات والفترات المالية مع الربط بمستخدم الإنشاء والإغلاق.
- التكامل مع أي نظام خارجي (حكومي أو بوابات دفع أو ERP خارجي) عبر integration_services وintegration_logs.
- دعم كامل للمهام المجدولة والتقارير الآلية والتنبيهات.
- إعدادات البريد والرسائل النصية مركزية وآمنة، مع حماية كلمات المرور والتوثيق.
- يدعم النظام مفاتيح ربط API Tokens للمستخدمين والخدمات الخارجية مع إدارة وتاريخ انتهاء وصلاحيات.
- جميع الجداول تدعم الربط مع المستخدمين، التدقيق، التفعيل/التعطيل، التوثيق والتحليل الفني.

---