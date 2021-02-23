# DP 200 - Implementing a Data Platform Solution
# Lab 5 - Performing Real-Time Analytics with Stream Analytics

**Estimated Time**: 60 minutes

**Pre-requisites**: It is assumed that the case study for this lab has already been read.

**Lab files**: The files for this lab are located in the `C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\Starter` folder inside the virtual machine..

## Lab overview

The students will be able to describe what data streams are and how event processing works and choose an appropriate data stream ingestion technology for the AdventureWorks case study. They will provision the chosen ingestion technology and integrate this with Stream Analytics to create a solution that works with streaming data.

## Lab objectives
  
After completing this lab, you will be able to:

1. Explain data streams and event processing
2. Ingest data with Event Hubs
3. Initiate a data generation application
4. Process Data with a Stream Analytics Jobs

## Scenario
  
As part of the digital transformation project, you have been tasked by the CIO to help the customer services departments identify fraudulent calls. Over the last few years the customer services departments have observed an increase in calls from fraudulent customer who are asking for support for bikes that are no longer in warranty, or bikes that have not even been purchased at AdventureWorks. 

The department are currently relying on the experience of customer services agents to identify this. As a result, they would like to implement a system that can help the agents track in real-time who could be making a fradulent claim.

At the end of this lad, you will have:

1. Explained data streams and event processing
2. Ingested data with Event Hubs
3. Initiated a data generation application
4. Processed Data with Stream Analytics Jobs

> **IMPORTANT**: As you go through this lab, make a note of any issue(s) that you have encountered in any provisioning or configuration tasks and log it in the table in the document located at _C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\DP-200-Issues-Doc.docx_. Document the Lab number, note the technology, Describe the issue, and what was the resolution. Save this document as you will refer back to it in a later module.

## Exercise 1: Explain data streams and event processing

Estimated Time: 15 minutes

Group exercise
  
The main task for this exercise are as follows:

1. From the case study and the scenario, identify the data stream ingestion technology for AdventureWorks, and the high-level tasks that you will conduct as a data engineer to complete the social media analysis requirements.


### Task 1: Identify the data requirements and structures of AdventureWorks.

1. Before starting just to familiarize, we will be using deployment id for naming convention where xxxxxx will be asked to replace with deployment id and it can be found from the environment details tab from the right side of your environment.

2. From the lab virtual machine, start **Microsoft Word**, and open up the file **DP-200-Lab06-Ex01.docx** from the **C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\Starter\DP-200.6** folder.

3. As a group, spend **10 minutes** discussing and listing the data requirements and data structure that your group has identified within the case study document.

## Exercise 2: Data Ingestion with Event Hubs.
  
Estimated Time: 15 minutes

Individual exercise
  
The main tasks for this exercise are as follows:

1. Create and configure an Event Hub Namespace.

2. Create and configure an Event Hub.

3. Configure Event Hub security.

### Task 1: Create and configure an Event Hub Namespace.

1. In the Azure portal, click on the **Home** hyperlink at the top left of the screen.

2. In the Azure portal, click on the **+ Create a resource** icon , type **Event Hubs**, and then select **Event Hubs** from the resulting search. In the Event Hubs screen, click **Create**.

3. In the Create Namespace blade, type out the following options:
    - **Subscription**: **Your subscription**
    - **Resource group**: use the existing resource group **awrgstud-DeploymentID**
    - **Namespace Name**: **phoneanalysis-ehn-xxxxxx**, where xxxxxx is the deploymentID
    - **Location**: select the location closest to you
    - **Pricing Tier**: **Standard**    
    - **Throughput Units**: **20**
    - Leave other options to their default settings

        ![Creating an Event Hub Namespace in Azure portal](Linked_Image_Files/M06-E02-T01-img01.png)

4. Then click **Review + Create** and then click **Create**

    > **Note**: The creation of the Event Hub Namespace takes approximately 1 minute.
   
### Task 2: Create and configure an Event Hub

