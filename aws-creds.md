# **Creating and attaching EC2 and Cost Explorer Permissions to an IAM user**.

For the purposes of this workshop, we will need a User that has the permissions to access EC2 and Cost Explorer APIs. Here are the instructions to create a user and allocate the appropriate permissions. 

## Option 1: Use Burner Credentials

If you do not have access to an AWS account, or are unable to create a user with the required permissions, for the purposes of this work, use the following burner account credentials. Please note that **these will be destroyed after the workshop, and you would not be able to use them**:

### Module 1

> - AWS Access Key ID: `AKIA4GSR46W2D5HIPG63`
> - AWS Secret Key ID: `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`

### Module 2
>
> > - **AWS Access Key ID**: `AKIARFLN3W5QNGVU4P7M`
> > - **AWS Secret Key ID**: `+8LoapuR6blaTKHAbc5Eapx5fDxq432e+wTDhvRn`


## Option 2: Use your own AWS Credentials
1. Log in to your AWS console.
2. Navigate to the IAM service.
3. Create a new policy for Cost Explorer using the JSON below. 
4. Create a new policy for EC2 using the JSON below. 
5. Create a new user with a name of your choice.
6. Open the user details, and click add permissions. Then attach the two policies you created to their permissions.  
7. In the user details page, click on security credentials. Then create new access key. 
8. Copy the `Access Key ID` and `Secret Access Key` for the new user, which will be used to connect Retool to the AWS API.


## JSON for Cost Explorer and EC2 Permissions required for these apps


### JSON for Cost Explorer Permissions**

This policy is required to retrieve billing data using the AWS Cost Explorer API in Retool.

This policy grants the user permission to perform the `GetCostAndUsage` action on the `ce` resource, which is the resource we need for getting the data from AWS Cost Explorer API. The `*` in the Resource field means that the policy applies to all resources, which is required for the `GetCostAndUsage` action to work.


### JSON for EC2 Permissions

This policy is required to launch and manage EC2 instances using the Amazon EC2 API in Retool.

This policy grants the user permission to perform the following actions, which are the actions we use to build our EC2 Instance Manager app -

`ec2:RunInstances` - Launch new EC2 instances

`ec2:DescribeInstances` - List all EC2 instances

`ec2:StopInstances` - Stop EC2 instances

`ec2:StartInstances` - Start EC2 instances

The `*` in the Resource field specifies that the policy applies to all EC2 instances.


## Burner Credentials for this workshop

