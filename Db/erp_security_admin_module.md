# قسم الصلاحيات وإدارة النظام – تحليل جداول قاعدة البيانات

---

## جدول: users (المستخدمون)
| الحقل            | النوع            | الوصف                                   | قيود/علاقات                      |
|------------------|------------------|-----------------------------------------|----------------------------------|
| user_id          | INT (PK)         | رقم المستخدم                            | PRIMARY KEY, AUTO_INCREMENT      |
| username         | VARCHAR(50)      | اسم المستخدم                            | UNIQUE, NOT NULL                 |
| password_hash    | VARCHAR(255)     | كلمة السر المشفّرة                      |                                  |
| email            | VARCHAR(100)     | البريد الإلكتروني                        | UNIQUE                           |
| full_name        | VARCHAR(200)     | الاسم الكامل                             |                                  |
| phone            | VARCHAR(30)      | رقم الجوال/هاتف                         |                                  |
| status           | ENUM             | (نشط، مقفل، موقوف، معلق)                |                                  |
| must_change_pass | BOOLEAN          | يجب تغيير كلمة السر عند الدخول           |                                  |
| last_login_at    | DATETIME         | آخر دخول                                 |                                  |
| failed_logins    | INT              | عدد محاولات الدخول الفاشلة               |                                  |
| is_superuser     | BOOLEAN          | صلاحية مدير النظام العام                 |                                  |
| branch_id        | INT (FK)         | الفرع الافتراضي                          | FK→branches.branch_id            |
| language         | VARCHAR(10)      | اللغة الافتراضية (ar/en ...)            |                                  |
| created_by       | INT (FK)         | أنشئ بواسطة                             | FK→users.user_id                 |
| created_at       | DATETIME         | تاريخ الإنشاء                            |                                  |
| updated_by       | INT (FK)         | آخر معدّل                                | FK→users.user_id                 |
| updated_at       | DATETIME         | تاريخ آخر تعديل                          |                                  |
| notes            | TEXT             | ملاحظات                                  |                                  |

---

## جدول: roles (الأدوار/الرتب)
| الحقل         | النوع            | الوصف                        | قيود/علاقات                  |
|---------------|------------------|------------------------------|------------------------------|
| role_id       | INT (PK)         | رقم الدور                    | PRIMARY KEY                  |
| role_code     | VARCHAR(30)      | كود الدور                    | UNIQUE, NOT NULL             |
| role_name_ar  | VARCHAR(100)     | اسم الدور بالعربي            |                              |
| role_name_en  | VARCHAR(100)     | اسم الدور بالإنجليزي         |                              |
| is_active     | BOOLEAN          | مفعل/غير مفعل                |                              |
| description   | TEXT             | وصف الدور                    |                              |

---

## جدول: user_roles (ربط المستخدمين بالأدوار)
| الحقل         | النوع            | الوصف                        | قيود/علاقات                        |
|---------------|------------------|------------------------------|------------------------------------|
| user_role_id  | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT        |
| user_id       | INT (FK)         | رقم المستخدم                 | FK→users.user_id                   |
| role_id       | INT (FK)         | رقم الدور                    | FK→roles.role_id                   |
| assigned_at   | DATETIME         | تاريخ الربط                  |                                    |

---

## جدول: permissions (الصلاحيات/الأذونات)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| permission_id     | INT (PK)         | رقم الصلاحية                 | PRIMARY KEY                  |
| permission_code   | VARCHAR(100)     | الكود (مثل sales.create)     | UNIQUE, NOT NULL             |
| name_ar           | VARCHAR(150)     | اسم الصلاحية بالعربي         |                              |
| name_en           | VARCHAR(150)     | اسم الصلاحية بالإنجليزي      |                              |
| module            | VARCHAR(50)      | الوحدة (مبيعات، مشتريات ...) |                              |
| description       | TEXT             | وصف إضافي                    |                              |

---

## جدول: role_permissions (ربط الأدوار بالصلاحيات)
| الحقل               | النوع            | الوصف                        | قيود/علاقات                      |
|---------------------|------------------|------------------------------|----------------------------------|
| role_permission_id  | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT      |
| role_id             | INT (FK)         | رقم الدور                    | FK→roles.role_id                 |
| permission_id       | INT (FK)         | رقم الصلاحية                 | FK→permissions.permission_id      |
| allow               | BOOLEAN          | السماح أم المنع               |                                  |

---

## جدول: user_permissions (استثناءات صلاحيات المستخدم)
| الحقل                | النوع            | الوصف                        | قيود/علاقات                      |
|----------------------|------------------|------------------------------|----------------------------------|
| user_permission_id   | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT      |
| user_id              | INT (FK)         | رقم المستخدم                 | FK→users.user_id                 |
| permission_id        | INT (FK)         | رقم الصلاحية                 | FK→permissions.permission_id      |
| allow                | BOOLEAN          | السماح أم المنع               |                                  |

---

