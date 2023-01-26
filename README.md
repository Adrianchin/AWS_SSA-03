# Cantrel SSA-03 Notes

## AWS Fundamentals

> Video 1:

### Public Services
- Accessed directly from an internet connection

### Private Services (AWS Private Zone)
- Contained in a VPC. Can only be contacted within the VPC. Cannot be directly connected via the internet

### AWS Public Zone
- Network zone where public services are operated from. Can be accessed directly from the internet. Sits between AWS Private Zone (VPC) and Public Zone (Internet)

> Video 2:

### Global Resilience
- Available globally. Service can be accessed from anywhere in the world.

### Regional Resilience 
- Available in the AWS Region. Example US-East-01. For Regional Resilience, you can use multiple regions (US-East-01 and US-West-01)

### Area Zone Resilience (AZ)
- Available within the Area Zone. For example, VPC(s) located in a region. Example, a VPC located in US-East-01 as US-East-01-2a, US-East-01-2b and US-East-01-2c.

> Video 3:

### VPC
- Virtual network inside AWS. These are regionally resilient. 
- On creation, Custom VPCs are Private and Isolated unless you decide otherwise.

2 Types: 
1) Default VPC (1 per region). Initially created by AWS and preconfigured in a very specific way.
   2) Custom VPCs are always configured with CIDR range 172.31.0.0/16
   3) /20 Subnet in each AZ in the region
   4) Internet Gate Way, Security Groups and NACL
   5) Subnets are assigned public IPv4 addresses
2) Custom VPCs

VPC CIDR: Provides a range of IPs that a VPC can use. Subnets are broken down into different AZs using the CIDR to provide regional resilience.

> Video 4:
### EC2
- IAAS - Provides Virtual Machines - Instances
- Private service by default (uses VPC networking only)
- Can attach EBS (Elastic Bloc Storage - Medium Term Storage)

Instance States:
1) Running
   1) Pay for all EC2 resources running
2) Stopped
   1) Only pay for memory
3) Terminated
   1) EC2 is completely removed

Amazon Machine Images (AMI)
1) Permissions
2) Root Volume
3) Block Device Mapping

- Connecting to EC2
- Log in with SSH Key Pair from your OS

> Video 5
### S3 Basics
- Globally accessible storage platform - regional based and resilient
- Unlimited data amounts and multi-user

Objects:
- Keys: The file seen in the bucket
- Values: The data of the object - Size up to 5TB

Buckets:
- Is region specific.
- Name requires to be globally unique
- Can hold an unlimited number of objects
- Flat structure. All files are stored at the same level. Folders are actually key names.

## Exam Powerups:
- Buckets are globally unique
- 3-63 Characters, all lower case, no underscores
- Start with a lowercase letter or number
- Can't be IP formatted
- Buckets - 100 soft limit, 1000 hard per account. For users you can subdivide a bucket by key names.
- Unlimited objects in buckets, 0 to 5TBs
- Key:Value

> Video 6
### CloudFormation
- Templates used to create and update infrastructure.
- Templates are written in YAML or JSON

Templates:

- AWSTemplateFormatVersion - Used to version the template.
- Description - User description for the template. NOTE: If the AWSTemplateFormatVersion is used, this HAS to come immediately after.
- Metadata - Specify/control the console UI and other specific uses
- Parameters - Set of parameters or fields to prompt the user. Example, sizes of EC2 instances to use, default values, etc.
- Mappings - Optional for creating dropdowns, such as instances that will be available
- Conditions - Does if/else statements. Create conditions that will be created or not based on conditionals
- Outputs - Outputs from the template being provided. 
- Resources - Instances and properties that will create a stack of logical resources that CF will make. A stack will create a physical resource (such as an EC2 resource) from a stack.

> Video 7
### CloudWatch
- Collects and manages operational data
- Metrics - AWS Products, Apps, on-premises. 
  - Example, CPU Usage, Network IN/OUT, Disk IO.
  - Includes 
    - A timestamp
    - A value
  - Sends dimensions with metrics, that allow us to look at specific instances (such as EC2s) and associate them with their specific logs
  - Metrics can be attached to an alarm to trigger events/actions or SNS notifications
- CloudWatch Logs - AWS Products, Apps, on-premise - Collection and monitoring of logs
- CloudWatch Events - AWS Services and Schedules - Event hub. Generates events to do another actions, or generate an event to do something based on time/schedules

CloudWatch ingests AWS Service data and generates metrics. This can be used to make alarms (create actions) or generate statistics (for API's to gather data).

>Video 10
### Route53
- Provides 2 main services
  1) Register Domains
     - Manages relationships with all high level domains (registries) 
     - Creates zone files (a database for your domain name)
     - Hosts the zone file in the AWS Name servers 
     - Adds name server records into the top level domains so they can find the zone file for your domain
  2) Host Zones
     - Lets you create zone files and manages 4 managed name servers
     - Can be hosted in a public DNS zone
     - Can be hosted in a private DNS zone (linked to VPC)
     - Stores record sets
- Global service (single database)
- Globally Resilient

>Video 11
### DNS Record Types
1) Name Server Records
    - Hosts the servers associated with the name server records that allows roots to point to the correct zones that host your domain (ie. hosted in Amazon Name Servers/zone)
2) Records: 
    - A Records
      - A www is mapped to IPv4 address
    - AAAA Records
      - AAAA www is pointed to a IPv6 address
    - CNAME Records
      - CNAME's are names that reference(point) towards A or AAAA records (CNAMES are alias)
    - MX Records
      - MX Records are used for delivering Emails. 
      - MX Records have 2 main parts: 
        - A priority
          - example MX 10 mail, 10 is priority
          - example MX 20 mail.other.com, 20 is priority
          - Lower is higher priority, so mail is used 1st. If it returns nothing, mail.other.com is used.
        - A value
          - example MX 10 mail, mail is the value. This is queried against
          - example MX 20 mail.other.com, mail.other.com is the value. This is queried against
      - TXT Records
        - Added text data to a domain
        - Can be queried against to ensure the domain is really yours
3) TTL - Time To Live
    - This is a number that allows an authoritative answer that is cached on a server, which allows users to avoid walking the tree by accessing the cache.

### Walking the tree with TTL and name servers
1) Client queries resolver
2) Resolver queries DNS root and returns the Authoritative servers (ex. .com)
3) Resolver queries Authoritative server (ex. .com) and returns the name server (ex. amazon.com)
4) Resolver queries the name server (ex. .com) and returns the record for your domain (ex. www.animals4lyfe.com) and contains a TTL, which is the authoritative answer
5) Authoritative answer is cached for the TTL time, so the results of the query are stored for x TTL seconds, which can be queried by other clients and avoid walking the tree.


