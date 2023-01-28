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

>Video 8
### CloudWatch Logs
- Public service - usable from AWS or on-premises
- Store, monitor and access logging data. Usually timestamp and data.
- AWS Integrations - Services in AWS or the unified cloudwatch agent for external services not covered in AWS
- Can generate metrics based on logs. Metric Filter
- Can generate alarms based on Metrics which actions can use

How it works
1) Logging sources push log events to AWS
2) Log events are entered into a Log Stream
  - Log events are ordered events for a specific thing (think a single EC2 instance)
3) Log group - A group of log streams. (think a group of EC2 instances)
4) Metric filters are placed on log groups
5) Metric filters produce metrics
6) Metrics can produce Alarms
7) Alarms can have actions listening to it

>Video 9
### CloudTrail
- Logs AWS account API calls/actions/activities as CloudTrail Events
- Stores 90 days by default in Event History
- Enabled by default. No cost for 90 days
- To customize the service, create 1 or more trails
- 2 types:
  1) Management Events - By default
  2) Data Events - NOT by default
- By default, CloudTrail ONLY logs management events
- CloudTrail is defaulted to a regional service (logs to the region it is in), but can be set to all regions
  1) Regional - only logs trails for regions it is in
  2) All regions - Includes Logs events for Global Services (Ex. IAM, STS, CloudFront are classified as global)
- Stores events in a defined S3 bucket. Only charged for space used (stored efficiently in JSON).
- Can be integrated into CloudWatch Logs to access metrics and alarms
- Can create an organizational trail and track all accounts in the organization to manage multiple accounts in an org.
- NOT REAL TIME! There is a delay.

>Video 10
### AWS Control Tower
- Allow the quick and easy setup of multi-account environment
- Orchestrates other AWS services to provide this functionality
- Organizations, IAM Identity Center, CloudFormation, Config and more....
  1) Landing Zone - Multi-account environment. 
    - SSO/ID Federation, Centralization of logging and auditing
  2) Guard Rails - Detect/Mandates rules/standards across all accounts
  3) Account Factory - Automates and Standardises new account creation
  4) Dashboard - Single page oversight of the entire environment
- How it works
  - Control tower is managed inside the Management Account
    - Single Sign On (SSO) operates inside the management account that allows Federation ID and internal ID signon
  - AWS Organizations creates 2 OU's
    1) Foundational OU
       1) Audit account
          - SNS, Cloudwatch for auditing
       2) Log Archive Account
          - AWS Config, CloudTrail for Archive
    2) Custom OU 
       - Account Factory Provisioned Accounts 
         1) Account Baseline (template) - Provides guardrails
            - Templates you configure for account creation
         2) Network Baseline (template) - Provides guardrails
            - Templates you configure for account creation

#### Control Tower - Landing Zone
- Well Architected multi-account environment - Home Region
- Built with AWS Organizations, AWS Config and CloudFormation
- Security OU - Log Archive & Audit Accounts (CloudTrail and Config Logs)
- Sandbox OU - Test/less rigid security
- You can create other OUs and accounts
- IAM Identity Center (AWS SSO) - SSO, Multiple-Accounts, ID Federation
- Monitoring and Notifications - CloudWatch and SNS
- End User account provisioning via Service Catalog

#### AWS Control Tower - Guard Rails
- Guardrails are rules - multi-account governance
- Types: 
  1) Mandatory
  2) Strongly Recommended 
  3) Elective
- Preventive - stops you from doing things (AWS ORG SCP)
  - Enforced or not enabled (ie. allow or deny regions. disallow bucket policy changes)
- Detective - Compliance checks (AWS CONFIG Rules)
  - Clear, in violation or not enabled (ex. detect if CloudTrails is enabled)

#### AWS Control Tower - Account Factory
- Automated Account Provisioning
  - ex. Creations of cloud admins or end users (with approprate permissions)
- Guardrails - automatically added
- Account admin given to a named user (IAM Identity Center)
- Account and network standard configuration
- Accounts can be closed or altered
- Can be fully integrated with businesses Software Development Life Cycle (SDLC)

### Simple Storage Service (S3)
> Video 1
#### S3 Security
- S3 is private by default
- #### S3 Bucket Policies
  - A form of resource policy
  - Link identity policies, but attached to a bucket. Resource permissions
  - Allow/Deny same or different accounts
  - Allow/Deny anonymous principals (unknown access, for example files and images for a web application)
  - #### Same as identity policies but contain a principal
    - Principle - Defines which identity this policy applies to
    - ex. "Principal" : "*" <- applies to everyone (the account itself, other accounts (partner accounts) and unknown principles (anonymous))
    - Can be combined with Conditions to do conditional logic (for example, a condition can say a policy only applies if the condition is met, so you can say, block specific accounts or IP addresses)
  - Can only have 1 policy attached to a bucket, but multiple statements in the policy
  - Remember, your bucket policy applies with the sum of the principles identity policy too

- #### ACLs (access control lists)
  - Not recommended, as they are not flexible. Identity or resource policies should be used.
  - Only allows the following:
    1) READ - Allows grantee to list objects in bucket
    2) WRITE - Allows grantee to create, delete and modify objects in bucket
    3) READ_ACP - Allows grantee to read bucket ACL
    4) WRITE_ACP - Allows grantee to write the ACL for the bucket
    5) FULL_CONTROL - Allows full control of bucket

- #### Block Public Access
  - Added a further level of security to a bucket. A final fail-safe
  - Blocks all access to the bucket on default
    - Further settings can block access based on different cases.

### Exam Power Up
- When to use what policy
  - Identity policy: Controlling different resources (controlling all policies in 1 place)
  - Identity policy: You have a preference for IAM 
  - Identity policy: Same account
  - Bucket policy: Just controlling S3
  - Bucket policy: Anonymous or Cross Account
  - ACLs policy: NEVER (unless you must)

> Video 2
#### S3 Static (website) Hosting
- Normally you are accessing S3 via the AWS APIs (you are already logged in and authenticated)
- Static hosting allows access via HTTP
- Requires Index and Error documents to be set - points to a specific file in the S3 bucket. Need to be HTML files.
- Website endpoint is created. The name is influenced by the bucket name that AWS provides
- Can only use a custom domain via R53 if the bucket name matches the domain
- Priced based on:
  1) Storage (per GB/month)
  2) Data transferred (GB transferred out only) 
  3) Anount of requests
     - Note: There is a free amount of requests on the free tier, so you can host files and get very few charges.

Example: If a website has static content (such as images), you can host the images on S3 and host these images statically. This can accept HTTP requests and return the static content without impacting compute resources.
Example: Out-of-bound pages - Error pages if something fails or something is out of bounds (ex. contact x for help)

> Video 2
#### Object Versioning and MFA Delete
- Controlled at a bucket level. Defaulted as disabled. Can be enabled.
- Once enabled, it cannot EVER be disabled, but it can be suspended and re-enabled.
- If disabled
  - the object uses it's key and an Id = null. The object will be overwritten if the file is updated
- If enabled with versioning:
  - the key stays the same but an Id is assigned. Any new file with the same key will be assigned a unique Id and it will show on-top of the old version, but the old version will allways be there.
  - If deleted, a delete marker will be placed on top of the previous versions.
  - You can delete the actual version of the object, but you need to specify the Key and the Id.
  - Space is consumed by all versions. You are billed for all versions. The only way to remove all the space is to remove all versions.
  - You can only suspend versioning.
