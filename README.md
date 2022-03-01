# Smart Receipts Scanning for Reward Programs Solution Accelerator

## About this accelerator
The Smart Receipts Scanning for Reward Programs Solution Accelerator is an easy-to-deploy solution, that creates an automated process of scanning receipts, extracts insights and information, approves or rejects receipts based on business rules, and consumes the outcome in one wholistic report.

The solution aims to accelerate information extraction using the pre-built receipt model in Form Recognizer. It applies intelligent business rules to check the eligibility of a receipt based on pre-defined criteria. It can be considered as a complete and automated end-to-end solution that leverages the power of Logic Apps, and creates a summarized view with a Power BI Report to consume the extracted insights.

### Business Use Case 
Reward Programs are a proven marketing strategy, designed to encourage customers of a business, to continue using their services and buying their goods. One way of achieving such results is by analyzing purchase receipts and rewarding the customer with redeemable points during a campaign period. 

This solution accelerator aims to create an end-to-end solution, which automates the process of scanning receipts, extracting purchase information like brand details and amount spent, and apply business rules to approve receipts and reward qualified customers with equivalent points, by leveraging the capabilities of Form Recognizer and Azure Cognitive Services.

### Resources and Architecture 
The following are the Azure resources that are required to deploy this accelerator, along with the architecture of the solution:
- Storage Account -- to store the input receipts and the output CSVs.
- Cognitive Services (Form Recognizer) -- to extract insights from receipts with the pre-built models.
- Logic App -- to work as an orchestrator and automate the whole process. 

