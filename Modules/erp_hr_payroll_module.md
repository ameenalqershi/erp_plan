# قسم الموارد البشرية والرواتب – تحليل جداول قاعدة البيانات

---

## جدول: employees (الموظفون)
| الحقل                | النوع               | الوصف                                    | قيود/علاقات                           |
|----------------------|---------------------|------------------------------------------|---------------------------------------|
| employee_id          | INT (PK)            | رقم الموظف                               | PRIMARY KEY, AUTO_INCREMENT           |
| employee_code        | VARCHAR(30)         | كود الموظف                               | UNIQUE, NOT NULL                      |
| national_id          | VARCHAR(50)         | رقم الهوية الوطنية/الجنسية               |                                       |
| passport_no          | VARCHAR(50)         | رقم الجواز                               |                                       |
| first_name_ar        | VARCHAR(100)        | الاسم الأول بالعربي                      | NOT NULL                              |
| father_name_ar       | VARCHAR(100)        | اسم الأب بالعربي                         |                                       |
| family_name_ar       | VARCHAR(100)        | اسم العائلة بالعربي                      |                                       |
| first_name_en        | VARCHAR(100)        | الاسم الأول بالإنجليزي                   |                                       |
| father_name_en       | VARCHAR(100)        | اسم الأب بالإنجليزي                      |                                       |
| family_name_en       | VARCHAR(100)        | اسم العائلة بالإنجليزي                   |                                       |
| gender               | ENUM                | الجنس                                    |                                       |
| date_of_birth        | DATE                | تاريخ الميلاد                            |                                       |
| place_of_birth       | VARCHAR(100)        | مكان الميلاد                             |                                       |
| marital_status       | ENUM                | الحالة الاجتماعية                        |                                       |
| nationality          | VARCHAR(100)        | الجنسية                                  |                                       |
| photo_url            | VARCHAR(255)        | صورة الموظف                              |                                       |
| mobile               | VARCHAR(30)         | الجوال                                   |                                       |
| phone                | VARCHAR(30)         | هاتف آخر                                 |                                       |
| email                | VARCHAR(100)        | البريد الإلكتروني                         |                                       |
| address              | VARCHAR(200)        | العنوان                                  |                                       |
| city                 | VARCHAR(100)        | المدينة                                  |                                       |
| country              | VARCHAR(100)        | الدولة                                   |                                       |
| hire_date            | DATE                | تاريخ التعيين                            |                                       |
| employment_status    | ENUM                | (نشط، موقوف، مستقال، مفصول، متوفى)      |                                       |
| department_id        | INT (FK)            | القسم/الإدارة                            | FK→departments.department_id           |
| job_id               | INT (FK)            | الوظيفة                                  | FK→jobs.job_id                         |
| manager_id           | INT (FK)            | المدير المباشر                           | FK→employees.employee_id                |
| branch_id            | INT (FK)            | الفرع                                    | FK→branches.branch_id                   |
| contract_type        | ENUM                | (دائم، مؤقت، جزئي، موسمي)               |                                       |
| contract_start       | DATE                | بداية العقد                              |                                       |
| contract_end         | DATE                | نهاية العقد                              |                                       |
| grade_id             | INT (FK)            | الدرجة/السلم الوظيفي                     | FK→grades.grade_id                      |
| bank_account_id      | INT (FK)            | الحساب البنكي                            | FK→bank_accounts.bank_account_id         |
| salary               | DECIMAL(18,2)       | الراتب الأساسي                           |                                       |
| currency_id          | INT (FK)            | العملة                                   | FK→currencies.currency_id               |
| insurance_no         | VARCHAR(50)         | رقم التأمين                              |                                       |
| insurance_start      | DATE                | بداية التأمين                            |                                       |
| insurance_end        | DATE                | نهاية التأمين                            |                                       |
| notes                | TEXT                | ملاحظات                                  |                                       |
| created_by           | INT (FK)            | المستخدم المُنشئ                         | FK→users.user_id                        |
| created_at           | DATETIME            | تاريخ الإنشاء                             |                                       |
| updated_by           | INT (FK)            | المستخدم المعدّل                         | FK→users.user_id                        |
| updated_at           | DATETIME            | تاريخ آخر تعديل                           |                                       |

