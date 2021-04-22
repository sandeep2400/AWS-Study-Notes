# Mastering the cloud. 
_These study notes are from my AWS certification courses with A Cloud Guru_

Amazon Web Services (AWS) is Amazon’s complete set of cloud tools. The sheer scale of tools that AWS offers is breathtaking. From servers to to file storage to caching, if you can think of a system or tool-set, AWS offers it. 

Although AWS overshadows the cloud industry, it’s origins were humble. As Amazon’s retail division, its core business entered different lines, each sub-division needed compute resources like servers and storage. To avoid duplicating work, a few smart engineers constructed an architecture of services that could be accessed through an API.

And Jeff Bezos, the smartest businessman of his time, saw the value in opening this API to the world. The rest is history. 

__AWS Regions__
Lets start with the where AWS is physically located. AWS has data centres (Regions) spread across the globe and availability zones in each region. 
![AWS Regions](/images/aws_regions-1.png)
_Image 1: Map of AWS Regions as of April 2021. (Copyright Amazon.com)_
_Source:[Now Open – Third Availability Zone in the AWS Canada (Central) Region | AWS News Blog](https://aws.amazon.com/blogs/aws/now-open-third-availability-zone-in-the-aws-canada-central-region/)_

__Availability Zones:__
Each Region has more than one Availability Zone. Each zone is physically isolated from the rest. They all have independent power, cooling and independent security. Their exact locations are kept secret. 

## AWS Security
Macie (Protecting sensitive Data), Key Management Service (encryption Keys), CloudHSM (hardware based key storage), Certificate Manager and Secrets Manager, Sheild (Infra protection) Firewall (filter malicious traffic) are just some of the services that AWS offers. 

### AWS Secrets Manager
SM protects secret, passwords, keys and tokens. An application can request passwords at run time. SM can also rotate passwords at regular intervals. 

### AWS IAM
Provision access to users and services. IAM uses __Policies__ (JSON Files), __Groups__ and __Roles__ to open and close access to users. 

IAM is a global resource, which means all users and permissions you create will be applicable anywhere. 

When you create a user, you can assign them to a group. Each group can be assigned a policy/policies which allow/deny access to a Resource. 
A sample policy is a JSON file that looks like this 
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListAndDescribe",
            "Effect": "Allow",
            "Action": [
                "dynamodb:List*",
                "dynamodb:DescribeReservedCapacity*",
                "dynamodb:DescribeLimits",
                "dynamodb:DescribeTimeToLive"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SpecificTable",
            "Effect": "Allow",
            "Action": [
                "dynamodb:BatchGet*",
                "dynamodb:DescribeStream",
                "dynamodb:DescribeTable",
                "dynamodb:Get*",
                "dynamodb:Query",
                "dynamodb:Scan",
                "dynamodb:BatchWrite*",
                "dynamodb:CreateTable",
                "dynamodb:Delete*",
                "dynamodb:Update*",
                "dynamodb:PutItem"
            ],
            "Resource": "arn:aws:dynamodb:*:*:table/MyTable"
        }
    ]
}
```
Policies can grow to become large and complicated. 
To assign a user cetain permissions: 
* first, make a Group 
* then assign permissions to the group using polciy documents. 
* add users to the groups. Permissions applied to the group are automatically assinged to the user. 
