# Machine Learning in the Oracle Database #

## Introduction ##

OAS offers a handful of native ML algorithms that work fine against reasonably sized data sets, you may find your organization has larger, more complex data sets than OAS can reasonably handle.  It’s also possible you may wish to evaluate a more comprehensive and varied set of algorithms as compared to what OAS natively supports.

In such situations, a professional data scientist may wish to leverage the more comprehensive set of Oracle Machine Learning (OML) algorithms, now offered for free with each of your Oracle Database licenses. OML not only affords you the ability to leverage more algorithms, but because it is able to leverage the advanced parallel processing capabilities of the Oracle database you can work with significantly larger, more complex data sets. A final advantage of doing such work in an Oracle database is that once trained your machine learning algorithms can be quickly and readily operationalized via simple SQL statements. This is a far easier and more approachable mechanism than say trying to teaching all of your application developers languages such as Python and R.

*Estimated Lab Time:  45 minutes*

### Objectives ###

In this lab, you will play the role of a data scientist assisting the HR Analyst within your organization. The analyst has asked you to assist in evaluating what ML algorithms are best suited to understanding and preventing unwanted attrition in order to decrease the rate of attrition and identify potential strategies to mitigate risk.   Additionally, you will identify employees still with the organization who are at greatest risk for leaving.

### Prerequisites ###

This lab assumes you have:
- A Free Tier, Paid or LiveLabs Oracle Cloud account
- SSH Private Key to access the host via SSH
- You have completed:
    - Lab: Generate SSH Keys (Free Tier and Paid Account only)
    - Lab: Prepare Setup (Free Tier and Paid Account only)
    - Lab: Environment Setup
    - Lab: Initialize Environment

- The following files are required to complete this lab.
    - DATA TO BE IMPORTED IN ORACLE DB:
         - Employee_Attrition.csv file 
         - Employee_Data.csv file 

     - WORKFLOW TO BE IMPORTED IN ORACLE DB:
         - Employee Attriton.xml 

     - SQL Files:
         - Step01\_Employee\_Attrition.sql
         - Step02\_Attrit\_AttribImport.sql 
         - Step03\_Attrit\_AttribImport_Result.sql 
         - Step04\_SplitDataToTrainTestModels.sql  
         - Step05\_GLM\_Model.sql 
         - Step05\_NB\_Model.sql 
         - Step05\_SVM\_Model2.sql 
         - Step06\_CumulativeGainsTbl.sql 
         - Step06\_CumulativeGains\_Results.sql 
         - Step07\_NewEmpToPredictl.sql 
         - Step08\_PredictAttrition.sql

     - DVA FILE:
         - EmployeeAttrition_OML.dva 

## Task 1: Using Data Miner GUI to Train, Test and Evaluate a Machine Learning Model

In this step we’ll show Oracle Data Miner which enables developers to work directly with data inside the database using a graphical “drag and drop” workflow editor. Oracle Data Miner (ODMr), an extension to Oracle SQL Developer, captures and documents in graphical analytical workflows the steps users take while exploring data and developing machine learning methodologies.

1. Login to SQL Developer via Guacamole by entering the URL provided by your instructor.
![](./images/ml1.1.png " ")
2. Set up a new connection to the Oracle database using the "biworkshop_nn_" userid assigned you by the instructor.
   
  Press Test to verify the connection works
![](./images/ml1.2.png " ")
3. Open the newly created connection, right click on "Tables" and import the "Employee\_Attrition.csv" file.
   
  Accept all defaults but name the table **EMPLOYEE_ATTRITON** when prompted
![](./images/ml1.3.png " ")
4. Scroll down the list of tables to locate your newly imported table, then Select the table and click on the Data tab to verify your data was properly imported.  
   
  This is a file of employees we will use to train our machine learning models containing employees who have left the organization as well as some who have not.
![](./images/ml1.4.png " ")
5. Open the newly created connection, right click on Tables and import the "Employee\_Data.csv" file.  
   
  Accept all defaults but name the table **EMPLOYEE_DATA** when prompted.
![](./images/ml1.5.png " ")
6. Scroll down the list of tables to locate your newly imported table, then Select the table and click on the Data tab to verify your data was properly imported.  
   
  This is a file of employees still with the organization that we will run through our trained Machine Learning model in order to predict which employees are most likely to leave.
![](./images/ml1.6.png " ")
7. Using SQL Data Miner, create a "New Project" and give it a name of **Employee Attrition**.
![](./images/ml1.7.png " ")
8. Right click the new Employee Attrition project you created and select "Import Workflow".
       
  Now navigate to the lab file "Employee Attriton.xml" and Open it.  

  Accept the defaults and import the workflow.
