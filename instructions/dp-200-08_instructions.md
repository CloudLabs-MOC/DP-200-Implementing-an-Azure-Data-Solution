# DP 200 - Implementing a Data Platform Solution
# Lab 7 - Securing Azure Data Platforms

**Estimated Time**: 75 minutes

**Lab files**: The files for this lab are located in the _C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\Starter\DP-200.8_ folder.

## Lab overview

The students will be able to describe and document the different approaches to security that can be taken to provide defence in depth. This will involve the student documenting the security that has been set up so far in the course. It will also enable the students to identify any gaps in security that may exists for AdventureWorks.

## Lab objectives
  
After completing this lab, you will be able to:

1. Explain Security
2. Describe key security components
3. Secure Storage Accounts and Data Lake Storage
4. Secure Data Stores
5. Secure Streaming Data

## Scenario
  
As a senior data engineer within AdventureWorks, you are responsible for ensuring that your data estate is secured. You are performing a security check of your current infrastructure to ensure that you have diligently placed security where it is required. This check should be a holistic check of all the services and data that you have created so far, and an identification of any gaps that there may be in the configuration of the security. 

You have also been asked to tighten up the security of the SQL Database DeptDatabasesxx and have been asked to setup auditing against the database so that you can monitor access to the database. Furthermore, you have learned that that the Manage permission for your event hub is not restrictive enough, and you want to remove this permission.

At the end of this lad, you will have:

1. Explained Security
2. Described key security components
3. Secured Storage Accounts and Data Lake Storage
4. Secured Data Stores
5. Secured Streaming Data

> **IMPORTANT**: As you go through this lab, make a note of any issue(s) that you have encountered in any provisioning or configuration tasks and log it in the table in the document located at _C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\DP-200-Issues-Doc.docx_. Document the Lab number, note the technology, Describe the issue, and what was the resolution. Save this document as you will refer back to it in a later module.

## Exercise 1: An introduction to security

Estimated Time: 15 minutes

Group exercise
  
The main task for this exercise are as follows:

1. Security as a layered approach.


### Task 1: Security as a layered approach.

1. From the lab virtual machine, open up the file **DP-200-Lab08-Ex01.docx** from the **C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\Starter\DP-200.8** folder.This file will open in wordpad.

2. From the course content, case study and the scenarios taken in the course so far, spend **10 minutes** in a group identifying the layers of security that you have impacted so far to secure AdventureWorks in the labs. Find three examples.

## Exercise 2: Key security components
  
Estimated Time: 10 minutes

Individual exercise
  
The main tasks for this exercise are as follows:

1. Assessing Data and Storage Security Hygiene

### Task 1: Assessing Data and Storage Security Hygiene.

1. In the Azure portal tab, click **Security Center**.

    ![Security Center in the Azure Portal](Linked_Image_Files/M08-E01-T01-img01.png)

2. In the Security Center - Overview screen, click **Recommendations**.

    ![Security Center - Data Storage in the Azure Portal](Linked_Image_Files/M08-E01-T01-img02.png)

3. Identify the top two key data and storage components that require attention.

    1. __Answers may vary_____
    1. __Answers may vary_____

> **Result**: After you completed this exercise, you have learned where you can look to identify any data and storage security weaknesses that is in your Azure subscription.

## Exercise 3: Securing Storage Accounts and Data Lake Storage
  
Estimated Time: 15 minutes

Individual exercise
  
The main tasks for this exercise are as follows:

1. Determining the appropriate security approach for Azure Blob

2. Discuss the findings with the Instructor

### Task 1: Determining the appropriate security approach for Azure Blob

1. You have been approached by your in-house web developer to help give access to a third party web design company to the web images that are in the awsastudxx storage account. As a senior data engineer within AdventureWorks, what steps would you need to take to ensure this can happen while applying the correct due diligence.

2. From the lab virtual machine, start **Microsoft Word**, and open up the file **DP-200-Lab08-Ex03.docx** from the **C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\Starter\DP-200.8** folder.