---

## جدول: jobs (الوظائف)
| الحقل         | النوع            | الوصف                        | قيود/علاقات                |
|---------------|------------------|------------------------------|----------------------------|
| job_id        | INT (PK)         | رقم الوظيفة                  | PRIMARY KEY                |
| job_code      | VARCHAR(30)      | كود الوظيفة                  | UNIQUE, NOT NULL           |
| job_name_ar   | VARCHAR(100)     | اسم الوظيفة بالعربي          | NOT NULL                   |
| job_name_en   | VARCHAR(100)     | اسم الوظيفة بالإنجليزي       |                            |
| description   | TEXT             | وصف الوظيفة                  |                            |
| is_active     | BOOLEAN          | مفعل/غير مفعل                |                            |

---

## جدول: grades (الدرجات/السلم الوظيفي)
| الحقل         | النوع            | الوصف                        | قيود/علاقات                |
|---------------|------------------|------------------------------|----------------------------|
| grade_id      | INT (PK)         | رقم الدرجة                   | PRIMARY KEY                |
| grade_code    | VARCHAR(20)      | كود الدرجة                   | UNIQUE, NOT NULL           |
| grade_name_ar | VARCHAR(50)      | اسم الدرجة بالعربي           |                            |
| grade_name_en | VARCHAR(50)      | اسم الدرجة بالإنجليزي        |                            |
| min_salary    | DECIMAL(18,2)    | الحد الأدنى                  |                            |
| max_salary    | DECIMAL(18,2)    | الحد الأعلى                  |                            |
| is_active     | BOOLEAN          | مفعل/غير مفعل                |                            |

---

## جدول: departments (الأقسام/الإدارات)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| department_id     | INT (PK)         | رقم القسم                    | PRIMARY KEY                  |
| department_code   | VARCHAR(20)      | الكود                        | UNIQUE, NOT NULL             |
| department_name_ar| VARCHAR(100)     | الاسم بالعربي                | NOT NULL                     |
| department_name_en| VARCHAR(100)     | الاسم بالإنجليزي             |                              |
| parent_id         | INT (FK)         | القسم الأعلى                 | FK→departments.department_id |
| manager_id        | INT (FK)         | المدير/رئيس القسم            | FK→employees.employee_id     |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                              |
| note              | TEXT             | ملاحظات                      |                              |

---

## جدول: employee_contacts (جهات اتصال الموظفين)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                           |
|-------------------|------------------|------------------------------|---------------------------------------|
| contact_id        | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT           |
| employee_id       | INT (FK)         | رقم الموظف                   | FK→employees.employee_id              |
| contact_type      | ENUM             | (قريب، طارئ، ولي أمر، إلخ)   |                                       |
| contact_name      | VARCHAR(100)     | اسم جهة الاتصال              |                                       |
| relation          | VARCHAR(50)      | صلة القرابة                  |                                       |
| mobile            | VARCHAR(30)      | رقم الجوال                   |                                       |
| phone             | VARCHAR(30)      | رقم هاتف                     |                                       |
| email             | VARCHAR(100)     | بريد إلكتروني                |                                       |
| address           | VARCHAR(200)     | العنوان                      |                                       |
| notes             | TEXT             | ملاحظات                      |                                       |

---

## جدول: employee_documents (مستندات الموظفين)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| document_id       | INT (PK)         | رقم المستند                  | PRIMARY KEY, AUTO_INCREMENT  |
| employee_id       | INT (FK)         | رقم الموظف                   | FK→employees.employee_id     |
| doc_type          | ENUM             | نوع المستند (جواز، شهادة ..) |                              |
| doc_number        | VARCHAR(50)      | رقم المستند                  |                              |
| issue_date        | DATE             | تاريخ الإصدار                |                              |
| expiry_date       | DATE             | تاريخ الانتهاء               |                              |
| issued_by         | VARCHAR(100)     | جهة الإصدار                  |                              |
| attachment_id     | INT (FK)         | مرفق المستند                 | FK→attachments.attachment_id |
| notes             | TEXT             | ملاحظات                      |                              |

