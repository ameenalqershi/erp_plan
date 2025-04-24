# 🟡 Application Layer Structure (تفصيلي لكل موديول حسب تحليل مخططات مشروعك)

---

## الهدف من الطبقة  
طبقة Application هي عقل تدفق التطبيق ("Application Logic")، وتعمل كوسيط بين الـDomain والبنية التحتية والواجهات.  
تحتوي أوامر واستعلامات CQRS، المعالجات، التحقق، خدمات تطبيقية، ونماذج نقل البيانات.

---

## الهيكل العام  
```
src/
└── Application/
    ├─ Abstractions/
    ├─ Behaviors/
    ├─ Common/
    ├─ ContextValidators/
    ├─ Enums/
    ├─ Exceptions/
    ├─ Features/
    │   ├─ Accounts/
    │   ├─ Journals/
    │   ├─ Invoices/
    │   ├─ Customers/
    │   ├─ Suppliers/
    │   ├─ Products/
    │   ├─ Warehouses/
    │   ├─ Sales/
    │   ├─ Purchases/
    │   ├─ HR/
    │   ├─ Users/
    │   ├─ Settings/
    │   ├─ Notifications/
    │   ├─ Reports/
    │   ├─ Audit/
    │   ├─ Inventory/
    │   ├─ FixedAssets/
    │   ├─ Projects/
    │   ├─ Contracts/
    │   ├─ Loans/
    │   ├─ Payroll/
    │   ├─ Taxes/
    │   ├─ Banking/
    │   ├─ POS/
    │   ├─ CRM/
    │   ├─ ECommerce/
    │   ├─ Manufacturing/
    │   └─ ...
    └─ Application.csproj
```

---

## 🟢 المجلدات العامة ومحتواها

### Abstractions/
```
Abstractions/
├─ Interfaces/
│   ├─ ICurrentUserService.cs
│   ├─ IDateTimeService.cs
│   ├─ IEmailService.cs
│   ├─ INotificationService.cs
│   ├─ IReportingService.cs
│   ├─ IFileStorageService.cs
│   └─ ...
├─ Services/
│   ├─ CurrentUserService.cs
│   ├─ DateTimeService.cs
│   ├─ EmailService.cs
│   ├─ NotificationService.cs
│   ├─ ReportingService.cs
│   └─ ...
```

### Behaviors/
```
Behaviors/
├─ ValidationBehavior.cs
├─ LoggingBehavior.cs
├─ PerformanceBehavior.cs
├─ CachingBehavior.cs
├─ ExceptionHandlingBehavior.cs
└─ ...
```

### Common/
```
Common/
├─ Configurations/
│   ├─ MappingProfile.cs
│   ├─ ValidationConfig.cs
├─ Extensions/
│   ├─ StringExtensions.cs
│   ├─ DateTimeExtensions.cs
├─ Mappings/
│   ├─ DtoToEntityMapping.cs
│   ├─ EntityToDtoMapping.cs
├─ Responses/
│   ├─ Result.cs
│   ├─ ApiResponse.cs
│   ├─ ErrorResponse.cs
│   ├─ PaginatedResult.cs
├─ Serialization/
│   ├─ JsonSerializerService.cs
├─ Specifications/
│   ├─ PaginatedSpecification.cs
├─ Validators/
│   ├─ EmailValidator.cs
│   ├─ PaginationValidator.cs
```

### ContextValidators/
```
ContextValidators/
├─ PermissionValidator.cs
├─ UserContextValidator.cs
├─ OrganizationContextValidator.cs
```

### Enums/
```
Enums/
├─ ApplicationStatus.cs
├─ NotificationType.cs
├─ UserRole.cs
├─ ReportType.cs
├─ PaginationOrder.cs
```

### Exceptions/
```
Exceptions/
├─ ApplicationException.cs
├─ ValidationException.cs
├─ NotFoundException.cs
├─ UnauthorizedAccessException.cs
├─ ConflictException.cs
├─ ForbiddenException.cs
```

---

## 📦 تفصيل كل مجلد موديول حسب تحليلكم لمخططات المشروع

---

### 1. **Accounts/**
```
Commands/
    CreateAccountCommand.cs
    UpdateAccountCommand.cs
    DeleteAccountCommand.cs
    ArchiveAccountCommand.cs
    CreateAccountGroupCommand.cs
    UpdateAccountGroupCommand.cs
Queries/
    GetAccountByIdQuery.cs
    GetAccountsListQuery.cs
    GetAccountGroupTreeQuery.cs
Dtos/
    AccountDto.cs
    AccountGroupDto.cs
    AccountTypeDto.cs
Handlers/
    CreateAccountHandler.cs
    ArchiveAccountHandler.cs
    GetAccountsListHandler.cs
Validators/
    CreateAccountValidator.cs
    ArchiveAccountValidator.cs
```

