## **0. Global foundation**

- [x]  **Core app shell**
- [x]  Recreate App.vue shell, global layout, and main entry (router + store + i18n + axios).
- [x]  Rebuild Header, HeaderPortal, Footer, SidebarSection, TopBar, NavDropdown navigation patterns.
- [x]  Re‑implement global loaders, error handling, and notification/toast system.
- [x]  **Shared form & UI components**
    - [x]  Recreate generic form inputs: TextInput, NumberInput, PasswordInput, TextAreaInput, SelectInput, CheckboxInput, RadioInput, FileInput, CalendarInput.
    - [x]  Rebuild custom shared components: Loader, OTP, Timer, calendar, error messages, reCAPTCHA, icons, etc.
    - [x]  Ensure consistent validation messages and UX across all forms.
- [ ]  Access control & roles (Partially done, could be revisited)

### **1. Public/Main Views**

- [x]  **Home Page** (`/home/`) - Landing page
- [x]  **Execution Sanads** (`/execution-sanads/`) - Public sanad execution view
- [x]  **FAQ** (`/faq/`) - Frequently asked questions
- [x]  **Privacy Policy** (`/privacy-policy/`)
- [x]  **Contact Us** (`/contact-us/`)
- [ ]  **Short URL Redirect** (`/:urlHash`, `/main/:urlHash`) - Dynamic URL hash redirection system

---

### **2. Authentication Module**

- [x]  **Individual Login (IAM SSO)** (`/accounts/login/individual/`) - IAM-based login
- [x]  **Individual Login (Credentials)** (`/accounts/login/credentials/individual/`) - Username/password login (when IAM SSO disabled)
- [x]  **Company Login** (`/accounts/login/company/`)
- [x]  **Logout** (`/accounts/logout/`)
- [x]  **IAM Callback** (`/iam-callback/`) - SSO callback handler
- [x]  **Mobile Random Code Verification** - Two-factor authentication flow
- [x]  **Show Mobile Random Code** (`/accounts/login/individual/show-number/:trans_id/`)

---

### **3. Account Management Module**

- [ ]  **Verify Phone Number** (`/accounts/verify-phone-number/`)
- [x]  **OTP Verification** (`/accounts/otp-verification/:token`)
- [ ]  **Change Phone Number** (`/accounts/change-phone-number/:token`) - Requires auth
- [ ]  **Change Password** (`/accounts/change-password/:token`) - For non-individual users
- [ ]  **Force Change Password** (`/accounts/force-change-password/`) - Expired password handling
- [ ]  **Generate OTP** (`/accounts/generate-otp/`)
- [ ]  **Verify Email** (`/verify-email/:uidb64/:token`)

---

### **4. Company Onboarding Module**

- [ ]  **Onboarding Index** (`/company-onboarding/`) - Registration path selection
- [ ]  **Company Registration Steps** (`/company-onboarding/registration/:step/:uuid?`)
- [ ]  **Terms & Conditions** (`/company-onboarding/terms-conditions/:uuid?`)
- [ ]  **Request Status** (`/company-onboarding/request-status/:status/:uuid`)
- [ ]  **Process T&C Approval/Rejection** (`/company-onboarding/process-terms-conditions-approval/:status/:uuid`)
- [ ]  **Update Company Request** (`/company-onboarding/update-company-request/:uuid`)
- [ ]  **Legal/Law Firms Onboarding** (redirects to backend)

---

### **5. Portal Dashboard**

- [ ]  **Portal Index/Dashboard** (`/portal/`) - Main dashboard with conditional routing based on user type/permissions
- [ ]  **Najiz Integration** (`/portal/najiz/`) - For individuals and companies

---

### **6. Account Profile Module**

- [ ]  **Account Profile** (`/portal/account/profile/`) - Individual user profile with IBAN verification

---

### **7. Sanad (Individual) Creation Module**

- [ ]  **Sanad Creation Types Selection** (`/portal/sanad-create/`)
- [ ]  **Sanad Creation Steps** (`/portal/sanad-create/sanad/creation/:step/:uuid?`)
    - Steps: `sanad-shape`, and other steps defined in `sanadCreationSteps` enum
- [ ]  **Blocked Sanad Creation** (`/portal/sanad-create/sanad/creation/blocked`)

---

### **8. Sanad B2B Creation Module**

- [ ]  **B2B Sanad Creation Steps** (`/portal/sanad-create/sanad-b2b/creation/:step/:uuid?`)
    - Steps defined in `sanadB2BCreationSteps` enum (starts with `sanad-type`)
- [ ]  **Blocked B2B Sanad Creation** (`/portal/sanad-create/sanad-b2b/creation/blocked`)
- [ ]  **B2B Sanad Description** (`/portal/sanad-create/sanad-b2b/creation/description`)