## جدول: audit_trails (سجل الأحداث/التدقيق)
| الحقل            | النوع             | الوصف                                | قيود/علاقات                         |
|------------------|-------------------|--------------------------------------|-------------------------------------|
| audit_id         | INT (PK)          | رقم السجل                            | PRIMARY KEY, AUTO_INCREMENT         |
| event_time       | DATETIME          | تاريخ ووقت الحدث                     |                                     |
| user_id          | INT (FK)          | رقم المستخدم                         | FK→users.user_id                    |
| action           | VARCHAR(100)      | نوع الحدث (دخول، إنشاء، حذف...)      |                                     |
| resource_type    | VARCHAR(100)      | نوع المورد (جدول/شاشة/ملف...)        |                                     |
| resource_id      | VARCHAR(100)      | رقم المورد أو المفتاح                 |                                     |
| ip_address       | VARCHAR(50)       | عنوان الـ IP                         |                                     |
| device           | VARCHAR(100)      | الجهاز أو المتصفح                     |                                     |
| details          | TEXT              | تفاصيل إضافية                        |                                     |

---

## جدول: branches (الفروع)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                  |
|-----------------|------------------|------------------------------|------------------------------|
| branch_id       | INT (PK)         | رقم الفرع                    | PRIMARY KEY                  |
| branch_code     | VARCHAR(20)      | كود الفرع                    | UNIQUE, NOT NULL             |
| name_ar         | VARCHAR(100)     | اسم الفرع بالعربي            |                              |
| name_en         | VARCHAR(100)     | اسم الفرع بالإنجليزي         |                              |
| address         | VARCHAR(200)     | العنوان                      |                              |
| phone           | VARCHAR(30)      | رقم الهاتف                   |                              |
| manager_id      | INT (FK)         | مدير الفرع                   | FK→employees.employee_id     |
| is_active       | BOOLEAN          | مفعل/غير مفعل                |                              |
| note            | TEXT             | ملاحظات                      |                              |

---

## جدول: system_settings (الإعدادات العامة للنظام)
| الحقل           | النوع            | الوصف                        | قيود/علاقات                  |
|-----------------|------------------|------------------------------|------------------------------|
| setting_id      | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT  |
| setting_key     | VARCHAR(100)     | اسم الإعداد                  | UNIQUE                      |
| setting_value   | VARCHAR(500)     | قيمة الإعداد                 |                              |
| description     | TEXT             | وصف الإعداد                  |                              |
| updated_by      | INT (FK)         | المستخدم المعدّل             | FK→users.user_id             |
| updated_at      | DATETIME         | تاريخ آخر تعديل              |                              |

---

## جدول: notifications (الإشعارات)
| الحقل            | النوع            | الوصف                                | قيود/علاقات                  |
|------------------|------------------|--------------------------------------|------------------------------|
| notification_id  | INT (PK)         | رقم الإشعار                          | PRIMARY KEY, AUTO_INCREMENT  |
| user_id          | INT (FK)         | رقم المستخدم                         | FK→users.user_id             |
| message          | TEXT             | نص الإشعار                           |                              |
| is_read          | BOOLEAN          | تم القراءة                            |                              |
| created_at       | DATETIME         | تاريخ الإنشاء                         |                              |
| link             | VARCHAR(255)     | رابط الإشعار (داخلي/خارجي)           |                              |

---

## جدول: attachments (المرفقات والملفات)
| الحقل            | النوع            | الوصف                                | قيود/علاقات                  |
|------------------|------------------|--------------------------------------|------------------------------|
| attachment_id    | INT (PK)         | رقم المرفق                           | PRIMARY KEY, AUTO_INCREMENT  |
| file_name        | VARCHAR(200)     | اسم الملف                            |                              |
| file_path        | VARCHAR(255)     | مسار الملف أو رابط التخزين           |                              |
| file_type        | VARCHAR(50)      | نوع الملف (PDF, DOC, JPG...)         |                              |
| uploaded_by      | INT (FK)         | المستخدم الرافع                      | FK→users.user_id             |
| uploaded_at      | DATETIME         | تاريخ الرفع                          |                              |
| description      | TEXT             | وصف أو ملاحظات                       |                              |
| reference_type   | VARCHAR(50)      | نوع الربط (مستخدم، فاتورة، أصل ...)  |                              |
| reference_id     | VARCHAR(100)     | رقم المرجع المرتبط                   |                              |

---

## العلاقات الرئيسية في قسم الصلاحيات وإدارة النظام

- **users.branch_id → branches.branch_id**
- **users.created_by/updated_by → users.user_id**
- **user_roles.user_id → users.user_id**
- **user_roles.role_id → roles.role_id**
- **role_permissions.role_id → roles.role_id**
- **role_permissions.permission_id → permissions.permission_id**
- **user_permissions.user_id → users.user_id**
- **user_permissions.permission_id → permissions.permission_id**
- **audit_trails.user_id → users.user_id**
- **branches.manager_id → employees.employee_id**
- **notifications.user_id → users.user_id**
- **attachments.uploaded_by → users.user_id**

---

## ملاحظات عملية

- يدعم النظام صلاحيات دورية (RBAC) مرنة مع استثناءات للمستخدمين.
- كل عملية أو حدث تسجل في audit_trails بالتفصيل.
- يدعم النظام تعدد الفروع، وتخصيص الإعدادات حسب الفرع أو النظام ككل.
- يمكن ربط المستخدمين بأدوار متعددة، وكل دور يحدد صلاحياته عبر role_permissions.
- يدعم النظام إشعارات وتنبيهات متقدمة (موافقة، إشعار مهمة، رسائل سيستم).
- كل الملفات والمرفقات قابلة للربط مع أي كيان في النظام.
- إعدادات النظام تشمل كل ما يتعلق بالأمان، اللغة، البريد، الحدود، التكامل، إلخ.

---