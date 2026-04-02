1) User Scenario: Automated Order & Replenishment Flows in Salesforce

This scenario demonstrates end-to-end automation in Salesforce for handling low-stock Orders, creating Replenishment Requests, and notifying the Warehouse Manager via email.

---

## Flow 1: Update Orders

**Purpose:** Automatically updates Orders to "Low Stock" and triggers creation of a Replenishment Request.

- **Flow Label:** Update Orders & Create Replenishment Request
- **Flow API Name:** Update_Orders_Create_Replenishment_Request
- **Description:** Updates Order Status to "Low Stock" and creates a corresponding Replenishment Request.

**Steps:**
1. **Create Flow:** Setup → Flows → New Flow → Record-Triggered Flow → Create
2. **Trigger:** Object = `Order`, Trigger = Created or Updated, Optimize = Actions and Related Records
3. **Decision Element:** Check if `Product.Stock__c < 10`
4. **Update Records:** Set `Order.Status = Low Stock`
5. **Create Records (Replenishment Request):**
   - Object = `Replenishment Request`
   - Fields:
     - Product = `{!$Record.Product__c}`
     - Status = `Pending`
6. **Save & Activate Flow**

---

## Flow 2: Replenishment Request Creation

**Purpose:** Automatically creates a Replenishment Request when an Order is low on stock.

- **Flow Label:** Replenishment Request Creation
- **Flow API Name:** Replenishment_Request_Creation
- **Description:** Creates a Replenishment Request record automatically from the low-stock Order.

> Note: This is implemented as part of **Flow 1’s Create Records step**.

---

## Flow 3: Email Notification

**Purpose:** Sends an email to Warehouse Manager when a Replenishment Request is approved.

- **Flow Label:** Replenishment Email Notification
- **Flow API Name:** Replenishment_Email_Notification
- **Description:** Sends automated email to Warehouse Manager, including product details.

**Steps:**

### Step 1: Create Flow
- Setup → Flows → New Flow → Record-Triggered Flow → Create

### Step 2: Configure Trigger
- Object = `Replenishment Request`
- Trigger = Updated
- Condition = `Status = Approved`
- Optimize = Actions and Related Records

### Step 3: Add Send Email Action
- Action Label = Notify Warehouse Manager
- Recipient Addresses = `$Record.Warehouse_Manager__r.Email`
- CC Addresses = `"supply.chain@company.com"` (optional)
- BCC Addresses = `"audit@company.com"` (optional)
- Sender Type = Organization-Wide Email Address
- Sender Email = `no-reply@company.com`
- Subject = `"Stock Approved for Replenishment"` (if not using template)
- Body = `"Stock approved for replenishment for product {!Replenishment_Request__c.Product__c}"`
- Email Template = `Stock_Replenishment_Approved` (optional)
- Related Record ID = `$Record.Id`

### Step 4: Save & Activate
- Flow Label = `Replenishment Email Notification`
- Save → Activate

### Step 5: Test the Flow
1. Open a Replenishment Request
2. Change Status → Approved
3. Save
4. Verify email is sent and appears linked in Activity History

---

Demo drive link - https://drive.google.com/drive/folders/1Tx90QyUmVoAE445nbG1UQb48TH6eQwMm?usp=drive_link



# Data Management: Objects

## 1️⃣ Object – Re Plastic Innovations Plastic Waste

Story Duration: 1h 0m

### Description
The purpose of creating the Re Plastic Innovations Plastic Waste object is to have a clear record of all plastic waste items handled by Re Plastic Innovations.

### Step-by-Step Instructions

1️⃣ Navigate to Setup
- Click the gear icon (⚙️) → Click Setup

2️⃣ Open Object Manager
- In the Setup page → Click Object Manager → Click Create → Click Custom Object

3️⃣ Enter Object Details

| Field              | Value                                         |
|-------------------|-----------------------------------------------|
| Label             | Re Plastic Innovations Plastic Waste          |
| Plural Label      | Re Plastic Innovations Plastic Wastes        |
| Object Name       | (auto-generated)                              |
| Record Name Label | Plastic Waste Name                             |
| Data Type         | Auto Number                                   |
| Display Format    | PW-{0001}                                     |

4️⃣ Enable Options
- Check Allow Reports
- Check Allow Search

