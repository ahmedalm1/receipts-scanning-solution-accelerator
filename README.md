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
- Sample Receipts 

#### Steps
##### Step 1: Setting up the environment
1. Creat a new Resource Group in your Azure Subscription and provision the followng resources:
- Storage Account
- Logic App
- Form Recognizer 

![image](https://user-images.githubusercontent.com/88718044/150129298-2143e27a-0733-4eea-90fc-505f8fbddda4.png)

You can also deploy the required resources using this ARM template [TO-DO]:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/)

### Step 2: Setting up the Logic App
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

## License
For all licensing information refer to [LICENSE](https://github.com/AhmedAlmu/cv-knowledge-engine-accelerator/blob/main/LICENSE).