![](./images/ml1.8.png " ")
9.  Right click the "EMPLOYEE\_ATTRITION" data source node then select "View Data".
![](./images/ml1.9.png " ")
10. This Data Source node points to the "EMPLOYEE\_ATTRITION" table imported in **step 3**.  
    
  As noted earlier this table represents employees we will use to train our machine learning models and is a representative sample of employees who have left the organization as well as some who have not.
![](./images/ml1.10.png " ")
11. Right click the Sample node to the adjacent right and choose "Run".
![](./images/ml1.11.png " ")
12. Double click the Sample node.
    
  Note that we’re selecting approx 1/3rd of the data to use for training our machine learning models.
![](./images/ml1.12.png " ")
13. Next Right click the "Key Attributes and Data Profiling" node to the adjacent right of Sample and choose "Run".
    
  After it completes Right Click and choose "View Attribute Importance" to see those attributes which are most influential in determining attrition. 
![](./images/ml1.13.png " ")
14. JOBROLE, OVERTIME, TOTALWORKINGYEARS, etc. are clearly key attributes that determine if an employee is likely to leave or not.
![](./images/ml1.14.png " ")
15. Next click the classification node named "Four Attrition Models" to the adjacent right and choose "Run". 
![](./images/ml1.15.png " ")    

  Notice this classification node actually trains 4 separate Machine Learning models (Generalized Linear Model, Support Vector Machine, Decision Tree & Naïve Bayes) in parallel showcasing how moving the algorithms to the data helps speed up processing.  
16. Right click the "Four Attrition Models" node to the adjacent right of Sample and choose "Compare Test Results" to see which model does the best job of predicting attrition.
![](./images/ml1.16.png " ")
17. Double click on the "Overall Accuracy %" column header.
  Note the "Support Vector Machine" model seems to be the best predictor of attrition.
![](./images/ml1.17.png " ")
18. Right click the "EMPLOYEE\_DATA" data source node then choose "View Data".
![](./images/ml1.18.png " ")
19. This Data Source node points to the "EMPLOYEE\_DATA" table imported in **step 3**. 
    
  Recall that these are employees still with the organization that we want to run through our newly trained Support Vector Machine model in order to predict employees most likely to leave.
![](./images/ml1.19.png " ")
20. Click the "Apply" node to see how we can apply the Support Vector Machine model to predict employees likely to leave the organization.
![](./images/ml1.20.png " ")
21. Right click the "EMPLOYEE\_PREDICTION" node to the adjacent right and choose "Run".  
![](./images/ml1.21.png " ")
22. Right click the "EMPLOYEE\_PREDICTION" data source node then choose "View Data". 
  Double click the column header "CLAS-SVM\_1\_66\_Prob\_YES" to see those employees at highest risk of leaving the organization. 
![](./images/ml1.22.png " ") 

  NOTE : The Data Miner GUI enables developers to quickly and graphically create workflows by dragging and dropping nodes from the workflow editor out onto the design pallet. Each node is then configured in accordance with the task to be performed. To form a workflow each node is connected by right clicking it, selecting Connect, then dragging the connection point to the intended node.
   ![](./images/ml1.23.png " ")

23. Lastly, any workflow can be readily output as SQL statements to be run as a script or to incorporate the logic into an application. This is accomplished by simply right clicking any node then choosing "Save SQL".
![](./images/ml1.24.png " ")

## Task 2: Using SQL to Train, Test and Evaluate a Machine Learning Model

In this step we’ll implement essentially that exact workflow using a series of SQL statements in order to illustrate how developers can readily incorporate machine learning into their applications. Often referred to as “operationalizing” the algorithm(s), this illustrates how to overcome a huge stumbling block that often thwarts the efforts of business to realize value from machine learning. 

1. In SQL Developer, from the top menu bar choose File, then select open. Now, navigate to the SQL files for this lab and find "AttritionSQL\Step01\_Employee\_Attrition.sql".
![](./images/ml2.1.png " ")

2. Open the file "Step01\_Employee\_Attrition.sql" and run the statement using the 1st  icon (Run Statement) on the tool bar or by pressing _CTRL + Enter_.   
![](./images/ml2.2.png " ")
   
  This pulls back all columns for all rows from the table "EMPLOYEE\_ATTRITON".  The table contains employees that we will use to train our machine learning models and contains an ATTRITION column indicating employees who have left the organization as well as those who have not.  

  The next lab uses OAS Data Visualization to create a series of charts to help us gain a better business understanding of the data.
3. Open the next file "Step02\_Attrit\_AttribImport.sql" and run the script using the 2nd icon (Run Script) on the tool bar or by pressing _F5_. 
![](./images/ml2.3.png " ")
   
  This SQL statement creates a mining model that enable us to evaluate the each attribute within the "EMPLOYEE\_ATTRITON" table in terms of its importance relative to predicting attrition.