5️⃣ Save Object
- Click Save

### Tips
- After saving, you can add custom fields for properties like waste type, weight, source, etc.
- You can also configure page layouts, validation rules, and relationships if needed.

Demo link : https://drive.google.com/file/d/1AVgsXH_dg-TzebjTEhmr7qW5KzpMH8wg/view?usp=drive_link

## 2️⃣ Object – Re Plastic Innovations Recycling Center

Story Duration: 1h 0m

### Description
The purpose of creating the **Re Plastic Innovations Recycling Center** object is to have detailed information about Dealers and Recycling Centers.

### Step-by-Step Instructions

1️⃣ Navigate to Setup
- Click the gear icon (⚙️) → Click **Setup**

2️⃣ Open Object Manager
- In the Setup page → Click **Object Manager** → Click **Create** → Click **Custom Object**

3️⃣ Enter Object Details

| Field              | Value                                           |
|-------------------|-------------------------------------------------|
| Label             | Re Plastic Innovations Recycling Center         |
| Plural Label      | Re Plastic Innovations Recycling Centers       |
| Object Name       | (auto-generated)                                |
| Record Name Label | Re Plastic Innovations Recycling Center        |
| Data Type         | Text                                            |

4️⃣ Enable Options
- Check **Allow Reports**  
- Check **Allow Search**

5️⃣ Save Object
- Click **Save**

### Tips
- After saving, you can add custom fields such as Dealer ID, Contact Info, Location, and Status.  
- You can also configure page layouts, validation rules, and relationships if needed.

Demo link : https://drive.google.com/file/d/1V8PJkMsnG_bH2IF0149IAy6YNLKcvBjH/view?usp=drive_link

## 3️⃣ Object – Re Plastic Innovations Recycled Product (Re_Plastic_Innovations_Recycled_Product__c)

Story Duration: 1h 0m

### Description
The purpose of this object is to track all **recycled products**, their stock levels, thresholds, and pricing.

### Step-by-Step Instructions

1️⃣ Navigate to Setup  
- Click the gear icon (⚙️) → Click **Setup**

2️⃣ Open Object Manager  
- In Setup → Click **Object Manager** → Click **Create** → Click **Custom Object**

3️⃣ Enter Object Details  

| Field              | Value                                |
|-------------------|--------------------------------------|
| Label             | Re Plastic Innovations Recycled Product |
| Plural Label      | Re Plastic Innovations Recycled Products |
| Object Name       | (auto-generated)                     |
| Record Name Label | Recycled Product Name                 |
| Data Type         | Text                                 |

4️⃣ Enable Options  
- Check **Allow Reports**  
- Check **Allow Search**

5️⃣ Save Object  
- Click **Save**

### Fields to Add

| Field API Name   | Data Type  | Description                                |
|-----------------|-----------|--------------------------------------------|
| Stock_Level__c   | Number    | Current stock available                     |
| Threshold__c     | Number    | Minimum stock before restock is triggered  |
| Price__c         | Currency  | Price per unit                              |

## 4️⃣ Object – Re Plastic Innovations Order (Re_Plastic_Innovations_Order__c)

Story Duration: 1h 0m

### Description
The purpose of this object is to track **orders of recycled products** placed by customers.

### Step-by-Step Instructions

1️⃣ Navigate to Setup  
- Click the gear icon (⚙️) → Click **Setup**

2️⃣ Open Object Manager  
- In Setup → Click **Object Manager** → Click **Create** → Click **Custom Object**

3️⃣ Enter Object Details  

| Field              | Value                                |
|-------------------|--------------------------------------|
| Label             | Re Plastic Innovations Order          |
| Plural Label      | Re Plastic Innovations Orders         |
| Object Name       | (auto-generated)                     |
| Record Name Label | Order ID                              |
| Data Type         | Auto Number                           |

4️⃣ Enable Options  
- Check **Allow Reports**  
- Check **Allow Search**

5️⃣ Save Object  
- Click **Save**

### Fields to Add

| Field API Name         | Data Type                     | Description                     |
|-----------------------|-------------------------------|---------------------------------|
| Customer__c           | Lookup (Account)              | Customer placing the order      |
| Recycled_Product__c   | Lookup (Recycled_Product__c)  | Ordered product                 |
| Quantity__c           | Number                        | Quantity ordered                |
| Delivery_Date__c      | Date                          | Expected delivery date          |