- #### MFA Delete
  - Enabled in versioning configuration
  - An MFA is required to change bucket versioning state
  - An MFA is required to delete versions
  - Serial number (MFA) + Code passed with all API Calls.

>Video 3
#### S3 Performance Optimization
#### Single Put upload (Default)
- Files are uploaded as a single data stream
  - If file fails, it requires a full restart. The entire file fails. 
  - Speed is limited to 1 stream, so it is slower and less reliable
  - Limited to 5GB upload
#### Multipart Upload
- Data is broken up. This is what BitTorrent uses
  - Minimum data size is 100MB for multipart
  - 10,000 max parts, 5MB to 5GB sizes
  - Last part can be smaller than 5MB
  - Parts can fail and be restarted
  - Transfer rate = speed of all parts

#### Accelerated Transfer (OFF by default)
- Uses AWS Transfer Acceleration 
- Connects to edge locations and uses the AWS network instead of using the normal internet connections to access the S3 bucket
  - Bucket name cannot contain periods
  - Bucket needs to be DNS compatible

>Video 4
#### Key Management Service (KMS)
- Regional and public service
- Creates, stores and manages keys
- Manages Symmetric and Asymmetric keys
- Cryptographic operations 
- #### Keys NEVER leave KMS. Provides FIPS 140-2 (L2) service (US level cert for safety)
- Called KMS keys (used to be called Customer Managed Keys, CMKs)
  - KMS Keys contain Id, Date, Policy, Description and State. Backed by physical key material
  - Generated or imported
  - Can be used for up to 4KB of data

How it works:
1) CreateKey - Key is created (or placed) in KMS
2) Encrypt call - Specifies what key to use and sends data to encrypt. KMS accepts the data, and uses the key to encrypt data if certs are good. Data is returned
3) Decrypt call - Data encrypt is sent to KMS. Key data is included inside the data and KMC knows what key to use to decrypt. KMC uses the key to decrypt the data and is returned to the user.

During this entire process, the Key NEVER leaves KMS. The user needs permissions to make calls to KMS (role separation)

#### Data Encryption Keys (DEKs)
- A type of key KMS can generate using KMS keys
- GeneratedDataKey - works on data that is > 4KB
- KMS does not store the DEK in any way. It just provides a DEK to the user/service for use and it is then thrown away.
- How it works
  1) A plaintext version and a ciphertext version of the key is provided to you or the server
  2) The plaintext version is used to encrypt the file
  3) The ciphertext version is attached to the file
  4) The file is stored with the ciphertext version and the plain text version is thrown away
- To decrypt
  1) Send the ciphertext key to KMS
  2) KMS will use the KMS key and decrypt the ciphertext key. The decrypted encryption key is provided back
  3) The decrypted encryption key is used to decrypt the data and is the discarded
- You are responsible for the DEK storage

To summarize:
1) KMS Keys are isolated to a region and never leave
2) KMS keys can be AWS Owned and Customer Owned
3) Customer managed keys are more configurable
4) KMS Keys support rotation - AWS managed KMS are auto rotated (cant be configured) but Customer Managed Keys can be configured
5) KMS Keys contain backing keys and previous backing keys (so new keys can be used on things with rotated keys that have been changed already)

#### Key Policies and Security
- Key policies (resource policy). Every key has one
  - KMS Key policies can be updated. This provides information to KMS on what is allowed to access the KMS key.
- Can use Key policies + IAM policies for managing KMS Keys. For more security, only using Key policies may be required. This will prevent admin from doing specific actions (say encrypt only)

>Video 5
#### S3 Object Encryption
Buckets are not encrypted, objects are.
- When you upload to S3 , it is encrypted automatically- Encryption in Transit
- Client-Side Encryption
  - You encrypt before you send, so AWS never gets to see unencrypted data
  - You do not rely on S3 for trusting their encrypting
- Server-Side Encryption
  - Objects themselves are encrypted by AWS. You trust AWS with your data

#### Server-Side Encryption with Customer-Provided Keys (SSE-C)
- Customer manages the customer keys
- S3 manages encryption
1) Provide the encryption key with your object upon upload
2) Object is encrypted with the key and a 1-way hash is generated
3) The key is thrown away
4) When you decrypt, you need to supply the same key. The object hash is checked against the provided key and only decrypted if the hash matches.

#### Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3) - AKA AES256 - Good default security option
- AWS (S3) manages the keys. You have no control over the keys. If someone is in full control of S3, they can technically access the plaintext data as they have access to the keys
- S3 manages encryption
1) Provide your object for upload to S3
2) S3 creates a single root key to encrypt objects in the S3 bucket
3) S3 creates a unique key for each object uploaded to S3
4) The unique key is used to encrypt the object
5) The unique key encrypted with the S3 root key and the plain text key is thrown away
6) The object and the encrypted key are now stored side-by-side

#### Server-Side Encryption with KMS Keys Stored in AWS Key Management Service (SSE-KMS) - More strict security
- AWS KMS manages the keys. KMS Keys provide Customer control and Rotation control if required.
- You can use customer supplied KMS keys for this, or AWS generated KMS keys
- To access encryption, you need access to both KMS and S3. This provides role separation by KMS
- S3 manages encryption
1) Provide your object for upload to S3
2) KMS creates a single root key to encrypt objects in the S3 bucket. This is stored in KMS
3) S3 creates a unique key for each object uploaded to S3
4) The unique key is used to encrypt the object
5) The unique key encrypted with the KMS root key and the plain text key is thrown away
6) The object and the encrypted key are now stored side-by-side

#### Bucket Default Encryption:
- When putting objects:
  - Header: x-amz-server-side-encryption
    - Specifying AES256 - SSE-S3 used
    - Specifying aws:kms - SSE-KMS used
  - If no header specified, no encryption is used
  - You can set a default setting on bucket default.
    - Default encryption is AES256
    - You can also use a bucket policy to specify the encryption type.

>Video 6
#### S3 Object Storage Classes

#### S3 Standard (default)
- #### Used for frequently accessed data that is important and non-replaceable - Super durable
- Stored across 3 AZ's in the AWS region with replication and Content-MD5 Checksums and Cyclic Redundant Checks (CRCs) to detect data corruption
- For upload of object in S3: Responds with HTTP/1.1 200 OK response
- Objects can be made publicly available
- "milliseconds" first byte latency - fast
  - Costs: 
    - GB/month for data stored
    - $ per GB for transfer OUT
    - Price/1000 requests
    - No specific retrieval fee, no minimum duration, no minimum size

#### S3 Standard-IA (Infrequent Access)
- #### Used for long-lived data which is important but access is infrequent
- Same as S3 Standard but differences:
  - Cheaper than S3 Standard
  - New cost: Cost per GB data retrieval fee
    - Cost increased with frequent data access
  - Minimum duration charge of 30 days
  - Has minimum capacity charge of 128KB/Object

#### S3 One Zone-IA (Infrequent Access)
- #### Long-lived data which is NON-CRITICAL and REPLACEABLE where access is infrequent
- Same as S3 Standard-IA but differences:
    - Cheaper than S3 Standard-IA
    - Only in 1 AZ - Does not provide multi-AZ resilience model of Standard and Standard-IA. High durability (same as others) but no redundancy - could lose it

>Video 7

#### S3 Glacier - Instant
- #### Long-lived data which is accessed once per quarter with millisecond access
- Cheap
- Same as S3 Standard-IA but differences:
  - Minimum duration charge of 90 days
  - Has minimum capacity charge of 128KB/Object
  - Costs more if you access the data, less if you don't

