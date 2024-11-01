# PWC_Task2_Customer_Retention_Dashboard

![My Image](https://user-images.githubusercontent.com/118357991/227788348-b988c4df-7923-46d6-8af7-102b8042f721.png)

# Table of Contents:
- [Problem Statement](#problem-statement)
- [Data Preparation](#data-preparation)
- [Data Modelling](#data-modelling)
- [Data Analysis (DAX)](#data-analysis-dax)
- [Data Visualizations (Dashboard)](#data-visualizations-dashboard)
- [Insights](#insights)
- [Recommendations](#recommendations)

# Problem Statement:
The purpose of this task is to:
  - Define Proper KPI's
  - Create a dashboard for the retention manager reflecting the KPI's
  - Write a short email to him (the engagement partner) explaining your findings, and include suggestions as to what needs to be changed.
  - Services each customer has signed up for: phone, multiple lines, internet, online security, online backup, device protection, tech support, and streaming TV and movies.
  - Customer account information: how long as a customer, contract, payment method, paperless billing, monthly charges, total charges and number of tickets opened in the categories administrative and technical.

# Data Preparation: 
- Removed missing values from Total Charges column.
- Added one additonal column Churn Tenure that categorizes customers based on their duration of services using DAX
  ```dax
  Churn Tenure =
  SWITCH( TRUE(),
    customer_churn[tenure] >=1 && customer_churn[tenure] < 12, "< 1 Years",
    customer_churn[tenure] >=12 && customer_churn[tenure] < 24, "< 2 Years",
    customer_churn[tenure] >=24 && customer_churn[tenure] < 36, "< 3 Years",
    customer_churn[tenure] >=36 && customer_churn[tenure] < 48, "< 4 Years",
    customer_churn[tenure] >=48 && customer_churn[tenure] < 60, "< 5 Years",
    customer_churn[tenure] >=60 && customer_churn[tenure] < 72, "< 6 Years",
  )

# Data Modelling
- Below is a screenshot of the data model created in Power BI, showcasing the relationships between entities:

   ![Power BI Data Model](https://storage.googleapis.com/kagglesdsdata/datasets/5937527/9765607/customer_churn_model.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20241101%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20241101T140837Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=83e36bb0ff8dd4268446675a21e3a8e9010b63b7462763c9c285456f1a2a577e2083ce6d06b933afc03105cc04dee247884bd817c79accb75735f85c1496a82ed66334dc94e13abb79cb6f3c1ca2fe8ea8b9503d3ca9d00e77905f8e9ccd2a572b2cfe2eed412dd265c412479127ce4abd109a515ba78844200b440215c405f0ab2b9edbaa051e452a7b7e8378cfd2fdb9b3e81741418657615afccb6e43a8c897376febf10ba09548a54e1454230f80c7801641c6af18c5e9f3e25dcc5169ba1cb1ebad2e96dc2fb6974f6723896e0f481c1ae39adfa320c85af7129596b60492e04f404d2ae87cf4954bd33b7a7adb42e73af10d4494bc7d764ca969fd363a)

# Data Analysis (DAX):
Measures used in all visualizations are: 

```dax
Average_tenure_of_churned_customers = AVERAGEX(FILTER('customer_churn','customer_churn'[Churn]="Yes"), 'customer_churn'[tenure])/12

Average_tenure_of_retained_customers = AVERAGEX(FILTER('customer_churn','customer_churn'[Churn]="No"), 'customer_churn'[tenure])/12

Churn_Rate = DIVIDE(customer_churn[Churned_Customers],COUNT(customer_churn[Churn]))

Churned_Customers_With_Dependents = DIVIDE(CALCULATE(COUNT(customer_churn[customerID]),customer_churn[Churn]="Yes", customer_churn[Dependents] = "Yes"),[Churned_Customers])

Churned_Customers_With_Partners = DIVIDE(CALCULATE(COUNT(customer_churn[customerID]),customer_churn[Churn]="Yes", customer_churn[Partner] = "Yes"),[Churned_Customers])

Churned_Senior_Citizens = CALCULATE(COUNT(customer_churn[customerID]),customer_churn[Churn]="Yes", customer_churn[SeniorCitizen] = 1)

Projected_Revenue_Loss = [Total_Charges_Churned] * 1.5
```

# Data Visualizations (Dashboard):

Dashboard: ![View Dashboard](https://drive.google.com/file/d/1jNAM83FqvzE0K3Y06NSfNJTSREnQtVHa/view?usp=drive_link)

Visualizations from Customer Retention Dashboards:

![Customer Churn Dashboard](https://storage.googleapis.com/kagglesdsdata/datasets/5937527/9781409/Customer%20Churn%20Dashboard.jpg?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20241101%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20241101T145743Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=944cc55bfa89795ffbe5f93c911860c6670310671e437f9c7de87f7b5c8d740925d2da29e6a744a26c55244b8f7c53fd220d2e75865319b90f5b006ba641c4cdc4d51025ae4966a9c559329f6e80e848071b25609a980ba43a9b1429977365023ced266fb0a7486fe1788a20c45788ba632edd82c61fdc8a6447e3899b6cf61e9863d6d1411fa1389b34886c303014a61d937669292fa056c6691de76b4b2c6957193b6e2787b0ed29b76f87cc14cc7a932808d73377331a4a79fc363dcb2e1294a410ccafe9a8e24aa40c4a4f5b5ca01d0c7d676e7e0efdc46b4dbea71f58c59477299a69a0c4dc41bc9098d96a7467bcbbfed1d06721a29f2d824bc2032d64)

![Customer Risk Dashboard](https://storage.googleapis.com/kagglesdsdata/datasets/5937527/9781409/Customer%20Risk%20Dashboard.jpg?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20241101%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20241101T150117Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=268b0cbe803225fb605551c6e833e5b74f17c1a401edde03b2d7cadeb711af2464bafe1fe0cbe096b6e06ea8ec410c363668c0b045769fdbd99eb25ae1eb027087a477dfa0ec79b20eadacbd6df2fb13d3a0dd9f53edfb2b5c0a2d443b5c37442ac19e030189c894884b01f523bdee2c42041cf2215c055aada65dfbf5464e2baea549a95c75e43f6c9be75ba1c429bb51c7e08884debd00cb4f95b130ebdcf82e67a447bfefc73bc8901d6c638d4847c428e829432edb948ef5a812c2fccf3970aea2ab82b9a79ee979afba7037ec2e1a16c15b6a479f0cb667ae8783b9e492ff20bd7df4c0f5279679fbb600fce0d2e931861589a7fd88bce1be8783570c07)

![Service Usage Dashboard](https://storage.googleapis.com/kagglesdsdata/datasets/5937527/9781409/Service%20Usage%20Dashboard.jpg?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20241101%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20241101T150153Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=60774e193a24cfcc6482e7b8ea3c4769be1c5de28106d662a583215b6f3228af11041c8beb2c2adc432119ff78002863f1fd2f7535d540b87affe0514714b233c4b5b64ee63b1569d2034bed223a518f349cece362d45ea6f8cdadf89949f5dd92613d8db5643d53c6ccbd55618711e54368bb13dc48f580d08857536c65e444878409723146bdfaf6292a5fd1d6cf438c0964464881912659ea910a0a2e8ddcb22657f0b0506b1a5ece854a8b0827bcd2e895190f6f7fb0f44291a9d19c967897cda0cacad5cd29b58fe7e261b2d8cc3c49b96f9b2da66ee720529790e0f5ef0132f1cf72e7b42eaaf07a85388be7feebf75e828b90522d9f48361a6798ff4b)

# Insights:
- 26.67% of customers have churned, with a significant portion (53.45%) leaving within the first year.
- Nearly 88% of churned customers hold month-to-month contracts, indicating a significant opportunity to implement targeted retention strategies.
- 44% of customers are experiencing issues with fiber optic service, highlighting service-related problems as a major concern for a substantial portion of the customer base.
- Almost 20% of admin tickets are associated with churned customers using electronic payment methods.

# Recommendations: 
- The company should implement engagement strategies, such as regular check-ins, special offers or loyalty rewards can encourage new customers to renew or transition to longer-term contracts.
- The company should implement a proactive technical support framework by establishing a dedicated support team focused on high impact tickets, and employing data analytics to monitor customer engagement metrics post-support interactions to effectively reduce churn.
- Improve the reliability and efficiency of the electronic payment system which involves upgrading payment processing software, streamlining billing procedures, or integrating more robust payment gateways.
  
