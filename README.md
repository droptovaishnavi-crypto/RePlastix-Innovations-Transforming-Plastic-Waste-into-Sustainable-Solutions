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

## 8️⃣ Data Security – Profiles

### 1️⃣ Create Custom Profile

1️⃣ Navigate to Setup  
- Click the gear icon (⚙️) → Click **Setup**  
- Search **Profiles** in Quick Find → Select **Profiles** → Click **New Profile**  

2️⃣ Fill Profile Details  
- **Existing Profile Name:** Standard Platform User  
- **User License:** Salesforce Platform  
- **Profile Name:** Platform 1  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd0GKmenr1pzFlUSAV327jR0AScDA6VHs1gF4Cscqhgjzev_S4T4TFnsCwd0CJeOw65O_gRGWjGmTJ0oKuaMs8-HJV8nbQpJvEfjWvwjf2sxZZRENx1cuiUv-4jcQEkrX4BwXql?key=W7stvKjAlqPw6HHAEQGheTEM)  

3️⃣ Click **Save**  

---

### 2️⃣ Create More Profiles
- **Platform 2** → Follow same steps as above  
- **Platform 3** → Follow same steps as above  

---

### 3️⃣ Modify Platform 1
1️⃣ Click **Edit** on Platform 1 Profile  
2️⃣ Add Access for Platform 1 Profile  

**Object Level Access:**  
- **Re Plastic Innovations Plastic Waste:** Read / Create  
- **Re Plastic Innovations Restock Request:** Read-Only  

3️⃣ Click **Save**  

---

### 4️⃣ Modify Profiles – Platform 2 & Platform 3

**Platform 2 Profile**  
- **Read/Create:** Re Plastic Innovations Order, Account  
- **Read-Only:** Re Plastic Innovations Plastic Waste, Re Plastic Innovations Recycled Product  

**Platform 3 Profile**  
- **Read/Create/Edit:** All Custom Objects  

---

### Tips
- Profiles control **object-level access, field-level access, and user permissions**.  
- Always review object access after creating or modifying profiles to maintain proper **data security**.  
- Platform 3 profile has full access to test and manage all custom objects.

Demo link : https://drive.google.com/file/d/1IyZ7_89EY2pryDuYmbTJnKD-98zCBWvS/view?usp=drive_link , https://drive.google.com/file/d/1x3RQpgZNHc1qaf0YE9qjDui1QdvJCm7M/view?usp=drive_link, https://drive.google.com/file/d/1-Sdp5F1D1qlO-a98MyKWSfAZY_pAYxXD/view?usp=drive_link, https://drive.google.com/file/d/1E_ks2vyMs3FAme7YNutEbD-frS9cW5-M/view?usp=drive_link

## 9️⃣ Data Security – Users

### 1️⃣ User – John Production Engineer

### User Creation
1️⃣ Navigate to Setup  
- Click the gear icon (⚙️) → Click **Setup**  
- Search **Users** in Quick Find → Select **Users** → Click **New User**  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd0GKmenr1pzFlUSAV327jR0AScDA6VHs1gF4Cscqhgjzev_S4T4TFnsCwd0CJeOw65O_gRGWjGmTJ0oKuaMs8-HJV8nbQpJvEfjWvwjf2sxZZRENx1cuiUv-4jcQEkrX4BwXql?key=W7stvKjAlqPw6HHAEQGheTEM)

2️⃣ Fill User Information  
- **First Name:** John Production Engineer  
- **Last Name:** Sandbox 1  
- **Alias:** Auto-populate  
- **Email:** Personal Email ID  
- **Username:** JohnProductionEngineer@sandbox1.com  
- **Role:** Recycling Manager  
- **Profile:** Platform 1  
- **License:** Salesforce Platform  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd8g36RkJOL_floqLaoP940EfH7boGXxGbjOOT44YdflLfQD_5P1kOAvddVmlXZ4Iga9hdr6CiNI-UbMnpS8Qzc5Z1RSpbVrKHhH3W8RRWNqEzqHIj0r9sHsn9nTGezz3UIlzSO6A?key=W7stvKjAlqPw6HHAEQGheTEM)