#### S3 Glacier - Flexible 
- #### Archival data where frequent or realtime access is NOT needed (e.g. use yearly). Minutes - hours retrieval
- Super Cheap
- Same as S3 Standard but differences:
  - Objects are cold - Objects cannot be made publicly. This requires a retrieval process to restore to S3 Standard-IA
    - Retrieval costs money. Faster is more expensive:
      - Expedited - 1-5 minutes
      - Standard - 3-5 hours
      - bulk - 5-12 hours
    - 40KB minimum size
    - 90 day minimum duration

#### S3 Glacier Deep Archive
- #### Archival data where it is rarely accessed. Ex. Legal or Regulation Data storage
- Cheapest
- Same as S3 Glacier - Flexible but differences:
    - Objects are cold - Objects cannot be made publicly. This requires a retrieval process to restore to S3 Standard-IA
        - Retrieval costs money. Faster is more expensive:
            - Standard - 12 hours
            - bulk - up to 48 hours
        - 40KB minimum size
        - 180 day minimum duration

#### S3 Intelligent-Tiering - Action Based
- #### S3 Intelligent-Tiering used for long-lived data where there are changing or unknown patterns of access
- There are no retrieval fees for accessing objects
- Allows storage with the following active changes to:
  - Frequent Access (S3 Standard)
  - Infrequent Access Tier (No access after 30 days)
  - Archive Instant Access (No access for 90 days)
  - Archive Access (No access for 90 - 270 days)
  - Deep Archive Access (No access for 180 - 730 days)
- These objects will be moved actively when they are accessed. You pay for management fee (per 1000 objects)

>Video 8
#### S3 Lifecycle Configuration - Time based
- S3 Lifecycle Configuration is a set of rules applied to a bucket or a group of objects
- Rules consist of actions:
  - Transition Actions (ex. transition to IA after 30 days and glacier after 180 days)
  - Expiration Actions (delete a file after n time period)
- Note: THESE ARE NOT BASED ON ACTIONS
- Transition Process: Goes from top down. You cannot go up, only down.
  1) S3 Standard (in lifecycle configuration, you need to wait a minimum 30 days before transition)
  2) S3 Standard-IA (in lifecycle configuration, you need to wait another minimum 30 days before transition to Glacier classes)
  3) S3 Intelligent-Tiering 
  4) S3 One Zone-IA (in lifecycle configuration, you need to wait another minimum 30 days before transition to Glacier classes)
  5) S3-Glacier - Instant Retrieval
  6) S3 Glacier - Flexible Retrieval
  7) S3 Glacier Deep Archive
- Smaller jobs can cost more money due to minimum sizes

>Video 9
#### S3 Replication
- Allows replication of a source S3 bucket to a destination bucket
- 2 Types:
  1) Cross Region Replication (CRR)
     - Replication of a source S3 bucket to a destination bucket in a different region
  2) Same Region Replication (SRR)
     - Replication of a source S3 bucket to a destination bucket in the same region
- Buckets can be in the same account or a different account
  - A replication configuration is applied to the source bucket to specify the
    1) IAM Role to be used
    2) The destination bucket to replicate to
  - Roles used to determine replication between source and destination buckets
    - Same Account 
      - Both buckets trust IAM and the Buckets and the Role. IE. The role just needs access to the buckets
    - Different Account
      - The bucket in the source account is not trusted by the other bucket, so a bucket policy will also need to be added to reference and allow the source bucket
#### Replication Options
1) All objects or a subset of objects
2) Which storage class in the new bucket to use - Default is the same class as the source bucket (can be overwritten)
3) Ownership - Default is to be owned by the source account (can be overwritten)
4) Replication Time Control (RTC) - Guaranteed level of predictability (15 min sync time Guaranteed)

### Exam Power Up
- Replication is not retroactive and versioning needs to be on. 
  - A bucket that already had objects in it will NOT have the objects in it replicated to the new bucket
- One-Way replication only.
- Unencrypted, SSE-S3 and SSE-KMS (with extra configuration). 
  - NOT able to replicate SSE-C
- Source bucket owner needs permission to the objects (only if the source account owns the objects, so if an object was placed in there that the owner does not have access to, there is no replication)
- No system events, Glacier or Glacier Deep Archive replication
- NO DELETES - Delete markers are not replicated, but it can be configured

#### Why use replication? (Same Region Replication vs Cross Region Replication)
1) SSR - Log Aggregation
2) SSR - Production to Test Sync
3) SSR - Resilience with strict sovereignty 
4) CRR - Global Resilience Improvements
5) CRR - Latency Reduction

> Video 10
#### S3 Presigned URLs
- A way to give people access to your private S3 bucket that they couldn't normally access
- Options to give someone access
  1) Give AWS Identity
  2) Provide AWS Credentials
  3) Make public
  4) use Pre-signed URLs

How to generate presignedURLs
1) Admin request S3 to generate presignedURL with a expiry date/time
2) S3 returns a presignedURL configured to expire at a specific date and time. This URL contains information about the permissions for the bucket that matches the admin who requested it
3) The presignedURL can be sent to an unauthenticated user
4) The user can use the URL to access the bucket with the permissions provided while the presignedURL is valid

For example, if a web application needs a presignedURL, we can create an IAM User for this 1 server (single principle) to allow it to create a presignedURL from the S3 bucket. This can create a presignedURL to provide access to the unknown user without the user authenticating

#### Exam Power Ups
1) You can create a presignedURL for an object you have no access to. Since it is linked to the user, it will have no access also (it has the same access as you)
2) The presignedURL permissions match the identity that generates it
3) Access denied could mean that the generating ID never had access, or it does not have access anymore (access changed)
4) Don't generate with a role. URL stops working when temporary credentials expire. (Roles are temporally provided, so when the role STS token access ends, so does the presignedURL - could cut the validity of the URL short)

>Video 11
#### S3 Select and Glacier Select 
- Normally, when you access an object (say a max size object of 5TB) you need to download the entire thing. You cant partially download something if you only needed part of it.
- S2/Glacier select lets you use SQL-Like statements to select part of an object, and only the pre-filtered selected part of the object is provided.
  - CSV, JSON, Parquet, BZIP2 compression for CSV and JSON

The 2 options between no select and select
- Download the entire object and filter on app
- Filter in S3 (with S3/Glacier select) before its transferred to the application

>Video 12
#### S3 Events
- When enabled, a notification is generated when an event occurs in a bucket
- Can deliver messages to SNS, SQS and Lambda functions
  - Objects Created (Put, Post, Copy, CompleteMultiPartUpload)
  - Objects Delete (*, Delete, DeleteMarkerCreated)
  - Objects Restore (Post (initiated), Complete)
  - Replication (OperationMissedThreshold, OperationReplicatedAfterThreshold, OperationNotTracked, OperationFailedReplication)
- Used with an Event Notification Config
- Require resource policies allowing S3 Principal Access 
- Event notification messages are JSON objects
- Can also use EventBridge as an alternative <- good alternative

>Video 13
#### S3 Access logs
- You can configure a S3 Log Delivery Group to listen to the source bucket and detect when the source bucket and it's objects are accessed. These interactions are then logged and saved in a target bucket.
- Configure/enabled via the console UI or via PUT Bucket Logging
- You need to configure a bucket ACL to allow S3 Log Delivery Group
- Requires you to manage the log files, no automated deletion etc.

>Video 14
#### S3 Object Lock 
- A group of features you enable on new buckets (Support requests for existing buckets)
- Implements a Write-Once-Read-Many (WORM) 
  - No delete, No overwrite