---

### 2. **Journals/**
```
Commands/
    CreateJournalCommand.cs
    UpdateJournalCommand.cs
    DeleteJournalCommand.cs
    PostJournalCommand.cs
    ReverseJournalCommand.cs
    ApproveJournalCommand.cs
Queries/
    GetJournalByIdQuery.cs
    GetJournalsListQuery.cs
    SearchJournalsQuery.cs
Dtos/
    JournalDto.cs
    JournalDetailDto.cs
Handlers/
    CreateJournalHandler.cs
    PostJournalHandler.cs
    GetJournalsListHandler.cs
Validators/
    CreateJournalValidator.cs
    PostJournalValidator.cs
```

---

### 3. **Invoices/**
```
Commands/
    CreateInvoiceCommand.cs
    UpdateInvoiceCommand.cs
    DeleteInvoiceCommand.cs
    PayInvoiceCommand.cs
    CancelInvoiceCommand.cs
Queries/
    GetInvoiceByIdQuery.cs
    GetInvoicesListQuery.cs
    GetPaidInvoicesQuery.cs
Dtos/
    InvoiceDto.cs
    InvoiceLineDto.cs
Handlers/
    CreateInvoiceHandler.cs
    PayInvoiceHandler.cs
    GetInvoicesListHandler.cs
Validators/
    CreateInvoiceValidator.cs
    PayInvoiceValidator.cs
```

---

### 4. **Customers/**
```
Commands/
    CreateCustomerCommand.cs
    UpdateCustomerCommand.cs
    DeleteCustomerCommand.cs
    AssignCustomerGroupCommand.cs
Queries/
    GetCustomerByIdQuery.cs
    GetCustomersListQuery.cs
    GetCustomerGroupsQuery.cs
Dtos/
    CustomerDto.cs
    CustomerGroupDto.cs
Handlers/
    CreateCustomerHandler.cs
    AssignCustomerGroupHandler.cs
Validators/
    CreateCustomerValidator.cs
    AssignCustomerGroupValidator.cs
```

---

### 5. **Suppliers/**
```
Commands/
    CreateSupplierCommand.cs
    UpdateSupplierCommand.cs
    DeleteSupplierCommand.cs
    AssignSupplierGroupCommand.cs
Queries/
    GetSupplierByIdQuery.cs
    GetSuppliersListQuery.cs
    GetSupplierGroupsQuery.cs
Dtos/
    SupplierDto.cs
    SupplierGroupDto.cs
Handlers/
    CreateSupplierHandler.cs
    AssignSupplierGroupHandler.cs
Validators/
    CreateSupplierValidator.cs
    AssignSupplierGroupValidator.cs
```

---

### 6. **Products/**
```
Commands/
    CreateProductCommand.cs
    UpdateProductCommand.cs
    DeleteProductCommand.cs
    AssignProductCategoryCommand.cs
Queries/
    GetProductByIdQuery.cs
    GetProductsListQuery.cs
    GetProductCategoriesQuery.cs
Dtos/
    ProductDto.cs
    ProductCategoryDto.cs
    ProductUnitDto.cs
Handlers/
    CreateProductHandler.cs
    AssignProductCategoryHandler.cs
Validators/
    CreateProductValidator.cs
    AssignProductCategoryValidator.cs
```

---

### 7. **Warehouses/**
```
Commands/
    CreateWarehouseCommand.cs
    UpdateWarehouseCommand.cs
    DeleteWarehouseCommand.cs
    TransferStockCommand.cs
Queries/
    GetWarehouseByIdQuery.cs
    GetWarehousesListQuery.cs
    GetStockBalanceQuery.cs
Dtos/
    WarehouseDto.cs
    WarehouseLocationDto.cs
    StockBalanceDto.cs
Handlers/
    CreateWarehouseHandler.cs
    TransferStockHandler.cs
Validators/
    CreateWarehouseValidator.cs
    TransferStockValidator.cs
```

---

### 8. **Sales/**
```
Commands/
    CreateSalesOrderCommand.cs
    UpdateSalesOrderCommand.cs
    DeleteSalesOrderCommand.cs
    ApproveSalesOrderCommand.cs
    CreateSalesInvoiceCommand.cs
    ReturnSalesOrderCommand.cs
Queries/
    GetSalesOrderByIdQuery.cs
    GetSalesOrdersListQuery.cs
    GetSalesStatisticsQuery.cs
Dtos/
    SalesOrderDto.cs
    SalesOrderLineDto.cs
    SalesStatisticsDto.cs
Handlers/
    CreateSalesOrderHandler.cs
    ApproveSalesOrderHandler.cs
Validators/
    CreateSalesOrderValidator.cs
    ApproveSalesOrderValidator.cs
```