1. In the Azure portal, click on the **Home** hyperlink at the top left of the screen.

1. In the Azure portal, in the blade, click **Resource groups**, and then click **awrgstud-DeploymentID**

1. Click on **phoneanalysis-ehn-xxxxxx**, where **xxxxxx** is the deploymentID.

1. In the **phoneanalysis-ehn-xxxxxx** screen, click on **+ Event Hubs**.

1. Provide the name **phoneanalysis-eh-xxxxxx**, where xxxxxx is the deploymentID, leave the other settings to thier default values and then select **Create**.

    ![Creating an Event Hub in Azure portal](Linked_Image_Files/M06-E02-T02-img01.png)

    > **Note**: You will receive a message stating that the Event Hub is created after about 10 seconds

### Task 3: Configure Event Hub security

1. In the Azure portal, in the **phoneanalysis-ehn-xxxxxx**, where xxxxxx is the deploymentID. Scroll to the bottom of the window, and click on **phoneanalysis-eh-xxxxxx** event hub.

2. To grant access to the event hub, in the blade under the section **settings** on the left click **Shared access policies**.

1. Under the **phoneanalysis-eh-xxxxxx - Shared access policies** screen, create a policy with **Manage** permissions by selecting **+ Add**. Give the policy the name of **phoneanalysis-eh-sap-xxxxxx**, where xxxxxx is the deploymentID , check **Manage**, and then click **Create**.

    ![Creating a Shared Access Policy for Event Hubs in Azure portal](Linked_Image_Files/M06-E02-T03-img01.png)

1. Click on your new policy **phoneanalysis-eh-sap-xxxxxx** after it has been created, and then select the copy button for the **CONNECTION STRING - PRIMARY KEY** and paste the CONNECTION STRING - PRIMARY KEY  into Notepad, this is needed later in the exercise.

    >**NOTE**: The connection string looks as follows:
    > ```CMD
    >Endpoint=sb://<Your event hub namespace>.servicebus.windows.net/;SharedAccessKeyName=<Your shared access policy name>;SharedAccessKey=<generated key>;EntityPath=<Your event hub name>
    >```
    > Notice that the connection string contains multiple key-value pairs separated with semicolons: Endpoint, SharedAccessKeyName, SharedAccessKey, and EntityPath.

5. Close down the Event hub screens in the portal

> **Result**: After you completed this exercise, you have created an Azure Event Hub within an Event Hub Namespace and set the security for the Event Hub that can be used to provide access to the service.

## Exercise 3: Starting the telecom event generator application

Estimated Time: 15 minutes

Individual exercise

The main tasks for this exercise are as follows:

1. Updates the application connection string

2. Run the application

### Task 1: Updates the application connection string.

1. Browse to the location **C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\Starter\DP-200.6\DataGenerator**

2. Open the **telcodatagen.exe.config** file in a text editor of your choice

