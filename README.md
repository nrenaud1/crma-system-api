# Extracting Data from CRMA with MuleSoft : Part 1

Have you ever wanted to export data from CRMA and navigated the APIs only to find out that it's not possible? This post shows you how to get the data out using MuleSoft!


### Considerations

This solution extracts the data from CRMA by wrapping the process with a MuleSoft API. This process can take a bit of time depending on the dataset, so executing that Recipe Sync over and over may not be feasible, data is only valid up to the last execution. However this is good to get a point in time extraction for your dataset.

Additionally exports that have been completed are not permanently available. These are cleaned periodically by Salesforce.

### Initial Configuration in CRMA

To facilitate this, there are manual steps which require setting up a recipe/dataflow to extract the dataset from CRMA to SF. An example of how to do that is located in the link below, once you get to “Where is your data now?” you've completed the setup. https://www.salesforceblogger.com/2020/08/19/export-your-einstein-analytics-datasets/

![image](https://user-images.githubusercontent.com/85581932/199068905-a08e201a-8bda-429c-901d-0a7a04537d8f.png)

### Flow Steps

In the screenshot below, call the API endpoints in the following order. You'll need to pass specific Ids from each call to each subsequent call to get the data you need.

![image](https://user-images.githubusercontent.com/85581932/199068996-0f327338-0d79-4745-8d59-4ce5ffdfc3b8.png)

1. MuleSoft can query the recipes to find the appropriate targetDataflowId, if you already have the targetDataflowId, this step can be skipped.

![image](https://user-images.githubusercontent.com/85581932/199069054-a9937374-193b-44c1-8973-1aeb4ad3a44e.png)

2. MuleSoft will start the recipe, and receive the executionPlanId which is the specific Id needed to specify this execution of the job.

![image](https://user-images.githubusercontent.com/85581932/199069278-4379e9c9-e971-4f6a-9fc3-4242e7d38894.png)

3. MuleSoft will query using the executionPlanId until the status is “Success”.

![image](https://user-images.githubusercontent.com/85581932/199069326-64f8094e-c931-476d-b499-cd796acbc72e.png)

4. MuleSoft will then get the array of data files created in the data export using the executionPlanId, and will return an array of all the file parts.

![image](https://user-images.githubusercontent.com/85581932/199069381-f3055aeb-9e39-4fd3-b190-6491e9edad04.png)

5. MuleSoft can then pull each data file individually and return the dataset in JSON for use in other processes, or to import into a database.

![image](https://user-images.githubusercontent.com/85581932/199069410-d2ac3c72-4613-4e8d-978e-b0f541df860a.png)

### Summary

As you can see, instead of leveraging a command line tool or calling SOQL from the Workbench, you can easily wrap the process with an API. In the next part, we'll show you how to setup a single flow in Anypoint Studio to push the data to a Database or CSV file.