- Requires versioning - Individual versions are locked
- Has 2 things
  1) Retention Period
     - Specify Days and Years for a retention period
     - Modes:
       1) Compliance 
          - Cannot be adjusted, deleted or overwritten. 
          - No changes can be done during the retention period, not even the account root user
       2) Governance Mode
          - Special permissions can be granted allowing lock settings to be adjusted
            - s3:BypassGovernanceRetention AND
            - x-amz-bypass-governance-retention:true (default for console ui)
  2) Legal Hold
     - You do not set a retention period. It can only be turned on or off.
     - No deletes or changes until removed
       - s3:PutObjectLegalHold is required to add or remove
- Will have both, one or the other, or none
- A bucket can have default Object Lock Settings

### Virtual Private Cloud (VPC) Basics
>Video 1
#### VPC Sizing and structure 
- Need to think about the size of the VPC
- Networks you cannot use
- Ranges that other VPC's, Cloud, On-premises, Partners and Vendors use
- Predict the future
- Structure of the VPC - Ex. Tiers (Public and Private) & Resiliency (AZ)
- Minimum:
  - /28 (16IPs)
- Maximum
  - /16 (65536 IPs)
- Avoid common ranges

>Video 2
#### Custom VPC
- Nothing is allowed in or out without explicit configuration
- Isolated network expanding a region (can use all AZ in a region)
- Can work with Hybrid Networking - Other cloud and on-premises
- Default or Dedicated Tenancy <- Dedicated Tenancy is run on dedicated hardware, cannot be undone. All stuff is now placed in your equipment
- IPv4 private CIDR blocks and public IPs
- 1 Primary Private IPv4 CIDR Block
- Minimum /28 (16 IP)
- Maximum /16 (65536 IP)
- Optional secondary IPv4 Blocks
- Optional single assigned IPv6 /56 CIDR Block
- Fully equipped with DNS in a VPC provided by R53
- VPC "Base IP +2" address
- 2 Options:
  1) enableDnsHostnames - gives instances DNS Names
  2) enableDnsSupport - enables DNS resolution in VPC

>Video 3
#### Using Subnets
- Subnets inside a VPC start entirely private
- It is an AZ resilient area - a subnetwork of a VPC - within a particular AZ
- 1 Subnet is in 1 AZ. 1 Subnet CANNOT be in more than 1 AZ. 1 AZ can have multiple subnets.
- IPv4 CIDR is a subset of the VPC CIDR.
- The CIDR cannot overlap with other Subnets
- Optional for a IPv6 CIDR 
- Subnets can communicate with other subnets in the VPC
- There are 5 IPs you can never use in a Subnet (ex. 10.16.16.0/20)
  1) Network Address - The starting network of the network - (10.16.16.0)
  2) Network+1 - VPC Router - (10.16.16.1)
  3) Network+2 - Reserved (DNS*) - (10.16.16.2)
  4) Network+3 - Reserved Future Use - (10.16.16.3)
  5) Broadcast Address - Last IP in subnet - (10.16.31.255)
- Dynamic Host Configuration Protocall - DHCP Option Set - How VPC receives IP addresses
- Can define if Auto Assign Public IPv4 address
- Can define if Auto Assign IPv6 address

>Video 4
#### VPC Routing and Internet Gateway
- Highly available device inside any VPC located in every subnet (VPC Router IP = network+1)
- Routes traffic between subnets
- Controlled by route tables. Each subnet has one
- A VPC has a Main route table - subnet default, or a custom table (customized)
- A VPC Route table compares the destination address of the data with the route table and matches the highest priority address to the destination. (ex. /16 va /32, /32 is the highest priotity)
  - The target field will point to an external or internal route
    - Local = inside the VPC
    - If the VPC also has an IPv6, it will also have an IPv6 address for local targets

#### Internet Gateway
- Region resilient gateway attached to a VPC
- 1 IG per VPC, or 0 IG in a VPC
- Runs within the AWS Public Zone
- Gateway traffic between the VPC and the internet or AWS Public Zone (S3, SQS, SNS, etc.)
- Managed by AWS. From a user perspective: It just works.
  1) Create an IGW
  2) Attach IGW to a VPC
  3) Create a custom Route Table (RT)
  4) Associate a RT
  5) Default routes to the IGW
  6) Subnet allocate IPv4
- The VPC is now public when the subnet can communicate directly with the internet
- #### When you create a public IPv4 address in an instance, it actually gets held by the IG and the private IPv4 address gets transformed once it hits the IGW. Therefore, IPv4 is configured in the IGW.
- Therefore, the IG holds a record of the private IPv4 address. When a packet comes to the IG from a service destined to an external public IPv4 address, it changes the "source" to the IG IPv4 address and holds a record. When it comes back, the destination is to itself, and it changes the destination to the private IPv4 resource from the record.

#### Bastian Host/Jump Boxes
- The same thing. 
- Instance in a public subnet
- Incoming management connections arrive there. You can then access internal VPC resources
- Often the only way INTO a private only VPC

>Video 5
#### Stateful and Stateless Firewalls
- Recall Transmission Control Protocol (TCP) is used to transfer data in packages from port to port (req and response)
  1) Client picks a temporary source port (ephemeral)
  2) Client initiates a connection to the server on a well known destination port
  3) The server responds using source port connected to with the destination on the ephemeral port 
- When you add firewall routes, you need to look at the reference of where the firewall is to be implemented and apply inbound and outbound rules (wrt directionality)
- #### For Stateless:
  - Every communication will require 2 rules - Inbound and Outbound which are the inverse of each other
  - One port is always ephemeral ports -> This is why we use stateless firewalls, because we do not know what the ephemeral port is (not the well known app port)
- #### For Stateful
  - Stateful firewalls are intelligent enough to identify the request and response components of a connection as being related
  - You will only allow the request (inbound or outbound) and the response is automatically allowed 

>Video 6
#### Network Access Control Lists (NACL) - Important for VPC Subnet security
- Similar to a traditional Firewall associated with all Subnets in a VPC
- Each network NACL only affects data entering or leaving the subnet. 
- It does not affect anything inside the subnet (connections inside subnet are NOT affected)
- NACL's are stateless. Only direction matters.
  - Each connection requires 2 rules: 
    1) A Request rule
    2) A Response rule
  - Note: For ephemeral ports, we will need to use " * " as we have no idea what the ephemeral port will be
- Inbound rules
- Outbound rules
- Rules match DST IP/Range, SDT Port and Protocol 
- No logical resources
- Allow or Deny
- Rules are processed in order, lowest rule number first. 
- Once a match occurs, processing STOPS
- " * " is an implicit Deny if nothing else
- On default (when a VPC is created with a Default NACL) 
  - Inbound and Outbound rules have an implicit deny ( * ) and an Allow all rule
  - The result is all traffic is allowed, the NACL has no effect (to be beginner friendly)
- On Custom VCPs, they are configured with NO subnets and only have 
  - 1 Inbound rule, an Implicit ( * ) deny
  - 1 Outbound rule, an Implicit ( * ) deny
- Cannot be assigned to AWS resources
- Used together with Security Groups to add explicit Deny
- Each subnet can only have 1 NACL (Default or custom)
- NACL can be associated with MANY subnets

>Video 7
#### Security Groups
- Security groups are Stateful - Detect response traffic automatically
- Allow (IN or OUT) requests = allow responses
- No explicit deny - Only Allow or Implicit deny
  - Cannot be used to block specific IPs or ranges
  - This is why NACLs and SG are used together
