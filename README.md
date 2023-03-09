# Welcome to the Retool DevOps workshop!
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

To be able to build these modules, you would need **AWS client ID and secret** of an AWS user that has permissions to access Amazon EC2 AP, and AWS Cost Explorer API.

This guide **includes a burner client id and secret** you can use for the purposes of this workshop. Please note that **these will be destroyed after the workshop** and you would not be able to use them. 

Alternatively, if you would like to use your own AWS account so you can explore your own data through these apps, please see the section at the end of this guide for step-by-step instructions on [Creating and attaching EC2 and Cost Explorer Permissions to an IAM user](/aws-creds.md).

## Let’s get started!

- [Module 1: Build a Custom AWS Billing Dashboard](/module-1.md)
- [Module 2: Build a Custom Amazon EC2 Instance Admin Panel](/module-2.md)

### Option to Import JSON File to Create App
> We highly recommend following each step to get the most out of the guide. 
> 
> However, if you prefer, you can skip Part 2 of each module and [import](https://docs.retool.com/docs/import-export-apps?ref=retool-blog) JSON files provided at the end of this guide to create the apps. 
> 
> Keep in mind that **you will still need to go through Part 1 of each module**, as it connects to the AWS API with your AWS credentials.
> 
> - [JSON for AWS Cost Explorer app](/cost_explorer_app.json)
> - [JSON for Amazon EC2 Instance Manager app](/ec2_instance_manager_app.json)
> - [Instructions to create a new app using a JSON file](https://docs.retool.com/docs/import-export-apps?ref=retool-blog)


## Help/Questions

- Twitter: [@retool](https://twitter.com/retool)
- [Retool Community](https://community.retool.com/)
- Retool's Developer Advocacy team
  - Amit Jotwani - [@amit](https://twitter.com/amit)
  - Christina Zhu - [@cszhu](https://twitter.com/cszhu)
  - Kevin Whinnery - [@kevinwhinnery](https://twitter.com/kevinwhinnery)