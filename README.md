# Steps

1. Problem Statement
2. Description of Data
3. Architecture
4. Coding
    - Data Ingestion
    - Data Preprocessing
    - Model Selection
    - Model Tuning
    - Prediction
    - logging Framework
    - Deployment

## Problem Statement

To build a classification methodology to determine whether a customer is placing a genuine or fraudulent insurance claim.

## Description of Data

The data contains the following attributes:

Features:

1. months_as_customer: It denotes the number of months for which the customer is associated with the insurance company.
2. age: continuous. It denotes the age of the person(customer).
3. policy_number: The policy number.
4. policy_bind_date: Start date of the policy.
5. policy_state: The state where the policy is registered.
6. policy_csl-(combined single limits)How much of the bodily injury will be covered from the total damage.
<https://www.berkshireinsuranceservices.com/arecombinedsinglelimitsbetter>  
7. policy_deductable: The amount paid out of pocket by the policy-holder before an insurance provider will pay any expenses.
8. policy_annual_premium: The yearly premium for the policy.
9. umbrella_limit: An umbrella insurance policy is extra liability insurance coverage that goes beyond the limits of the insured's homeowners, auto or watercraft insurance. It provides an additional layer of security to those who are at risk of being sued for damages to other people's property or injuries caused to others in an accident.
10. insured_zip: The zip code where the policy is registered.
11. insured_sex: It denotes the person's gender.
12. insured_education_level: The highest educational qualification of the policy-holder.
13. insured_occupation: The occupation of the policy-holder.
14. insured_hobbies: The hobbies of the policy-holder.
15. insured_relationship: Dependents on the policy-holder.
16. capital-gain: It denotes the monitory gains by the person.
17. capital-loss: It denotes the monitory loss by the person.
18. incident_date: The date when the incident happened.
19. incident_type: The type of the incident.
20. collision_type: The type of collision that took place.
21. incident_severity: The severity of the incident.
22. authorities_contacted: Which authority was contacted.
23. incident_state: The state in which the incident took place.
24. incident_city: The city in which the incident took place.
25. incident_location: The street in which the incident took place.
26. incident_hour_of_the_day: The time of the day when the incident took place.
27. property_damage: If any property damage was done.
28. bodily_injuries: Number of bodily injuries.
29. Witnesses: Number of witnesses present.
30. police_report_available: Is the police report available.
31. total_claim_amount: Total amount claimed by the customer(injury_claim + property_claim,vehicle_claim).
32. injury_claim: Amount claimed for injury
33. property_claim: Amount claimed for property damage.
34. vehicle_claim: Amount claimed for vehicle damage.
35. auto_make: The manufacturer of the vehicle.
36. auto_model: The model of the vehicle.
37. auto_year: The year of manufacture of the vehicle.

## Architecture

![MODEL ARCHITECTURE](/README_IMG/Architecture.png)

- **Data Ingestion**:

  - Data(Batches) for training - Data is received from the client in batches in the shared file location. We then perform data validation to find out if the file format sent by the client is in the desired format.
  
  - Data Validation- A certain type of data format that is agreed upon between us and the client should be received from the client, if the client is not sending us data in the desired format we discard it by sending it into the bad data folder and the data is archived over there. Now, once all the validation is passed on the files sent by the client we perform data transformation.

  - Data Transformation- We perform data transformation before inserting data into a local database. Missing values are replaced by NULL. Categorical values which are in the single quote are replaced with double quotes ( because this is how our local database understands the value).

  - Data Insertion in Database - Why share the data again in the local database? It is because the client is sharing data in a shared file location. The client is sharing different files, using those different files we are not gonna create an n-number of modules and separate modules for separate files. We are going to aggregate valid data that the client has sent to us and validate that data in a common database and using that data we are going to start model training.
  - Export data into CSV- The data from the database is exported in the CSV format. This CSV file will act as an input for our training.

- **Data Preprocessing**:

  - Data Preprocessing- Impute missing values, convert categorical values into numerical values, and data which is not normally distributed into a normal distribution.
  
  - Data Clustering- Before feeding the data into a machine learning algorithm, Clustering is needed because there is a mathematical observation that many times if we create one machine learning model for the entire data and we calculate the performance of that model and if we divide the data into individual clusters and train individual models over those clusters and create different models for those clusters then the performance for those individual models there are going to be a lot better then the one model for the entire dataset.

- **Model Selection and Tuning**:
  - Get the best model for each cluster- Train individual models for individual clusters.
  - Hyperparameter Tuning- Using hyperparameter tuning, we are going to enhance the performance of the model which we already have selected, and those models which perform the best for a given cluster by comparing their Individual performance using the AUC score (Area under curve) will be selected. Finally, we will get a model for individual clusters which performs the best.

  - Model Saving- After we get the best model, we are going to save that model file into our local system.

- Cloud:
  - Cloud setup- Creating Flask application and setting up the application.

  - Pushing app to cloud- Setting up our framework on Cloud i.e. to host our framework on Heroku.