3️⃣ Click **Save**  
4️⃣ Check Gmail for Reset Password email  
5️⃣ Click **Verify Account**  
6️⃣ Click **Reset Password**  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe-upCb6yI9nLGTnr4xQBelGeKmm8YwR81IdTJR6OEHPzr35q0vPIi87I1gWVq3_xZwq3Q9p4QlfGhSKWFmkTIkI6qq5j4Pg2Qcwd1uRLLGTojlvE9eLwpwcIP44uE8b9HWFzuldA?key=W7stvKjAlqPw6HHAEQGheTEM)

7️⃣ Set Password and Click **Change Password**  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfP8c13Vth_tvtUTUN-yzenLr8X7V3lbVdP2tx7fj7FeYYWkTjjcTqNUID9dinaIk1v_AV5DNd52NOt23eh9Yo2ZnqUJKkv4P_02LcdYbgNQ2pm9noNjhTqF0oD9q8UmYvj_K4N?key=W7stvKjAlqPw6HHAEQGheTEM)

---

### 2️⃣ User – Quality Inspector

- **First Name:** Quality Inspector  
- **Last Name:** Mike  
- **Email:** Personal Email ID  
- **Username:** qualityinspector@company.com  
- **Role:** Sales Representative  
- **Profile:** Platform 2  

> Follow the same steps as User 1 to save and reset the password.

---

### 3️⃣ User – Plant Manager

- **First Name:** Plant Manager  
- **Last Name:** Albert  
- **Email:** Personal Email ID  
- **Username:** plantmanager@company.com  
- **Role:** Warehouse Supervisor  
- **Profile:** Platform 3  

> Follow the same steps as User 1 to save and reset the password.

---

### Tips
- Ensure each user has **assigned role and profile** corresponding to their responsibilities.  
- Reset passwords immediately to allow users to log in.  
- Check email for verification before first login.

Demo link : https://drive.google.com/file/d/1HgWIgFGi761DnPPjYPQPDYoHvHge5wNZ/view?usp=drive_link

## 10️⃣ Automation – Flow Builder

### 1️⃣ Flow Builder – Stock Level Alert

**Story Duration:** 1h 59m  
**Assigned to:** R. Vaishnavi  

### Steps to Create Flow

1️⃣ Navigate to Setup  
- Click the gear icon (⚙️) → Select **Setup**  

2️⃣ Open Flows  
- In the Quick Find box, type **Flows** → Click **Flows** → Click **New Flow**  

3️⃣ Configure Flow  
- Select **Start From Scratch** → Click **Next**  
- Select **Scheduled-Triggered Flow** → Click **Create**  

4️⃣ Set Schedule  
- **Date:** Today’s date  
- **Time:** 6:00 AM  
- **Frequency:** Daily  

5️⃣ Select Object  
- Choose **Re_Plastic_Innovations_Recycled_Product__c**  

6️⃣ Add Decision Element  
- Click **+ (Plus Icon)** → Select **Decision**  
- **Label Name:** Decision 1  
- **Outcome Name:** Outcome 1 of Decision 1  
- **Condition:** `{!$Record.Stock_Level__c} < {!$Record.Threshold__c}`  

7️⃣ Add Create Record Element  
- Click **+ (Plus Icon)** → Search for **Create Records**  
- **Label:** Create Task  
- **How to Set Field Values:** Manually  
- **Object:** Task  

8️⃣ Set Task Field Values  
- **Assigned To ID:** `{!$Record.OwnerId}`  
- **Related To ID:** `{!$Record.Id}`  
- **Priority:** High  
- **Subject:** Please Look, Stock Is Low – Fill the Stock ASAP  
- **Status:** In Progress  

9️⃣ Save Flow  
- **Label Name:** Stock Level Is Low → Click **Save**  

🔟 Activate Flow  
- Click **Activate**  

### Testing the Flow
- Set a test time → Create a record → Verify task creation  
- Check **Upcoming & Overdue** tasks to confirm the flow works  

---

### Tips
- Use scheduled flows to automate **daily stock monitoring**.  
- Ensure **Threshold__c** and **Stock_Level__c** are correctly set to trigger alerts.  
- Always test flows in **sandbox** before activating in production.