## 5️⃣ Object – Re Plastic Innovations Restock Request (Re_Plastic_Innovations_Restock_Request__c)

Story Duration: 1h 0m

### Description
The purpose of this object is to track **restock requests** for recycled products.

### Step-by-Step Instructions

1️⃣ Navigate to Setup  
- Click the gear icon (⚙️) → Click **Setup**

2️⃣ Open Object Manager  
- In Setup → Click **Object Manager** → Click **Create** → Click **Custom Object**

3️⃣ Enter Object Details  

| Field              | Value                                |
|-------------------|--------------------------------------|
| Label             | Re Plastic Innovations Restock Request |
| Plural Label      | Re Plastic Innovations Restock Requests |
| Object Name       | (auto-generated)                     |
| Record Name Label | Request ID                            |
| Data Type         | Auto Number                           |

4️⃣ Enable Options  
- Check **Allow Reports**  
- Check **Allow Search**

5️⃣ Save Object  
- Click **Save**

### Fields to Add

| Field API Name          | Data Type                     | Description                     |
|------------------------|-------------------------------|---------------------------------|
| Product__c             | Lookup (Recycled_Product__c)  | Product to restock              |
| Requested_Quantity__c  | Number                        | Quantity requested              |
| Status__c              | Picklist                       | ["Pending", "Approved", "Completed"] |

Demo link : https://drive.google.com/file/d/16bEV-Q4CUTLFCshCBfcgAguTvgBY6p2g/view?usp=drive_link

## 3️⃣ Data Management: Tabs

### 1️⃣ Tab – Re Plastic Innovations Recycling Center

### Description
The purpose of this tab is to provide easy access to the **Re Plastic Innovations Recycling Center** object from the Salesforce app interface.

### Step-by-Step Instructions

1️⃣ Navigate to Setup  
- Click the gear icon (⚙️) → Click **Setup**

2️⃣ Open Tabs  
- Type **Tabs** in the Quick Find bar → Click **Tabs** → Click **New** (under Custom Object Tabs)

3️⃣ Select Object  
- Choose **Re Plastic Innovations Recycling Center**  

4️⃣ Choose Tab Style  
- Select any style you prefer → Click **Next**

5️⃣ Add to Profiles  
- Keep default settings → Click **Next**

6️⃣ Add to Custom Apps  
- Keep default settings → Click **Next**

7️⃣ Save  
- Click **Save**

### Tips
- After saving, this tab will appear in the app menu, making it easy for users to view and manage Recycling Center records.  
- Repeat the process for other objects as needed.

Demo link : https://drive.google.com/file/d/1ON5OA-DLSWyFYN_qCawD7oxjQfr9qAZf/view?usp=drive_link

## 5️⃣ Data Management – App Manager

### 1️⃣ App – Re Plastic Innovations

Story Duration: 45m

### Description
The purpose of this Lightning App is to provide a centralized interface for managing **Plastic Wastes, Recycling Centers, Recycled Products, Orders, and Restock Requests** in Salesforce, improving workflow and reporting for Re Plastic Innovations.

### Step-by-Step Instructions

Navigate to Setup  
- Click the gear icon (⚙️) → Click **Setup**

2️⃣ Open App Manager  
- Search **App Manager** in the Quick Find bar → Click **App Manager** → Click **New Lightning App**

 Fill App Details & Branding  
- **App Name:** Re Plastic Innovations  
- **Developer Name:** Auto-populated by Salesforce  
- **Description:**  
  > “A Salesforce app to manage plastic waste collection, recycling centers, recycled products, orders, and restock requests efficiently, ensuring streamlined operations and reporting for Re Plastic Innovations.”  
- **Image:** Optional (e.g., green recycling icon)  
- **Primary Color (Hex):** Keep default  

 Click **Next** → App Options page: Keep default → **Next** → Utility Items: Keep default → **Next**

Add Navigation Items  
- Search and add these items using the arrow button:  
  - **Restock Requests**  
  - **Recycling Centers**  
  - **Recycled Products**  
  - **Plastic Wastes**  
  - **Orders**  
- **Note:** Select the custom objects you created in previous steps.  
- Click **Next**

 Assign User Profiles  
