# Software Requirements Specification (SRS) - Stationary Shop Management System

## Document Version: 1.2
## Date: June 30, 2025

## 1. Introduction

### 1.1. Purpose of this Document
The primary purpose of this document is to comprehensively and clearly define all functional and non-functional requirements for the Stationery Shop Management System. It will serve as a comprehensive reference point for the system's development, ensuring that all stakeholders (shop owner, developers, testers, etc.) have a shared understanding of the system's capabilities and expected behavior.

### 1.2. Product Scope
This system will be a standalone web-based application designed to streamline the daily operations of a stationery shop. It will include core functionalities such as product and inventory management, sales billing, purchase management, user management, basic accounting, and reporting. The aim is to minimize manual processes, increase efficiency, and provide data-driven insights for business decisions.

### 1.3. Definitions, Acronyms, and Abbreviations
* **SRS:** Software Requirements Specification
* **RAD:** Requirement Analysis Document
* **SKU:** Stock Keeping Unit
* **MERN Stack:** MongoDB, Express.js, React, Node.js
* **API:** Application Programming Interface
* **RBAC:** Role-Based Access Control
* **PO:** Purchase Order
* **GRN:** Goods Receipt Note
* **NEF:** Near-Expiry First
* **GSTIN:** Goods and Services Tax Identification Number
* **UPI:** Unified Payments Interface
* **SA:** System Admin

## 2. Overall Description

### 2.1. Product Perspective
The Stationery Shop Management System is an independent web-based application. It will replace existing manual or partially digital processes and integrate all key business operations within the shop. Its goal is to act as a centralized system catering to the needs of various user roles.

### 2.2. Product Functions
The system will provide the following key functionalities:
* **User Management:** Secure login, role-based access control, user creation/modification, account status management.
* **Product and Inventory Management:** Product details, SKU and variation management, stock tracking for different packing units, stock updates (on purchase and sales), expired/junk stock management, re-order alerts.
* **Sales and Billing:** Quick bill generation, offline bill entry, support for various payment methods, discount management, customizable bill printing and PDF generation, bill attachments.
* **Supplier and Purchase Management:** Supplier/brand/company details, Purchase Order (PO) generation, Goods Receipt Note (GRN) entry, supplier invoice tracking.
* **Basic Accounting and Expense Tracking:** Record of all monetary transactions (expenses and receipts), cash/bank balance tracking, payables/receivables reports.
* **Reporting and Analytics:** Sales reports, purchase reports, inventory reports (stock levels, low stock, expired stock), financial summary reports.

### 2.3. User Characteristics
* **System Admin (SA / Super Admin):** Technically proficient, responsible for system setup and high-level configurations.
* **Admin (Shop Owner):** Deep understanding of business processes, access to most system modules, responsible for user management and key decisions. Basic understanding of technical knowledge.
* **Manager:** Proficient in specific business functions (e.g., inventory management, sales reports), limited access as defined by Admin.
* **Cashier/Salesperson:** Focus on sales billing and customer service, minimal technical knowledge required.
* **Other Users (e.g., Stock Keeper, Accountant):** Limited knowledge with specific function-based access.

### 2.4. General Constraints
* **Technical Stack:** MERN Stack (MongoDB, Express.js, React, Node.js).
* **Deployment:** Initial deployment will be on a local server, with future plans for cloud scaling.
* **Capacity:** Initially designed to handle 30 concurrent users and 5000 SKUs.
* **Language:** The default language of the system will be English. The System Admin (SA) will have the option to change the default mode to native languages (e.g., Hindi) during configuration or at a later stage.
* **Hardware Integration (Initial Phase):** Barcode/QR code scanning functionality will be developed, but testing and initial operations will be done through manual entry as scanner hardware is not currently available. In the future, when item count and operational load increase, barcode/QR code scanners will be integrated into the system.
* **Internet Connectivity:** The system will require a stable internet connection on the server for its operation (for remote access and updates).

### 2.5. Assumptions and Dependencies
* Users will have the necessary hardware (computers, printers) and operating system to run the system.
* Database integrity will be maintained (though strict schema enforcement might be less in the initial version).
* Sufficient disk space will be available for local file storage.
* Regular backups will be performed to ensure secure operation of the system.
* Budget and resources will be available for future migration to cloud storage (e.g., S3).

## 3. Specific Requirements

### 3.1. Functional Requirements

#### 3.1.1. User Management Module
* **FR1.1:** The system shall provide secure user login (email/employee ID and password).
* **FR1.2:** Passwords shall be stored in a hashed format.
* **FR1.3:** The system shall implement Role-Based Access Control (RBAC).
* **FR1.4:** SA and Admin shall be able to create new users and manage their roles/permissions.
* **FR1.5:** New users created via 'Registration' will require manual SA/Admin approval.
* **FR1.6:** Accounts shall automatically be 'Deactivated' after a specified number of failed login attempts (3 for Admin, 5 for others).
* **FR1.7:** Only SA shall be allowed to 'Activate' 'Deactivated' Admin accounts.
* **FR1.8:** Users shall be allowed to reset their passwords, after which the account will be 'Deactivated' and require 'Activation' by SA/Admin.
* **FR1.9:** The system shall be made compatible for detailed audit trail and activity logging in the future.
* **FR1.10:** There shall be a facility to upload and display user profile images (stored locally in `/uploads/users/`).

