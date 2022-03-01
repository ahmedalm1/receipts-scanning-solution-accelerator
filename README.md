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
TODO

## License
For all licensing information refer to [LICENSE](https://github.com/AhmedAlmu/cv-knowledge-engine-accelerator/blob/main/LICENSE).
