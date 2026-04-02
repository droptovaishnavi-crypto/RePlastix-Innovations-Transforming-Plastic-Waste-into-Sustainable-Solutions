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