---

### **9. Sanad Approval Module (Guest & Authenticated)**

### **Guest Sanad Approval:**

- [ ]  **Sanad Approval** (`/sanad/approval/:uuid`)
- [ ]  **Sanad Approval Preview** (`/sanad/approval/preview/:uuid`)
- [ ]  **Sanad Approval Confirmation** (`/sanad/approval/confirmation/:uuid`)
- [ ]  **Sanad Approval Success** (`/sanad/approval/confirmation/success/:uuid`)

### **Authenticated Sanad Approval:**

- [ ]  **Portal Sanad Approval** (`/portal/sanad/approval/:uuid`)
- [ ]  **Portal Sanad Approval Preview** (`/portal/sanad/approval/preview/:uuid`)
- [ ]  **Portal Sanad Approval Confirmation** (`/portal/sanad/approval/confirmation/:uuid`)
- [ ]  **Portal Sanad Approval Success** (`/portal/sanad/approval/confirmation/success/:uuid`)

---

### **10. Sanad B2B Approval Module (Guest & Authenticated)**

### **Guest B2B Approval:**

- [ ]  **B2B Sanad Approval** (`/sanad-b2b/approval/:uuid/:approverId`)
- [ ]  **B2B Sanad Approval Preview** (`/sanad-b2b/approval/preview/:uuid/:approverId`)
- [ ]  **B2B Sanad Approval Confirmation** (`/sanad-b2b/approval/confirmation/:uuid/:approverId`)
- [ ]  **B2B Sanad Approval Success** (`/sanad-b2b/approval/confirmation/success/:uuid/:approverId`)

### **Authenticated B2B Approval:**

- [ ]  **Portal B2B Sanad Approval** (`/portal/sanad-b2b/approval/:uuid/:approverId`)
- [ ]  **Portal B2B Sanad Approval Preview** (`/portal/sanad-b2b/approval/preview/:uuid/:approverId`)
- [ ]  **Portal B2B Sanad Approval Confirmation** (`/portal/sanad-b2b/approval/confirmation/:uuid/:approverId`)
- [ ]  **Portal B2B Sanad Approval Success** (`/portal/sanad-b2b/approval/confirmation/success/:uuid/:approverId`)

---

### **11. Sanad Export Module**

- [ ]  **Sanad PDF Export** (`/sanad/export/:uuid/:sanad_uuid?`)
- [ ]  **B2B Sanad PDF Export** (`/sanad-b2b/export/:uuid/:approverId?/:sanad_uuid?`)

---

### **12. Sanad Management Module (Individual)**

- [x]  **Sanads Types Selection** (`/portal/sanads/`)
- [x]  **Sanad Management Container** (`/portal/sanads/sanad/management/`)
- [x]  **Pending Sanads (Awaiting Approval)** (`pending-sanads`)
- [x]  **Debit Sanads** (`debit-sanads`)
- [x]  **Credit Sanads** (`credit-sanads`)
- [x]  **Credit Own Sanads** (`credit-own-sanads`)

---

### **13. Sanad B2B Management Module**

- [x]  **B2B Sanad Management Container** (`/portal/sanads/sanad-b2b/management/`)
- [x]  **Pending B2B Sanads** (`pending-sanads`)
- [x]  **Guarantor Sanads** (`guaranteed-sanads`)
- [x]  **Signatory Sanads** (`signatory-sanads`)
- [x]  **Debit B2B Sanads** (`debit-sanads`)
- [x]  **Credit B2B Sanads** (`credit-sanads`)
- [x]  **Credit Own B2B Sanads** (`credit-own-sanads`)
- [ ]  **Preview Sanad Details** (`/portal/sanads/:category/:uuid`)

---

### **14. Sanad Settlement Module**

- [ ]  **Settlement Payment** (`/portal/sanad/settlement/payment/:uuid`)
- [ ]  **Settlement Payment History** (`/portal/sanad/settlement/payment-history/:uuid`)
- [ ]  **Settlement Invoice (Sadad)** (`/portal/sanad/settlement/invoice/:invoiceId/:uuid`)

---

### **15. Payment Module**

- [ ]  **Payment Result** (`/portal/payment/payment-result/`)
- [ ]  **B2B Payment Result** (`/portal/payment/b2b-payment-result/`)

---

### **16. Company Integration Module**

- [ ]  **Integration Container** (`/portal/company/integration/`)
- [ ]  **App Information** (`/portal/company/integration/app-information/`)
- [ ]  **Onboarding Details** (`/portal/company/integration/onboarding-details/`)
- [ ]  **APIs Documentation** (`/portal/company/integration/apis-documentation/`)
- [ ]  **B2B APIs Documentation** (`/portal/company/integration/b2b-apis-documentation/`)

