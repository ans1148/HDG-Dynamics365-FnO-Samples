# Microsoft Dynamics 365 Finance & Operations - HDG Samples

Welcome to the **HDG-Dynamics365-FnO-Samples** repository. This repository provides robust, production-ready X++ sample models designed for Microsoft Dynamics 365 Finance and Operations (D365 F&O). Instead of generic tutorials, this project focuses on isolated, model-driven architectural patterns that solve real-world business requirements.

---

## 🛠️ Sample Models & Business Scenarios

Below is a detailed breakdown of each sample model contained within this repository, highlighting their primary functions, target business scenarios, and key technical components.

## 1. HDG_EmailSSRSReport
* **Description:** A dedicated sample model demonstrating how to programmatically generate and distribute SQL Server Reporting Services (SSRS) reports via email.
* **Business Scenario:** Automating the end-of-month statement distribution or emailing specific pro-forma invoices directly to customers without manual user intervention in the UI.
* **Key Technical Stacks:**
    * `SRSReportRunController` / `SrsReportRunService`: Controlling report execution and print destinations.
    * `SRSPrintDestinationSettings`: Configuring email parameters (To, CC, Subject, Body) programmatically.
    * `SysEmailTable` / `SysOutgoingEmailTable`: Managing email templates and tracking outgoing mail queues.

## 2. HDG_CustomServiceApi
* **Description:** A synchronous integration model showcasing the creation of custom REST/JSON endpoints within D365 F&O.
* **Business Scenario:** Real-time data validation or instant transaction processing initiated by third-party systems (e.g., e-commerce platforms, external legacy MES systems).
* **Key Technical Stacks:**
    * `[DataContractAttribute]` & `[DataMemberAttribute]`: Defining structured request and response payloads.
    * `[SysEntryPointAttribute]`: Exposing service methods securely to the AIF (Application Integration Framework) endpoint.
    * **X++ Service Groups:** Grouping services to automatically expose them as JSON/REST endpoints.

## 3. HDG_SysOperationBatch
* **Description:** An asynchronous, high-performance background processing sample model using the modern `SysOperation` framework.
* **Business Scenario:** Processing thousands of daily sales orders, re-calculating complex inventory closed states, or executing heavy periodic cleanup tasks without freezing the user interface.
* **Key Technical Stacks:**
    * **Controller (`SysOperationServiceController`):** Manages the execution context, UI dialogs, and routing.
    * **Data Contract:** Holds user-defined parameters and handles serialization across tiers.
    * **Service Class:** Houses the core business logic executed safely within batch threads.
    * **UI Builder (`SysOperationAutomaticUIBuilder`):** Customizes runtime parameters dialogs dynamically.

## 4. HDG_DataEntityIntegration
* **Description:** A bulk data management sample model focusing on asynchronous high-volume data import/export via the Data Import/Export Framework (DIXF).
* **Business Scenario:** Nightly synchronization of master data (e.g., Customers, Vendors, Released Products) from an enterprise-wide MDM or Data Lake system.
* **Key Technical Stacks:**
    * `AxDataEntity`: Custom data entities combining multiple normalized tables into a single denormalized view.
    * **Staging Tables (`AxTable` with Staging category):** Temporary storage for schema validation, mapping, and error logging.
    * **Entity Methods (`insertEntityDataSource`, `mapEntityToDataSource`):** Overriding standard behavior for advanced transformation logic during import.

## 5. HDG_ChainOfCommand
* **Description:** An extensibility-focused sample model displaying how to intercept and augment standard D365 F&O business logic without modifying the base application.
* **Business Scenario:** Injecting custom credit-limit checks right before a sales order is confirmed, or adding custom logging when a vendor record is modified.
* **Key Technical Stacks:**
    * `[ExtensionOf(...)]` Attribute: Declaring class, table, or form extensions.
    * `next` Keyword: Essential for maintaining the chain, ensuring standard code executes before or after custom logic.
    * **Event Subscribers (`[DataEventHandler(...)]`, `[FormEventHandler(...)]`):** Subscribing to framework-level structural triggers (e.g., `onValidatedRecord`).

## 6. HDG_BusinessEvents
* **Description:** An event-driven architecture sample model demonstrating how to publish real-time notifications to external message brokers.
* **Business Scenario:** Alerting an external warehousing system (WMS) via Azure Service Bus or Webhooks the exact moment a purchase order is officially approved.
* **Key Technical Stacks:**
    * `BusinessEventsBase` & `BusinessEventsContract`: Base classes for defining the structure and payload of the event.
    * **Business Event Catalog:** Registering and exposing custom events to the D365 F&O framework so administrators can link them to endpoints like HTTPS Webhooks, Azure Event Grid, or Power Automate.

---

## 📁 Folder Directory Structure

The metadata repository follows the native Visual Studio package layout required for automated build pipelines:
```text
📁 HDG-Dynamics365-FnO-Samples
 ├── 📁 HDG_EmailSSRSReport
 ├── 📁 HDG_CustomServiceApi
 ├── 📁 HDG_SysOperationBatch
 ├── 📁 HDG_DataEntityIntegration
 ├── 📁 HDG_ChainOfCommand
 └── 📁 HDG_BusinessEvents
      ├── 📁 Descriptor          # Model metadata and reference definitions
      └── 📁 XppSource            # Contains AxClass, AxTable, AxForm metadata XMLs

🚀 Deployment & Compilation Guide
Clone the Repository:

Bash
git clone [https://github.com/ans1148/HDG-Dynamics365-FnO-Samples.git](https://github.com/ans1148/HDG-Dynamics365-FnO-Samples.git)
Link to Packages Folder:
Copy or symlink the target model folders into your local Dev machine's package root directory (e.g., C:\AosService\PackagesLocalDirectory\).

Build Models via Visual Studio:
Open Visual Studio as Administrator, navigate to Dynamics 365 > Build models..., select the imported models, and trigger a full build with Database Synchronization enabled.