3. Update the <appSettings> element in the config file with the following details:

    - Set the value of the **EventHubName** key to the value of the **EntityPath** in the connection string.
    - Set the value of the **Microsoft.ServiceBus.ConnectionString** key to the connection string **without the EntityPath value** (don't forget to remove the semicolon that precedes it).

4. Save the file.

### Task 2: Run the application.

1. Click on **Start**, and type **CMD** 

2. Right click **Command Prompt**, click **Run as Administer**, and in the User Access Control screen, click **Yes**

1. In Command Prompt, browse to the location **cd C:\AllFiles\DP-200-Implementing-an-Azure-Data-Solution-master\Labfiles\Starter\DP-200.6\DataGenerator**

4. Type in the following command: 

    ```CMD
    telcodatagen.exe 1000 0.2 2
    ```

    > NOTE: This command takes the following parameters:
Number of call data records per hour.
Percentage of fraud probability, which is how often the app should simulate a fraudulent call. The value 0.2 means that about 20% of the call records will look fraudulent.
Duration in hours, which is the number of hours that the app should run. You can also stop the app at any time by ending the process (Ctrl+C) at the command line.

    ![Running the Data Generator Application in Command Prompt](Linked_Image_Files/M06-E03-T02-img01.png)

After a few seconds, the app starts displaying phone call records on the screen as it sends them to the event hub. The phone call data contains the following fields:

|Record | Definition |
|-|-|
|CallrecTime |The timestamp for the call start time.|
|SwitchNum |The telephone switch used to connect the call. For this example, the switches are strings that represent the country/region of origin (US, China, UK, Germany, or Australia).|
|CallingNum |The phone number of the caller.|
|CallingIMSI |The International Mobile Subscriber Identity (IMSI). It's a unique identifier of the caller.|
|CalledNum | The phone number of the call recipient.|
|CalledIMSI| International Mobile Subscriber Identity (IMSI). It's a unique identifier of the call recipient.|

5. Minimize the command prompt window. 

> **Result**: After you completed this exercise, you have conmfigured an application to generate data to minimic phone calls recieved by a call center.

## Exercise 4: Processing Data with Stream Analytics Jobs

Estimated Time: 15 minutes

Individual exercise

The main tasks for this exercise are as follows:

1. Provision a Stream Analytics job.

2. Specify the a Stream Analytics job input.

3. Specify the a Stream Analytics job output.

4. Defining a Stream Analytics query.

5. Start the Stream Analytics job.

6. Validate streaming data is collected

### Task 1: Provision a Stream Analytics job.

1. Go back to the Azure portal, navigate and click on the **+ Create a resource** icon, type **STREAM**, and then click the **Stream Analytics Job**, and then click **Create**.

2. In the **New Stream Analytics job** screen, fill out the following details and then click on **Create**:
    - **Job name**: phoneanalysis-asa-job.
    - **Subscription**: select your subscription
    - **Resource group**: awrgstud-DeploymentID
    - **Location**: choose a location nearest to you.
    - Leave other options to their default settings

        ![Create a Stream Analytics Job in the Azure Portal](Linked_Image_Files/M06-E04-T01-img01.png)

    > **Note**: You will receive a message stating that the Stream Analytics job is created after about 10 seconds. It may take a couple of minutes to update in the Azure portal.

### Task 2: Specify the a Stream Analytics job input.

1. In the Azure portal, in the blade, click **Resource groups**, and then click **awrgstud-DeploymentID**.

2. Click on **phoneanalysis-asa-job**.

3. In your **phoneanalysis-asa-job** Stream Analytics job window, in the left hand blade, under **Job topology**, click **Inputs**.

4. In the **Inputs** screen, click **+ Add stream input**, and then click **Event Hubs**.

5. In the Event Hub screen, type in the following values and click the **Save** button.
    - **Input alias**: Enter a name for this job input as **PhoneStream**.
    - **Select Event Hub from your subscriptions**: checked
    - **Subscription**: Your subscription name
    - **Event Hub Namespace**: phoneanalysis-ehn-xxxxxx
    - **Event Hub Name**: Use existing named phoneanalysis-eh-xxxxxx
    - **Event Hub Consumer Group**: Use existing
    - **Authentication Method**: Connection string
    - **Event Hub Policy Name**: phoneanalysis-eh-sap-xxxxxx
    - Leave the rest of the entries as default values. Finally, click **Save***.

        ![Create a Job Input Stream Analytics Job in the Azure Portal](Linked_Image_Files/M06-E04-T02-img01.png)

6. Once completed, the **PhoneStream** Input job will appear under the input window. Close the input widow to return to the Resource Group Page

### Task 3: Specify the a Stream Analytics job output.

1. Click on **phoneanalysis-asa-job**.

2. In your **phoneanalysis-asa-job** Stream Analytics job window, in the left hand blade, under **Job topology**, click **Outputs**.

3. In the **Outputs** screen, click **+ Add**, and then click **Blob storage/ADLS Gen2**.

4. In the **Blob storage/ADLS Gen2** window, type or select the following values in the pane:
    - **Output alias**: **PhoneCallRefData**
    - **Select Event Hub from your subscriptions**: checked
    - **Subscription**: Your subscription name
    - **Storage account**:  Select **:awsastudxxxxxx**:, where xxxxxx is the deploymentID
    - **Container**: **Use existing** and select **phonecalls**
    - Leave the rest of the entries as default values. Finally, click **Save**.

        ![Create a Job Output in Stream Analytics Job in the Azure Portal](Linked_Image_Files/M06-E04-T03-img01.png)

5. Close the output screen to return to the Resource Group page

### Task 4: Defining a Stream Analytics query.

1. Click on **phoneanalysis-asa-job**.

2. In your **phoneanalysis-asa-job** window, in the **Query** screen in the middle of the window, click on **Edit query**

3. Replace the following query in the code editor:

    ```SQL
    SELECT
        *
    INTO
        [YourOutputAlias]
    FROM
        [YourInputAlias]
    ```

4. Replace with

    ```SQL
    SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
    INTO "PhoneCallRefData"
    FROM "PhoneStream" CS1 TIMESTAMP BY CallRecTime
    JOIN "PhoneStream" CS2 TIMESTAMP BY CallRecTime
    ON CS1.CallingIMSI = CS2.CallingIMSI
    AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    WHERE CS1.SwitchNum != CS2.SwitchNum
    GROUP BY TumblingWindow(Duration(second, 1))
    ```

    > NOTE: This query performs a self-join on a 5-second interval of call data. To check for fraudulent calls, you can self-join the streaming data based on the CallRecTime value. You can then look for call records where the CallingIMSI value (the originating number) is the same, but the SwitchNum value (country/region of origin) is different. When you use a JOIN operation with streaming data, the join must provide some limits on how far the matching rows can be separated in time. Because the streaming data is endless, the time bounds for the relationship are specified within the ON clause of the join using the DATEDIFF function.
    This query is just like a normal SQL join except for the DATEDIFF function. The DATEDIFF function used in this query is specific to Stream Analytics, and it must appear within the ON...BETWEEN clause.

    ![Create a Query in Stream Analytics Job in the Azure Portal](Linked_Image_Files/M06-E04-T04-img01.png)

5. Select **Save Query**.

6. Close the Query window to return to the Stream Analytics job page.


### Task 5: Start the Stream Analytics job

1. In your **phoneanalysis-asa-job** window, in the **Query** screen in the middle of the window, click on **Start**
 
2. In the **Start Job** dialog box that opens, click **Now**, and then click **Start**. 

>**Note**: In your **phoneanalysis-asa-job** window, a message appears after a minute that the job has started, and the started field changes to the time started

>**Note**: Leave this running for 2 minutes so that data can be captured.

### Task 6: Validate streaming data is collected

1. In the Azure portal, in the blade, click **Resource groups**, and then click **awrgstudxxxxxx**, and then click on **awsastudxxxxxx**, where **xxxxxx** is the deploymentID.

2. In the Azure portal, click **Containers** box, and then click on the container named **phonecalls**.

3. Confirm that a JSON file appears, and note the size column.

    ![Viewing Files in the Azure Portal](Linked_Image_Files/M06-E04-T06-img01.png)

4. Refresh Microsoft Edge, and when the screen has refreshed note the size of the file

    >**Note**: You could download the file to query the JSON data, you could also output the data to Power BI.

> **Result**: After you completed this exercise, you have configured Azure Stream Analytics to collect streaming data into an JSON file store in Azure Blob. You have done this with streaming phone call data.

## Close down

1. In the Azure portal, in the blade, click **Resource groups**, and then click **awrgstud-DeploymentID**, and then click on **phoneanalysis-asa-job**.

2. In the **phoneanalysis-asa-job** screen, click on **Stop**. In the **Stop Streaming job** dialog box, click on **Yes**.

3. Close down the Command Prompt application.