Demo link : https://drive.google.com/file/d/1pGh93f-1OtfL9IvWZ_UDHkwJpJ_QvhCK/view?usp=drive_link

## 11️⃣ Data Configuration – Formula Field

### 11.1 Formula Field – Stock Low On Product

1️⃣ **Select Object**  
- From Setup → Quick Find box → Select **Object**  
- Object Name: **Re Plastic Innovations Recycled Product**  

2️⃣ **Create New Formula Field**  
- Click **Fields & Relationships** → Click **New**  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXejB-N3cRICsyY_tHJc8ZPXU_5OH0lfbskTBNpB_CW2ZSA5v5AvnWkAv__JYvyDy32iisz3B2TNcVnIud2o_pado_7IsoGAIkyBpBAgTcOPd7U8-UHDluoOSAu_lvoHntu13W_NwA?key=W7stvKjAlqPw6HHAEQGheTEM)

3️⃣ **Select Data Type**  
- Choose **Formula** → Click **Next**  
- **Label Name:** Stock Low On Product  
- **Data Type:** Text → Click **Next**  

4️⃣ **Write Formula Logic**  
- Click **Advanced Formula**  
- Formula Logic:  


5️⃣ **Test the Logic**  
- Create a record to verify the formula  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcZKBMmzg-VjZP1gkdhl8cKSJXfddYxTQ_orn8EXBJ07A6HdsM4ggc3S7HDFPjco8-G1tkoafSfn370vkqZuIye9Oognpb2lIagXALOJTQLvhcF-ggY0cgpIxwQ5h7yxqWEJucx?key=W7stvKjAlqPw6HHAEQGheTEM)

6️⃣ **Change Stock Value**  
- Modify **Stock_Level__c** to test different outcomes  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfs_AjTKByFgyiIBWGBPgve-w3fpIc5Q0uctq08XUo96Zm9meNMhxcIQWpdx2Zxlul-Tsvw59HE7U5hbaWSBRAkm-w-FwuQYnDQKUXOyW50zhhi8Y7l5EJfgA_8dBlVT1hjnQOM4Q?key=W7stvKjAlqPw6HHAEQGheTEM)

---

### Tips
- Formula fields **automatically calculate values** based on other field data.  
- Use formula fields for **real-time stock monitoring** and alerts.  
- Always test formula with multiple records to ensure accuracy

 Demo link : https://drive.google.com/file/d/17vDJGcvGsMDaYx_iu2N7-S0W1dFV7qgc/view?usp=drive_link

 ## 11️⃣ Data Configuration – Formula Field

### 11.1 Formula Field – Stock Low On Product

1️⃣ **Select Object**  
- From Setup → Quick Find box → Select **Object**  
- Object Name: **Re Plastic Innovations Recycled Product**  

2️⃣ **Create New Formula Field**  
- Click **Fields & Relationships** → Click **New**  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXejB-N3cRICsyY_tHJc8ZPXU_5OH0lfbskTBNpB_CW2ZSA5v5AvnWkAv__JYvyDy32iisz3B2TNcVnIud2o_pado_7IsoGAIkyBpBAgTcOPd7U8-UHDluoOSAu_lvoHntu13W_NwA?key=W7stvKjAlqPw6HHAEQGheTEM)

3️⃣ **Select Data Type**  
- Choose **Formula** → Click **Next**  
- **Label Name:** Stock Low On Product  
- **Data Type:** Text → Click **Next**  

4️⃣ **Write Formula Logic**  
- Click **Advanced Formula**  
- Formula Logic:  

5️⃣ **Test the Logic**  
- Create a record to verify the formula  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcZKBMmzg-VjZP1gkdhl8cKSJXfddYxTQ_orn8EXBJ07A6HdsM4ggc3S7HDFPjco8-G1tkoafSfn370vkqZuIye9Oognpb2lIagXALOJTQLvhcF-ggY0cgpIxwQ5h7yxqWEJucx?key=W7stvKjAlqPw6HHAEQGheTEM)

6️⃣ **Change Stock Value**  
- Modify **Stock_Level__c** to test different outcomes  