#### 3.1.2. Product & Inventory Module
* **FR2.1:** The system shall allow managing basic product information such as product name, main category, sub-category, brand.
* **FR2.2:** It shall manage different variations (e.g., color, size, lining type) with SKU-based uniqueness.
* **FR2.3:** It shall track MRP, selling price, current stock, and initial stock for each SKU.
* **FR2.4:** It shall generate and manage a unique custom barcode/QR code for each SKU.
* **FR2.5:** It shall provide the ability to define different packing units like 'Single Piece', 'Packet', 'Carton' and track conversion factors between them.
* **FR2.6:** It shall allow entering different purchase prices and selling prices for each packing unit.
* **FR2.7:** Stock shall always be tracked in the smallest measurable unit (e.g., 'Piece').
* **FR2.8:** It shall support product entry via manual entry, and in the future, barcode/QR code scanning, and bulk upload (CSV/Excel).
* **FR2.9:** Stock shall be updated upon receipt of new stock from suppliers by scanning barcodes/manual entry, including an option to enter batch numbers and expiry dates (if applicable).
* **FR2.10:** Stock shall automatically decrease upon sales and check for immediate stock availability at the time of sale.
* **FR2.11:** Expired items shall be automatically moved to 'Junk' stock, and the estimated value of 'Junk' stock shall be tracked.
* **FR2.12:** It shall suggest Near-Expiry First (NEF) priority at the time of sale.
* **FR2.13:** Item quantities shall be marked as 'Reserved' when creating a supply order and removed from effective stock. Items shall be added back to available stock if the order is canceled.
* **FR2.14:** There shall be a facility to upload and manage product images and SKU-specific images (stored locally in `/uploads/products/`).

#### 3.1.3. Sales & Billing Module
* **FR3.1:** Customer name shall be optional, defaulting to 'Walk-in Customer'.
* **FR3.2:** It shall allow entering offline bills into the system, with 'Offline Bill Reference Number' being mandatory and unique.
* **FR3.3:** It shall support real-time immediate bill generation and stock updates.
* **FR3.4:** It shall allow adding items to a bill by manual SKU entry, searching, dropdown, or in the future, barcode/QR code scanning.
* **FR3.5:** It shall allow recording non-billing sales and updating total sales and stock via a one-time 'Checkout' at shop closing.
* **FR3.6:** It shall support Cash, Card, UPI, and multiple/mixed payment methods for a single bill.
* **FR3.7:** It shall allow partial payments for school/office customers and update bill details on each payment.
* **FR3.8:** It shall display a generalized UPI QR code on the bill.
* **FR3.9:** It shall support discount management (item-wise as the difference between MRP and actual selling price), and on total bill (percentage-wise discount 1, discount 2, and round-off).
* **FR3.10:** It shall generate and print customizable bill/invoice formats including shop name, address, contact, GSTIN, UPI/Bank details, bill type, item-wise details, financial summary, and terms & conditions.
* **FR3.11:** It shall have the facility to save and download/print the bill as a PDF.
* **FR3.12:** It shall have the ability to generate three copies of the bill (customer, shop, backup), each appropriately labeled.
* **FR3.13:** It shall display watermarks like 'Paid', 'Pending', or 'Partially Paid' in the background of the bill.
* **FR3.14:** There shall be a facility to save additional files related to a bill (e.g., receipts, payment screenshots) as attachments (stored locally in `/uploads/bills/`).

#### 3.1.4. Supplier, Company, & Brand Management Module
* **FR4.1:** It shall manage supplier information (name, contact, address, GSTIN, payment terms, bank details).
* **FR4.2:** It shall track a detailed history of all purchases and payments made to each supplier.
* **FR4.3:** It shall manage company and brand information and link brands to the manufacturing company.
* **FR4.4:** It shall generate Purchase Orders (POs) in a customizable format for sending to suppliers (also in PDF).
* **FR4.5:** It shall support Goods Receipt Voucher (GRV)/Stock In entry to record actual quantities of received goods and handle variances from PO.
* **FR4.6:** It shall allow manual entry of bills/invoices received from suppliers and link them to respective POs and receipts.
* **FR4.7:** It shall generate supplier-wise purchase history, payment outstanding reports, and receipt reports (downloadable/printable in PDF/Excel).

#### 3.1.5. Reporting & Analytics Module
* **FR5.1:** It shall generate sales reports for daily, weekly, monthly, yearly, and custom date ranges (total revenue, items sold, top/least selling products, sales by payment mode, impact of discounts).
* **FR5.2:** It shall generate purchase reports for daily, weekly, monthly, yearly, and custom date ranges (total purchase value, items purchased, purchases by supplier/brand/category).
* **FR5.3:** It shall generate inventory reports including current stock levels, low stock, expired/junk stock, stock movement, and stock valuation reports.
* **FR5.4:** All reports shall be downloadable/printable in PDF and Excel in a fixed format.

