# 🟣 Domain Layer Structure (تفصيلي - جميع الموديولات)

## الهدف من الطبقة
طبقة الـDomain هي قلب النظام وتحتوي منطق الأعمال المجرد (Business Logic)، الكيانات، كائنات القيمة، التجميعات، الأحداث، العقود، التعدادات، والاستثناءات، بدون أي ارتباط مباشر بالتقنيات أو الطبقات الأخرى.

---

## الهيكل الكامل (شامل جميع الموديولات)

```
src/
└── Domain/
    ├─ Common/
    │   ├─ Entities/
    │   │   ├─ BaseEntity.cs
    │   │   ├─ AuditableEntity.cs
    │   │   ├─ AggregateRoot.cs
    │   ├─ ValueObjects/
    │   │   ├─ Money.cs
    │   │   ├─ Address.cs
    │   ├─ Primitives/
    │   │   ├─ EntityId.cs
    │   ├─ Aggregates/
    │   │   ├─ AggregateRoot.cs
    │   ├─ Events/
    │   │   ├─ IDomainEvent.cs
    │   │   ├─ AbstractDomainEvent.cs
    │   ├─ Enums/
    │   │   ├─ CurrencyType.cs
    │   ├─ Exceptions/
    │   │   ├─ BusinessException.cs
    │   │   ├─ NotFoundException.cs
    │   └─ Interfaces/
    │       ├─ IRepository.cs
    │       ├─ IUnitOfWork.cs
    │       ├─ IDomainEventDispatcher.cs
    ├─ ERP/
    │   ├─ Accounts/
    │   │   ├─ Entities/
    │   │   │   ├─ Account.cs
    │   │   │   ├─ AccountGroup.cs
    │   │   │   ├─ AccountType.cs
    │   │   ├─ ValueObjects/
    │   │   │   ├─ AccountCode.cs
    │   │   ├─ Events/
    │   │   │   ├─ AccountCreatedEvent.cs
    │   │   ├─ Enums/
    │   │   │   ├─ AccountNature.cs
    │   │   │   ├─ AccountStatus.cs
    │   │   ├─ Exceptions/
    │   │   │   ├─ InvalidAccountTypeException.cs
    │   ├─ Journals/
    │   │   ├─ Entities/
    │   │   │   ├─ JournalEntry.cs
    │   │   │   ├─ JournalDetail.cs
    │   │   ├─ Events/
    │   │   │   ├─ JournalPostedEvent.cs
    │   │   ├─ Enums/
    │   │   │   ├─ JournalType.cs
    │   │   │   ├─ JournalStatus.cs
    │   ├─ Invoices/
    │   │   ├─ Entities/
    │   │   │   ├─ Invoice.cs
    │   │   │   ├─ InvoiceLine.cs
    │   │   ├─ ValueObjects/
    │   │   │   ├─ TaxDetail.cs
    │   │   ├─ Events/
    │   │   │   ├─ InvoicePaidEvent.cs
    │   │   ├─ Enums/
    │   │   │   ├─ InvoiceType.cs
    │   │   │   ├─ InvoiceStatus.cs
    │   ├─ Customers/
    │   │   ├─ Entities/
    │   │   │   ├─ Customer.cs
    │   │   │   ├─ CustomerGroup.cs
    │   │   ├─ ValueObjects/
    │   │   │   ├─ CustomerAddress.cs
    │   │   ├─ Events/
    │   │   │   ├─ CustomerCreatedEvent.cs
    │   ├─ Suppliers/
    │   │   ├─ Entities/
    │   │   │   ├─ Supplier.cs
    │   │   │   ├─ SupplierGroup.cs
    │   │   ├─ ValueObjects/
    │   │   │   ├─ SupplierAddress.cs
    │   │   ├─ Events/
    │   │   │   ├─ SupplierCreatedEvent.cs
    │   ├─ Products/
    │   │   ├─ Entities/
    │   │   │   ├─ Product.cs
    │   │   │   ├─ ProductCategory.cs
    │   │   │   ├─ ProductUnit.cs
    │   │   ├─ ValueObjects/
    │   │   │   ├─ ProductBarcode.cs
    │   │   ├─ Events/
    │   │   │   ├─ ProductCreatedEvent.cs
    │   │   ├─ Enums/
    │   │   │   ├─ ProductType.cs
    │   │   │   ├─ ProductStatus.cs
    │   ├─ Warehouses/
    │   │   ├─ Entities/
    │   │   │   ├─ Warehouse.cs
    │   │   │   ├─ WarehouseLocation.cs
    │   │   ├─ Events/
    │   │   │   ├─ WarehouseCreatedEvent.cs
    │   ├─ Sales/
    │   │   ├─ Entities/
    │   │   │   ├─ SalesOrder.cs
    │   │   │   ├─ SalesOrderLine.cs
    │   │   ├─ Events/
    │   │   │   ├─ SalesOrderCreatedEvent.cs
    │   │   ├─ Enums/
    │   │   │   ├─ SalesOrderStatus.cs
    │   ├─ Purchases/
    │   │   ├─ Entities/
    │   │   │   ├─ PurchaseOrder.cs
    │   │   │   ├─ PurchaseOrderLine.cs
    │   │   ├─ Events/
    │   │   │   ├─ PurchaseOrderCreatedEvent.cs
    │   │   ├─ Enums/
    │   │   │   ├─ PurchaseOrderStatus.cs
    │   ├─ HR/
    │   │   ├─ Entities/
    │   │   │   ├─ Employee.cs
    │   │   │   ├─ Department.cs
    │   │   │   ├─ JobTitle.cs
    │   │   │   ├─ SalaryRecord.cs
    │   │   ├─ Events/
    │   │   │   ├─ EmployeeHiredEvent.cs
    │   ├─ Users/
    │   │   ├─ Entities/
    │   │   │   ├─ User.cs
    │   │   │   ├─ Role.cs
    │   │   │   ├─ UserRole.cs
    │   │   ├─ Events/
    │   │   │   ├─ UserCreatedEvent.cs
    │   │   ├─ Enums/
    │   │   │   ├─ UserStatus.cs
    │   ├─ Settings/
    │   │   ├─ Entities/
    │   │   │   ├─ SystemSetting.cs
    │   │   │   ├─ NumberingSetting.cs
    │   │   ├─ Events/
    │   │   │   ├─ SettingsChangedEvent.cs
    │   ├─ Notifications/
    │   │   ├─ Entities/
    │   │   │   ├─ Notification.cs
    │   │   │   ├─ NotificationTemplate.cs
    │   │   ├─ Events/
    │   │   │   ├─ NotificationSentEvent.cs
    │   ├─ Reports/
    │   │   ├─ Entities/
    │   │   │   ├─ ReportDefinition.cs
    │   │   │   ├─ ReportParameter.cs
    │   │   ├─ Events/
    │   │   │   ├─ ReportGeneratedEvent.cs
    │   ├─ Audit/
    │   │   ├─ Entities/
    │   │   │   ├─ AuditLog.cs
    │   │   │   ├─ ChangeHistory.cs
    │   │   ├─ Events/
    │   │   │   ├─ AuditLogCreatedEvent.cs
    │   ├─ Inventory/
    │   │   ├─ Entities/
    │   │   │   ├─ InventoryItem.cs
    │   │   │   ├─ InventoryTransaction.cs
    │   │   │   ├─ StockBalance.cs
    │   │   ├─ Events/
    │   │   │   ├─ InventoryAdjustedEvent.cs
    │   ├─ FixedAssets/
    │   │   ├─ Entities/
    │   │   │   ├─ FixedAsset.cs
    │   │   │   ├─ DepreciationRecord.cs
    │   │   ├─ Events/
    │   │   │   ├─ AssetPurchasedEvent.cs
    │   ├─ Projects/
    │   │   ├─ Entities/
    │   │   │   ├─ Project.cs
    │   │   │   ├─ ProjectTask.cs
    │   │   ├─ Events/
    │   │   │   ├─ ProjectCreatedEvent.cs
    │   ├─ Contracts/
    │   │   ├─ Entities/
    │   │   │   ├─ Contract.cs
    │   │   │   ├─ ContractParty.cs
    │   │   ├─ Events/
    │   │   │   ├─ ContractSignedEvent.cs
    │   ├─ Loans/
    │   │   ├─ Entities/
    │   │   │   ├─ Loan.cs
    │   │   │   ├─ LoanPayment.cs
    │   │   ├─ Events/
    │   │   │   ├─ LoanCreatedEvent.cs
    │   ├─ Payroll/
    │   │   ├─ Entities/
    │   │   │   ├─ PayrollRecord.cs
    │   │   │   ├─ PayrollDetail.cs
    │   │   ├─ Events/
    │   │   │   ├─ PayrollProcessedEvent.cs
    │   ├─ Taxes/
    │   │   ├─ Entities/
    │   │   │   ├─ Tax.cs
    │   │   │   ├─ TaxRate.cs
    │   │   ├─ Events/
    │   │   │   ├─ TaxRateChangedEvent.cs
    │   ├─ Banking/
    │   │   ├─ Entities/
    │   │   │   ├─ BankAccount.cs
    │   │   │   ├─ BankTransaction.cs
    │   │   ├─ Events/
    │   │   │   ├─ BankTransferEvent.cs
    │   ├─ POS/
    │   │   ├─ Entities/
    │   │   │   ├─ POSReceipt.cs
    │   │   │   ├─ POSSession.cs
    │   │   ├─ Events/
    │   │   │   ├─ POSReceiptIssuedEvent.cs
    │   ├─ CRM/
    │   │   ├─ Entities/
    │   │   │   ├─ Lead.cs
    │   │   │   ├─ Opportunity.cs
    │   │   ├─ Events/
    │   │   │   ├─ LeadConvertedEvent.cs
    │   ├─ ECommerce/
    │   │   ├─ Entities/
    │   │   │   ├─ EOrder.cs
    │   │   │   ├─ EOrderItem.cs
    │   │   ├─ Events/
    │   │   │   ├─ EOrderPlacedEvent.cs
    │   ├─ Manufacturing/
    │   │   ├─ Entities/
    │   │   │   ├─ WorkOrder.cs
    │   │   │   ├─ BillOfMaterials.cs
    │   │   ├─ Events/
    │   │   │   ├─ WorkOrderCreatedEvent.cs
    │   └─ ... (أي Modules تخصصية أخرى)
    └─ Domain.csproj
```

---

## أهم الملاحظات
- **كل Module مستقل بذاته** ويحتوي جميع كياناته وأحداثه وقيمه وأنواعه ومستثنياته.
- **Common**: كل ما هو مشترك وقاعدي لجميع الموديولات (مثل الكيانات الأساسية، كائنات القيمة، الأحداث العامة، العقود، إلخ).
- **Events**: تدعم الأحداث التفاعلية بين كيانات الدومين (مثل إصدار فاتورة، ترحيل قيد، إلخ).
- **Entities/ValueObjects/Enums/Exceptions**: تُستخدم داخل كل Module كما هو متعارف عليه في تصميم DDD.
- **يمكنك إضافة أو حذف أو تخصيص أي كيان أو ملف حسب متطلبات مشروعك الفعلية**.

---

**هذه البنية تضمن وضوح الكود وقابلية التوسع والصيانة لأي نظام ERP احترافي.**