---

## جدول: employee_attendance (الحضور والانصراف)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                           |
|-------------------|------------------|------------------------------|---------------------------------------|
| attendance_id     | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT           |
| employee_id       | INT (FK)         | رقم الموظف                   | FK→employees.employee_id              |
| attendance_date   | DATE             | اليوم                        |                                       |
| in_time           | TIME             | وقت الحضور                    |                                       |
| out_time          | TIME             | وقت الانصراف                  |                                       |
| status            | ENUM             | (حاضر، غائب، إجازة، متأخر...)|                                       |
| note              | TEXT             | ملاحظات                      |                                       |
| device_id         | INT (FK)         | جهاز البصمة/الحضور           | FK→attendance_devices.device_id       |

---

## جدول: attendance_devices (أجهزة الحضور والانصراف)
| الحقل         | النوع            | الوصف                        | قيود/علاقات                   |
|---------------|------------------|------------------------------|-------------------------------|
| device_id     | INT (PK)         | رقم الجهاز                   | PRIMARY KEY                   |
| device_code   | VARCHAR(30)      | كود الجهاز                   | UNIQUE, NOT NULL              |
| location      | VARCHAR(100)     | الموقع                       |                               |
| description   | TEXT             | وصف                          |                               |
| is_active     | BOOLEAN          | مفعل/غير مفعل                |                               |

---

## جدول: leaves (الإجازات)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                           |
|-------------------|------------------|------------------------------|---------------------------------------|
| leave_id          | INT (PK)         | رقم الإجازة                  | PRIMARY KEY, AUTO_INCREMENT           |
| employee_id       | INT (FK)         | رقم الموظف                   | FK→employees.employee_id              |
| leave_type_id     | INT (FK)         | نوع الإجازة                  | FK→leave_types.leave_type_id          |
| start_date        | DATE             | بداية الإجازة                |                                       |
| end_date          | DATE             | نهاية الإجازة                |                                       |
| days_count        | DECIMAL(6,2)     | عدد الأيام                   |                                       |
| status            | ENUM             | (بانتظار، معتمد، مرفوض، تم)  |                                       |
| applied_at        | DATETIME         | تاريخ تقديم الطلب            |                                       |
| approved_by       | INT (FK)         | من اعتمد                     | FK→users.user_id                      |
| approved_at       | DATETIME         | تاريخ الاعتماد                |                                       |
| note              | TEXT             | ملاحظات                      |                                       |

---

## جدول: leave_types (أنواع الإجازات)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| leave_type_id     | INT (PK)         | رقم النوع                    | PRIMARY KEY                  |
| type_code         | VARCHAR(20)      | كود النوع                    | UNIQUE, NOT NULL             |
| name_ar           | VARCHAR(100)     | الاسم بالعربي                |                              |
| name_en           | VARCHAR(100)     | الاسم بالإنجليزي             |                              |
| is_paid           | BOOLEAN          | مدفوعة أو لا                 |                              |
| max_days_per_year | DECIMAL(6,2)     | الحد الأقصى سنويًا           |                              |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                              |
| note              | TEXT             | ملاحظات                      |                              |

---

## جدول: employee_salaries (الرواتب الشهرية للموظفين)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                           |
|-------------------|------------------|------------------------------|---------------------------------------|
| salary_id         | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT           |
| employee_id       | INT (FK)         | رقم الموظف                   | FK→employees.employee_id              |
| period_id         | INT (FK)         | الفترة المالية               | FK→fiscal_periods.period_id           |
| base_salary       | DECIMAL(18,2)    | الراتب الأساسي               |                                       |
| allowances        | DECIMAL(18,2)    | إجمالي البدلات               |                                       |
| deductions        | DECIMAL(18,2)    | إجمالي الاستقطاعات           |                                       |
| overtime_hours    | DECIMAL(8,2)     | عدد ساعات العمل الإضافي       |                                       |
| overtime_amount   | DECIMAL(18,2)    | قيمة العمل الإضافي           |                                       |
| leave_days        | DECIMAL(6,2)     | أيام الإجازة                 |                                       |
| net_salary        | DECIMAL(18,2)    | صافي الراتب                  |                                       |
| payment_date      | DATE             | تاريخ الصرف                   |                                       |
| payment_method    | ENUM             | (تحويل بنكي/نقدي/شيك...)     |                                       |
| bank_account_id   | INT (FK)         | الحساب البنكي                 | FK→bank_accounts.bank_account_id      |
| status            | ENUM             | (مسودة، معتمد، مدفوع، ملغي)  |                                       |
| created_at        | DATETIME         | تاريخ الإنشاء                 |                                       |
| note              | TEXT             | ملاحظات                      |                                       |