---

### **17. Integrator Module**

- [ ]  **Integrator Container** (`/portal/company/integrator/`)
- [ ]  **Integrator App Information** (`/portal/company/integrator/app-information/`)

---

### **18. Company Settings Module**

- [ ]  **Platform Settings** (`/portal/company/settings/`)

---

### **19. Employee Management Module**

- [x]  **Employee List Container** (`/portal/company/employees`)
- [x]  **Active Employees** (`active-employees`)
- [x]  **Inactive Employees** (`inactive-employees`)
- [x]  **Mailing List Employees** (`mailing-list-employees`)
- [x]  **Add New Employee** (`/portal/company/employee/add-new`)
- [x]  **Set Employee Password** (`/portal/company/employee/set-password/:employeeId`)
- [x]  **Update Employee Email** (`/portal/company/employee/update-email/:employeeId`)
- [x]  **Set Employee Permissions** (`/portal/company/employee/set-permissions/:status/:employeeId`)

---

### **20. Callback History Module (Individual)**

- [x]  **Callback History Types** (`/portal/callback/`)
- [x]  **Callback History Container** (`/portal/callback/callback-history/`)
- [x]  **Work In Progress** (`in-progress`)
- [x]  **Communicated** (`communicated`)
- [x]  **Communication Failed** (`communicate-failed`)
- [x]  **Transmission Failed** (`transmit-failed`)

---

### **21. Callback History B2B Module**

- [x]  **B2B Callback History Container** (`/portal/callback/callback-history-b2b/`)
- [x]  **B2B Work In Progress** (`in-progress`)
- [x]  **B2B Communicated** (`communicated`)
- [x]  **B2B Communication Failed** (`communicate-failed`)
- [x]  **B2B Transmission Failed** (`transmit-failed`)

---

### **22. Financial Details Module**

- [x]  **Financial Details Index** (`/portal/financial-details/`)
- [x]  **Sanad Payment History** (`/portal/financial-details/sanads/payment-history/`)

### **Prepaid Packages:**

- [x]  **Packages List** (`/portal/financial-details/packages/list/`)
- [x]  **Packages History** (`/portal/financial-details/packages/packages-history/`)
- [x]  **Sadad Invoice Success** (`/portal/financial-details/packages/sadad-invoice-success/:invoiceId/`)
- [x]  **Packages Balance** (`/portal/financial-details/packages/packages-balance/`)

### **Monthly Invoices:**

- [x]  **Monthly Invoices List** (`/portal/financial-details/monthly-invoices/list/`)
- [x]  **Invoiced Sanads** (`/portal/financial-details/monthly-invoices/invoiced-sanads/:invoiceId`)

### **Subscriptions:**

- [x]  **Annual Subscription** (`/portal/financial-details/annual-subscription/`)
- [x]  **API Subscription** (`/portal/financial-details/api-subscription/`)

---

### **23. Reports Module**

- [ ]  **Reports Index** (`/portal/reports/`)
- [ ]  **Detailed Sanad Report** (`/portal/reports/sanad-detailed/`)
- [ ]  **Financial Consumption Report** (`/portal/reports/financial-consumption/`)

---

### **24. Digital Signature Module**

- [ ]  **Digital Signature Management** (`/portal/digital-signature/management/`)

---

### **25. Legacy/V1 Redirects (Backend)**

- [ ]  **Contracts** (`/portal/contracts/`) - Redirects to backend
- [ ]  **Lawyers Management** (`/portal/contracts/lawyers/`) - Redirects to backend
- [ ]  **Config** (`/portal/config/`) - Redirects to backend

---

### **26. Error Handling Views**

- [ ]  **Unauthorized** (`/unauthorized`)
- [ ]  **404 Not Found** (`/:pathMatch(.*)*`)

---

### **27. Tasks not related to a specifc module**

- [ ]  UI/UX changes to match old design
- [ ]  Forced user actions ⇒ (Password change, company info update, terms approval, etc)

---

### **28. Third-Party Integrations**

- [x]  **Recaptcha.vue** - Google reCAPTCHA v3 (dynamic script loading, token emission)
- [x]  **ZohoSalesIQ.vue** - Zoho chat widget (hidden on portal routes)
- [ ]  **HyperpayGateway.vue** - Payment gateway integration
- [ ]  **Google Analytics 4** - vue-gtag (`VITE_GA_MEASUREMENT_ID`)
- [ ]  **Google Tag Manager** - vue-gtm (`VITE_GTM_ID`)