4. Open the next file "Step03\_Attrit\_AttribImport\_Result.sql" and run the statement using the 1st icon (Run Statement) on the tool bar or by pressing _CTRL + Enter_. 
![](./images/ml2.4.png " ")
   
  This SQL statement returns information regarding the top 10 most influential attributes from the "EMPLOYEE\_ATTRITON" table based on their importance in predicting attrition.
5. Open the next file "Step04\_SplitDataToTrainTestModels.sql"  and run the script using the 2nd icon (Run Script) on the tool bar or by pressing _F5_.
![](./images/ml2.5.png " ")
   
  This script creates two views. One holds data for training our machine learning models and the other will be used to test the models after they are trained.
6. Open the next file "Step05\_GLM\_Model.sql" and run the script using the 2nd icon (Run Script) on the tool bar or by pressing _F5_.
![](./images/ml2.6.png " ")

  This script trains a CLASSIFICATION model using the ALGO\_GENERALIZED\_LINEAR\_MODEL based on the training data. After which the test data is used to score the data and compute lift to evaluate the predictive accuracy of the algorithm. 
7. Open the next file "Step05\_NB\_Model.sql" and run the script using the 2nd icon (Run Script) on the tool bar or by pressing _F5_.  

  This script trains a CLASSIFICATION model using the ALGO\_NAÏVE\_BAYES based on the training data. 
![](./images/ml2.7.png " ")

  After which the test data is used to score the data and compute lift to evaluate the predictive accuracy of the algorithm.
8. Open the next file "Step05\_SVM\_Model2.sql" and run the script using the 2nd icon (Run Script) on the tool bar or by pressing _F5_.  
![](./images/ml2.8.png " ")
   
  This script trains a CLASSIFICATION model using the ALGO\_SUPORT\_VECTOR\_MACHINES based on the training data. After which the test data is used to score the data and compute lift to evaluate the predictive accuracy of the algorithm.
9. Open the next file "Step06\_CumulativeGainsTbl.sql" and run the statement using the 1st icon (Run Statement) on the tool bar or by pressing _CTRL + Enter_. 
![](./images/ml2.9.png " ")
    
  This SQL statement creates a view of all lift data from all three models. This will be used to evaluate which model does the best job predicting attrition.
10. Open the next file "Step06\_CumulativeGains\_Results.sql" and run the statement using the 1st icon (Run Statement) on the tool bar or by pressing _CTRL + Enter_. 
![](./images/ml2.10.png " ")
    
  This SQL statement returns all lift data from all three models which can be charted to evaluate which model does the best job predicting attrition.

  The next lab uses OAS Data Visualization to create a chart that compares the accuracy of all three models.
11. Open the next file "Step07\_NewEmpToPredictl.sql" and run the script using the 2nd icon (Run Script) on the tool bar or by pressing _F5_.
![](./images/ml2.11.png " ")
  
  This script creates a table of employees without the ATTRITION column. We will then apply the SUPPORT VECTOR MACHINES model against this table to predict which employees are most likely to leave the organization. 
12. Open the next file "Step08\_PredictAttrition.sql" and run the statement using the 1st icon (Run Statement) on the tool bar or by pressing _CTRL + Enter_.
![](./images/ml2.12.png " ")
    
  This SQL statement applies our SUPPORT VECTOR MACHINES model to all employees in order to identify the employees with the highest risk of leaving.

  The next lab uses OAS Data Visualization to illustrate how this same query could be run in any application to quickly and effectively “operationalize” machine learning.

## Task 3: Using OAS to Understand the Data Machine and call OML in the Oracle DB

1. Login to Oracle Analytics by entering the URL provided by your instructor.
![](./images/ml3.1.png " ")
2. From the OAS Home page select "Import Project/Flow” using the Page Menu (hamburger along the top right side of page) to import a project that illustrates how we can leverage Oracle DB Machine Learning (OML) from OAS Data Visualization.
![](./images/ml3.2.png " ")
3. Select the **EmployeeAttrition\_OML.dva** file from the workshop labs folder. 
   
  When prompted input “Admin123” as the password in order to import the project.
![](./images/ml3.3.png " ")
4. In the Catalog find the newly imported "EmployeeAttrition\_OML" project and open it.
![](./images/ml3.4.png " ")
5. The Data Understanding canvas contains a number of charts that enable you to quickly visualize key attributes that influence attrition. 
![](./images/ml3.5.png " ")
6. Now let’s see where this data comes from. 
     - 1st choose the Prepare tab at the top left of the page. 
     - 2nd select the "EMPLOYEE\_ATTRITION" dataset at the bottom of the page. 
     - 3rd click the Edit Data Set icon at the top RHS under the Save button.

  Now we can see that this data set is actually a live link back to the "EMPLOYEE\_ATTRITION" table in our Oracle DB.