![Architecture](https://user-images.githubusercontent.com/88718044/155709795-c1651f61-b1aa-436f-8ef7-50ae88f3b82c.png)

### Sample Receipts 
The sample receipts dataset used in this accelerator is from an open-sources dataset, containing 200 sample receipts, and acquired from [ExpressExpense.com](https://expressexpense.com/blog/free-receipt-images-ocr-machine-learning-dataset/).

### Extracted Information 
Using the pre-built receipts model in Form Recognizer, we can extract the following infrormation from the receipts:
- Brand name
- Transaction date and time
- Total (sub total, tax, total)
- Line-items (name, quantity, price) 

### Business Rules 
There are four, customizable business rules currently supported in this solution accelerator, that approves or rejects a receipt, which are:
- Brand Eligibility -- checks the Brand Name and qualifies the receipt if accepted or not.
- Campaign Period -- checks the Transaction Date and qualifies the receipt if accepted or not.
- Points Earned -- checks the Total Spent and rewards the user with points if the receipt is accepted.
- Confidence Score -- checks the Confidence Score and qualifies the receipt if accepted or not. 

### Deployment 
#### Pre-requisites
- Azure Subscription
- Sample Receipt 

#### Steps
##### Step 1: Setting up the environment
1. Creat a new Resource Group in your Azure Subscription and provision the followng resources:
- Storage Account
- Logic App
- Form Recognizer 

![image](https://user-images.githubusercontent.com/88718044/156177925-6eaf6d0b-a24e-430b-befc-2fd9e571622d.png)

You can also deploy the required resources using this ARM template:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fahmedalm1%2Freceipts-scanning-solution-accelerator%2Fmain%2Ftemplate.json) 

##### Step 2: Setting up the Logic App
1. Create a blank Logic App. 

![image](https://user-images.githubusercontent.com/88718044/150130109-4f7051fa-f14d-4bcf-ae76-e3832187fa19.png)

2. Search for "Azure Blob Storage" and select "When a blob is added or modified" trigger. 

![image](https://user-images.githubusercontent.com/88718044/150131381-14d8cd63-2f33-4d5d-bcc2-9baf97bcdc0f.png)

3. You will be required to create a connection to the storage account. Fill in the information and click on Create to proceed. 

![image](https://user-images.githubusercontent.com/88718044/150132267-337aa540-f7ec-46f4-8a96-ffdd5d87d669.png)

4. You will be required to select a container in the storage account to monitor. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150132720-81ded702-b031-49fd-a569-ccd55b44dcc7.png)

5.From the "Azure Blob Storage" list of actions, select "Get blob content". 

![image](https://user-images.githubusercontent.com/88718044/150133252-07650a5a-cedd-42d8-894a-e60ec339b183.png)

6. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150160238-3158d125-d525-48e7-afa3-748c2aa8d400.png)

7. Search for "Form Recognizer" and select "Analyze Receipt" from the list of actions.

![image](https://user-images.githubusercontent.com/88718044/150135090-fcfbb294-cae9-471b-b903-7b146aaa82e5.png)

8. You will be required to create a connection to Form Recognizer. Fill in the information and click on Create to proceed.

![image](https://user-images.githubusercontent.com/88718044/150135287-5b6d4d44-cad3-4e08-8e34-8fdcb708226d.png)

9. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150135574-e5399848-76d7-4088-aca2-8e99e9fb7406.png)

10. From the "Variables" list of actions, select "Initialize variable".

![image](https://user-images.githubusercontent.com/88718044/150136043-cd2cf53d-d865-4f9b-9617-e491de3cdb50.png)

11. Create an empty array variable that will hold the line items. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150143477-64df897d-d3d7-4333-80e9-e92d240b82a4.png)

##### Step 3: Setting up the Logic App - Receipt Information
1. From the "Data Operations" list of actions, select "Create CSV table".

![image](https://user-images.githubusercontent.com/88718044/156182793-07b1c740-abb0-4611-b82d-234ed35010ea.png)

2. Search for "documentResults" and select it as the input for the CSV table. Select "Custom" for the "Columns" option. 

![image](https://user-images.githubusercontent.com/88718044/156179259-294acb14-2e0e-4ce2-b44e-67a20eb35ab2.png)

3. Fill in the table with the following information:
Receipt information
```json
"brand_name": "<Merchant name field Merchant name>"
"brand_confidence": "<Merchant name field Confidence>"
"transaction_date": "<Transaction date field Transaction date>"
"transaction_confidence": "<Transaction date field Confidence>"
"total_spent": "<Total field Total>"
"total_confidence": "<Total field Confidence>"
```

![image](https://user-images.githubusercontent.com/88718044/156180084-9daa1a52-bb02-46c9-b1f7-8dd34301f0f0.png)

Business rules
```json
"eligible_brand": "<IF merchant_name CONTAINS eligible_brands_list THEN true ELSE false>"
"campaign_period": "<IF transaction_date CONTAINS campaign_period THEN true ELSE false>"
"reward_points": "<MUL total BY conversion_rate>"
"accuracy_flag": "<IF confidence_score LESS THAN threshold THEN true ELSE false>"
```

![image](https://user-images.githubusercontent.com/88718044/156180230-fcb5bf8f-f92e-4e28-933d-bf86f7474cd6.png)

4. From the "Azure Blob Storage" list of actions, select "Create blob".

![image](https://user-images.githubusercontent.com/88718044/150155806-680b0be0-7d22-4684-b4df-daafbccd144b.png)

5. Fill in the information. For "Blob name" use a concat function to append ".csv" to the file name for the generated CSV file. For "Blob content", use "Outputs" of the "Create CSV table" action. Save your Logic App to proceed.

![image](https://user-images.githubusercontent.com/88718044/150156330-7ddb5b02-4eb2-4d5c-a917-3c779c17e706.png)

##### Step 4: Setting up the Logic App - Line-items Information
1. Select "Add a parallel branch" after the "Initialize variable" action.

![image](https://user-images.githubusercontent.com/88718044/156184503-58b7b930-2151-4f46-9820-fc6cab70d4a2.png)

2. From the "Control" list of actions, select "For each".

![image](https://user-images.githubusercontent.com/88718044/150143731-8589d3e0-473f-417f-bd72-b4a87f35d975.png)

3. Search for "documentResults" and select it as the loop parameter. Click on Add an action inside the loop to proceed.

![image](https://user-images.githubusercontent.com/88718044/150144230-09d44f23-2182-48df-bf84-17cb5e0d3437.png)

4. Add another "For each" loop. Search for "items" and select "Items field Items" as the loop parameter. Click on Add an action inside the second loop to proceed.

![image](https://user-images.githubusercontent.com/88718044/150144798-06fff3f6-459e-4215-acba-bc528f753f4b.png)

5. From the "Data Operations" list of actions, select "Compose".

![image](https://user-images.githubusercontent.com/88718044/150151338-38fb893b-0856-4f43-b2a7-3cd38ce15244.png)

6. Use the following structure as input and replace the placeholders with the corrosponding dynamic values. Click on Add an action inside the second loop to proceed.

```json
{
  "item": "<Item field value Name>",
  "price": "<Item field value Price>",
  "quantity": "<Item field value Quantity>",
  "total_price": "<Item field value Total price>"
}
```

![image](https://user-images.githubusercontent.com/88718044/150153614-e85d1212-9036-416e-bca5-233301121ce6.png)

7. From the "Variables" list of actions, select "Append to array variable".

![image](https://user-images.githubusercontent.com/88718044/150154440-3c7633ac-7164-4b6b-a9fa-77d653036a55.png)

8. Select the name of the array and choose "Outputs" of "Compose" as the value. Click on New step outside the loops to proceed.

![image](https://user-images.githubusercontent.com/88718044/150154989-d860eddf-2c88-45e8-a478-fe37fd9914ff.png)

9. From the "Data Operations" list of actions, select "Create CSV table".

![image](https://user-images.githubusercontent.com/88718044/150155475-94a9483a-1bf2-4279-8f15-209e9e337540.png)

10. Fill in the information and click on New step to proceed. 

![image](https://user-images.githubusercontent.com/88718044/150155592-5442ae81-354e-4932-935d-934b218be597.png)

11. From the "Azure Blob Storage" list of actions, select "Create blob".

![image](https://user-images.githubusercontent.com/88718044/150155806-680b0be0-7d22-4684-b4df-daafbccd144b.png)

12. Fill in the information. For "Blob name" use a concat function to append "-items.csv" to the file name for the generated CSV file. For "Blob content", use "Outputs" of the "Create CSV table" action. Save your Logic App to proceed.

![image](https://user-images.githubusercontent.com/88718044/150156330-7ddb5b02-4eb2-4d5c-a917-3c779c17e706.png)

##### Step 5: Testing the Logic App
1. From "Run Trigger", click on "Run".

![image](https://user-images.githubusercontent.com/88718044/150158782-5decf1eb-52d7-4b0a-b424-705f6263316f.png)

2. Upload the sample receipt "1000-receipt.jpg" to the container in the storage account.

![Receipt](https://user-images.githubusercontent.com/88718044/150165816-85e7bdc8-4437-4a6a-aa25-03cdc07a2e6d.png)

3. Wait for the Logic App flow to finish. 

![image](https://user-images.githubusercontent.com/88718044/156182974-0fd6cbb8-8468-4b37-a061-a82f7254ece9.png)

4. The result will be two CSV files, one for the Receipt information, and one for the Line-items.

![image](https://user-images.githubusercontent.com/88718044/156183944-8b2010b1-fd00-4e84-8e65-74d13d6946df.png)

## License
For all licensing information refer to [LICENSE](https://github.com/AhmedAlmu/cv-knowledge-engine-accelerator/blob/main/LICENSE).
