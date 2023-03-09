# Welcome to the Retool workshop!
This workshop will guide you through building two Retool apps that integrate with AWS services. It will show you how Retool can help you create powerful, user-friendly interfaces for managing your AWS resources.

## What is Retool

[Retool](https://retool.com/) is a development platform that lets you build and deploy internal applications quickly with a drag-and-drop interface. It includes all the dev tools and runtime components you need to create an application, including a visual IDE and [UI component library](https://retool.com/components) to build front ends, as well as backend services to handle, with [native connectors for most databases and APIs](https://retool.com/integrations/). 

So instead of building a custom application from scratch using a mix of different tools, frameworks, and infrastructure, Retool provides a complete development stack where all the pieces are designed to work together.

## What you will build

### Module 1: Custom AWS Billing Dashboard

In the first module, you will build an AWS Billing Dashboard using Retool and [AWS Cost Explorer API](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-api.html). This dashboard will provide you with a quick overview of costs incurred for a given time range, grouped by service and time period. You will learn how to use Retool to connect to AWS Cost Explorer API, retrieve data, and create tab views, charts, and tables to visualize and display your AWS billing data.

#### Why build a custom AWS Billing Dashboard

- **Simple cost overview:** Provides an easy-to-understand overview of costs incurred for a given time range, grouped by service and time period, allowing business leaders and non-technical users to quickly view costs without having to navigate the complex AWS console
- **Intuitive data visualization:** Offers an intuitive and user-friendly interface for visualizing and understanding AWS costs and usage patterns, making it easier for non-technical users to identify areas where costs can be optimized.

![alt_text](images/image7.png "image_tooltip")

### Module 2: Custom Amazon EC2 Instance Admin Panel

In the second module, you will build an Amazon EC2 Instance Manager using Retool and [Amazon EC2 API](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/making-api-requests.html). This app will allow you to launch new EC2 instances from a list of available Amazon images and view and manage a list of EC2 instances with start and stop buttons for each instance. You will learn how to use Retool to connect to Amazon EC2 API, retrieve data, and create forms, tables, and buttons to manage your EC2 instances.

![alt_text](images/image15.png "image_tooltip")

#### Why build a custom Amazon EC2 Instance Manager

- **Simplified image selection:** Allows users to select a limited list of Amazon images to choose from, providing more control over the virtual servers to launch and avoiding confusion in a team environment
- **Quick instance launching:** Users can quickly launch new instances with just a few clicks, as opposed to manually creating a new instance in the AWS console
- **Easy instance management:** Users can easily view and manage a list of EC2 instances with start and stop buttons for each instance, providing a more efficient and user-friendly experience compared to using the AWS console

Throughout the workshop, you will learn how to use Retool's intuitive drag-and-drop interface to create custom applications that are tailored to your specific needs. By the end of this workshop, you will have two powerful Retool apps that will help you better manage your AWS resources and optimize costs.


## Prerequisite

You will need AWS client ID and secret of an AWS user that has permissions to access Amazon EC2 AP, and AWS Cost Explorer API.

This guide **includes a burner client id and secret** you can use for the purposes of this workshop. Please note that **these will be destroyed after the workshop** and you would not be able to use them. 

Alternatively, if you would like to use your own AWS account so you can explore your own data through these apps, please see the section at the end of this guide for step-by-step instructions on **Creating and attaching EC2 and Cost Explorer Permissions to an IAM user**.

Let’s get started!


---


# Module 1: Creating an AWS Billing Dashboard


## Overview

In this module, you will learn how to use Retool and [AWS Cost Explorer API](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-api.html) to build an AWS Billing Dashboard. The dashboard will provide a quick overview of costs incurred for a given time range, grouped by Service and Time Period. You will create three tab views: "All," "By Service," and "By Time Period." Here are some screenshots of what we will be building - 

![alt_text](images/image10.png "image_tooltip")


_The main screen shows the costs for all services during the chosen date range. Anything more than $20 is highlighted in red._

![View costs by Service](images/image21.png "image_tooltip")

_View costs by Service_

![alt_text](images/image7.png "image_tooltip")


_View costs by Time Period_


### What you will learn

1. How to use Retool to connect to [AWS Cost Explorer API](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-api.html)
2. How to retrieve and display data retrieved from AWS Cost Explorer API using [Retool’s component library](https://retool.com/components)
3. How to create tab views using Retool’s <code>[Tabbed Container](https://retool.com/components/tabbed-container)</code> component
4. How to use [Listbox](https://retool.com/components/listbox) component as an input for a query.
5. How to use Chart component to visualize the AWS billing data
6. How to use a [Table](https://retool.com/components/table) component to display data
7. How to use a [Date Range](https://retool.com/components/date-range) component to allow users to select a time range


## Part 1: Create a Resource in Retool for AWS Cost Explorer API (20 Mins)



1. Get your AWS Credentials following the instructions here. See section “Setting up the AWS Credentials”
2. Signup for a free Retool account
3. Create a new Resource for AWS Cost Explorer API in Retool using your AWS credentials
    1. Login to Retool, and then click on the Resources tab in the top menu bar. 
    2. Click Create new, and then choose Resource


![alt_text](images/image17.png "image_tooltip")




4. On the “Select a resource type” page, choose REST API

![alt_text](images/image20.png "image_tooltip")


5. Configure the REST API using the configuration below -

| Property              | Value                                           |
|-----------------------|-------------------------------------------------|
| Name                  | `AWS Cost Explorer API`                         |
| Base URL              | `https://ce.us-east-1.amazonaws.com`            |
| Authentication        | `AWS v4`                                        |
| AWS Region            | `us-east-1`                                     |
| AWS Access Key ID     | `<YOUR AWS Access Key ID>`                      |
| AWS Secret Access Key | `<YOUR AWS Secret Access Key>`                  |

> 📘 **NOTE:** If you do not have access to an AWS account, or are unable to create a user with the required permissions,  for the purposes of this work, use the following burner account credentials. Please note that these will be destroyed after the workshop, and you would not be able to use them:
>
> - AWS Access Key ID: `AKIA4GSR46W2D5HIPG63`
> - AWS Secret Key ID: `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`

6. Click **Create resource**
![alt_text](images/image26.png "image_tooltip")

7. The Resource will be created, and you will be prompted to “Create an app”
8. Click <strong>Create an app</strong>, and give it a name - example <code>AWS Cost Explorer App</code>


![alt_text](images/image29.png "image_tooltip")

We are done creating the Resource. Let's now connect to it by creating an app.

## Part 2: Create the Retool app using this Resource (45 mins)


### Create a query to get AWS billing data

[Queries](https://docs.retool.com/docs/queries) are the way we pull in data (read or write data) into a Retool app, so we can use them in components. In this case, we'll create a query that gets the costs from the AWS Cost Explorer API, grouped by month and service.

Here’s what the [App editor](https://docs.retool.com/docs/app-editor) for a new Retool app looks like. 

![alt_text](images/image32.png "image_tooltip")

1. With your new Retool app open, on the bottom panel, click the **+** button to create a new Resource Query, 
2. Rename it to `getCostByServiceAndTimePeriod` (right click the query name in the left side bar)

![alt_text](images/image1.png "image_tooltip")


2. Configure the Resource query as below -

| Property | Value |
|----------|-------|
| Resource | `AWS Cost Explorer API` (this is the resource you created earlier)|
| Action type | `POST` |
| Headers | `Content-Type` : `application/x-amz-json-1.1`<br>`X-Amz-Target` :  `AWSInsightsIndexService.GetCostAndUsage` |
| Body | `Raw` : See JSON below|

3. Paste this JSON in the `Body`

```json
{
  "TimePeriod": {
    "Start": "2022-08-01",
    "End": "2023-03-04"
  },
  "Granularity": "MONTHLY",
  "Metrics": ["BlendedCost"],
  "GroupBy": [
    {
      "Type": "DIMENSION",
      "Key": "SERVICE"
    }
  ]
}
```

![alt_text](images/image2.png "image_tooltip")


3. Set this query to run on page load 
    1. With `getCostByServiceAndTimePeriod` query selected, click on `Advanced` tab
    2. Check the box for `Run this query on page load?`
    3. **Save**

![alt_text](images/image14.png "image_tooltip")

4. Click **Preview** to get the confirmation that the “Query ran successfully”, and see the response and the API request in the Results panel at the bottom.

![alt_text](images/image6.png "image_tooltip")

> **Note:** You can also explore the data in the **State** tab on the left side bar.
> 
> ![alt_text](images/image28.png "image_tooltip")

### Write a JavaScript Transformer to restructure the data returned by the Cost Explorer API

As you can see, the data returned by AWS Cost Explorer API is quite nested, and can be difficult to navigate. To make it easier, we want to format it to return this simple structure:


```json
[
  {
    "TimePeriod": "07-2022",
    "Service": "AWS Amplify",
    "Amount": 0.0000110789
  },
  {
    "TimePeriod": "07-2022",
    "Service": "AWS Lambda",
    "Amount": 0
  },
  {
    "TimePeriod": "07-2022",
    "Service": "Amazon DynamoDB",
    "Amount": 17.4096
  },
  {
    "TimePeriod": "07-2022",
    "Service": "EC2 - Other",
    "Amount": 1.0000589595
  },
]
```

We will do that using [Transformers](https://docs.retool.com/docs/transformers), which let you manipulate data with JavaScript.

1. Click on the **+** button in the query window, and click **JavaScript transformer**
2. Rename it to `transformCostByServiceAndTimePeriodData`

![alt_text](images/image22.png "image_tooltip")

3. Paste the following code in this Transformer. 
4. Click **Save**

```javascript
// Get the cost data for each service by time period from the API response
const originalArray = {{ getCostByServiceAndTimePeriod.data.ResultsByTime}}

// Flatten the array of groups for each time period and create a new object for each service to make it easier to work with.

const newArray = originalArray.flatMap(obj => {
  // Extract the time period from the current object and format it as "MM-YYYY"
  const timePeriod = obj.TimePeriod;
  const startDate = new Date(timePeriod.Start);
  const monthYear = (startDate.getMonth() + 1).toString().padStart(2, '0') + '-' + startDate.getFullYear();

  // Extract the groups array from the current object
  const groups = obj.Groups;

  // Map over the groups array to create a new object for each service with the TimePeriod, Service, and Amount properties
  return groups.map(group => {
    // Extract the service name and cost amount from the current group
    const serviceName = group.Keys[0];
    const costAmount = parseFloat(group.Metrics.BlendedCost.Amount);

    // Create a new object with the TimePeriod, Service, and Amount properties
    return {
      TimePeriod: monthYear,
      Service: serviceName,
      Amount: costAmount
    };
  });
});

// Log the resulting array and return it
console.log(newArray);
return newArray;
```

![alt_text](images/image19.png "image_tooltip")


### Use the Date Range component to allow selection of  start and end dates


1. Drag the **Date Range** component to the canvas

> **Note:** Amazon only provides data for the last 12 months. So, we will set the default `Start date` for Date Range component to (today -12 months), and the default `End date` to today’s date. Set the properties as below -

| Property           | Value                                                                                                          |
|--------------------|----------------------------------------------------------------------------------------------------------------|
| Name               | billingDateRange                                                                                               |
| Format             | `yyyy-MM-dd`                                                                                                   |
| Start date default | `{{moment().subtract(12, 'months').format('YYYY-MM-DD')}}` (Amazon only provides data for the last 12 months)                                                     |
| End date default   | `{{moment().format('YYYY-MM-DD')}}`                                                                             |



![alt_text](images/image23.png "image_tooltip")




2. Add an `on change` event handler for `billingDateRange` component to trigger the `getCostByServiceAndTimePeriod`. This will ensure that the Cost Explorer API is called as soon as the date is changed using the date range component.


![alt_text](images/image5.png "image_tooltip")




3. Update the `getCostByServiceAndTimePeriod` query to reference the start and end dates selected in our date range component, by pasting the following JSON as the Body

```json
{
  "TimePeriod": {
    "Start": {{billingDateRange.value.start}},
    "End": {{billingDateRange.value.end}}
  },
  "Granularity": "MONTHLY",
  "Metrics": ["BlendedCost"],
  "GroupBy": [
    {
      "Type": "DIMENSION",
      "Key": "SERVICE"
    }
  ]
}
```


### Use a Tabbed Container component to set up the three views 

1. Drag a **Tabbed Container** component to the canvas  
    1. Name it `billingTabbedContainer`
    2. Add three views to this container using the Inspector in the right sidebar. 
        1. Rename the first view of the tabbed container to `All`
        2. Rename the second view of the tabbed container to `By Service`
        3. Rename the third view of the tabbed container to `By Time Period`



![alt_text](images/image11.png "image_tooltip")



### Populate the Table with the costs

1. Drag a Table component inside the **first view** of the tabbed container. 
2. Set its properties on the right panel to -

| Property             | Value                                          |
|----------------------|------------------------------------------------|
| Name                 | `mainBillingTable`                             |
| Data                 | `transformCostByServiceAndTimePeriodData.value` |
| Sort column          | `TimePeriod`                                   |
| Sort direction       | `Asc`                                          |

You should now see the data in the table.

TODO: Add Screenshot of the table with data

## Designing the second tab (By Service View):


### Add a Listbox to show the list of services to choose from

1. Create a Transformer to to populate the ListBox for Services
    1. Name it `getListofServices` 
    2. Paste the following code in this Transformer. 
    3. Click **Save**
```javascript
// Get the API response data
const data = {{getCostByServiceAndTimePeriod.data}}

// Create a new Set to store unique service names
const serviceSet = new Set();

// Loop through the ResultsByTime array and extract the service names from each group
data.ResultsByTime.forEach((result) => {
  result.Groups.forEach((group) => {
    serviceSet.add(group.Keys[0]);
  });
});

// Convert the Set to an array to make it easier to work with
const serviceList = [...serviceSet];

// In Unicode, uppercase letters have lower numeric values than lowercase letters. This means that when sorting an array of strings that start with uppercase letters and lowercase letters, the strings starting with uppercase letters will be placed before the strings starting with lowercase letters.
// By using localeCompare() with no arguments, we sort the strings in alphabetical order while ignoring differences in case.

serviceList.sort((a, b) => {
  return a.localeCompare(b);
});

// Log the unique service names to the console and return the sorted list
console.log(serviceList);
return serviceList;
```

2. Drag a **Listbox** component inside the second tab of the tabbed container
    1. Name it `servicesListBox`
    2. Data source: `getListofServices`



### Display the total cost for the selected service

1. Create a Transformer to filter the results returned by the Cost Explorer API, and calculate the total amount spent on the selected service.
    1. Name it `filterCostByService`
    2. Paste the following code in this Transformer.
    3. Click **Save**

```javascript
const newArray = {{transformCostByServiceAndTimePeriodData.value}};
const serviceName = {{servicesListBox.value}};

const filteredArray = newArray.filter(item => item.Service === serviceName);
const totalAmount = filteredArray.reduce((accumulator, item) => accumulator + item.Amount, 0).toFixed(2);

const resultObject = {
  serviceName,
  totalAmount,
  data: filteredArray
};

console.log(resultObject);
return resultObject;
```

2. We will now create a **Temporary State** variable to store the date range for easy reference in multiple UI components. 

  1. Name it `currentDateRange`
  2. Paste the following code 

```javascript
{{billingDateRange.value.start}} to {{billingDateRange.value.end}}
```
![alt_text](images/image18.png "image_tooltip")

3. Drag a **Statistic** component inside the second tab of the tabbed container, and set the properties in the right panel inspector to - 

| Property       | Value                                       |
| -------------- | ------------------------------------------- |
| Rename it to   | `serviceCostStatistic`                      |
| Label          | `Cost for {{servicesListBox.value}}`        |
| Primary value  | `{{filterCostByService.value.totalAmount}}` |
| Caption        | `{{currentDateRange.value}}`                |
| Positive trend | `{{ self.value &lt;= 20 }}`                 |

4. Drag a **Chart** component inside the second tab of the tabbed container, and set the properties in the right panel inspector to - 
    1. Name it `serviceCostChart`
    2. Data source: `{{filterCostByService.value.data}}`
    3. Chart type: `Line chart`
    4. X-axis values: `TimePeriod`
    5. Datasets: Amount : `sum`
        1. Dataset name: `Amount`
        2. Dataset values: `{{formatDataAsObject(filterCostByService.value.data)['Amount']}}`
        3. Aggregation method: `Sum`
    14. Type: `Line chart`
    15. Color: `#E9AB11`
    6. Title: `Costs for {{servicesListBox.value}}`
    7. X-axis title: `Time Period`
    8. Y-axis title: `USD`
    9. Y-axis tick format: `$`
5. Drag a **Table** component inside the second tab of the tabbed container, and set the properties in the right panel inspector to - 

| Property                        | Value                                |
| ------------------------------- | ------------------------------------ |
| Name                            | `servicesTable`                      |
| Data                            | `{{filterCostByService.value.data}}` |
| Column type (for Amount column) | `Currency`                           |
| Sorting column                  | `TimePeriod`                         |
| Sort direction                  | `Asc`                                |

6. Finally, hide the Service column by clicking on the "eye" icon for the column in the right panel.


## Designing the third tab (By Time Period View)


### Add a Listbox to show the list of time periods to choose from

1. Create a new Transformer to populate the ListBox for Time Period
    1. Name it `getListofTimePeriods` 
    2. Paste the following code in this Transformer. 
    3. Click **Save**

```javascript
const data = {{getCostByServiceAndTimePeriod.data}};

const timePeriodSet = new Set();
data.ResultsByTime.forEach(result => {
  const startDate = new Date(result.TimePeriod.Start);
  const monthYear = (startDate.getMonth() + 1).toString().padStart(2, '0') + '-' + startDate.getFullYear();
  timePeriodSet.add(monthYear);
});

const timePeriodList = [...timePeriodSet];

console.log(timePeriodList);
return timePeriodList;

```
### Display the total cost for the selected time period



1. Create a Transformer to filter the results returned by the Cost Explorer API, and calculate the total amount spent in each time period.
    1. Name it `filterCostByTimePeriod`
    2. Paste the following code in this Transformer.
    3. Click **Save**

```javascript
const newArray = {{transformCostByServiceAndTimePeriodData.value}};
const serviceName = {{servicesListBox.value}};

const filteredArray = newArray.filter(item => item.Service === serviceName);
const totalAmount = filteredArray.reduce((accumulator, item) => accumulator + item.Amount, 0).toFixed(2);

const resultObject = {
  serviceName,
  totalAmount,
  data: filteredArray
};

console.log(resultObject);
return resultObject;
```

2. Drag a **Listbox** component inside the second tab of the tabbed container, and set the properties in the right panel inspector to - 
    | Property    | Value                  |
    | ----------- | ---------------------- |
    | Name        | `timePeriodListBox`    |
    | Data source | `getListofTimePeriods` |
    
3. Drag a **Statistic** component inside the second tab of the tabbed container, and set the properties in the right panel inspector to - 
    | Property       | Value                                          |
    | -------------- | ---------------------------------------------- |
    | Name           | `timePeriodCostStatistic`                      |
    | Label          | `Total Cost for {{timePeriodListBox.value}}`   |
    | Primary value  | `{{filterCostByTimePeriod.value.totalAmount}}` |
    | Caption        | `{{currentDateRange.value}}`                   |
    | Positive trend | `{{ self.value &lt;= 20 }}`                    |
    
4. Drag a **Chart** component inside the second tab of the tabbed container, and set the properties in the right panel inspector to - 
    1. Name it `timePeriodCostChart`
    2. Data source: `{{filterCostByTimePeriod.value.data}}`
    3. Chart type: `Bar chart`
    4. X-axis values: `Service`
    5. Group by: `Service`
    6. Datasets: `Amount : sum`
        1. Dataset name: `Amount`
        2. Dataset values: `{{formatDataAsObject(filterCostByTimePeriod.value.data)['Amount']}}`
        3. Aggregation method: `Sum`
        4. Type: `Bar chart`
    7. Title: `Cost by Service for {{timePeriodListBox.value}}`
    8. X-axis title: `AWS Service`
    9. Y-axis title: `USD`
    10. Y-axis tick format: `$`

​	

2. Drag a **Table** component inside the second tab of the tabbed container, and set the properties in the right panel inspector to - 
    | Property                        | Value                                   |
    | ------------------------------- | --------------------------------------- |
    | Rename it to                    | `timePeriodTable`                       |
    | Data                            | `{{filterCostByTimePeriod.value.data}}` |
    | Column type (for Amount column) | `Currency`                              |
    | Sorting column                  | `Amount`                                |
    | Sort direction                  | `Desc`                                  |

3. Preview the app by clicking on the **Preview** button

# Module 2: Building the Amazon EC2 Management Admin Panel (45 mins)


## Overview

In this module, you will learn how to use Retool and Amazon EC2 API to build an EC2 Instance Manager. This custom admin panel will allow you to launch a new EC2 instance by choosing an Amazon image (using a preselected list of virtual server images available in AWS Free tier), list all EC2 instances in a table, along with start and stop buttons for each instance.


![alt_text](images/image15.png "image_tooltip")

## What you will learn:

1. How to use Retool to connect to [Amazon EC2 API](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/making-api-requests.html)
2. How to retrieve data from Amazon EC2 API to display using [Retool’s component library](https://retool.com/components)
3. How to use [Listbox](https://retool.com/components/listbox) component in Retool as an input for a query to launch an EC2 instance
4. How to use [Button](https://retool.com/components/button) component to trigger a query, which will launch a new EC2 Instance
5. How to use [Table](https://retool.com/components/table) component to display a list of EC2 instances
6. How to add action buttons for each table row to trigger an EC2 API call to start or stop an instance
7. How to cache the results of a Resource Query


## Part 1: Create a Resource in Retool for Amazon EC2 API (20 Mins)

1. Get your AWS Credentials following the instructions here.
2. Create a new Resource for Amazon EC2 API in Retool using the AWS credentials
    | Property          | Value                                 |
    | ----------------- | ------------------------------------- |
    | Name              | `Amazon EC2 API`                      |
    | Base URL          | `https://ec2.us-east-1.amazonaws.com` |
    | Authentication    | `AWS v4`                              |
    | AWS Region        | `us-east-1`                           |
    | AWS Access Key ID | `YOUR AWS Access Key ID>`             |
    | AWS Secret Key ID | `YOUR AWS Secret Key ID>`             |



> NOTE: If you do not have access to an AWS account, or are unable to create a user with the required permissions, for the purposes of this work, use the following burner account credentials for this app. Please note that these will be destroyed after the workshop, and you would not be able to use them:
>
> > - **AWS Access Key ID**: `AKIARFLN3W5QNGVU4P7M`
> > - **AWS Secret Key ID**: `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`

![alt_text](images/image8.png "image_tooltip")

> - 

## Part 2: Create the Retool app using this Resource (45 mins)

### Writing Resource Queries to connect with the EC2 API 

1. Create a new Retool App
2. In the new Retool app, create a new Resource query to launch a new instance using EC2 API’s `RunInstances` action.
    | Property     | Value                                                        |
    | ------------ | ------------------------------------------------------------ |
    | Rename it to | `launchNewEC2Instance`                                       |
    | Action Type  | `POST`                                                       |
    | Headers      | `Content-Type` : `application/x-www-form-urlencoded; charset=utf-8` |
    | Body         | `x-www-form-urlencoded`                                      |
    | Body Params  |                                                              |
    | Action       | `RunInstances`                                               |
    | ImageId      | `ami-0dfcb1ef8550277af`                                      |
    | InstanceType | `t2.micro`                                                   |
    | MinCount     | `1`                                                          |
    | MaxCount     | `1`                                                          |
    | Version      | `2016-11-15`                                                 |
2. Create a new Resource query to list all instances using EC2 API’s `DescribeInstances` action.
    | Property    | Value                                                        |
    | ----------- | ------------------------------------------------------------ |
    | Name        | `getAllEC2Instances`                                         |
    | Action type | `GET`                                                        |
    | URL Params  | `?Action=DescribeInstances&Filter.1.Name=instance-type&Filter.1.Value.1=t2.micro&Filter.2.Name=architecture&Filter.2.Value.1=x86_64&Version=2016-11-15` |

> **Note:** In this request, we are using two filters to search for instances with specific attributes -
>1. **instance-type:** Instance type refers to an EC2 template that specifies the amount of resources (such as CPUs and memory) that a virtual machine will have. In this case, the value for the instance-type filter is "t2.micro", which are instances that have 1 vCPU and 1GB of memory.
> 2. **architecture:** Architecture refers to the type of processor used by the instance. In this case, the value for the architecture filter is "x86_64", which refers to a 64-bit processor architecture commonly used in personal computers.






![alt_text](images/image30.png "image_tooltip")



3. Click on **Run** to see the results of the API. You can also explore the data in the **State** tab in the left panel.

![alt_text](images/image4.png "image_tooltip")


4. **Enable caching of query results and run the query on page load**
    1. Click **Advanced** tab in `getAllEC2Instances`’s query editor
    2. Check the `Cache the results of this query` box. This will store the results of the query and prevent the need to make the request each time, improving performance and load speeds of the application.
    3. Check the `Run this query on page load?` box. This will ensure that our table of EC2 instances is automatically populated when the app opens, we will also check the box 

![alt_text](images/image27.png "image_tooltip")

5. **Transforming the data to make it easy to navigate:** The XML data returned by the EC2 API is deeply nested, making it difficult to extract the relevant information for each instance. So, we will write a bit of custom JavaScript code to “transform” the data into an easily readable array of objects using a JavaScript Transformer.

6. Add a new Transformer to format instance data for `getAllEC2Instances` Resource query
    1. Name it `transformInstanceData`
    2. Paste the following code - 

![alt_text](images/image25.png "image_tooltip")


```javascript
// Store data from the EC2 API in the variable data
const data = {{getAllEC2Instances.data.parsedXml.DescribeInstancesResponse.reservationSet['0'].item}};
      
// Initialize an empty array to hold final data
let finalData = [];

// Function to capitalize the first letter of a string
function capitalizeFirstLetter(string) {
  return string.charAt(0).toUpperCase() + string.slice(1);
}

// Loop through each key in the data object
Object.keys(data).forEach(key => {

  // Retrieve specific data points for each instance and assign to variables
  let architecture = data[key].instancesSet[0].item[0].architecture[0];
  let dnsName = data[key].instancesSet[0].item[0].dnsName[0];
  let instanceState = capitalizeFirstLetter(data[key].instancesSet[0].item[0].instanceState[0].name[0]);
  let imageId = data[key].instancesSet[0].item[0].imageId[0];
  let reservationId = data[key].reservationId[0];
  let ipAddress = typeof(data[key].instancesSet[0].item[0].ipAddress) !== 'undefined' ? data[key].instancesSet[0].item[0].ipAddress[0] : 'N/A';
  let instanceName = typeof(data[key].instancesSet[0].item[0].tagSet) !== 'undefined' ? data[key].instancesSet[0].item[0].tagSet[0].item[0].value[0] : 'N/A'; 
  let launchTime = data[key].instancesSet[0].item[0].launchTime[0];
  let instanceId = data[key].instancesSet[0].item[0].instanceId[0];
  let groupName = data[key].instancesSet[0].item[0].groupSet[0].item[0].groupName[0];
  let region = data[key].instancesSet[0].item[0].placement[0].availabilityZone[0];

  // Create a new object containing all relevant data points
  let newObj = {
    'architecture': architecture,
    'dnsName': dnsName,
    'instanceState': instanceState,
    'imageId': imageId,
    'reservationId': reservationId,
    'ipAddress': ipAddress,
    'instanceName': instanceName,
    'launchTime': launchTime,
    'instanceId': instanceId,
    'groupName': groupName,
    'region': region
  };

  // Add new object to the finalData array
  finalData.push(newObj);
});

// Log finalData to the console and return it
console.log(finalData);
return finalData;
```

6. Click on **Preview** to see the results of this transformer. As you can see, it’s much cleaner and easier to read and navigate.

![alt_text](images/image31.png "image_tooltip")

7. **Create a new Resource query to Stop an instance:** using EC2 APIs StopInstances action 
    
    | Property     | Value                                                        |
    | ------------ | ------------------------------------------------------------ |
    | Name         | `stopEC2InstanceByInstanceID`                                |
    | Action Type  | `POST`                                                       |
    | Headers      | `Content-Type: application/x-www-form-urlencoded; charset=utf-8` |
    | Body         | `x-www-form-urlencoded`                                      |
    | Body Params  |                                                              |
    | Action       | `StopInstances`                                              |
    | InstanceId.1 | `{{manageInstancesTable.selectedRow.data.instanceId}}`       |
    | Version      | `2016-11-15`                                                 |



7. **Create a new Resource query to Start an instance:** using EC2 APIs `StartInstances` action 

    1. Duplicate the `stopEC2InstanceByInstanceID` query

    | Property     | Value                          |
    | ------------ | ------------------------------ |
    | Rename it to | `startEC2InstanceByInstanceID` |
    | Body Params  |                                |
    | Action       | `StartInstances`               |

    2. Everything else remains the same as `stopEC2InstanceByInstanceID`

    

8. Add a **JavaScript Query** that returns a simple JSON object for list of EC2 images to choose from
    > **Why?** You can use Amazon EC2’s DescribeImages action to get details about the images. But since we only want to list the images available in the free tier, instead of calling an API, we will simply create a JSON object with the list of images we want to display.

    1. Click on the **+** button in the left side panel, and click **JavaScript query** 
        1. Name this query `imageDataJSON` 
        2. Paste this code below into it

![alt_text](images/image24.png "image_tooltip")

```javascript
const imageData = [
  {
    "id": 1,
    "image_id": "ami-0c2b0d3fb02824d92",
    "image_title": "Microsoft Windows Server 2022 Base",
    "show_flag": true
  },
  {
    "id": 2,
    "image_id": "ami-0dfcb1ef8550277af",
    "image_title": "Amazon Linux 2 AMI (HVM) - Kernel 5.10",
    "show_flag": true
  },
  {
    "id": 3,
    "image_id": "ami-0c9978668f8d55984",
    "image_title": "Red Hat Enterprise Linux 9",
    "show_flag": true
  },
  {
    "id": 4,
    "image_id": "ami-0557a15b87f6559cf",
    "image_title": "Ubuntu Server 22.04 LTS",
    "show_flag": true
  }
]

return(imageData)
```

![alt_text](images/image12.png "image_tooltip")


10. **Pre-load the Image data when the app launches:** To ensure our Listbox is automatically populated when the app opens, we will check the box Run this query on page load? in the advanced tab for the `imageDataJSON` query.

![alt_text](images/image12.png "image_tooltip")

### Designing the UI, and connect it to the Queries and Transformers **(20 mins)**

1. Drag a **Text component** to the canvas. Name it appTitleText
    1. Value: `## AWS EC2 Instance Manager`
2. Drag two **Container components** and align them vertically
    2. The top container will contain the UI for choosing and launching a new instance.
    3. The bottom container will contain the table listing the instances

![alt_text](images/image3.png "image_tooltip")

#### Designing the top container to choose and launch EC2 images

1. Name the top container `launchInstanceContainer`
2. Set the title for container Title to `#### Launch new EC2 Instance`
3. Drag a ListBox component to the first container, and set the properties in the right panel inspector to - 

| Property                                   | Value                           |
| ------------------------------------------ | ------------------------------- |
| Rename it to                               | `ec2ImagesListBox`              |
| Data source                                | `imageDataJSON`                 |
| <u>*Under Mapped options*</u>              |                                 |
| Value                                      | `{{ item.image_id }}`           |
| Label                                      | `{{ item.image_title }}`        |
| Under *<u>Label Section</u>*, set label to | `Choose an EC2 Image to launch` |

![alt_text](images/image9.png "image_tooltip")

1. Drag a Button component to the first container, and set the properties in the right panel inspector to - 

| Property      | Value                     |
| ------------- | ------------------------- |
| Rename it to  | `launchNewInstanceButton` |
| Text          | `Launch New EC2 Instance` |
| Event Handler |                           |
| Action        | `Control query`           |
| Query         | `launchNewEC2Instance`    |
| Method        | `Trigger`                 |
| Prefix Icon   | Set it to your choice     |



#### Designing the bottom container 

1. Name the bottom container manageInstancesContainer
2. Set the title for Container Title to `#### Manage Instances`
3. Drag a Table component to the second container, and set the properties in the right panel inspector to - 

    | Property       | Value                             |
    | -------------- | --------------------------------- |
    | Name           | `manageInstancesTable`            |
    | Data           | `{{transformInstanceData.value}}` |
    | Sort column    | `Launch Time`                     |
    | Sort direction | `Desc`                            |

  4. Add two action buttons for the Table
      1. Action 1 (Stop)
            | Property           | Value                         |
            | ------------------ | ----------------------------- |
            | Action button text | `Stop`                        |
            | Action button type | `Run a query`                 |
            | Action query       | `stopEC2InstanceByInstanceID` |
      2. Action 2 (Start)
            | Property           | Value                          |
            | ------------------ | ------------------------------ |
            | Action button text | `Start`                        |
            | Action button type | `Run a query`                  |
            | Action query       | `startEC2InstanceByInstanceID` |
  5. Add background color for instanceState column in the table using RGBA css color codes
        1. With the table selected, navigate to Columns in the right panel inspector
        2. Click on `instanceState` column, and paste this conditional code in it.

```javascript
{{self.toLowerCase() === "running" ? 'rgba(165, 255, 110, 0.5)':self.toLowerCase() === "stopped" ? 'rgba(255, 141, 150, 0.5)':'rgba(255, 188, 99, 0.5)'}}
```
> **What this does**
> 
> If the state is “running”, then the color code rgba(165, 255, 110, 0.5) is returned, representing a light green color with a 50% transparency.
> 
> If the state is “stopped”, then the color code rgba(255, 188, 99, 0.5) is returned, representing a light red color with a 50% transparency.

![alt_text](images/image13.png "image_tooltip")

11. **Refresh the table after launching a new instance:** To ensure that the table automatically displays data when an instance is launched, stopped, or restarted, we will add `getAllEC2Instances` query to the Success section of the `launchNewEC2Instance`, stopEC2InstanceByInstanceID and startEC2InstanceByInstanceID queries, so it’s fired off automatically when these queries run successfully.

![alt_text](images/image16.png "image_tooltip")

# Wrapping Up

Congratulations! You've successfully completed this self-guided workshop and built two Retool apps that connect to AWS APIs. You learnt how to use Retool to retrieve and display data, create tabbed containers, use list boxes as inputs for queries, and visualize data with charts.

Remember, these two apps are just the tip of the iceberg when it comes to what you can build with Retool and AWS. The possibilities are endless, and we encourage you to continue exploring and building on what you've learned here.

If you have any questions or feedback, please don't hesitate to reach out to us. We'd love to hear your thoughts and help in any way we can.

Thanks again for your time and effort in completing this workshop. We hope you found it valuable and look forward to seeing what you'll create next with Retool.


# Resources

[JSON for Retool AWS Cost Explorer App](https://gist.githubusercontent.com/ajot/6ecab2518cb56e05a21ba8b1ea2c1102/raw/8f614f16819ddbf12017a67a9efa3ff64177be99/retool_aws_cost_explorer_app.json)

[JSON for Retool Amazon EC2 Instance Admin Panel app](https://gist.githubusercontent.com/ajot/e84dff09500d47ba09cd7f5c33e63e6e/raw/3c809237b6fefa2314ab738e6baa62f292cc6eb8/retool_amazon_ec2_instance_admin_panel.json)

[Retool Templates](https://retool.com/templates/)


---


## **Creating and attaching EC2 and Cost Explorer Permissions to an IAM user**.

For the purposes of this workshop, we will need a User that has the permissions to access EC2 and Cost Explorer APIs. Here are the instructions to create a user and allocate the appropriate permissions. 



1. Log in to your AWS console.
2. Navigate to the IAM service.
3. Create a new policy for Cost Explorer using the JSON below. 
4. Create a new policy for EC2 using the JSON below. 
5. Create a new user with a name of your choice.
6. Open the user details, and click add permissions. Then attach the two policies you created to their permissions.  
7. In the user details page, click on security credentials. Then create new access key. 
8. Copy the `Access Key ID` and `Secret Access Key` for the new user, which will be used to connect Retool to the AWS API.


### JSON for Cost Explorer and EC2 Permissions required for these apps


#### JSON for Cost Explorer Permissions

This policy is required to retrieve billing data using the AWS Cost Explorer API in Retool.

This policy grants the user permission to perform the `GetCostAndUsage` action on the `ce` resource, which is the resource we need for getting the data from AWS Cost Explorer API. The `*` in the Resource field means that the policy applies to all resources, which is required for the `GetCostAndUsage` action to work.


#### JSON for EC2 Permissions 

This policy is required to launch and manage EC2 instances using the Amazon EC2 API in Retool.

This policy grants the user permission to perform the following actions, which are the actions we use to build our EC2 Instance Manager app -

`ec2:RunInstances` - Launch new EC2 instances

`ec2:DescribeInstances` - List all EC2 instances

`ec2:StopInstances` - Stop EC2 instances

`ec2:StartInstances` - Start EC2 instances

The `*` in the Resource field specifies that the policy applies to all EC2 instances.