![BlockNote image](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfs_AjTKByFgyiIBWGBPgve-w3fpIc5Q0uctq08XUo96Zm9meNMhxcIQWpdx2Zxlul-Tsvw59HE7U5hbaWSBRAkm-w-FwuQYnDQKUXOyW50zhhi8Y7l5EJfgA_8dBlVT1hjnQOM4Q?key=W7stvKjAlqPw6HHAEQGheTEM)

---

### Tips
- Formula fields **automatically calculate values** based on other field data.  
- Use formula fields for **real-time stock monitoring** and alerts.  
- Always test formula with multiple records to ensure accuracy.

Demo link : https://drive.google.com/file/d/17vDJGcvGsMDaYx_iu2N7-S0W1dFV7qgc/view?usp=drive_link

## 12️⃣ Automation – Apex (Short)

1. Create Apex Class `InventoryManager` with methods `processOrderStock` (reduces stock & creates restock request) and `processRestockApproval` (increases stock after approval).  
2. Trigger `UpdateStockAfterOrder` on `Re_Plastic_Innovations_Order__c` → calls `processOrderStock` after insert.  
3. Trigger `UpdateStockAfterRestockApproval` on `Re_Plastic_Innovations_Restock_Request__c` → calls `processRestockApproval` & `EmailNotificationHelper.sendRestockNotification` after approval.  
4. Apex Class `EmailNotificationHelper` → sends email to warehouse manager when restock is approved.  
5. Test Class `InventoryManagerTest` → creates product & orders, tests order processing, restock approval, ensures stock updates correctly.  
6. Steps to test: create product with stock, create order exceeding stock, check restock request creation.  
7. Approve restock request → stock updates automatically.  
8. Email notification sent to manager.  
9. Run test class → ensure 100% coverage.  
10. All triggers and classes handle stock automation seamlessly.

Demo link : https://drive.google.com/file/d/1LXfUeNQ3zKGaCaC0WxDmxYQPf848bfyn/view?usp=drive_link

## 13️⃣ Data Security – Record Level Access

1. Go to **Sharing Settings** → Click **Edit** → Set OWD (Org-Wide Defaults) for Re_Plastic_Innovations objects → Save.  
2. **Create Sharing Rule: Recycling Manager Access**  
   - Object: Re_Plastic_Innovations_Plastic_Waste__c  
   - Shared From: CEO → To: Recycling Manager  
   - Access: Read Only → Save.  
3. **Create Sharing Rule: Sales Representative Access**  
   - Object: Re_Plastic_Innovations_Recycled_Product__c  
   - Shared From: CEO → To: Sales Representative  
   - Access: Read Only → Save.  
4. **Create Sharing Rule: Warehouse Supervisor Access**  
   - Object: Re_Plastic_Innovations_Restock_Request__c  
   - Shared From: Sales Representative → To: Warehouse Supervisor  
   - Access: Read Only → Save.  
5. Only specified users get record-level access per role; CEO controls sharing.

Demo link: https://drive.google.com/file/d/1xyR_k0L7-IigGoTiqzQ_41bY8VoUptB6/view?usp=drive_link, https://drive.google.com/file/d/1voC_CES_Igh5K-ziWGPxn6aat0l92fWl/view?usp=drive_link

## 14️⃣ Data Configuration Modules

1. **Formula Field – Stock Low On Product**  
   - Object: Re Plastic Innovations Recycled Product → Fields → New → Formula (Text)  
   - Formula: IF(Stock_Level__c < Threshold__c, "Low Stock - Restock Needed", "Sufficient Stock")  
   - Test by changing stock values.  

2. **Validation Rule – Check_Quantity_Not_Zero**  
   - Object: Re Plastic Innovations Order → Validation Rule → New  
   - Formula: Quantity__c <= 0  
   - Error: "Quantity must be greater than zero."  

3. **Validation Rule – Future_Date_Collection**  
   - Object: Re Plastic Innovations Plastic Waste  
   - Formula: Collection_Date__c > TODAY()  
   - Error: "Collection Date cannot be in the future."  

4. Ensures accurate data entry and prevents invalid records.

Demo link : https://drive.google.com/file/d/17vDJGcvGsMDaYx_iu2N7-S0W1dFV7qgc/view?usp=drive_link
