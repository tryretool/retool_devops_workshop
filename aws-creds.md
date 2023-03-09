# **Creating and attaching EC2 and Cost Explorer Permissions to an IAM user**.

For the purposes of this workshop, you need a user with permissions to access AWS Cost Explorer APIs and Amazon EC2 APIs. You have two options:

## Option 1: Use Burner Credentials

If you don't have an AWS account or can't create a user with the necessary permissions, you can use the provided [burner account credentials](https://bit.ly/retool_workshop_aws). However, please be aware that **these credentials will be destroyed after the workshop, and you won't be able to use them again**.

- [burner account credentials](https://bit.ly/retool_workshop_aws)
- pwd: `gnvdxx8ctmk`

## Option 2: Use your own AWS Credentials
Here are the instructions to create a user and allocate the appropriate permissions. 

1. Log in to your [AWS console](https://aws.amazon.com).
2. Navigate to the IAM service.
3. Create a new policy for Cost Explorer using the JSON below. 
4. Create a new policy for EC2 using the JSON below. 
5. Create a new user with a name of your choice.
6. Open the user details, and click add permissions. Then attach the two policies you created to their permissions.  
7. In the user details page, click on security credentials. Then create new access key. 
8. Copy the `Access Key ID` and `Secret Access Key` for the new user, which will be used to connect Retool to the AWS API.


### JSON for Cost Explorer Permissions
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowGetCostAndUsage",
            "Effect": "Allow",
            "Action": [
                "ce:GetCostAndUsage"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

This policy is required to retrieve billing data using the AWS Cost Explorer API in Retool.

This policy grants the user permission to perform the `GetCostAndUsage` action on the `ce` resource, which is the resource we need for getting the data from AWS Cost Explorer API. The `*` in the Resource field means that the policy applies to all resources, which is required for the `GetCostAndUsage` action to work.


### JSON for EC2 Permissions
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EC2Access",
            "Effect": "Allow",
            "Action": [
                "ec2:RunInstances",
                "ec2:DescribeInstances",
                "ec2:StopInstances",
                "ec2:StartInstances"
            ],
            "Resource": "*"
        }
    ]
}
```

This policy is required to launch and manage EC2 instances using the Amazon EC2 API in Retool.

This policy grants the user permission to perform the following actions, which are the actions we use to build our EC2 Instance Manager app -

`ec2:RunInstances` - Launch new EC2 instances

`ec2:DescribeInstances` - List all EC2 instances

`ec2:StopInstances` - Stop EC2 instances

`ec2:StartInstances` - Start EC2 instances

The `*` in the Resource field specifies that the policy applies to all EC2 instances.