---

### 9. **Purchases/**
```
Commands/
    CreatePurchaseOrderCommand.cs
    UpdatePurchaseOrderCommand.cs
    DeletePurchaseOrderCommand.cs
    ApprovePurchaseOrderCommand.cs
    CreatePurchaseInvoiceCommand.cs
    ReturnPurchaseOrderCommand.cs
Queries/
    GetPurchaseOrderByIdQuery.cs
    GetPurchaseOrdersListQuery.cs
    GetPurchaseStatisticsQuery.cs
Dtos/
    PurchaseOrderDto.cs
    PurchaseOrderLineDto.cs
    PurchaseStatisticsDto.cs
Handlers/
    CreatePurchaseOrderHandler.cs
    ApprovePurchaseOrderHandler.cs
Validators/
    CreatePurchaseOrderValidator.cs
    ApprovePurchaseOrderValidator.cs
```

---

### 10. **HR/**
```
Commands/
    CreateEmployeeCommand.cs
    UpdateEmployeeCommand.cs
    DeleteEmployeeCommand.cs
    AssignDepartmentCommand.cs
    RecordAttendanceCommand.cs
Queries/
    GetEmployeeByIdQuery.cs
    GetEmployeesListQuery.cs
    GetDepartmentsListQuery.cs
Dtos/
    EmployeeDto.cs
    DepartmentDto.cs
    JobTitleDto.cs
Handlers/
    CreateEmployeeHandler.cs
    AssignDepartmentHandler.cs
Validators/
    CreateEmployeeValidator.cs
    AssignDepartmentValidator.cs
```

---

### 11. **Users/**
```
Commands/
    CreateUserCommand.cs
    UpdateUserCommand.cs
    DeleteUserCommand.cs
    AssignRoleCommand.cs
Queries/
    GetUserByIdQuery.cs
    GetUsersListQuery.cs
    GetRolesListQuery.cs
Dtos/
    UserDto.cs
    RoleDto.cs
    UserRoleDto.cs
Handlers/
    CreateUserHandler.cs
    AssignRoleHandler.cs
Validators/
    CreateUserValidator.cs
    AssignRoleValidator.cs
```

---

### 12. **Settings/**
```
Commands/
    UpdateSystemSettingCommand.cs
    UpdateNumberingSettingCommand.cs
Queries/
    GetSystemSettingQuery.cs
    GetNumberingSettingQuery.cs
Dtos/
    SystemSettingDto.cs
    NumberingSettingDto.cs
Handlers/
    UpdateSystemSettingHandler.cs
Validators/
    UpdateSystemSettingValidator.cs
```

---

### 13. **Notifications/**
```
Commands/
    SendNotificationCommand.cs
    CreateNotificationTemplateCommand.cs
Queries/
    GetNotificationByIdQuery.cs
    GetNotificationsListQuery.cs
Dtos/
    NotificationDto.cs
    NotificationTemplateDto.cs
Handlers/
    SendNotificationHandler.cs
Validators/
    SendNotificationValidator.cs
```

---

### 14. **Reports/**
```
Commands/
    GenerateReportCommand.cs
Queries/
    GetReportByIdQuery.cs
    GetReportsListQuery.cs
Dtos/
    ReportDefinitionDto.cs
    ReportParameterDto.cs
Handlers/
    GenerateReportHandler.cs
Validators/
    GenerateReportValidator.cs
```

---

### 15. **Audit/**
```
Commands/
    LogAuditEventCommand.cs
Queries/
    GetAuditLogsQuery.cs
Dtos/
    AuditLogDto.cs
    ChangeHistoryDto.cs
Handlers/
    LogAuditEventHandler.cs
Validators/
    LogAuditEventValidator.cs
```

---

### 16. **Inventory/**
```
Commands/
    AdjustInventoryCommand.cs
    AddInventoryItemCommand.cs
Queries/
    GetInventoryItemByIdQuery.cs
    GetInventoryListQuery.cs
Dtos/
    InventoryItemDto.cs
    StockBalanceDto.cs
Handlers/
    AdjustInventoryHandler.cs
Validators/
    AdjustInventoryValidator.cs
```

---

### 17. **FixedAssets/**
```
Commands/
    CreateFixedAssetCommand.cs
    RecordDepreciationCommand.cs
Queries/
    GetFixedAssetByIdQuery.cs
    GetFixedAssetsListQuery.cs
Dtos/
    FixedAssetDto.cs
    DepreciationRecordDto.cs
Handlers/
    CreateFixedAssetHandler.cs
    RecordDepreciationHandler.cs
Validators/
    CreateFixedAssetValidator.cs
```