- Support IP/CIDR and Logical Resources - Including other SG and itself
- Attached to Elastic Network Interfaces (ENI's) and not instances
- #### SG are attached to Network Interfaces!

#### SG Logical References
- You can reference another SG in the SG, which will allow any instance with the other SG to access the resource. This will allow you to scale an application without worrying about IP ranges or CIDR ranges.
- This is something you should use for scaling (automatically works for scaling up and down)
- Referencing any resource with a SG in it
- Allows self-referencing 
  - Allows all applications with the SG attached to it to reference other applications with the SG attached. IP Changes are automatically handeled

>Video 8 and 9
#### Network Address Translation (NAT) and NAT Gateway
- A set of processes that can remap source (SRC) or destination (DST) IPs
- IP Masquerading - Hiding a CIDR block behind 1 IP
  - Works well because IPv4 are running out
- Gives private CIDR range outgoing internet access
- You cannot access IP addresses behind a NAT, only they can access external addresses and accept responses
- NAT Gateway is provisioned into a public web subnet (so it can access the Internet Gateway)
- Uses a route table to point all instances (0.0.0.0/0) at the NAT Gateway, which will then point out to the IG
- Uses Elastic IPs (static IPv4 Public)
- AZ Resilient Service (HA in the AZ)
- For regional resilience, NATGW need to be in each AZ
  - #### Then you need Route Tables in each AZ to point at that AZ's NATGW as the target
  - Managed by AWS, scales up to 45 Gbps, $ Duration and Data Volume

1) NAT Gateway stores where the package is from and data (in a translation table). 
2) It then changes the source IP from the private IP into the NAT Gateway's private IP address. 
3) This package is then sent to the IG and the NAT Gateway's source private IP address is changed to the IG Public IP Address is sent to the destination
4) When the package is returned, the new destination is for the IG's Public Address
5) The IG translates the destination from it's destination Public IP address to the NAT Gateway's private IP Address
6) The NAT Gateway then translates the package's destination IP address to the original private IP address using the translation table

#### NAT Instance
- NAT Instances are just NAT Gateways that are run on EC2 instances
- If you implement a NAT Instance, you need to disable Source and Destination Checks to use an EC2 as a NAT instance
- In general, use a NAT Gateway as its easier

#### General Notes
- NATGW you can only use NACLs, NAT Instances you can use both NACLs and Security Groups (as they are instances)
- NAT does NOT work with IPv6
- If you add ::/0 for an IPv6 and point it at the IGW, iw will be good for Bi-directional connectivity 
- If you want to give outgoing only internet access, you need to use an Egress Internet Gateway

### EC2

>Video 1
#### EC2 Architecture
- EC2 Instances are virtual machines (OS + Resources)
- EC2 Instances are run on EC2 Hosts
- Shared Hosts or Dedicated Hosts (your own dedicated host, you do not share the physical hardware)
- Host = 1 AZ -> AZ Fails, host fails
- EC2 Has
  - Instance Store (Temporary storage on host)
  - Storage EBS (Storage accessible on the AZ) - Volumes that can be shared among instances in AZ
  - If a host falls over, a new host is provisioned in the same AZ
  - You CANNOT connect AZ specific things like EBS to another EBS or instance in another AZ
- EC2 is good for long-running compute
- Traditional OS and Application
- Server style application
- Burst or steady-state load
- Monolithic application stacks
- Migrated application workloads or disaster recovery

>Video 2 and 3
#### EC2 Instance Types
- Raw CPU, Memory, Local Storage Capacity and Type
- Resource Ratios ($/what you get)
- Storage and DAta Network Bandwidth
- System Architecture / Vendor
- Additional Features and Capabilities (ex. GPUs)

#### EC2 Main Categories
1) General Purpose - Default - Diverse workloads, equal resource ratio
   - A1, M6g - ARM based processors. Efficient
   - T3, T3a - Burst Pool - Cheaper assuming nominal low levels of usage with occasional peaks
   - M5, M5a, M5n - Steady state workload alternative to T3a/3a - Intel/AMD
2) Compute Optimized - Media processing, HPC, Scientific Modelling, Gaming, Machine Learning
   - C5, C5n - Media encoding, scientific modeling, gaming servers, machine learning
3) Memory Optimized - Processing large in-memory datasets, database workloads
   - R5, R5a - Real time analysis, in memory caches
   - X1, X1e - Large sale in-memory applications
   - High Memory (u-Xtb1) - Highest memory of all AWS instances
   - z1d - Large memory and CPU
4) Accelerated Computing - Hardware GPU, field programmable gate arrays (FPGAs)
    - P3 - GPU instances (Tesla v100) - Parallel processing and AI
    - G4 - GPU instances (NVIDIA T4 Tensor) - Machine learning
    - F1 - Field Programmable Gate Arrays - Genomics, Big data
    - Inf1 - Machine learning
5) Storage Optimized - Sequential and random IO - Scale-out transactional databases, data warehousing, Elasticsearch, analytic workloads
    - I3/I3en - Local high perfoamnce SSD (NVMe) - NoSQL databases, warehouses
    - D2 - Dense Storage (HDD) - Data Warehouses, HADOOP, Data processing - Lowest price disk throughput
    - H1 - High Throughput, balance CPU/Mem, Big data

#### EC2 Naming Scheme
- R5dn.8xlarge <- instance type
  - R <- Instance Family
  - 5 <- Instance Generation
  - dn <- Additional capabilities 
  - 8xlarge <- instance size for the generation
    - How much resources are allocated to the generation

>Video 4
#### S3 Storage
- Direct (local) attached storage - Storage on EC2 hosts
- Network attached storage - Volumes delivered over the network (EBS)
- Ephemeral storage - Temporary storage
- Persistent storage - Permanent storage - lives on past the lifetime of the instance

#### Key terms
- Block storage - Volume presented to the OS as a collection of blocks. No structure is provided. 
  - Mountable and bootable.
- File storage - Presented as a file share. Has structure. 
  - Mountable. NOT bootable
- Object storage - Collection of objects. Flat storage 
  - Not mountable. Not bootable

#### Storage performance
- IO (Block size)
  - The SA area
  - Size - 15K vs 64K vs 1MEG etc. Each block is n size
- IOPS
  - The velocity
  - How many read/writes the storage can accomplish
- Throughput
  - The flux
  - The rate at which the storage can store data.
  - This is calculated by IO x IOPS => (Block Size) x (Rate of Read/write)

>Video 5
#### Elastic Block Store EBS
- Block storage - Raw disc allocations (volumes) - can be encrypted using KMS
  - Instances see a block device and creates a file system on this device (ext3/4, xfs)
- Storage is provisioned in One AZ. Isolated to that AZ. Resilient in that AZ
- Attach a volume to one EC2 instance at a time (or other service) over a storage network. (can be attached to clusters but it needs to be controlled by the cluster to not overwrite stuff)
  - Can be deteched and reattached and is not lifecycle linked to one instance. Persistant
- Snapshot (backups) into S3. Can create a volume from a snapshot (migrate between AZs)
- Different physical storage types, different sizes, different performance profiles
- Billed based on GB/month (and sometimes by performance)

>Video 6
#### EBS Volume Types

#### General Purpose SSD - GP2
- GP2 is built around bursting IOPs.
  - You have a burst pool of IOP credits and your stream rate will consume credits
  - Credits are refilled based on the instance you have
  - You need to manage credit volumes over all your volumes
  - Volumes >1TB do not have a credit system and you always achieve baseline