![](./images/ml3.6.png " ")
7. The Model Evaluation canvas surfaces data from our "CUMULATIVE\_GAIN" table as well as data from the  "ALL\_LIFT\_DATA\_V" view in the underlying Oracle DB enabling us to evaluate which machine learning model most accurately predicts attrition.

  From these charts the Support Vector Machines model appears to afford the best accuracy.
![](./images/ml3.7.png " ") 
8. As with the EMPLOYEE\_ATTRITION data set, both the ALL\_LIFT\_DATA\_V and CUMULATIVE\_GAIN data set charted on this canvas are being sourced from views / tables in the Oracle DB.
![](./images/ml3.8.png " ")

  The Predicted Attritions canvas surfaces data from the "OML\_PredictAttrition"  query enabling us to immediately and easily “operationalize” the  Support Vector Machines algorithm that we trained on the Oracle DB to dynamically predict potential attritions.
9. The charts enable us to visualize what employees are most likely to leave which can help us figure out appropriate programs we may want to put in place in order to prevent unwanted turnover.
![](./images/ml3.9.png " ")
10. Looking at the **OML\_PredictAttrition** data set we can view the query that dynamically calls the attrition\_model\_SVM algorithm to generate a **ATTRITTION\_PREDICTION** column indicating if that employee is likely to leave as well as a  **ATTRITION\_PREDICTION\_PCT** indicating how confident we are in that prediction. 
![](./images/ml3.10.png " ")


**This concludes this lab.**

With this, you have learned that OAS affords end users the ability to perform augmented analytics readily and easily on their own without requiring the assistance from a data scientist.  Examples include the ability to readily explain any metric or attribute, adding a forecast, trendline, or identify outliers /clusters in your data with a single click, data prep recommendations that significantly improve your data for analysis, natural language processing when interacting with data and the ability to create data flows that train and apply ML models are all included in OAS.    

Oracle’s converged database Machine Learning (OML) capabilities enable end users to work with data scientists inside your organization to enhance and extend the OAS capabilities.  OML affords access to more models as well as more sophisticated algorithms including R scripts and these enhanced capabilities are now included for free in your database license.   Oracle Converged Database enables you to build and train models using significantly more data and attributes than is possible using OAS alone.  

Finally, the more sophisticated data models your data science team crafts in OML can quickly and readily be surfaced via OAS (or any other application) using a few SQL statements.  This is because all aspects of OML can be wrapped in a series of SQL statements, making it extremely fast and easy to operationalize the models you build


## Want to learn more
  
* [https://blogs.oracle.com/machinelearning](https://blogs.oracle.com/machinelearning/)
* [Oracle Machine Learning Overview: from Oracle Data Professional to Oracle Data Scientist in 6 Weeks!](https://www.youtube.com/watch?v=jFBMhOapGL8&feature=youtu.be&t=1)
* [Learn How to Use Oracle Data Miner UI in 45 Minutes](https://blogs.oracle.com/machinelearning/learn-how-to-use-oracle-data-miner-ui-in-45-minutes)
* [OML Getting Started Documentation](https://docs.oracle.com/en/cloud/paas/autonomous-data-warehouse-cloud/omlug/get-started-oracle-machine-learning.html)
* [Oracle Machine Learning for R Learning Path](https://apexapps.oracle.com/pls/apex/f?p=44785:24:9589340370727:PRODUCT:NO::P24_CONTENT_ID,P24_PREV_PAGE,P24_PROD_SECTION_GRP_ID:8984,141,)
* [Partner Built Tutorial On YouTube](https://www.youtube.com/playlist?list=PL99-DcFspRUq8VbbgXe2lQ559VDr7BSCr)


## Acknowledgements
* **Authors** - Diane Grace, Manager, Analytics Platform Specialist Team, NA Technology
* **Contributors** - John Miller, Jyotsana Rawat, Satya Pranavi Manthena
* **Last Updated By/Date** - Jyotsana Rawat, Solution Engineer, NA Technology, February 2021

## Need Help?
Please submit feedback or ask for help using our [LiveLabs Support Forum](https://community.oracle.com/tech/developers/categories/livelabsdiscussions). Please click the **Log In** button and login using your Oracle Account. Click the **Ask A Question** button to the left to start a *New Discussion* or *Ask a Question*.  Please include your workshop name and lab name.  You can also include screenshots and attach files.  Engage directly with the author of the workshop.

  