---

## جدول: salary_allowances (البدلات)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| allowance_id      | INT (PK)         | رقم البدل                    | PRIMARY KEY                  |
| allowance_code    | VARCHAR(20)      | كود البدل                    | UNIQUE, NOT NULL             |
| name_ar           | VARCHAR(100)     | الاسم بالعربي                |                              |
| name_en           | VARCHAR(100)     | الاسم بالإنجليزي             |                              |
| is_percentage     | BOOLEAN          | نسبة أو مبلغ ثابت            |                              |
| value             | DECIMAL(10,2)    | النسبة أو المبلغ             |                              |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                              |
| note              | TEXT             | ملاحظات                      |                              |

---

## جدول: salary_deductions (الاستقطاعات)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| deduction_id      | INT (PK)         | رقم الاستقطاع                | PRIMARY KEY                  |
| deduction_code    | VARCHAR(20)      | كود الاستقطاع                | UNIQUE, NOT NULL             |
| name_ar           | VARCHAR(100)     | الاسم بالعربي                |                              |
| name_en           | VARCHAR(100)     | الاسم بالإنجليزي             |                              |
| is_percentage     | BOOLEAN          | نسبة أو مبلغ ثابت            |                              |
| value             | DECIMAL(10,2)    | النسبة أو المبلغ             |                              |
| is_active         | BOOLEAN          | مفعل/غير مفعل                |                              |
| note              | TEXT             | ملاحظات                      |                              |

---

## جدول: employee_salary_details (تفاصيل الرواتب الشهرية)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                           |
|-------------------|------------------|------------------------------|---------------------------------------|
| detail_id         | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT           |
| salary_id         | INT (FK)         | رقم سجل الراتب               | FK→employee_salaries.salary_id        |
| allowance_id      | INT (FK)         | رقم البدل (إن وجد)           | FK→salary_allowances.allowance_id     |
| deduction_id      | INT (FK)         | رقم الاستقطاع (إن وجد)       | FK→salary_deductions.deduction_id     |
| amount            | DECIMAL(18,2)    | القيمة                       |                                       |
| type              | ENUM             | (بدل/استقطاع)                |                                       |
| note              | TEXT             | ملاحظات                      |                                       |

---

## جدول: employee_overtimes (سجلات العمل الإضافي)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                           |
|-------------------|------------------|------------------------------|---------------------------------------|
| overtime_id       | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT           |
| employee_id       | INT (FK)         | رقم الموظف                   | FK→employees.employee_id              |
| period_id         | INT (FK)         | الفترة المالية               | FK→fiscal_periods.period_id           |
| overtime_hours    | DECIMAL(8,2)     | عدد الساعات                  |                                       |
| overtime_rate     | DECIMAL(10,2)    | سعر الساعة                   |                                       |
| overtime_amount   | DECIMAL(18,2)    | قيمة الإضافي                 |                                       |
| status            | ENUM             | (مسودة، معتمد، مدفوع، ملغي)  |                                       |
| note              | TEXT             | ملاحظات                      |                                       |

---