#### General Purpose SSD - GP3
- You get a standard IOPS and MiB/s as standard
- If you want to exceed the IOPS, you need to pay for more throughput
- Basically the same as GP2 but easier to understand

>Video 7
#### EBS Provisioned IOPS SSD (io1/2)
- IOPS are configured independently of the volume size
  - These storage options are consistent low latency with low jitter
- Per Instance Performance is maxed per type of instance. You can only add up to a maximum IOPS rate 

>Video 8
#### EBS Hard Disc Drive (HDD) Based - ST1 and SC1
- General Usage types
  - st1 - Throughput Optimized
    - Cheap
    - IOPS max of 500 (in 1 MB chunks)
    - Block rate max of 500 MB/s
    - Big data, data warehouses, log processing
  - sc1 - Cold HDD
    - Cheaper (lowest cost)
    - IOPS max of 250 (in 1 MB chunks)
    - Block rate max of 250 MB/s
    - Cold data requiring fewer scans per day

>Video 9
#### Instance Store Volumes
- Instance store volumes provide Block Storage Devices
- Physically connected to one EC2 Host. Local to EC2 Host Only
  - Lost on instance move, resize or hardware failure
- Instances on the host can access them
- Highest storage performance in AWS
- Included in the instance price
- #### Need to attach them at launch. CANNOT ATTACH AFTER LAUNCH!
- The IOPS of Instance Store Volumes are MUCH higher than any other storage class

>Video 10
#### Choosing between Instance Store and EBS
- Persistence Required = EBS (Instance Store can have data loss)
- Resilience = EBS (EBS can be backed up into S3)
- Storage isolated from instance lifecycle = EBS
- Resilience w/app in-built replication = It depends
- High performance needs = It depends
- Super high performance needs = Instance Store
- Cost = Instance Store (its often included)

#### Important Notes
- Cheaper = ST1 or SC1
- Throughput or Streaming = ST1
- Boot = NOT ST1 or SC1 <- Mechanical storage type
- GP2/3 - can deliver up to 16000 IOPS
- IO1/2 - can deliver up to 64000 IOPS (can deliver 2560000 with a huge instance)
- You can take multiple EBS volumes and create a RAID0 set - Up to 260000 IOPS (io1/2-BE/GP2/3)
- More than 260000 IOPS - Use Instance Store

>Video 11
#### EBS Snapshots
- Backups of volume copies stored in S3
- Snapshots are incremental in nature - the first snapshot has a full copy of all data on the volume
  - Subsequent snapshots of the same volume only stores the difference between the initial snapshot and the current snapshot
  - There is no risk of losing data if you delete an incremental snapshot. Each snapshot can be thought of as self sufficient
  - Volumes can be created (restored) buy snapshots
  - Snapshots can be copied between regions

#### EBS Snapshot/Volume Performance
- New EBS Volume = full performance immediately
- Snapshots restore lazily - fetched gradually
  - If you requested blocks that are not yet restored, they are fetched immediately
  - You can also force a read of all data immediately
- Fast Snapshot Restore (FSR) - Immediate restore a snapshot. Allows you to restore faster (costs more)
  - Can have up to 50 FSR snaps per region. Set on the Snapshot and AZ

#### Billing
- You are billed on used data. You only get charged what you use for your snapshot.
  - A snapshot will not cost more if you do more snapshots, as it consumes only the changed amount of data (you only pay what you add)
  - You are only charged for changed data

>Video 12
#### EBS Encryption
- KMS can be used to encrypt a EBS volume using a KMS key (supplied by you or created by AWS)
- An EBS encryption key is created to encrypt data stored on the volume. The EBS key is held on the EC2 host to encrypt all data saved on the volume by the EC2.
- AWS accounts can be set to encrypt by default - default to KMS Key 
  - Otherwise choose a KMS key to use
- Each volume uses 1 unique Data Encryption Key (DEK)
- Snapshots and future volumes use the same DEK
- You cannot change a volume to NOT be encrypted
- The OS itself is NOT aware of any encryption. There is no performance loss. You can use OS encryption, but you have to configure it on the OS itself

> Video 13
#### Network Interfaces, Instance IPs and DNS
- Every instance has an Elastic Network Interface (ENI) attached to the instance
  - You can add more than 1 ENI to an instance (secondary ENI)
- ENI's have a MAC Address (unique)
- Primary IPv4 Private IP <- this is what holds the IP address - Does not change for the life of the instance - ex. 10.16.0.10 -> ip-10-16-0-10.ec2.internal <- inside VPC
  - 0 or more secondary IPs
  - 0 or 1 Public IPv4 Address <- This will change whenever you turn off the instance
    - 3.89.7.146 -> ec2-3-89-7-136.compute-1.amazonaws.com (this is provided by AWS, you cannot change)
  - 1 elastic IP per private IPv4 address
    - Renoves the Public IPv4 
    - Replaces with the Elastic IP
  - 0 or more IPv6 addresses
  - Security Groups
    - This means you may have multiple ENI's, in which case you need to configure the SG for each ENI
  - Source/Destination Check - Traffic is discarded if there is no Source and Destination
- A secondary IPv4 address with the same as above

#### Exam Power Ups
- Secondary ENI + MAC = Can be used for Licensing
- Multi-homed (subnets) Management and Data
- Different Security Groups - Multiple Interfaces required
- The instance is different from the ENI (even though it looks like you connect directly to the instance)
- OS - Does not see the Public IPv4 EVER, only sees the private! 
- IPv4 public IPs are dynamic. When you stop and start, the public IP changes
  - If you want it to NOT change, you need to assign an Elastic IP address
- Inside the VPC, the Public DNS will always resolve to the Private IP. Outside the VPC, the Public DNS will resolve to the Public IP

>Video 14
#### Amazon Machine Images (AMI)
- Images of EC2 (templates of instances)
  - Can be used to launch EC2 instances
  - AWS or community provided
  - Marketplace (commercial software)
  - Regional with unique ID. Each region has their own AMI IDs
  - Permissions (Public, Your Account, Specific Accounts)
  - You can create an AMI from an existing EC2 instance to make new EC2 instances

#### AMI Baking
1) Launch an AMI instance (example attach boot volumes)
2) Configure the AMI (example configure the volumes)
3) Create Image (Create snapshot of the AMI instance) - It will create a snapshot that contains 
   1) the same EBS volume configuration as the original (same device Ids)
   2) the EBS volume itself
4) You can launch the new AMI, which will reference the snapshot to create replicas of the boot volumes with the same configurations as your initial AMI

#### Exam Powerup
- AMI - One region only - Only works in that one region
- AMI Baking - Creating an AMI from a configured instance + application
- An AMI cannot be edited. You need to launch the instance, update the configuration and make a new AMI.
- AMIs can be copied between regions (including its snapshots)
- Remember permissions, default = your account
- AMI's contain Snapshots, so you will be charged for snapshots

>Video 15, 16, 17
#### EC2 Purchase Options (Launch Types)

#### On-Demand
- On demand instances are isolated, but multiple customer instances run on shared hardware
- Instances of different sizes run on the same EC2 Hosts, consuming a defined allocation of resources
- Per-second billing while an instance is running. Associated resources such as storage, consume capacity, so bill, regardless of instance state
- Default purchase option
  - No interruptions
  - No capacity reservations
  - Predictable pricing
  - No upfront costs
  - No discount
  - Short term workload
  - Unknown Workload
  - Apps that CANNOT be interrupted

#### Spot Pricing
-  AWS selling unused EC2 host capacity for up to a 90% discount. The spot price is based on the spare capacity at a given time, which AWS publishes
  - Going rate is what you pay. You define your price and if the spot price is below your maximum price, you will run your instance at the spot price
