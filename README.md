# PWC_Task2_Customer_Retention_Dashboard

![My Image](https://user-images.githubusercontent.com/118357991/227788348-b988c4df-7923-46d6-8af7-102b8042f721.png)

# Table of Contents:

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