## جدول: payroll_runs (تشغيل الرواتب الشهري)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                           |
|-------------------|------------------|------------------------------|---------------------------------------|
| payroll_id        | INT (PK)         | رقم التشغيل                  | PRIMARY KEY, AUTO_INCREMENT           |
| period_id         | INT (FK)         | الفترة المالية               | FK→fiscal_periods.period_id           |
| run_date          | DATE             | تاريخ التشغيل                |                                       |
| status            | ENUM             | (مسودة، معتمد، مدفوع، ملغي)  |                                       |
| created_by        | INT (FK)         | المستخدم المُنشئ              | FK→users.user_id                      |
| created_at        | DATETIME         | تاريخ الإنشاء                 |                                       |
| approved_by       | INT (FK)         | من اعتمد                      | FK→users.user_id                      |
| approved_at       | DATETIME         | تاريخ الاعتماد                |                                       |
| note              | TEXT             | ملاحظات                      |                                       |

---

## جدول: payroll_settings (إعدادات الرواتب)
| الحقل             | النوع            | الوصف                        | قيود/علاقات                  |
|-------------------|------------------|------------------------------|------------------------------|
| setting_id        | INT (PK)         | رقم السطر                    | PRIMARY KEY, AUTO_INCREMENT  |
| setting_key       | VARCHAR(100)     | اسم الإعداد                  | UNIQUE                       |
| setting_value     | VARCHAR(500)     | قيمة الإعداد                 |                              |
| description       | TEXT             | وصف الإعداد                  |                              |
| updated_by        | INT (FK)         | المستخدم المعدّل             | FK→users.user_id             |
| updated_at        | DATETIME         | تاريخ التعديل                |                              |

---

## العلاقات الرئيسية في قسم الموارد البشرية والرواتب

- **employees.department_id → departments.department_id**
- **employees.job_id → jobs.job_id**
- **employees.manager_id → employees.employee_id**
- **employees.branch_id → branches.branch_id**
- **employees.grade_id → grades.grade_id**
- **employees.bank_account_id → bank_accounts.bank_account_id**
- **employees.currency_id → currencies.currency_id**
- **departments.parent_id → departments.department_id**
- **departments.manager_id → employees.employee_id**
- **employee_contacts.employee_id → employees.employee_id**
- **employee_documents.employee_id → employees.employee_id**
- **employee_documents.attachment_id → attachments.attachment_id**
- **employee_attendance.employee_id → employees.employee_id**
- **employee_attendance.device_id → attendance_devices.device_id**
- **leaves.employee_id → employees.employee_id**
- **leaves.leave_type_id → leave_types.leave_type_id**
- **leaves.approved_by → users.user_id**
- **employee_salaries.employee_id → employees.employee_id**
- **employee_salaries.period_id → fiscal_periods.period_id**
- **employee_salaries.bank_account_id → bank_accounts.bank_account_id**
- **employee_salary_details.salary_id → employee_salaries.salary_id**
- **employee_salary_details.allowance_id → salary_allowances.allowance_id**
- **employee_salary_details.deduction_id → salary_deductions.deduction_id**
- **employee_overtimes.employee_id → employees.employee_id**
- **employee_overtimes.period_id → fiscal_periods.period_id**
- **payroll_runs.period_id → fiscal_periods.period_id**
- **payroll_runs.created_by/approved_by → users.user_id**
- **payroll_settings.updated_by → users.user_id**

---

## ملاحظات عملية

- يدعم النظام كامل دورة شؤون الموظفين: بيانات، توثيق، عقود، إدارات، وظائف، درجات، حضور وانصراف، إجازات، مستندات، جهات اتصال، رواتب، بدلات، استقطاعات، تشغيل الرواتب.
- كل العمليات تدعم الربط مع المالية، البنوك، الفروع، الأقسام، نظام الإشعارات، الدوام، المرفقات، سجلات التدقيق.
- يدعم النظام تعدد أنواع العقود، تعدد العملات، المرونة في البدلات والاستقطاعات، جداول الرواتب التفصيلية، العمل الإضافي، الإجازات بأنواعها، السلم الوظيفي.
- جاهز للربط مع الأنظمة الحكومية (تأمينات، ضرائب)، أنظمة الحضور بالبصمة، بوابات الموظفين، التقارير التحليلية.
- كل جداول الرواتب والتشغيل قابلة للربط التلقائي بالقيود المحاسبية عند الصرف.

---