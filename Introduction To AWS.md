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

## Compute
### EC2: Compute Cloud 
Rent Virtual Servers with different sizes, processers and speeds. Pay for these instances by the hour or second with no long term committments. This flexibiltiy decreases costs by orders of magnitude and can be gamechanging for some industries.

### Containers
Lets you create a package of your program along with all the code it needs to produce an image that can be run in any computer in the world. 
Once created, you can deploy your container anywhere. 

### Lambda 
Serverless comoute service which runs your program in response to events. The code will run in scratchpad containers that are destroyed afterwards. All you have to do is upload your code, specifiy your triggers and the code will run when the code is triggered. You are also charged by the millisecond. 


## Storage
There are 3 types of storage: File(directory structure like on a laptop), Block(files are split into chunks of data of equal size and then stored in blocks - commonly used in servers, and dataabase systems) and Object Storage(files are stored in objects, flat memory model and given a unique id - present the id and it will give you the file.)
AWS has SFS for file storage, EBS for block storage, and S3 for object storgae.

### S3: Simple Storage Service
S3 has a very high reliability. Industry leading durability. S3 has different storage classes. S3 Glacier is for files that are not required immediately. 

### AWS EFS: 
Filesystem on the cloud, very durable. and has different storage classes. 

### AWS Storage Gateway
Access to Amazon storage on prem. 

## Databases
Has engines for relational, key-value, in-memory, document, graph series and time series. 
Relational (Aurora - MYSQL and Postgres), RDS(Relational), Key-value (dynamodb - nosql), im-memory(elasticache), Document Database (Mongo)

### RDS Relational Database Service
Engines for mysql, mariadb, mssql, oracle . Easy to set up, scalable, available, automated backups, automatic host replacement. 

### DynamoDB
NoSQl Database. Key-valye document

## Networking

### VPC is a gated private community for your AWS resources. You can add NAT gateways, Internet gateways and Network Access Control Lists to connect to the internet and control who can access resources inside the VPC. 

### CloudFront
CDN that can server your website, video, data and applications to your customers with low latency and high transfer speeds. Use Edge Locations, using multiple points of presence across the globe. Cloudfront will server the website from the nearest edge location. It alwasy checks if it is serving the latest version of the site. If not, it will reset the cache. 

Cloudfront also protects the site from DDOS attacks. AWS is better prepared for DDOS attacks than you. CDNs can handle traffic spikes if your site sees a surge in traffic. 

Cloudfront also has Lambda@Edge so that you can run Lambda code at the edge. Realtime metric and logging. Cost effective and pay as you go pricing. 

### Route 53
DNS service that has extra features. 
* Simple Routing Policy: Basic functionality of returning IP address for a domain 
* Weighted Policy: provide multiple IP addresses to spread your load. (0 to 100). so 50-50 will split the traffic. 
* GEo location policy: Geo loc based policty. 
* Latency Policy: Latency based serving. 
* Failover policy: Can point to another loction if the primary target fails .
* Multi Value Answer Policy: Replies with muiltiple healthy values for the domain. Checks if the ip addresses are up. 

## Machine Learning
### Rekognition
Analyze Images and Videos with ML 

### Deep Racer
Self Driving Car to teach reinforcement learning. 
We create a model, give it an algorithm,a reward function and  abunch of data from the car

### CodeGuru
AI based Code Review. 
Profiler: Analyzes code and identifies expensive lines. 
CodeGuru Reviewer: Identify issues and bugs in your repos, identifies deviations from Best practives. 

## Some Case Studies on Using AWS

### Simplest website
A site hosted on an EC2 server on a single VPC in a single availability zone. 

A basic blog can be on EC2 on a VPC and EC2 connects to AN RDS db. 

A load balanced blog where 2 ec2 servers read/write to an RDS DB, but the servers are load balanced with an ELB.

We can also add an ELasticache and the servers will read the cache before reading the db. 

We can also add CDN (CloudFront) to speeden up the site. 

We can also add multiple 2 availabilty zones which will split traffic between AZs

