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

## IAM and Organizations 
>Video 1

### Identity policies
- Policy documents are created using JSON
- Statements that grant or deny actions that grants access to resources
  - Sid: Statement description
  - Effect: Allow or Deny
  - Action: Lists:
    - 1 Specific action (example Get, Put)
    - Wild Cards (ex. ["s3:*"] <- allow all in S3) 
    - List of individual (multiple actions)
  - Resources: Individual resources or a list of resources this applies to
- #### Explicit Deny > Explicit Allow > Implicit Deny. 
- Default is Implicit Deny

### IAM Policies
- AWS Collects all policies together that apply to a user and sums up all policies to grant access.
- Policies can reference identities (ex users, resources). They cannot reference groups, as groups are not identities
- 2 Types of policies: 
  1) Inline-policies - Applying the policies to each user individually (ex. IAM user policy). Usually used for exceptional access rights
  2) Managed-policies - Create a policy and attach it to a user (ex. for large # of users, for groups or roles)

>Video 2
### IAM Users
- IAM Users are identities used for anything requiring Long-Term AWS access (ex. Humans, Applications or Service Accounts).
#### - A single principle that needs to use an identity.
- Authentication goes like this:
  1) Authentication - A principal (user or an application) requests access to IAM with Authentication. This is used with Username and Password (U&P - usuers) or Access Keys (application or user using Command Line Tools)
  2) Authorization - AWS knows the policies that are attached to the Authenticated user and will check the policies whenever an action is done on the authenticated user 

### Amazon Resource Name (ARN) 
- Uniquely identify resources within any AWS account
  - Example: arn:aws:s3:::catgifs <- refers to the bucket itself. This does not provide access to the objects itself
  - Example: arn:aws:s3:::catgifs/* <- refers to Objects inside the bucket. This does not provide access to the bucket itself
  - You may need to include both if you need access to the bucket itself AND objects inside
- Examples:
  - (arn):(partition):(service):(region):(account-id):(resource-id)
  - (arn):(partition):(service):(region):(account-id):(resource-type)/(resource-id)
  - (arn):(partition):(service):(region):(account-id):(resource-type):(resource-id)
## Exam PowerUp
- 5,000 IAM User limit per account
- IAM Users can be a member of a max of 10 groups

These limits could impact design choices
- IAM Roles and Identity Federation can fix these limits

>Video 2
### IAM Groups
- IAM Groups are containers for Users. They are not true identities and cannot be referenced in policies.
- You cannot log into groups. 
- They have no credentials. 
- They are for organizing users
- Users can be part of multiple groups
- Groups have managed policies attached to them. Users can therefore be governed by the sum of:
  - Inline (users own individually attached policies)
  - Managed (from group memberships)
- There is no limit to the # of users in an IAM group
- You could create a group that manages all users, but it does not come natively
- No nesting allowed
- 300 limit per account, with a 500 limit with a support ticket
#### - Since groups are not a true identity, they CANNOT be referenced as a principal in a policy

>Video 3
### IAM Roles
- Where an unknown number of users or principles that need to use an identity.
- A principal uses a role by assuming the role. The principle is borrowing the identity for a short period of time.
- Roles are identities which can be referenced by other policies
#### - Have 2 types of policies attached:
1) Trust Policy
   - Controls what principles can assume the role. 
   - Can reference different things. 
     - Resources in the account (ARN Numbers) or other accounts
     - External identities (example from Facebook, Google)
     - Creates temporary security credentials via Secure Token Service (STS) for the principle to assume the role
2) Permission Policy
   - Whenever actions using the temporary credentials are used, access is checked against the permission policy
   - If you change the permission policy, the permissions of the temporary credentials also change

>Video 4
### When to use IAM Roles
- For AWS Services themselves, for example, where you do not know the # of instances
  - Ex. AWS Lambda has no permissions by default. It needs some way to get permissions for accessing other services.
    - Create a Lambda Execution role, which has permission policies
    - When the STS provides temporary credentials and accesses AWS resources the policies are checked and the Lambda Function can complete the task
    - Prevents you needing to manually assign policies, which does not work well with automation
- For users to get emergency access to AWS services outside the access provided by their normal policies (Ex. read but no write access)
- Identity Federation - For large corporations larger than 5000 users or existing identities from on-premises using Identity Federation
  - Ex. Existing identities using Active Directory from on-premises identity services (AWS requires AWS IAM and it CANNOT directly use external identity, so assume a role)
- Web Federation - Unknown amount of users that need to access services (Ex. Web app that needs access to a DB or S3). Allows unknown principles to assume the role.
    - Scales to 100,000,000's of accounts
- Cross account access (multi account management)
  - Ex. If you have 1000's of identities that need to access to a partner account, a role can be assumed in the partner account by users from your account

>Video 5
### Service Linked Roles
- IAM Role linked to a Specific AWS Service
- Predefined by a service
- Providing permissions that a service needs to interact with other AWS services on your behalf
- Service might create/delete the role, or allow you to create/delete the role during setup or within IAM
- You cannot delete the role until it is no longer required
- #### AKA providing a role the ability to make a role
- Allows role separation: You can provide access to users to make new roles with access that the current user does not currently have

>Video 6
### AWS Organizations
- Allows management of many AWS accounts under an organization 
- An organization has only 1 Master account or Organizational Root account (different from an account root user)
- Organizations can be made inside organizations called Organizational Units (OU) to nest organizations inside organizations
1) Use an account to create an organization
2) The account used to create the organization become the Management Account
3) Using the management account, you can invite standard accounts into the organization
4) When the standard organization accepts the invite, they become a member account of the organization
- Allows Consolidated billing. 
  - Members inside an organization account have their payment method removed and use the Organizational Root account for all billing
  - Removes financial overhead so all billing can be managed by the Organizational Root user
  - Consolidation allows for grouping of reservations (EC2 reservations) and volume discounts among all organizational members
- Can create new accounts directly in an organization.
  - Removes the need to approve an invite to an organization
- You can use a method called Role Switch to assume roles using one account and assuming a role in another organizational account
  - Can use Identity federation to log into an account and then use role switch
  - Can log into an account normally and use role switch
- Best practice is to separate the Organization Root User account for billing only and to create an organization account for admin purposes

>Video 7
### Service Control Policies
- SCP do NOT provide permissions. The only provide boundaries.
- Service Control Policies (SCP) are policies that can be attached to the organization as a whole, to Organizational Units (OU) or to member accounts themselves
- Affect everything under it (nested OU's)
- The SCP does NOT affect the master management account - no restrictions
- They can limit what the accounts can do under the SCP - This includes the account root user INSIDE the account affected by the SCP (restricts what the account can do, the account root user has 100% access to the account)
- This is just an allow/deny list. The default policy is a implicit deny list
  - A deny list is useful because you can explicitly deny resources and its low overhead
  - Explicit allow is useful because it requires you to explicitly say what services are allowed, but it could require more admin because it implicitly denys anything not on allow list
- The Identity Policies in the Account and the SCP(s) are summed together to determine what the user in an account can access. Remember, sum of policies.