---

### 18. **Projects/**
```
Commands/
    CreateProjectCommand.cs
    AssignTaskCommand.cs
Queries/
    GetProjectByIdQuery.cs
    GetProjectsListQuery.cs
Dtos/
    ProjectDto.cs
    ProjectTaskDto.cs
Handlers/
    CreateProjectHandler.cs
Validators/
    CreateProjectValidator.cs
```

---

### 19. **Contracts/**
```
Commands/
    CreateContractCommand.cs
    SignContractCommand.cs
Queries/
    GetContractByIdQuery.cs
    GetContractsListQuery.cs
Dtos/
    ContractDto.cs
    ContractPartyDto.cs
Handlers/
    CreateContractHandler.cs
Validators/
    CreateContractValidator.cs
```

---

### 20. **Loans/**
```
Commands/
    CreateLoanCommand.cs
    AddLoanPaymentCommand.cs
Queries/
    GetLoanByIdQuery.cs
    GetLoansListQuery.cs
Dtos/
    LoanDto.cs
    LoanPaymentDto.cs
Handlers/
    CreateLoanHandler.cs
Validators/
    CreateLoanValidator.cs
```

---

### 21. **Payroll/**
```
Commands/
    ProcessPayrollCommand.cs
    AddPayrollRecordCommand.cs
Queries/
    GetPayrollRecordByIdQuery.cs
    GetPayrollListQuery.cs
Dtos/
    PayrollRecordDto.cs
    PayrollDetailDto.cs
Handlers/
    ProcessPayrollHandler.cs
Validators/
    ProcessPayrollValidator.cs
```

---

### 22. **Taxes/**
```
Commands/
    CreateTaxCommand.cs
    UpdateTaxRateCommand.cs
Queries/
    GetTaxByIdQuery.cs
    GetTaxesListQuery.cs
Dtos/
    TaxDto.cs
    TaxRateDto.cs
Handlers/
    CreateTaxHandler.cs
Validators/
    CreateTaxValidator.cs
```

---

### 23. **Banking/**
```
Commands/
    CreateBankAccountCommand.cs
    AddBankTransactionCommand.cs
Queries/
    GetBankAccountByIdQuery.cs
    GetBankAccountsListQuery.cs
Dtos/
    BankAccountDto.cs
    BankTransactionDto.cs
Handlers/
    CreateBankAccountHandler.cs
Validators/
    CreateBankAccountValidator.cs
```

---

### 24. **POS/**
```
Commands/
    OpenPOSSessionCommand.cs
    ClosePOSSessionCommand.cs
    IssuePOSReceiptCommand.cs
Queries/
    GetPOSReceiptByIdQuery.cs
    GetPOSReceiptsListQuery.cs
Dtos/
    POSReceiptDto.cs
    POSSessionDto.cs
Handlers/
    OpenPOSSessionHandler.cs
Validators/
    IssuePOSReceiptValidator.cs
```

---

### 25. **CRM/**
```
Commands/
    CreateLeadCommand.cs
    ConvertLeadCommand.cs
Queries/
    GetLeadByIdQuery.cs
    GetLeadsListQuery.cs
Dtos/
    LeadDto.cs
    OpportunityDto.cs
Handlers/
    CreateLeadHandler.cs
Validators/
    CreateLeadValidator.cs
```

---

### 26. **ECommerce/**
```
Commands/
    CreateEOrderCommand.cs
    AddEOrderItemCommand.cs
Queries/
    GetEOrderByIdQuery.cs
    GetEOrdersListQuery.cs
Dtos/
    EOrderDto.cs
    EOrderItemDto.cs
Handlers/
    CreateEOrderHandler.cs
Validators/
    CreateEOrderValidator.cs
```

---

### 27. **Manufacturing/**
```
Commands/
    CreateWorkOrderCommand.cs
    AddBOMCommand.cs
Queries/
    GetWorkOrderByIdQuery.cs
    GetWorkOrdersListQuery.cs
Dtos/
    WorkOrderDto.cs
    BillOfMaterialsDto.cs
Handlers/
    CreateWorkOrderHandler.cs
Validators/
    CreateWorkOrderValidator.cs
```

---

## 💡 ملاحظات ختامية
- كل موديول يحتوي نفس الهيكل التنظيمي مع تخصيص الأوامر والاستعلامات والنماذج حسب منطق ووظائف الموديول.
- الملفات يمكن أن تزيد أو تنقص طبقاً لعمق وتعقيد كل موديول.
- هذا التحليل والتفصيل يعكس أفضل الممارسات لمشاريع ERP ضخمة وحديثة.

**لأي موديول ترغب بشرح أعمق أو أمثلة كود واقعية أخبرني باسم الموديول بالتحديد.**