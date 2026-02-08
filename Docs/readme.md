# New Shipping Board & Blindshipping Guide

With 2025 around the corner, it felt like a good time to do an upgrade of the shipping board, in its functionality, seamlessness and its automation capabilities. The new shipping board is still going to be based on [Monday.com](http://Monday.com) , but with more functionality built in, and better automation to make shipping a more seamless process through utilizing the API’s for Netparcel and Quickbooks, connected via Make.com

**This new Shipping board automates the following processes without having to leave Monday.com**

1. pulling in shipping rates from Netparcel based off specs. 
2. Booking shipping labels through Netparcel by selecting an option through a dashboard in Monday.com, as well as the pickup. 
3. Invoicing and applying the markup of 20% to the client
4. Checking to see if the invoice for the client for their order has funds outstanding that need to be accounted for, as well as if the shipping invoice has been paid.
5. Sending tracking to the clients
6. Printing the labels for each box upon trigger of label generation

All of the steps above were done manually prior, and often with error or inconsistency, these automations will turn the entire shipping process into simply making sure fields are mapped, triggering an automation via a status update and indicating which shipping speed to proceed with, turning this into a super efficient process:

## **Overall Workflow of New Shipping Board:**

![Screenshot 2024-12-26 at 2.02.31 PM.png](New%20Shipping%20Board%20&%20Blindshipping%20Guide/Screenshot_2024-12-26_at_2.02.31_PM.png)

### **Key Adjustments and Input Requirements**:

[https://www.loom.com/share/65aa798b56044d50bde495560f9f5fb7](https://www.loom.com/share/65aa798b56044d50bde495560f9f5fb7)

### **Field Adjustments**:

- **Separate Address Fields**:
    - Split address into separate fields (e.g., street, unit number).
    - Enter "CA" for Canada in the country field.
- **Box Count**:
    - Specify the exact number of boxes per order.

### **Sub-item Details**:

- Enter for each box:
    - Weight (pounds)
    - Length
    - Height
    - Width

### **Automation Dependencies**:

- Missing data in fields (e.g., address, dimensions, box count) will break automation.
- Follow strict formatting rules.

---

### **Automation Dependencies**:

- Failure to input fields such as dimensions, separated address fields, and box counts correctly will disrupt the automation.
- While future updates may eliminate some constraints, they are currently non-negotiable.

 

## First Phase: Automation for Gathering/Quotes Shipping & Inserting Specs:

![Screenshot 2024-12-26 at 5.50.55 PM.png](New%20Shipping%20Board%20&%20Blindshipping%20Guide/Screenshot_2024-12-26_at_5.50.55_PM.png)

One of the issues that we could get away with in the past was  entering shipping details without proper formatting. This is passable for our size, but is inefficient and not something that will work at scale. Ideally, everything would be properly formatted to the fields of our systems, and since we are already entering the information, I wanted to make the labor of simply entering it into the proper place payoff as much as I thought possible. 

### **Please watch this video with the text below:**

[https://www.loom.com/share/5c992fdc53ca474f8a0f61a1409b3b27](https://www.loom.com/share/5c992fdc53ca474f8a0f61a1409b3b27)

## Documentation: Automation for Gathering Shipping Quotes - Part 1

---

### **Overview**

This document outlines the first phase of an automation system designed to streamline the process of gathering shipping quotes for orders. This solution addresses inefficiencies in the current shipping workflow by automating data organization and API integration, ultimately saving time and reducing manual effort.

---

---

---

### **Phase 1 Workflow: Automated Quote Trigger**

1. **Data Setup**:
    - Recipient and shipping details are entered into the system.
    - Number of boxes is automatically transferred from the production queue.
    - Sub-items are created for each box, allowing individual entries for weight and dimensions.
2. **Quote Trigger**:
    - When all sub-items have the required fields populated, the "Ready for Quotes" status is triggered.
3. **Automation Process**:
    - The automation connects to the NetParcel API to retrieve shipping quotes.
    - Required fields for API integration include:
        - Recipient details
        - Weight and dimensions for each box
4. **Output**:
    - The system generates a Google Sheet (or equivalent document) for each order, populated with shipping quotes. This includes:
        - Delivery dates
        - Prices
        - Service codes
    - Each sheet is tied to the corresponding order ID for easy tracking.

---

### **Technical Details**

- **Platforms used:**
    - Google Sheets: [Link To Template Here](https://docs.google.com/spreadsheets/d/1TioE4uiKUDVx6EGcaXM55vJfG6U-wWV5hAHLFfZEn2s/edit?usp=sharing)
    - [Monday.com](http://Monday.com)
    - [Make.com](http://Make.com) Automation Link: [https://us1.make.com/3645/scenarios/3480908/edit](https://us1.make.com/3645/scenarios/3480908/edit)
- **API Integration**: NetParcel API
    
    [Netparcel API Documentation](New%20Shipping%20Board%20&%20Blindshipping%20Guide/Netparcel%20API%20Documentation%201696e9dd11ca80dcba97fe7cf319dd57.md)
    
    - The API is essential for retrieving real-time shipping quotes.
    - API documentation is available but not publicly accessible. Copies are provided for team use.
- **Sheet Links**:
    - Each order has a dedicated sheet created from a template, ensuring organized and clear presentation of shipping quotes.

---

### **User Guide**

1. **Data Entry Requirements**:
    - All dimension fields & address fields must be filled out, as well as the number of boxes, which depending on how many listed, creates the appropriate amount of sub items.
2. **Trigger Automation**:
    - Once all required fields filled, the status of “Specs Confirmed” in [monday.com](http://monday.com) changes to "Ready for Quotes." This triggers the automation to retrieve quotes.
3. **Review Quotes**:
    - Upon Completion Open the generated sheet link to view all shipping options, including delivery dates, prices, and service codes.

**Once you have your quotes ready to review, you can pick based on your requirements for deadline, shipper reliability and cost. This moves to Phase 2** 

---

---

## PART 2 Documentation: Automation for Booking, Printing, Billing, Sending, AKA: The Rest

---

[https://www.notion.so](https://www.notion.so)

[https://www.loom.com/share/1c9d975c29d34a8797dd93cd645baefa](https://www.loom.com/share/1c9d975c29d34a8797dd93cd645baefa)

### Shipping Label Automation Documentation

This automation is designed to streamline the shipping process by generating shipment labels, printing them, creating invoices with a 20% markup, and sending tracking information to customers. The process is simple to trigger and happens automatically, ensuring efficiency and reducing manual errors.

---

### 1. **Overview of Automation Workflow**

- The automation handles the following:
    - **Generates a Shipping Label**
    - **Prints the Shipping Label**
    - **Triggers the creation of an invoice with a 20% markup**
    - **Sends tracking information to the customer**

---

### 2. **How to Trigger the Automation**

1. **Open the Shipment Sheet:** The sheet that is uploaded will be used to generate the shipping label.
2. **Choose a Shipper:** Select a shipper from the available options. For example, **ICS Next Day** is selected by marking it with an "X."
3. Change the “Label Booked” Status to “Book Label”
4. **Statuses to Monitor:**
    - **Failed:** If the automation fails, the reason will be displayed, often due to missing or incorrect data (check the comments on the Monday.com card).
    - **Awaiting Review:** This status appears after the shipment specifications have been entered.
    - **Booking:** This status indicates the label is in progress.
    - **Book Label:** To trigger the label generation, select this option.
    - **Done:** This status shows once the label and other tasks have been completed.

---

### 3. **The Label Generation Process**

- **Triggering the Label:** Once you click **Book Label**, the automation begins and runs in real-time without any manual intervention.
- **Processing Flow:**
    - The automation will grab all the details for the selected item.
    - It matches the details with the chosen service from the uploaded spreadsheet.
    - The system runs through an API call to **NetParcel** to book the shipping label, with added logic to determine if the booking is for afternoon delivery, in which case a pickup for the next day is arranged.
- **Time of Day:** The ideal times to run this automation are at the start or end of the day for optimal results.

---

### 4. **What Happens After Triggering**

- **Shipping Invoice:** The system automatically generates the invoice, including the 20% markup on shipping costs.
- **Label Creation:** A PDF shipping label is created and printed at the shop.
- **Tracking Information:** The automation sends the tracking number to the customer via email.
- **Label Access:** The shipping labels are stored and can be accessed directly. The label will look like a valid shipping label from the carrier and will be ready for use.

---

### 5. **Example of the Process**

- When the **Book Label** button is clicked, the automation starts immediately. In just a few seconds, the system processes the order, books the label, generates the invoice, and sends the tracking details to the customer.
- If you need to confirm the status, you can view the generated labels and invoices on the Monday.com board.

---

### 6. **Summary**

This automation simplifies the shipping process by managing the creation of labels, invoices, and tracking information with minimal manual effort. It's efficient, fast, and ensures that your customers receive timely shipping updates without unnecessary delays. For any issues, review the status updates, especially when something fails or requires manual intervention.

### PART 3: Financial and Fulfillment Loop

---

![Screenshot 2024-12-26 at 10.53.10 PM.png](New%20Shipping%20Board%20&%20Blindshipping%20Guide/Screenshot_2024-12-26_at_10.53.10_PM.png)

### **VIDEO:**

[https://www.loom.com/share/5106b2d30443416ea8e0f315baae978b](https://www.loom.com/share/5106b2d30443416ea8e0f315baae978b)

### **Overview**

This SOP details the design of the automated system and logic to manage financial and fulfillment processes. The system addresses inefficiencies in tracking order payments and shipping statuses by automating routine checks, updates, and notifications.

---

### **System Overview**

The automation system integrates **Monday.com**, **QuickBooks**, and **Make.com** to monitor and update order statuses. It ensures that orders are only moved to their appropriate statuses (e.g., "Completed") after financial and fulfillment requirements are met.

---

### **Workflow Components**

### **1. Automation Triggers**

- The system runs automated checks every **two hours** during the business day (e.g., 10 AM, 12 PM, 2 PM, and 4 PM).
- Triggers are based on order statuses and missing information, such as:
    - **Shipping unpaid**
    - **Balance and shipping unpaid**
    - **Balance owing**
    - Missing "Bills Paid" status

---

### **2. Key Automation Processes**

### **2.1. Financial Status Verification**

1. **Identify Pending Payments**:
    - The system scans orders in Monday.com with incomplete financial statuses.
    - Orders are identified by their unique **Pulse ID**.
2. **Retrieve Invoice Data**:
    - For each flagged order, the system retrieves the associated shipping invoice number.
    - A search step is performed in **QuickBooks** to verify payment status.
3. **Update Financial Status**:
    - If the invoice is marked as paid in QuickBooks:
        - Update the order status in Monday.com to **Bills Paid**.
        - If the order has been picked up, move it to the **Completed** board.
        - If the order is at the shop, update it to **Free to Release**.
    - If the invoice remains unpaid:
        - Continue monitoring for up to three business days.
        - Trigger a customer email reminder about the unpaid balance.

### **2.2. Fulfillment Status Verification**

1. **Monitor Order Pickup and Shipping**:
    - The system checks if the order has been picked up or shipped.
    - If picked up or shipped:
        - Mark the fulfillment status as **Fulfilled**.
        - Ensure the order moves to the **Completed** board only if the financial status is updated to "Paid."
2. **Coordinate Financial and Fulfillment Statuses**:
    - Both financial and fulfillment statuses must be complete before the order can leave the shipping board.

### **2.3. Handling Discrepancies**

- If an order's payment status or shipping invoice is not updated:
    - Continue monitoring the order.
    - Ensure manual intervention for issues requiring clarification, such as voided invoices or discrepancies in amounts owed.

---

### **Automated Status Flow**

1. **Initial Order Entry**:
    - Orders enter the shipping board with initial statuses.
    - Statuses are updated based on financial and fulfillment criteria.
2. **Status Checks**:
    - Every two hours, the system:
        - Checks for orders with incomplete financial and shipping statuses.
        - Retrieves and updates data from QuickBooks and Monday.com.
3. **Order Movement**:
    - **Completed Board**:
        - Orders are moved here if both financial and fulfillment statuses are complete.
    - **Free to Release**:
        - Orders ready for pickup but unpaid or unshipped are moved here temporarily.
    - **Awaiting Payment**:
        - Orders with unpaid invoices remain here until payment is confirmed.

### 

## **How to Cancel a Label Using the Automation:**

[https://www.loom.com/share/dde6c3d4235a4c18969b6278323297de](https://www.loom.com/share/dde6c3d4235a4c18969b6278323297de)

If you need to cancel a shipment label due to an issue like an incorrect address or any other reason, follow the steps below to efficiently process the cancellation.

---

### 1. **Prerequisites**

- **Make.com Login:** Ensure you have access to the Make.com account. If you don’t have login details, contact the appropriate person for access.
- Login for [make.com](http://make.com): [info@dadsprinting.com](mailto:info@dadsprinting.com) and Password: Hometown604!
- URL For automation: [https://us1.make.com/3645/scenarios/3481120/edit](https://us1.make.com/3645/scenarios/3481120/edit)
- **Caution:** Be careful not to make any unauthorized changes. Do not cancel or delete any other entries that are not related to this specific task, as it could result in serious consequences.

---

### 2. **Steps to Cancel the Label**

1. **Access the History:**
    - Navigate to the **Make.com** URL provided in the documentation. Once there, go to the **History** section for the label that you want to cancel. Ideally, this will be the most recent label, but if not, you may need to scroll through the sequence of activities to find it.
2. **Find the Correct Data:**
    - After booking the label, several pieces of information are generated in the **data column**, including the **origin**, **order ID**, and **tracking number**.
    - **Tracking Number:** This is the most reliable identifier. Use the tracking number to locate the correct shipment.
    - **Order ID:** Another key piece of information is the **Order ID**, which is usually a unique number associated with the label.
3. **Copy the Order ID:**
    - Once you find the correct label in the history, copy the **Order ID** from the data column.
4. **Cancel the Label:**
    - Leave the **History** section and navigate back to the main diagram.
    - Locate the **Cancel Shipment Order Manual** module (it’s located in the lower section).
    - Click on the module, which will bring up a **Request Content Field**.
    - Paste the copied **Order ID** into the **Cancel Order ID** field.
5. **Execute the Cancellation:**
    - Click **Run this module only** to trigger the cancellation process.
    - Once the process is complete, check the **data field** for the result. If it states **“Shipment Cancelled”**, it means the cancellation was successful.

---

### 3. **Confirmation and Next Steps**

- If the result says **“Shipment Cancelled”**, you’ve successfully cancelled the label. The status of the order should also show as **Cancelled** at the bottom.
- **Note:** This process ensures that if you’ve made a mistake or need to correct something, it’s not too late to rectify the situation.

---

### 4. **Summary**

- The process of cancelling a shipment label is straightforward but requires careful handling of the information and access to the correct modules in **Make.com**. By following the steps outlined, you can easily cancel labels that need corrections, such as incorrect addresses, without issues.

## Automarking Fulfillment Automation:

[https://www.loom.com/share/831b6e039c724f3e9a949d48c488f9fa](https://www.loom.com/share/831b6e039c724f3e9a949d48c488f9fa)

The video explains the configuration and finalization of a shipping automation system that streamlines the process of marking orders as completed once they are shipped. Here's a breakdown of the main points:

1. **Purpose of the Automation**:
    - The automation is designed to mark orders as shipped and move them to the "done" status when appropriate.
2. **Configuration**:
    - A new field, called `Shipment ID`, has been added on the Monday.com platform to enable this functionality.
    - The automation uses this field to track shipment data and perform API calls.
3. **Process Details**:
    - The system operates at specific intervals, every four hours (e.g., 9 a.m., 1 p.m., and 5 p.m.).
    - During these intervals, the system scans all orders on the board for a `Shipment ID` and tracking information.
4. **Workflow**:
    - If an order is marked as shipped and its status changes to "in transit," it implies the order has been picked up.
    - Orders with this status are moved from "fulfilled" to "done." However, they are not removed from the board to accommodate other tasks like billing.
5. **Efficiency Achieved**:
    - Once set up, the process requires minimal manual input:
        - Enter dimensions.
        - Select a shipment.
        - Print and attach the label to the package.
6. **Final Remarks**:
    - This automation is presented as the final step in fully automating the shipping board, simplifying the task to a few basic actions.

If you'd like, I can convert this summary into a structured article or assist with further analysis of the system discussed.

# Summary Of New Shipment Board:

### **Key Benefits**

- **Improved Accuracy**: Automates verification of financial and shipping statuses, reducing errors.
- **Time Savings**: Removes the need for manual checks on order statuses.
- **Customer Transparency**: Automates email reminders for unpaid invoices, improving communication.
- **Scalability**: Supports high order volumes with minimal manual intervention.

---

### **System Maintenance**

1. Regularly review automation settings and triggers in Make.com.
2. Update QuickBooks integration credentials as needed.
3. Train team members on how to address flagged issues manually.

By following this workflow, the system will ensure seamless tracking of financial and fulfillment statuses, reducing administrative burdens and improving operational efficiency.

/

[https://www.loom.com/share/858d35c299ce49f19e05b0a4300a7e5d](https://www.loom.com/share/858d35c299ce49f19e05b0a4300a7e5d)

## Blind Shipping Process:

[https://www.loom.com/share/8a56b133a11a42ec9eb2f051e5a3c066](https://www.loom.com/share/8a56b133a11a42ec9eb2f051e5a3c066)

## **Blind Shipping Process**

Blind shipping follows similar automation workflows as standard shipping but with key differences. Many systems and flows reference the **New Shipping Board & Blindshipping Guide**, which governs these processes.

### **Key Differences in Blind Shipping**

1. **Triggering a Blind Ship Order**:
    - When an order is completed in **Monday.com**, set status to **Ready to Blind Ship**.
    - This generates a blind ship ticket.
2. **Box Count Entry**:
    - Enter the **number of boxes** to populate shipping details.
    - Unlike in-house shipping, labels are not printed immediately but sent when booked.
3. **Vendor Matching**:
    - The system searches **Airtable** for the appropriate vendor.
    - The selected vendor’s details are used to generate quotes via the **NetParcel API**.
4. **Approval & Processing**:
    - Approve quotes in the same manner as standard shipping.
    - The system does not use **PrintNode** but instead emails labels to the vendor for booking.
5. **Tracking & Notification**:
    - Once booked, tracking is emailed to the customer automatically.
    - Automations ensure error handling via **Gmail integrations**.

### **Automation Dependencies**

- **Field requirements** (e.g., addresses, dimensions, box counts) must be strictly followed to prevent failures.
- Shipping invoices undergo an **automated financial check** via QuickBooks before moving forward.
- The system ensures seamless integration between **Monday.com, NetParcel, Make.com, and QuickBooks**.

By adhering to these steps, blind shipping processes maintain efficiency while ensuring accuracy in vendor selection and cost calculations.

## **Key Benefits**

- **Automated linking, invoicing & status tracking**
- **Minimal manual data entry**
- **Efficient workflow integration**

[Automations for Shipping & Blindshipping Board](New%20Shipping%20Board%20&%20Blindshipping%20Guide/Automations%20for%20Shipping%20&%20Blindshipping%20Board%2018f6e9dd11ca806ba906c6ea1f106e92.md)