- Spot is NOT reliable. You can have your instance end at any time depending on the spot price maximum
  - Non time critical
  - Anything that can be rerun
  - Bursty capacity needs
  - Cost sensitive workloads
  - Anything that is stateless

#### Reserved Instances
- A commitment made to AWS for usage of instances for an amount of time
- Reservations need to match the instance. You will then receive a reduction in price as long as it is within the reservation
- Unused reservation are still billed
- Reservations can also provide partial coverage for larger instances
- Commitments can be from 1 year to 3 year terms. You get more discount for longer
- You can pay:
  - No upfront saving - Full per second fee
  - Partial upfront - Reduced per second fee
  - Pay all upfront - No per second fee

#### Scheduled Reserved Instances
- Scheduled reserved is ideal for long term usage which does not run constantly
- Commitment to specify the frequency, schedule and size
  - Purchase a scheduled window where you have access to the instance
  - Minimum time period is to reserve for a minimum of 1 year
    
#### Capacity Reservations
- Reservations are specific. Unlike reserved instances where you just pay to access the instances, capacity reservations are more specific where you reserve capacity within the AZ
- Zonal reservations only apply to one AZ providing billing discounts and capacity reservations in that AZ
- On-demand capacity reservations can be booked to ensure you always have access to capacity in an AZ when you need it - but at full on-demand price. 
  - No term limits, but you pay regardless of if you use it

#### EC2 Saving Plan
- An hourly commitment for 1 to 3 year terms
- A reservation of general compute $ amounts and you then get a discount for the hourly rate
  - Or a specific EC2 Saving plan - flexibility on size and OS of the instance
- Applies to EC2, Fargate and Lambda
- Products have an on-demand rate and a saving plan rate
- Resource usage consumes sabing plan commitment at the reduced saving plan rate
- If you exceed the saving plan commitment, you pay the on-demand price

#### Dedicated Hosts 
- A host that is dedicated to you entirely. No shared pool of resources from other users
- Hosts come with all the resources you expect, so you can fill the resource capacity of the host with any size of instance that can fit on the host
- Used for things like Licensing requirements that are associated with a physical machine (socket/core licensing)

#### Dedicated Instances
- Your instances run on an EC2 host with other instances of yours, with NO ONE running on the host
- AWS commits to not sharing the host with others but manages the host for you (less overhead and admin)
- You do not own or share the host. Extra charges apply for the instance so you get dedicated hardware (for example, security reasons)

>Video 18
#### Instance Status Checks
- All instances have 2 high level status checks for setting up an EC2 Instance. These can be configured to take actions if they are triggered (example restart, start a new EC2 instance, SNS etc.)

>Video 19
#### Scaling
- #### Horizontal Scaling
  - Increasing capacity by increasing the number of instances and having them work together on a workload
    - Spinning up more instances to deal with a workload
    - Stateless
    - No disruption while you scale
- #### Vertical Scaling
  - Increase capacity by increasing the instance side
    - Upgrade the instance type

### Containers and ECS
>Video 1
#### Containers
- Dockerfiles are used to build images
- Use the parent OS for hosting.

>Video 2
#### Elastic Container Service (ECS)
- ECS orchestrates your containers for you in a cluster
- Runs in 2 modes
  1) Fargate mode - AWS manages the clusters and you run your containers on the AWS manages clusters (less overhead)
  2) EC2 Mode - You have an EC2 that has ECS running on the EC2 instance and EC2 manages the container in your instance
  - You use a container definition to provide information for the container you want to run. A pointer to where the container is stored and what port is exposed.
  - Task Definition - Represents the task as a whole. 
    - Resources used by the task
    - Networking
    - Compatability (EC2 or Fargate)
    - The task Role - The permission role that the container will have to access EC2 resources
      - IAM role that the task will have access to
    - Can include 1 or more containers
- A Service Definition - Determines how you want to scale the task
  - How many Copies, High Availability, Restarts, Monitoring. How to deal with incomming load.

>Video 3
#### ECS Cluster Types
#### EC2 Mode
- An EC2 Cluster runs in the VPC, so you can have multiple AZ's and this allows you to have High Availability
- An Auto Scaling Group will control the number of tasks that will run in each AZ
- Uses a container registry, tasks and AMI images to spin up containerized services inside the EC2 hosts 
- At a cluster level, you need to manage the capacity of the container hosts (you have to manage the containers inside the EC2 instances). 
- More admin.

#### Fargate Mode
- You do not have to manage EC2 instances for use as container hosts.
- Same as EC2 mode but AWS maintains a shared Fargate shared infrastructure platform
  - You can run containers on shared hardware and use it as a service
- Still use VPCs as if this was an EC2 mode
- The fargate resources are injected into your VPC (given network interfaces in your VPC) which you can access
- You only pay for the containers you are consuming

#### When to use what
- If you use containers - use ECS (EC2 mode or FG mode)
- When you have a large workload and are price conscious - EC2 Mode (only if you can manage the admin costs)
- When you have large workloads and are overhead conscious - Fargate mode
- Small/Burst workloads - Fargate
- Batch/Periodic workloads - Fargate

>Video 4
#### Elastic Container Registry (ECR)
- Managed container image registry
  - Think Docker Hub, but in AWS
- Each AWS account has a public and private registry
- Each registry can have many repositories
- Each repository can contain many images
- Images can have several tags
- Public = Public Read/Only access, but Read/Write requires permissions
- Private = Permissions required for any Read/Only or Read/Write access

#### Important points
- Integrated with IAM - Permissions
- (Inspector) - Image scanning, basic and enhances 
- Cloud Watch - near real-time metrics (auth, push, pull)
- Cloud Trail - API Actions
- EventBridge - Events
- Replication - Cross region and cross account

>Video 5
#### Kubernetes (K8s)
- Cloud agnostic product
- Cluster - A deployment of Kubernetes, management, orchestration...
- Node - Resources: Pods are placed on nodes to run
- Pod - 1+ containers: smallest unit in K8s; often 1 container 1 pod
- Service - Abstraction - service running on 1 or more pods
- Job - ad-hoc, creates 1 or more pods until completion
- Ingress - Exposes a way into a service (ingress -> routing -> service -> 1+ pods)
- Ingress controller - used to provide ingress (eg. AWS Load Balancer uses Application Load Balancer or a Network Load Balancer)
- Persistent Storage (PV) - Volume whose lifecycle lives beyond any 1 pod using it

>Video 6
#### Elastic Kubernetes Service (EKS)
- AWS managed K8s - A specially optimized AWS implementation of K8s
  - Run in different ways: 
    - AWS <- normal (running EKS in AWS)
    - Outpost
    - EKS Anywhere
    - EKS Distro
- Control plane scales and runs on multiple AZs
- Integrates with AWS services (ECR, ELB, IAM, VPC)
- EKS Cluster = EKS Control Plane and EKS notes
- etcd (the key value store that K8s uses) is managed by AWS and is distributed across multiple AZs
- Nodes options
  - Self Manages
  - Managed node groups 
  - Fargate pods
- Windows, GPU, Inferentia, Bottlerocket, Outpost, Local zones.... check node type
- Storage providers include EBS, EFS, FSx Lusture, FSx for NetApp ONTAP
- How it works
  - EKS control plane is managed in an AWS managed VPC.
  - Control plane ENI injected into customer VPC
  - The worker nodes (EC2) are controlled by the control plane
  - Secondary EKS Admin public endpoint available that connects the worker nodes back to the Control Plane and can also connect to the service consumer