#### 3.1.6. Basic Accounting & Expense Tracking Module
* **FR6.1:** It shall record all monetary transactions (payments and receipts), including date, amount, method, source, and category.
* **FR6.2:** It shall track payments such as stock purchases, investments, maintenance/wages, and other expenses.
* **FR6.3:** It shall track payments received from credit bills (date, amount, customer, bill number).
* **FR6.4:** It shall provide a dashboard displaying cash balance, bank balance, payments receivable (from customers), and payments pending (to suppliers).

### 3.2. Non-Functional Requirements

* **FRN1.1 - Performance:**
    * Critical operations like login, billing, and stock updates shall complete in less than 2 seconds.
    * Report generation shall not take more than 5 seconds (with exceptions for very large datasets).
* **FRN1.2 - Scalability:**
    * The system shall efficiently handle 30 concurrent users with minimal performance degradation.
    * The system shall support a minimum of 5000 SKUs and grow up to 20,000 SKUs in the future.
    * The database shall efficiently manage over 50,000 sales transactions.
* **FRN1.3 - Security:**
    * User passwords shall be hashed using bcrypt or a similar algorithm at minimum.
    * Sensitive data shall be encrypted (if required) and stored securely.
    * Role-Based Access Control (RBAC) shall be effectively enforced.
    * Login attempts shall be monitored, and accounts shall be locked after failed attempts.
    * Security measures against common web vulnerabilities like SQL injection and XSS (Cross-Site Scripting) shall be implemented.
* **FRN1.4 - Availability:**
    * The system shall be available 99.5% of the time (excluding scheduled maintenance windows).
    * Automated monitoring and alerting systems shall be implemented to minimize unexpected downtime.
* **FRN1.5 - Usability:**
    * The user interface shall be intuitive, consistent, and easy to understand, requiring minimal training.
    * Error messages shall be clear and corrective.
    * Navigation shall be logical and efficient.
* **FRN1.6 - Reliability:**
    * The system shall ensure data integrity and operate without data loss.
    * Transactions (sales, stock updates) shall be atomic and consistent.
    * Errors shall be handled gracefully, and proper logging shall be provided.
* **FRN1.7 - Maintainability:**
    * The codebase shall be modular, readable, and well-documented.
    * New features shall be easily added, and bug fixes efficiently implemented.
    * System configurations shall be easily changeable.
* **FRN1.8 - Backup and Recovery:**
    * A regular database backup mechanism shall be in place to prevent data loss.
    * There shall be a clear process for recovering data in case of disaster.

### 3.3. External Interface Requirements

* **FRX1.1 - User Interface:**
    * Shall use a web-based interface (React), compatible with modern browsers.
    * Shall have a responsive design that adapts to various screen sizes (desktop, laptop).
    * All text shall be displayed by default in English. The System Admin (SA) will have the option to change the default mode to native languages (e.g., Hindi) during configuration or at a later stage.
* **FRX1.2 - Hardware Interface:**
    * Computers/Laptops (client devices capable of running standard web browsers).
    * Printers (for receipt/bill printing, network or USB).
    * Barcode Scanners/QR Scanners: (Not currently available, but the system will be made compatible for future integration. Initial testing and operation will be through manual input.).
* **FRX1.3 - Software Interface:**
    * Operating System: Linux, Windows, or macOS (compatible for both server and client).
    * Node.js runtime environment (for the backend).
    * MongoDB database server.
    * Express.js framework (for backend APIs).
    * React library (for the frontend user interface).
    * File Storage: Files will be stored using a local file system (`/uploads/`) in the initial phase. In the future, cloud storage buckets like AWS S3 will be utilized.
* **FRX1.4 - Communications Interface:**
    * Communication between the client and server shall occur over HTTP/HTTPS protocols.
    * Access via local network or the internet.
    * JSON (JavaScript Object Notation) shall be used for data exchange.

### 3.4. Database Requirements
* **FRD1.1:** The database model will utilize MongoDB's collections and document-oriented structure.
* **FRD1.2:** The following main collections are defined (refer to the detailed database design document for detailed schema): users, products, packing Units, customers, suppliers, brands, companies, sales, purchaseOrders, goodsReceipts, supplierInvoices, financial Transactions, stockAdjustments.
* **FRD1.3:** Relationships in the database will primarily be established through referencing (by ID) and in some cases through de-normalization/embedding for performance.
* **FRD1.4:** Indexing will be applied on necessary fields to optimize query performance.
* **FRD1.5:** The database shall ensure data integrity (e.g., for unique fields).

## 4. Appendix

### 4.1. References
* Requirement Analysis Document (RAD) - (Previous discussions)
* Detailed Database Design Document - (Previous discussions)