- Search profiles (e.g., **System Administrator**) → Click the arrow to add → Click **Save & Finish**

### Tips
- After creation, the app will appear in the Salesforce app menu.  
- Users with assigned profiles can access all tabs and objects within the app.

Demo link : https://drive.google.com/file/d/1ht6FrcqUt9UYFEsK_feelUOOe94KM9pz/view?usp=drive_link

## Data Management – Fields

### 1️⃣ Field – Weight (Weight__c)

### Description
The **Weight** field stores the weight of plastic waste collected in kilograms. This field is essential for tracking the volume of waste handled, calculating processing capacity at recycling centers, and generating reports on collected plastic.  

### Field Details
- **API Name:** Weight__c  
- **Data Type:** Number (18,2)  
- **Object:** Re Plastic Innovations Plastic Waste  
- **Description:** Weight of plastic waste in kilograms  

### Tips
- Use this field to filter, sort, and report on heavy vs. light waste items.  
- Can be used in formulas to calculate total collected weight per center or per day.  
- Ensure data entry is consistent (e.g., always in kg) to avoid errors in reporting.

Demo link : https://drive.google.com/file/d/1Fc3IFs94ZYKMKDaX-VqISjJwuoKZYFcz/view?usp=drive_link


### 2️⃣ Field – Type (Type__c)

Story Duration: 10m

### Description
The **Type** field captures the category of plastic for each waste record. This helps in identifying the type of plastic (e.g., PET, HDPE, PVC) for proper recycling, segregation, and reporting purposes.  

### Field Details
- **API Name:** Type__c  
- **Data Type:** Picklist  
- **Object:** Re Plastic Innovations Plastic Waste  
- **Description:** Select the type of plastic waste from predefined options such as PET, HDPE, PVC, etc.  

### Tips
- Use this field to generate reports by plastic type for analysis and planning.  
- Ensure picklist values are consistent with recycling standards.  
- Can be used in dashboards to show the proportion of each plastic type collected.

Demo link : https://drive.google.com/file/d/1bQPvz_M0FOmOfBEmUxfts7ymMBFQY_P6/view?usp=drive_link


### 3️⃣ Field – Re Plastic Innovations Recycling Center (Recycling_Center__c)

Story Duration: 10m

### Description
The **Recycling Center** field links each plastic waste record to a specific recycling center. This allows tracking where the waste is being sent for processing and enables reporting on waste distribution across centers.  

### Field Details
- **API Name:** Recycling_Center__c  
- **Data Type:** Lookup (Recycling Center__c)  
- **Object:** Re Plastic Innovations Plastic Waste  
- **Description:** Assigns the plastic waste record to a specific recycling center  

### Tips
- Use this field to filter and generate reports by recycling center.  
- Helps in monitoring workload, capacity, and efficiency of each recycling center.  
- Ensure the lookup points to valid and active recycling center records to avoid data inconsistence

Demo link : https://drive.google.com/file/d/1rhhsNwir6h5Y47s6n1lw0RWFuunI9uvd/view?usp=drive_link

## Data Security – Roles

### 1️⃣ Role – Recycling Manager

### Creation Roles
From the setup menu, search and select **Roles**.  

1️⃣ Click **Add Roles** → Select **CEO Below** option  

2️⃣ Fill Role Details  
- **Label:** Recycling Manager  
- **This Role Reports To:** CEO  

3️⃣ Click **Save and New**  

### 2️⃣ Role – Sales Representative
- Repeat the steps to create this role  
- **Reports To:** CEO  
- Click **Save and New**  

### 3️⃣ Role – Warehouse Supervisor
- Repeat the steps to create this role  
- **Reports To:** Sales Representative  
- Click **Save**  

### Tips
- Each role should have a clear reporting hierarchy to maintain proper **data security and access control**.  
- Roles help manage **object access, visibility, and permissions** in Salesforce.  
- Ensure no role is missing a reporting manager to avoid access conflicts.

Demo link : https://drive.google.com/file/d/1J0p6vHJHfnUurOVneoAnqMKwc3Pnbla7/view?usp=drive_link , https://drive.google.com/file/d/151zajwS8c7BBGGXF_Gejj5AVg944bKCq/view?usp=drive_link