### Advanced EC2
> Video 1
#### Bootstrapping EC2 using User Data
- A process which allows a system to self configure. Allows EC2 Build Automation 
- This is enabled using User Data - Accessed via the meta-data IP
  - http://169.254.169.254/latest/user-data
- Anything on User Data is executed by the instance OS
- Only done once at Launch
- EC2 does NOT interpret the data. The OS needs to understand the User Data

#### How it works
1) An AMI is used to set up an instance
2) EC2 injects User Data into the instance
3) The instance executes the user data
   1) The service is ready to run (Running)
   2) The service fails to run, bad configuration (Failure)

#### User Data Key Points
- Its opaque to EC2 - It is just a block of data
- It's not secure. Do not use it for passwords or secrets or long term credentials
- User data is limited to 16kb in size
- User data can be modified when an instance stops
- Only executed once at launch

#### Boot-time-to-service-time
- Minutes from AMI -> Launch time (without Bootstrapping)
- Post Launch Time (Bootstrapping) can automate configuration, make it quicker
- AMI Baking - Pre-configure the instance, create an AMI, then launch the new AMI
  - You can Bake and then do small amounts of bootstrapping at the end to lower the time-to-boot

>Video 2
#### AWS::CloudFormation::Init
- cfn-init - helper script that is installed on EC2 OS
  - Simple configuration management system
  - Procedural (User Data) vs desired state (cfn-init)
- Can make sure packages, groups, users, sources, files, commands and services are installed/configured on boot of the OS
- Passed into the instance in User Data
- Provided with directives via Metadata and AWS::CloudFormation::Init on a CFN resource
- Can also piggyback off of a CreationPolicy (Located in the template) and Signals (located in the UserData) to signal an instance is complete ONLY when the bootstrapping has been completed successfully

#### How it works
1) A template contains the Metadata with AWS::CloudFormation::Init configured
2) The template also contains User Data
3) The stack is created that includes an instance
4) The instance is configured with the User Data
5) The stack then coordinates with CloudFormation to automate various configurations
6) If a CreationPolicy and Signal has been configured, this will be reported to the stack after bootstrapping and then it is reported to the Stack, which will report to CloudFormation as completed (and then CloudFormation can take further steps)

>Video 3
#### EC2 Instance Roles
- IAM Role has a permission policy attached to it
- IAM Role allows an EC2 Service to assume it
- This requires an InstanceProfile, which is what is actually attached to the instance via the meta-data
- in meta-data
  - iam/security-credentials/role-name
- Automatically rotated - always valid
- Should always use ROLES rather than adding access keys into instances
- CLI tools will use ROLE credentials automatically

>Video 4
#### AWS System Manager Parameter Store
- Storage for configurations and secrets
- Stores 3 types of params
  1) Strings
  2) StringLists
  3) SecureStrings
- License codes, Database strings, Full configs and Passwords
- Hierarchies and versioning
- Plaintext and Ciphertext
- Public Parameters - Latest AMIs per region
- A Public service - anything accessing it needs to have public access
- Connected to IAM - Services can access it 
- SSM Parameter Store can access KMS (you can secure parameters with KMS)
- You can initiate changes in parameter store to create events
- Can work in a tree format 
  - /wordpress/DBUser can store DBUsers in the /wordpress/ tree

>Video 5
#### System and Application Logging on EC2
- Cloudwatch is for metrics
- Cloudwatch Logs is for logging
- Neither of these 2 can natively capture data inside an instance
- Cloudwatch agent is required to capture OS visible data
  - Also requires permissions and configurations
- You can set up CloudWatch Agent in the Parameter Store and just set it to your instances, which automates configuration and logging at scale

#### How it works
1) CloudWatch Agent needs to be attached to an instance
2) An Agent Config needs to be configured to set what metrics and logs we want to measure
    - Metrics and logs are configured for every log file that we want to stream into CloudWatch Logs
      - ex. /var/log/httpd/error_log
      - ex. /var/log/secure
3) We need to configure a role to allow us to connect to CloudWatch Logs

>Video 6
#### EC2 Placement Groups
- Placement groups allow you to configure where your EC2 instances are placed
- There are 3 ways to place them
  1) Cluster placements - Pack instances close together
  2) Spread - Keep instances separated
  3) Partition - Group of instances spread apart

#### Cluster Placement Groups
- Used for the absolute highest level performance
- Typically you need to launch all identical instances all at the same time
- The idea is that all instances in a placement group are placed within the same rack and ideally in the same EC2 instance group
- All members have direct connections to each other and have the lowest latency possible with the max packages per second possible in AWS
- Due to placement, they offer little to no resilience (if EC2 fails, all fail)
- Cannot span AZs - ONLY 1 AZ (locked when launching first instance)
- Can span VPC Peers, but impacts performance
- Requires a supported instance type
- Use the same type of instance (not mandatory)
- Launch at the same time (not mandatory... but highly recommended)
- 10Gbps single stream performance
- Use Cases: Performance, Fast Speed, Low Latency

#### Spread Placement Groups
- Spread Placement Groups are seperated into different racks to isolate failure for each instance
- Max 7 instances per AZ - Isolated infrastructure limit
- Provides infrastructure isolation
  - Each instance runs from a different rack
- Each rack has its own network and power source
- Not supported for dedicated instances or hosts
- Use Case: Small number of critical instances that need to be kept separated from each other - Max resilience of your application

#### Partition Placement Groups
- Can be created across multiple AZ in a region
- Divided into partitions, with a max of 7 partitions per AZ
- Each partition has its own racks - no sharing between partitions
  - You get to control which partition you will launch into
- Meant for large scale processing with high resilience - Allows you to also place compute in specific areas that are topology aware (replication)
- Instances can be placed in specific locations or auto placed
- For topology aware applications
  - HDFS, HBase and Cassandra
- Contain the impact of failure to part of the application 

>Video 7
#### Dedicated Hosts
- EC2 Host that is dedicated to you entirely
- Specific family (ex. a1, c5, m5)
- No instance charges since you pay for the entire host
- On demand and reserved options available
- Host hardware has physical sockets and cores you have access to
  - various instances will take different amounts of cores
- Hosts are typically designed to operate specific sized instances in specific configurations. Only some allow you to mix and match

#### Limits and features
- AMI Limits - RHEL, SUSE Linex and Window AMIs are NOT support
- Amazon RDS instances not supported
- Placement groups are not supported for dedicated hosts
- Hosts can be shared with other ORG accounts. 
  - With RAM (Resource Access Manager) you can share the resources
    - There are ways you can use security separation where you can see all instances run in your host, but you cannot access them, and organizations can only see their instances but not other instances

>Video 8
#### Enhanced Networking and EBS optimized
#### Enhanced Networking
- Required for any advanced cluster groups
- Uses SR-IOV - NIC is virtualization aware
  - Without SR-IOV, all instances share 1 network card, which causes networking issues
  - With enhanced networking, there are multiple logical network cards per physical card that the instances individually connect to, which frees up the host resource requirements 
- Increases bandwidth
- Much higher I/O and lower Host CPU usage
- Higher package-per-second (PPS)
- No charge - available on most EC2 types
- Consistent lower latency

#### EBS Optimized instances
- EBS is block storage over the network
- Historically network was shared, both data and EBS
- EBS Optimized means there is dedicated capacity for EBS
- Most instances support and enabled by default
  - For some instances, it is supported but costs extra
- Adding dedicated capacity for storage networking to an EC2 instances