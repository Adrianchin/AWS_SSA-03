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

### Route 53
>Video 1
#### R53 Hosted Zones
- Route53 Hosted Zone is a DNS DB for a domain (eg. animals4life.org)
- Globally resilient (multiple DNS servers)
- Created with domain registration via R53 - can be created separately
- Host DNS Records (eg. A, AAAA, MX, NS, TXT ...)
- Hosted zones are what the DNS system references - Authoritative for a domain 

#### R53 Public Hosted Zones
- DNS Database (zone file) hosted by R53 (Public Name Servers)
- Accessible from the public internet and VPCs
- Hosted on "4" R53 name servers (NS) specific for the zone
  - Use the NS records to point at these NS (connect to global DNS)
- Resource Records (RR) created within the Hosted Zone
- Externally registered domains can point at R53 Public Zone

>Video 2
#### R53 Private Hosted Zones
- A public hosted zone which is not public
- Associated with a VPCs
  - Only accessible in those VPCs
  - Using different accounts is supported via the CLI/API
- Split-view (overlapping public and private) for public and internal use with the same zone name

#### R53 Split View Hosted Zones 
- You can create a public zone in your R53 private zone that is accessible from the public internet (The public DNS server will be able to point to your R53 private zone but not the public zone parts)
  - Example - Want some sections (say accounting) to be only accessible with your VPC but the rest accessible to the internet, you would place the accounting address in the private zone and make a public zone to cover the rest

>Video 3
#### R53 CNAME vs ALIAS
- "A" record maps a name to an IP Address
  - ex. "catagramio" = 1.3.3.7
- CNAME maps a name to another name
  - ex. "www.catagram.io" = "catagram.io"
  - CNAME is invalid for naked/apex (catagram.io)
  - Many AWS services use a DNS name (ELBs)
  - With just CNAME - catagram.io -> ELB would be invalid (cannot use CNAME)
- ALIAS records map to a NAME to an AWS resource
  - These can be used for Naked/Apex and normal records
  - For non apex/naked - functions like CNAME
  - There is no charge for ALIAS requests pointing at AWS resources
  - For AWS Services - default to picking an ALIAS
  - Should be the same "type" as what the record is pointing at
  - API Gateway, CloudFront, Elastic Beanstalk, ELB, Global Accelerator and S3
  - ALIAS is an AWS specific feature

>Video 4
#### R53 Simple Routing
1) Simple routing supports 1 record per name (www) - A Record
2) Each record can have multiple values that it points to and returns
3) All values are returned in a random order
4) The client chooses and uses 1 value (there is 1 record that is mapped to 1 A name)
- Simple routing does not support health checks
- Use simple touting when you want to route requests towards 1 service such as a web service

>Video 5
#### R53 Health Checks
- Health checks are separate from, but are used by records
- Health checks are located globally 
- Health checks can be done every 30s (every 10s for an extra cost)
- Covers the following checks for
  - TCP
  - HTTP/HTTPS
  - HTTP/HTTPS with string matching (checks application responds with a content of a response to a specific max amount of characters)
- Healthy/Unhealthy result
- Monitors Endpoint, State of CloudWatch Alarm or the State of Checks of Checks (calculated)

>Video 6
#### R53 Failover Routing
- Add multiple resources as a primary and a secondary
- Normally, the primary resource is returned when you query it
- If you have a failure of a health check on the primary, the secondary value of the routing is returned
- Use for when you want to configure active passive failover

>Video 7
#### R53 Multi Value Routing
- Multi value routing supports multiple records with the same name
- Up to 8 "healthy" records are returned. If more exist, 8 are randomly selected
  - Client chooses and uses 1 value (There are many A names pointing to many records, which are randomly returned)
- Multi value improves availability. It is NOT replaceable for load balancing
- Each record is independent and can have an associated health check
  - Any records that fail a health check will not be returned when queried

>Video 8
#### R53 Weighted Routing
- Used for simple load balancing or testing new software versions
- You have multiple A records pointing at a record. Each record has a weight associated with it.
  - This weight is summed and the record is returned based on its weight
- A weight of 0 is never returned
- If a chosen record is unhealthy, the process of selection is repeated until a healthy record is chosen

>Video 9
#### R53 Latency Based Routing
- Used fod optimising performance and the user experience
- An A record is associated with a record region.
  - Latency based routing supports 1 record with the same name in each AWS region
- AWS maintains a database of latency between the users general location and the regions tagged in the record
- Records are returned based on which record offers the lowest estimation latency and is healthy

>Video 10
#### R53 Geolocation Routing
- Very similar to latency, except the geolocation of the user is used to route and return a record
- A record is tagged with a location, continent or default
- An IP check verifies the location of the user (normally the resolver)
- Geolocation does NOT return the closest record. It will return the relevant location record.
  - Checks for 
    1) The state
    2) The country
    3) The continent
    4) Default (Optional) or a No answer
- Can be used for regional restrictions, language specific content or load balancing across regional endpoints

>Video 11
#### R53 Geoproximity Routing
- You need to define an AWS region or coordinates for where the resource is located
- You can define a bias and adjust how R53 calculates the shortest distance to a location
  - "+" or "-" bias can be added to rules. A "+" increases a region size and decreases the neighbouring regions.
  - Routing is distance based (including bias)

>Video 12
#### R53 Interoperability (AKA when you are using R53 for Registrar only or Hosting only)
- R53 normally has 2 jobs 
  1) Domain Registrar
  2) Domain Hosting
- R53 can do Both, or either Domain Registrar or Domain hosting
1) R53 accepts your money (domain registration fee)
2) R53 allocates 4 Name Servers (NS) (domain hosting)
3) R53 creates a zone file (domain hosting) on the above NS
4) R53 communicates with the registry of the TLD (Domain Registrar) <- top level domain
  - Sets the NS records for the domain to point at the 4 NS above

#### If you pay for registrar only (you pay for R53 to deal with the TLD, ie. point to your name server to get the zone files)
- Pay R53 to manage the TLD (top level domain) to point to the zone files
- Provide R53 with your name server to get your zone files (from another provider)

#### Hosting Only (you pay for the R53 name server to host your zone files)
- Domain is registered via a 3rd party provider (Another party deals with the TLD for pointing at the name servers to get the zone files)
- Use R53 to host your zone files in the R53 name servers
- You have to get the 3rd party provider information of the name servers (more admin)

>Video 13
#### Implementing DNSSEC with R53 - Adds the certificate record to the DNS TLD (you can check and confirm certifications when you walk the tree)
- KMS Creates a public and private key signing key.
  - -These are kept in US-EAST-1 
  - R53 uses the KMS keys for signing records
- R53 creates a Zone Signing Key (private) and uses this with the KMS Key Signing Key to create a Key Signing Key and Zone Signing Key (DNS Key) to verify records in the zone
- The DNSKey and KMS Private Keys create the RRSIG DNSKey (register record signing key)
  - Uses the Public KMS Key Signing Key along with the Key Signing Key in R53 to sign and certify the top level domain server register records
- You can create alarms for the DNSSECInternalFailure and DNSSECKeySigningKeyNeedingActions in CloudWatch to notify if there is an issue with DNSSEC
- DNSSEC Validation can be enabled for VPCs - invalid results on DNSSEC enabled zones won't be returned

### Relatinal Database Service (RDS)
>Video 1 and 2
#### Database Refresher and Models
- Relational (SQL)
  - Structured Query Language (SQL)
  - Structure in and between tables of data - Rigid Schema
- Non-Relational (NoSQL)
  - Not a single thing, different models
  - Generally, a much more relaxed schema
  - Relationships between tables are handled differently

#### Relational Databases
- Primary Key - A unique key schema that is set when a table is created. 
- Composite Key - A join between 2 tables that creates a link/relation between 2 sets of keys to make a composite key set
  - Table relationships use keys

#### Key-Value DB (Key-Value stores) - ex. Redis
- A key:value pair database
  - No real schema
  - No real structure
  - No joining or comparing 
- Super scalable
- Really fast

#### Wide Column Store - DynamoDB
- Partition Key (required)
- Other Key (sort or range key) (required)
- Every row can have attributes (no fixed requirement)
  - Every item in the store needs to have 1 or more keys (have a ridged key structure)
- Like a Relational DB for keys but with a non-standard attributes (value) storage for data

#### Document Databases
- Use unique ID's attached to documents
- Interacting with whole documents or deep attribute interactions

#### Column Databases (opposite of Row stores - SQL)
- Row based DB are ideal when you operate with rows
  - ex. adding updating, deleting <- transactional workloads
- Column DB are stored on columns
  - Every column is stored together <- data analysis or reporting workloads
- Very fast to grab same "type" of data, as they are all stored together

#### Graph Databases
- Nodes are connected to each other
  - Each node has a name-value pair associated with them
- Nodes have a connection to other nodes
- Relationships between nodes are stored in the DB along with the data themselves, so complicated structures can be efficiently queried based on relationships

>Video 3
#### ACID and BASE Databases
- ACID and BASE are DB transaction models
- CAP Theorem (Chose 2)
  - Consistency
  - Availability
  - Partition Tolerant (resilience)

- ACID - Consistency
  - Atomic
  - Consistent
  - Isolated
  - Durable
- BASE - Availability
  - Basically Available
  - Soft State
  - Eventually Consistent

#### ACID - Consistent
- Atomic
  - All or No components of a transaction succeed or fails
- Consistent
  - Transactions move the database from 1 valid state to another - nothing in-between is allowed
- Isolated
  - If multiple transactions occur at once, they do not interfere with each other. Each executes as if it's the only one
- Durable
  - Once committed, transactions are durable. Stored on non-volatile memory, resilient to power outage or crashes
- In general: SQL style DB
  - RDS
  - Note: DynamoDB Transactions is an ACID NoSQL style DB

#### BASE - Highly scalable
- Basically Available
  - Read and Write operations are available "as much as possible" but without any consistency guarantees - kina/maybe
- Soft State
  - The database does NOT enforce consistency. This is offloaded onto the application/user
- Eventually Consistent
  - If we wait long enough, reads from the system will be consistent
- In general: NoSQL style DB

>Video 4
#### Databases on EC2
- Generally, not a good idea when there are stand-alone DB products provided by AWS
- 2 normal situations
  1) DB is included as part of the application and webserver (monolith)
  2) DB is split from the other application instances (Seperated in another EC2)
     - You need to be aware of the transfer of data between different instances in another AZ
- Reasons for running a DB on EC2
  - Access to the DB instance OS
  - Advanced DB Options, tuning
    - Note: Many AWS DB services now provide tuning on the DBSaaS/DBaaS products, so this is difficult to justify
  - DB or DB version that AWS does NOT provide
  - Architecture that AWS does not provide (replication/resilience)
  - Decision maker just "wants it"
- Negatives
  - Admin overhead - managing both the EC2 and DBHost
  - Backup/Disaster Recover Management
  - EC2 is in a single AZ
  - Features - some of AWS DB products are very good (optimized for DB)
  - EC2 is On or Off - no serverless, no easily scaling
  - Replication - Skills, setup time, monitoring and effectiveness

>Video 5
#### Relational Database Service (RDS)
- This is a Database Server as a Service (DBSaaS. It is NOT a Database as a Service (DBaaS)
  - Multiple databases on one DB Server (instance)
- Choice of DB Engines (MySQL, MariaDB, PostgreSQL, Oracle, Microsoft SQL Server)
  - #### Amazon Aurora is a different product
- Managed service - No access to OS or SSH access

#### RDS Subnet Group
- This is a subnet group that RDS can use
  - For High Availability - AWS will pick one of the AZ in the subnets to be the primary DB and another AZ to run the standby
- RDS can be accessed from the VPC or any connected private networks (VPN or Direct Connect)
- You CAN place a RDS subnet group in a public subnet, but it is bad practice
  - This is a safety problem when the DB can be publicly connected by the internet
- RDS instances can have multiple databases on them
- Instances have their own dedicated EBS storage per instance
- Standby has synchronous replication (the primary replicates to standby synchronously)
- You also have asynchronous replication to read replicas (RRs) if chosen
- Backups and snapshots can be taken and saved in S3

#### RDS Costs
1) Instance size and type
2) Multi AZ - More than 1 instance is required
3) Storage type and amount (EBS volume) - GB/Month fee for cost 
4) Data transferred
5) Backups and Snapshots (GB/Month)
6) Licensing (if applicable)

>Video 6
#### Relational Database Service - MultiAZ - Older architecture
- Configuration for your primary instance to synchronously replicate to a StandBy
- All access to the DB are by CNAME - You always access the primary DB unless there is a failover, in which case the CNAME is changed to the standby
- All backups or snapshots are done from the standby so you do not affect the primary performance
  - Data -> Primary -> Replicated to StandBy - Committed(synchronous)
  - No free tier - Extra cost for replica
  - One StandBy replica ONLY
    - Standby cannot be used for reads or writes
    - 60-120 second failover
    - Same region only, but in different AZ's in the same region
    - Backups are taken from Standby to improve performance
    - Affected by AZ ourages, primary failure, manual failover, instance type change and Software updates

#### Relational Database Service - Multi AZ - Cluster
- FYI this is different from Cluster in Aurora
- Writer synchronous replication to readers
  - Replication is via transaction logs, which is more efficient
- Limited to 1 writer and 2 readers
  - Writer and readers are all located in different AZs
  - Used on much faster hardware, Gravitron + NVME SSD Storage
  - Fast writes to local storage and then flushed to EBS
  - Readers can be used for reads and this allows for some read scaling
- Writer is used for writing and reading
- Readers are used for reading
- Data is committed when 1+ reader finishes writing 
- Each instance has its own storage (EBS)
- Endpoints: 
  - Cluster endpoints points at the writer. Used for reads, writes and administration
  - Reader endpoints directs and reads to an available read instance
  - Instance endpoints at a specific instance, usually use these for testing/fault finding
- Failover is faster (35 s + transaction log apply)

>Video 7
#### Relational Database Service Backups - General
- 2 Types (Both stored in S3) 
  - #### You cannot see these in S3, only in the RDS Console
    1) Automated backups
    2) Snapshots
- Backups do cause some pauses, but since they are taken from the standby (if using multi AZ)
  - If you do not have multi-az, this is taken from the main instance so you may experience pauses
- Snapshots are taken of the instance data
  - Brief slowdown or pause
  - Initial snapshot is going to be longer than update snapshots
  - Snapshots are NOT automatically deleted - You need to manually do them
  - Higher rate snapshots = more data transfer, less potential data loss
- Automatic backups - Similar to automatic snapshots, but:
  - Every 5 minutes, transaction logs are stored in S3, so you have a higher granularity on your backups for failover
  - A retention period is enforced between 0 to 35 days, where data is then deleted after the time period
  - You can restore the database anywhere between the retention period
  - When you delete a database, you can choose to keep the backups BUT
    - the backup is kept only until the retention period
    - If you want to create a long term backup, you need to create a final snapshot that will not expire
- RDS can replicate backups to another region
  - Both backups and transaction logs
  - Charges apply to cross region replication 
    - Pay for data transfer
    - Pay for storage in different region
    - This is NOT default - You have to enable cross region backups explicitly

#### Relational Database Service Restore
- Creates a NEW RDS Instance with a NEW address
- Snapshots = a single point in time, creation time
- Automated - any 5 minute point in time
- Backup is restored and transaction logs are then "replayed" to bring the DB up to the desired point in time (GOOD RPO)
- Restores are NOT fast - think about RTO (*Instead use Read Replicas)

>Video 8
#### Relational Database Service Read Replicas
- Read only replicas of a primary instance
  - Read replicas are NOT part of the primary instance. 
    - These have their own address
    - These do not have automatic failover
    - They are their own unique instance that is fed from the primary
- Asynchronous replication -> Means Read Replicas
- Synchronous replication -> Means Standby instances (AKA Multi-AZ)
- #### Read replicas can be made in the same region OR different regions
- You can create up to 5 direct read-replicas per DB instance
  - Each providing an additional instance of read performance
  - You can do read-replicas of read-replicas, but this causes lag to occur
- Global performance improvements (as you can now place read replicas in different regions)
- Improves RPO Objectives
- RTO's are a problem (restores can take a long time, as you need to restore from a snapshot)
- Read Replicas offer near 0 RPO (assuming there are no errors reading from the primary instance)
- Read Replicas can be promoted quickly (Low RTO) -> switch RR to a primary
  - Be careful, as this is for failure only, as errors are also replicated

>Video 9
#### Amazon RDS Security 
- SSL/TLS (in transit) is available for RDS, can be manditory
- RDS supports EBS volume encryption 
  - Uses KMS
- Handled by Host/EBS
- AWS or Customer managed CMK generates Data Keys
- Data Keys are used for encryption operations
- Storage, Logs, Snapshots and Replicas are encrypted with the Data Keys
  - #### Encryption CANNOT be removed
- RDS MSSQL and RDS Oracle support TDE
  - Transparent Data Encryption
  - This is encryption that is supported by the DB engine (the moment data is written to disc, you know it is encrypted by the engine)
- #### RDS Oracle supports integration with CloudHSM (Cloud Key Controls, a cluster that manages keys)
  - CloudHSM - Much stronger key controls that AWS does NOT manage
- The 2 types
1) KMS - 
   1) KMS uses a AWS generated key or customer managed key and generates a DEK (Data Encryption Key) for RDS
   2) DEKs are loaded into the host as required
   3) Host encrypts the data and decrypts the data - DONE BY HOST
2) TDE - Removed AWS from the encryption process (AWS never actually sees the keys)
   1) TDE is native DB engine encryption. Encryption is done before it leaves the instance
      - ex. CloudHSM is used on Oracle to provide keys directly to the engine and data is encrypted before it leaves the instance. At the host, it is already encrypted

#### Amazon RDS IAM Authentication
- Normally login to a database is done separate from AWS (local users) - username and password
- You can configure an RDS to use AWS credentials though
  - RDS local DB account configured to use AWS Authentication tokens
    - Policy is attached to Users or EC2 Role and maps the IAM identity into the local RDS Database User
    - This generates a token with a 15 minute validity which can be used in place of a DB User Password (generate-db-auth-token) using IAM
  - NOTE: Authorization is controlled by the DB engine. Permissions are assigned to the local DB User. IAM is NOT used to authorize. It is only used to authenticate through IAM.

>Video 10
#### Relational Database Service - Custom
- Fills the gap between RDS and EC2 running a DB engine
- RDS is fully managed - OS/Engine access is limited
- DB on EC2 is self managed, but has overhead
  - Currently only works for MS SQL and Oracle
  - Can connect using SSH, RDP or Session Manager

>Video 11
#### Amazon Aurora Architecture
#### Amazon Aurora Provisioned (non-serverless)
- Very different from RDS
  - Uses a "Cluster"
- A single primary instance but +0 or more replicas
- No local storage - Uses Cluster Volume (all cluster shares storage)
  - You can have up to 15 replicas 
  - All SSD Based across 3 AZ
    - High availability
    - Auto detects issues and will fix corruption in a disc by using other nodes to recover the corrupted node
  - Storage is billed based on what you use
    - High water mark (highest amount used) (may be phased out)
      - Storage which is freed up can be re-used (may be phased out)
- Aurora endpoints have multiple endpoints that you can connect to
  - Cluster endpoint - Points to the primary endpoint for write and read
  - Reader endpoint - Points to the replicas for reading
    - Each instance actually has its own endpoint as well, so you can customize your connections
- No free-tier option
- Aurora does not support micro instances
- Compute - hourly charge, per second, 10 min mimumum
- Storage - GB/month consumed, IO cost per request
- 100% DB Size in backups are included
- Backups in Aurora work in same way as RDS
  - Restores create a New Cluster
  - Backtrack can be used which allow in-place rewinds to a previous point in time (ex. if you know the exact moment corruption occurred, you can back up to the exact time before it occurred)
    - Requires being enabled
  - Fast clones make a new database much faster than copying all the data - Copy-on-write

>Video 12
#### Amazon Aurora - Serverless (What Fargate is to ECS)
- Runs Aurora hosted by AWS
- Scalable - ACU - Aurora Capacity Units
- Aurora Serverless cluster has a Min and Max ACU
- Cluster adjusts based on load
- Can go from 0 to paused
- Consumption billing on a per second basis
- Same resilience as Aurora (6 copies across AZs)
- The storage is provisioned the same as a provisioned Aurora Cluster, but
  - ACU units (the Aurora instances) are shared in an AWS managed Pool of Aurora instances using ACU's to pay for the service
  - Application connects to a Proxy Fleet to allow Aurora to be scaled in and out (to paused)
    - The proxy fleet is connected to the instances in the proxy fleet, and the customer/application connects to the proxy fleet that handles all the provisioning 
- When to use?
  - Infrequently used application (you pay in a per second basis)
  - New applications
  - Variable workloads (scales in and out)
  - Unpredictable workloads (you can set a min/max for ACUs to scale) 
  - Development and test databases (you only pay for what you use and only pay for storage)
  - Multi-tenant application - If your incoming load is tied to your incoming revue, you can control your costs easily

>Video 13
#### Amazon Aurora Global Database 
- Allows you to operate a database across regions (a primary region and a secondary region)
  - Replication from the primary region to the secondary region is around 1s
- Primary Region - Contains the primary instance
- Secondary Region - Contains read only instances
- Can have cross-region fail-over
- Global read scaling - low latency performance improvements 
- Super fast replication at storage layer between regions (this is a 1 way replication)
- No impact on DB performance - the instances are not used to replicate
- Secondary regions can have 16 replicas
  - All can be promoted to R/W
- Currently max 5 secondary regions

>Video 14
#### Aurora Multi-Master
- Default Aurora is a single-master setting
  - One R/W and 0+ Read only replicas
- Cluster endpoint is used to write, read endpoint is used for load balanced reads
- Failover takes time, replica promoted to R/W
- In Multi-master mode, All instances are R/W
- The application can connect to one or all the instances in the cluster
- The instance then can connect to any storage cluster 
  - This uses the state machine idea
    - Requires a democracy for multi read/write multi master (as you have more than 1 master)
- The replication is then passed over to all instances of Aurora across all instances to be saved in the in-memory cache
- This also allows for instant fall-over (basically or almost fault tolerant)
  - Any master can connect to any storage node at any time - you can send your write to another writer

>Video 15
#### RDS Proxy
- RDS proxy is a group of instance that runs between the clients (accessing the DB) and keeps a long-term connection pool to the primary DB instance
- When an application tries to access data, it connects to the proxy and the proxy handles connections
  - This uses multiplexing
- Proxys maintain an open connection and wait, even if the target DB is unresponsive
- RDS Proxy => DB instance connections can be reused, avoiding lag associated with establishing a connection, using the connection and terminationg the connection after each invocation
  - Abstracts the client away from a DB failure or fallover event
  
#### Why use RDS Proxy?
- Opening and closing connections consumes resources
  - it takes time to open and close as well, latency is an issue
- With serverless, how do you handle lambdas that open and close?
- Handling failures of database instances is hard
  - doing this within your application asks risk
- DB Proxies help, but managing them is nor trivial (scaling and resilience)
- Application -> Proxy (connection pooling) -> Database

#### When do you use a proxy?
- Too many connections to handle on your DB instances
- DB instances using T2/T3 (ie. small burst type) instances
- AWS Lambda (time saved/connection reused and IAM auth is reused)
- Long running connections (SAAS apps) - Low latency
- Where resilience to database failure is a priority
  - RDS Proxy can reduce the time for failover
  - RDS Proxy can make it transparent to the application

#### Key Facts for the Exam
1) Fully managed DB Proxy for RDS/Aurora
2) Auto scaling and high available by default
3) Provides connection pooling - reduces DB loads
4) Only accessible from a VPC
5) Accessed via Proxy Endpoint - No app changes
6) Can enforce SSL/TLS
7) Can reduce failover time by over %60
8) Abstracts failover away from your application - it just awaits for a connection to be completed and then continues on

>Video 16
#### Database Migration Service (DMS)
- A managed database migration service
- No downtime required -> Default to DMS
- Need to have a DB supported by AWS
- Runs using a replication instance
  - You need to define a source and destination endpoints to point at
  - You need a source and target database
- One endpoint NEEDS to be AWS
- You define a replication task that is related to what needs to be done
- Jobs can be of 3 types
  1) Full load (one off migration of all data)
  2) Full Load + CDC (Change Data Capture) for ongoing replication which captures changes
     - Used and once you are up to date, you can transition off the old on onto the new
  3) CDC Only (Change Data Capture) - If you want to use an alternative tool to migrate your data, and then you want to keep an ongoing replication to keep the new database up to date
     - Say you bulk transport using something like snowball
- Schema Conversion Tool (SCT) can assist with Schema Conversion
  - Allows you to migrate from 1 database engine to another

#### Schema Conversion Tool
- SCT is used when converting one database engine to another
  - Including DB -> S3 (migrations useing DMS)
- SCT is NOT used when migrating between DB's of the same type
  - ex. On premise MySQL to RDS MySQL, it would NOT be used
- Works with OLTP DB Types (MySQL, MSSQL, Oracle)
- Works with OLAP Types (Teradata, Oracle, Vertica, Greenplum...)
- ex. On-premises MSSQL -> RDS MySQL or Aurora

#### DMS and Snowball
- Largest migrations might be multi TB in size
  - Moving data over networks takes up too much time and capacity
- DMS can utilize AWS SnowBall
1) Use SCT to extract data locally and move to a snowball device
2) Ship the device back to AWS. It is then loaded to an S3 bucket
3) DMS Migrate from S3 into the target store
4) Change Data Capture (CDC) can capture changes made between the transfer and via S3 intermediary, they are also written to the target DB

### Network Storage and Data Lifecycle
>Video 1
#### Elastic File System (EFS) Architecture
- EFS is an implementation of NFSv4 -> File system that can be mounted
  - EBS is block storage, while EFS is file storage
  - Linux Only
- EFS Filesystems can be mounted in Linux
- EFS Filesystems can be shared between many EC2 instances
- Mounted inside a VPC via Mount Targets
- Can be accessed from on-premises - using VPN or DX (Direct Connect)
  - ie. You can run EFS storage in AWS and use a direct connect to allow your local on-prem instance to use it for storage
- You need to have a Mount Target for the EFS file system in each subnet that the EC2 will be able to connect to as well
- Modes
  - General purpose - Default for almost all uses
  - Max I/O modes
- Bursting and Provisioned throughput modes
- 2 Classes - Mirror AWS S3 class types for these 2
  1) Standard - Default
  2) Infrequent Access (IA)
  - Lifecycle policies can be used with these classes

>Video 2
#### AWS Backup
- A fully managed data-protection service (backup/restore)
- #### Allows you to consolidate management of all backups in 1 place
  - Across accounts and across regions
- Supports a wide range of AWS products
  - Most compute, EBS, EFS, Databases and S3
- Backup Plans - Frequency, Window, Lifecycle, Vault, Region Copy
  - Resources - What resources to back up
  - Vaults - Backup destination (containers) - assign KMS keys for encryption
    - Vault lock - a Write-once, Read-Many (WORM), 72 hour cool off, then even AWS cannot delete
  - On-Demand - Manual backups created as needed
  - Point in time recovery - PITR

### High Availability and Scaling
>Video 1
#### Regional and Global AWS Architecture
- Global parts
  - Global service location and discovery
    - How a user detects your infra and connects globally
  - Content delivery network (CDN) and optimisation
    - How information/data gets delivered on a global scale
  - Global health check and failover
    - How a system is maintained globally
- Regional parts
  - Regional entry point
  - Scaling and resilience
  - Application Services and components

How an example works:
1) Globally DNS is used for service discover, regional health checks and routing 
   - Can use with failover (point to another location if required) and re-routing
   - Go to the regional side
     - R53 will enter the AWS region
     - Connect to the Web Tier (a load balancer or API gateway)
     - Connect to a compute tier (EC2, Lambda)
         - Connect to a caching tier
         - Connect to a DB tier
         - Connect to App services (ex. SQS, SNS, Kinesis)
         - Connect to a storage system
     - Can store or cache to CloudFront
     
2) A CDN layer Cloudfront - CDN's are used to cache content globally - as close to the end user as possible to improve user experience

>Video 2 and 3 and 4
#### Elastic Load Balancer
- 3 Types of ELB (ELB refers to all 3)
- 1 x Version 1 (avoid/migrate) and 2 x version 2 (prefer)
  1) Classic Load Balancer (CLB) - V1 - Introduced in 2009
     - Not really layer 7, lacking features, 1 SSL per CLB
  2) Application Load Balancer (ALB) - v2 - HTTP/S/WebSocket
  3) Network Load Balancer (NLB) - V2 - TCP, TLS and UDP
- V2 = faster, cheaper, support target groups and rules

#### Elastic Load Balancer Architecture
- A ELB accepts connections from a user and distributes it to your services
- Choices to pick
  - IPv4 only or Dual stack (IPv4 AND IPv6)
  - The AZ (2 or more) you will be using (specifically the subnet in 2 or more AZs)
- Based on the subnets you pick on the configuration, the load balancer will deploy 1 or more load balancer nodes in the AZs in the subnet (and scale within that AZ with load)
  - Each ELB is configured with an (A) record DNS name. This resolves to the ELB Nodes
  - Choice between internal or internet facing
    - Internal: Nodes have private IP Address
    - Public: Notes are allocated public IP Addresses
  - Load balancers (notes) are configured with listeners which accept traffic on a port and protocol and communicate with targets on a port and protocol
  - #### Internet facing Load Balancer Nodes can access Public AND private EC2 instances
  - Require 8 or more FREE IP addresses in the subnet they are operating in (/28 is technically correct but it should be more, like /27)
  - Load balancers are used to separate different tiers in an application
- ELB Architecture uses load balancers before the Auto Scaling Group. 
  - The ELB nodes will communicate with the Web Auto Scaling Group, which will point to the next tier Elastic Load Balance, etc.
  - ELB allows tiers (the Auto Scaling Groups) to scale independently from each other as they are abstracted from the other tiers 

#### Cross Zone Load Balancing
- A Load Balancer can have multiple nodes in various AZs, which splits incoming traffic to each of the AZ's. 
- Cross Zone Load Balancing means the load balancer can use nodes from one AZ and connect/point them to instances in different AZ's to more evenly distribute workloads
- Enabled by default

#### Important Notes about ELB
- ELB is a DNS A Record pointing at 1+ nodes per AZ
- Nodes (in one subset per AZ) can scale 
- Internet-facing means nodes have public IPv4 IPs
- Internal is private only IPs
- EC2 DO NOT need to be public to work with a Load Balancer
- Load balancers are configured with listener configurations 
  - What the Load balancer does
- 8+ Free IPs per subnet required, technically /28 subnet size for scaling but /27 suggested

#### Application and Network Load Balancers (ALB vs NLB)
#### Load Balancer Consolidation
- Version 1 does not scale, as we can only attach 1 SSL certificate
  - Example, if you want to point to 2 different websites with different SSL certificates, you would need 2 separate version 1 Load Balancers to accomplish this
- Version 2 scales, as you can attach multiple SSL certificates and create rules for the Load Balancer
  - You can have 1 version 2 application load balancer pointing at both websites and rules (using SNI) that determine the target group. Allows consolidation

#### Application Load Balancer Differences
- Layer 7 Load Balancer
  - Listens to HTTP and/or HTTPS
- No other layer 7 protocols (such as SMTP, SSH, Gaming, etc)
  - No TCP/UDP/TLS listeners
- L7 content type, cookies, custom headers, user location and app behaviour can be checked
- HTTP and HTTPS (SSL/TLS) is always terminated on the ALB
  - The SSL is ALWAYS broken at the Application Load Balancer, which could be a security issue
- All Application Load Balancers MUST have an SSL certification if HTTPS is used
- Application Load Balancers are slower than Network Load Balancers
  - More levels of the network stack to process
- Heal checks evaluate application health at Layer 7

#### Application Load Balancer Rules
- Rules direct connections which arrive at the listener
- Processed in priority order
- Default rule -> this will be a catchall
- Rule conditions: Host-header, HTTP-header, HTTP-request-method, path-pattern, query-string and source-IP
- Actions: Forward, Redirect, Fixed-response, Authenticate-oidc and Authenticate-cognito
  - For example, a rule can forward a connection to an alternative target group based on a customer static source IP
- You can redirect or take actions against anything you can see at layer 7

#### Network Load Balancers
- Layer 4 Load Balancers 
  - TCP, TLS, UDP, TCP_UDP
- No visibility or understanding of HTTP or HTTPS
- No headers, no cookies, no session stickiness 
- Really really fast (millions of rps, 25% of ALB latency)
  - Used for SMTP, SSH, Game servers, Financial apps (not HTTP/s)
- Health checks just check ICMP/TCP handshakes. Cannot be app aware
- NLB's have static IP's - useful for whitelisting
- Forward TCP to instances <- Unbroken encryption (HTTPs will be handled at the resource)
- Use with private link to provide services to other VPCs

#### ALB vs NLB (Why you would use a NLB over an ALB)
1) Unbroken encryption - NLB
2) Static IP for whitelisting - NLB
3) The fastest performance - NLB
4) Protocols NOT HTTP or HTTPS - NLB
5) Private Link - NLB
6) Anything else - ALB

>Video 5
#### Launch Configurations and Launch Templates
- Allows you to define the configuration of an EC2 instance in advanced
  - AMI, Instance type, Storage and Key-pair
  - Networking and Security Groups
  - Userdata and IAM role
- Both are NOT editable - define once. 
  - Launch Templates has versions
- Launch Templates provide new features. including T2/T3 unlimited, Placement Groups, Capacity reservations, Elastic Graphics
- Launch Configurations and Launch Templates are used by Auto Scaling Groups
- Launch Templates can ALSO be used for launching EC2 instances
  - Launch templates can save time when provisioning EC2 instances from the console UI/CLI

>Video 6
#### Auto Scaling Groups
- Provide Automatic Scaling and Self-Healing for EC2
- Uses Launch Templates or Configurations
  - Link a Launch Template version or a Launch Configuration
  - Had a Min, Desired and Maximum size for the group (ex. 1:2:4)
- Keep running instances at the Desired Capacity by provisioning or terminating instances
- Scaling policies automate based on metrics
- Autoscaling groups are attached to a subnet that contains AZs, and the autoscaling group will try to match each AZ to match the desired size of instances
- Scaling types:
  - Manual Scaling - Manually adjust the desired capacity
  - Scheduled Scaling - Time based adjustment (ex. Sales based on time)
  - Dynamic Scaling
    - Simple - Based on a metric - ex. "CPU Above %50 +1", "CPU Below %50 -1" or length of SQS queue
    - Stepped scaling - Bigger based on +/1 difference (allows you to add bigger in a non-linear way)
    - Target tracking - ex. Desired aggregate CPU = %50
- Cooldown Periods - An amount of time to prevent a change by the scaling group so you dont get charged or change something too fast (allows steady state changes)
- Note: If you put an autoscaling group in a subnet and set min:desired:max to 1:1:1, it will auto heal with 1 instance in all AZs in the subnet
- You can use the Application Load Balancer health checks rather than the EC2 status checks for confirming a healthy check (ex. if you detect failure on the ALB, you can restart the EC2 instances)

#### Scaling Processes
- Launch and Terminate - Suspend and Resume
- AddToLoadBalancer - add to LB on launch
- AlarmNotification - accept notification from CloudWatch
- AZRebalance - Balances instances evenly across the AZ
- HealthCheck - instance health checks on/off
- ReplaceUnhealthy - Teminate unhealthy and replace
- ScheduledActions - Scheduled on/off
- Standby - Use this for instances "InService vs Standby"

#### Final Points
- Autoscaling groups are free
  - Only charged for the resources that created to be billed
    - Use cool downs to avoid rapid scaling
- Think about more, smaller instances (granularity)
- Use ALB's for elasticity - abstraction
- ASG(Auto Scaling Groups) Defines WHEN and WHERE, LT (Launch Templates) defines WHAT

>Video 7
#### Auto Scaling Group Scaling Policies
- ASGs do NOT need scaling policies - they can have none
- Manual scaling - Min, Max and Desired - Testing and Urgent
- Scaling
  1) Simple Scaling - Add or remove a set amount based on an alarm (linear)
  2) Step Scaling - Add or remove different amounts based on an alarm (non-linear) 
  3) Target Tracking - Define an ideal value and the ASG will calculate adjustments to try to keep the metric at your target value
  5) Scaling based on SQS - ApproximateNumberOfMessagesVisible

>Video 8
#### ASG Lifecycle Hooks
- Custom actions on instances during ASG Actions
  - Instance launch or instance terminate transitions
- Instances are paused within the flow (they wait)
  - Until a timeout (then either continue or abandon)
  - Or you resume the ASG process CompleteLifecyleAction
- Can work/integrated with with EventBridge or SNS Notifications
- Example:
  1) The ASG scales out
  2) The instance is provisioned
  3) The instance is on Pending: wait <- lifecycle hook activated
  4) Load some data or index something
  5) Pending: Proceed <- lifecycle action
  6) Instance is now in service
  7) Notifications can be sent out to an SNS topic or an EventBridge can be used to initiate another process
- Can be done for scaling out or in

>Video 9
#### ASG HealthCheck Comparison - EC2 vs ELB
- EC2 (Default)
  - Stopping, Stopped, Terminated, Shutting Down or Impaired (not 2/2 status) = Unhealthy
- ELB (Can be enabled)
  - Running and Passing ELB Health Check = Healthy
    - Can be more application aware (Layer 7)
- Custom 
  - Instances marked healthy and unhealthy by an external system
  - Health check grace period (default 300s) - Delayed before starting checks
    - Allows system launch, bootstrapping and application start
    - Can be a cause for an instance failing and restarting continuously 

>Video 10
#### SSL Offload and Session Stickiness
- 3 ways a Load Balancer handles it's connections
  1) Bridging - ELB
     - Listener is configured for HTTPS. Connection is terminated on the ELB and the ELB needs a certificate for the domain name.
     - The ELB then initiates new SSL connections to the backend instances. Instances need SSL certificates and the compute required for cryptographic operations
       - The ELB decrypts the message to check the message, and then it will encrypt the message again, which needs to be decrypted at the instances (all instances will require a certificate to decrypt from the ELB)
  2) Pass-through - NLB
     - Each instance needs to have the appropriate SSL certificate installed. With this architecture, there is NO Certificate exposure to AWS... it is entirely self-managed and secured
     - Listener is configured for TCP. No encryption or decryption happens on the NLB. Connections are directly passed to the backend instance.
  3) Offload - ELB
     - ELB to instance connection use HTTP - no certificate or cryptographic requirements
     - Listener is configured for HTTPS. The connections are terminated at the ELB and the backend connections use HTTP
       - Much faster but there are un-encrypted connections happening behind the ELB

#### Connection Stickiness - ALB
- With no stickiness, connections are distributed across all in-service backend instances. Unless the application handles user state, this could cause user states to be lost
- Session stickiness:
  - Within the application load balancer, this is enabled on a target group
  - The load balancer contains a cookie that has a session with a customized duration (you define)
  - The user will be directed to the specific instance
  - If the instance fails, the user will be redirected to a new instance
- In an ideal world, you would likely want to use a stateless ASG and have a cache to handle the user state

>Video 11
#### Gateway Load Balancer (GWLB)
#### Why use a GLB?
- If you want to have a security appliance that scans data before a connection enters you application, you would have to put an instance infront of your application to filter information before it gets sent to your app
- The problem is that as things scale, it becomes increasingly difficult to scale both the app and the protection instances at the same time without causing confusion and issues with networking

#### What is a GWLB?
- A GLB helps you run and scale 3rd party appliances for security (that you would need instances to run in say, a separate security VPC)
  - Things like firewalls, intrusion detection and prevention systems
  - Inbound and Outbound traffic (transparent inspection and protection) is required
- Use GWLB endpoints - traffic enters and leaves via these endpoints
- The GWLB balances across multiple backend applicances
- The GWLB uses a tunnelling protocall called GENEVE for traffic and metadata
  - Geneve are where original packages remain unaltered encapsulated through the application and back
  - The GWLB then points to the horizontally scaled security appliance that checks the packets 
  - The information is returned with GENEVE
  - The packets are routed back to the traffic destination
- GWLB - How it works
  1) Traffic can be routed via a route table to the GWLB Endpoints 
  2) The GWLB Endpoint gets pushed to the GWLB itself
  3) With GENEVE the original packages remain unaltered encapsulated through to the security application and processed
  4) The information is returned with GENEVE to the GWLB
  5) The GENEVE encapsulation is removed
  6) The GWLB points back to the GWLB Endpoint 
  7) The packets are routed back to the traffic destination

### Example <- Important
1) Client accesses an Internet Gateway (as in ingress gateway)
2) The Gateway route table translates the destination IP to the private IP, which is directed to the GWLB Endpoint in the right AZ
3) The GWLB Endpoint forwards the packet to the GWLB itself with the original Source and Destination unchanged
4) Packets are encapsulated with GENEVE at the GWLB and are sent to the security appliances, which run in an Auto Scaling group 
5) Analysis takes place in a scalable way
6) Packets are returned and stripped of GENEVE and returned to the GWLB Endpoint
7) The GWLB Endpoint redirects the packet to the Application Load Balancer (the CIDR range) 
8) The ALB will send the packet to the application
9) The application processes the data
10) The application sends the packet back to the ALB (as the default route)
11) The application load balancer sends the packet to the GWLB Endpoint via default route
12) The GWLB Endpoint redirects the packet to the GWLB 
13) Packets are encapsulated with GENEVE at the GWLB and are sent to the security appliances, which run in an Auto Scaling group
14) Analysis takes place in a scalable way
15) Packets are returned and stripped of GENEVE and returned to the GWLB Endpoint
16) The GWLB Endpoint defaults to the Internet Gateway
17) The internet Gateway sends the packet to the client

### Serverless and Application Services
>Video 1
#### Architecture - Event-Driven-Architecture
- AWS has various producer and consumer services that you can build microservices around
  - Examples: SQS, SNS, EventBridge
- Producers generate events when something happens
- Event-Bridge
  - No constant running or waiting for things
  - Clicks, errors, criteria met, uploads, actions create events
  - Events are delivered to consumers
    - Actions are taken and the system returns to waiting
  - Mature event-driven architecture only consumes resources when handling events (serverless)

>Video 2
#### Lambda 
- Function-as-a-service (FaaS) - Short running and focused
  - Lambda function - a piece of code that lambda runs
  - Functions use a runtime that you choose
    - Python
    - Ruby
    - Java
    - Go
    - C#
    - Custom runtimes like Rust using layers
  - Functions are loaded and run in a runtime environment
- The environment has a direct memory (indirect CPU) allocation
  - You directly control the memory allocated for lambda functions whereas vCPU is allocated indirectly as a function of the memory increase
- You are billed for the duration that the function runs 
- Can be used to generate event-driven architecture 
- Completely stateless - 512MB storage is available as /tmp storage (up to 1240MB)
- #### Maximum function timeout - 15 minutes
- Lambda uses an execution role and attaching it to a lambda function (IAM)
- Generally used with 
  - Serverless Applications (S3 and API Gateway)
  - File processing (S3, S3 Events)
  - Database Triggers (DynamoDB, Streams)
  - Serverless CRON (EventBridge/CWEvents)
  - Realtime Stream Data Processing (Kinesis)

>Video 3
#### Lambda - Networking
#### Public Lambda - Lambda running in a Public Network
- By Default lambda functions are given public networking. 
  - They can access public AWS services and the public network
  - Public networking offers the best performance because no customer specific BPC networking is required
- Lambda function has NO ACCESS to VPC based security services unless public IPs are provided AND security controls allow external access

#### Private Lambda - Lambda configured to run inside a VPC
- Lambda functions running inside a VPC obey all VPC Networking rules
  - EX. If the VPC does not have access to the public network, you cannot access public AWS services like DynamoDB
    - To access public AWS services, you need to configure your VPC like you would a normal service:
      - Using a NATGW and IGW or a VPC Endpoint to access internet resources
- Lambda functions work like a Fargate cluster
  - AWS Lambda services are run in the AWS Lambda Service VPC
  - AWS analyze all the functions that are used and instead of injecting each function into a unique elastic network interface, AWS is able to consolidate all functions to access a single ENI
- #### Treat a lambda function running inside a VPC like any other service/instance running inside a VPC

#### Lambda Security
- Lambda functions are provided Execution Roles to access resources (same as an EC2 instance role)
  - Attached to lambda functions which control the permissions the Lambda function receives
- Lambda Resource policy controls what services and accounts can INVOKE the Lambda functions

#### Lambda Logging
- Lambda uses CloudWatch, CloudWatch Logs and X-Ray
  - Logs from Lambda executions - CloudWatch Logs
  - Metrics - invocation success/failure, retries, latency - CloudWatch
  - Distributed tracing - X-Ray
- To allow CloudWatch Logs, you need to give permissions via Execution Roles

>Video 4
#### Lambda Invocation
- 3 types of invocation
1) Synchronous Invocation
   - CLI/API invokes a lambda function, passing data and waiting for a response
     - Lambda function responds with data or fails
   - #### Results are Success or Failure
     - Returned during the request
   - #### Errors or Retries have to be handled within the client
   - Ex. A client sends a response to the APIGW, which proxies to the Lambda function. The lambda function responds or fails and the response is returned to the client
2) Asynchronous Invocation
   - Typically when AWS services invoke lambda functions
   - Reprocessing/retrying happens between 0-2 times (configurable)
     - The lambda function handles retries
     - For retry, Lambda function needs to be idempotent (same result each time the function is run)
     - Events that are failed n times can be sent to a Dead Letter Queue
       - Lambda supports destinations (SQS, SNS, Lambda and EventBridge) where successful or failed events can be sent
   - Ex. S3 gets an object uploaded and an S3 Event is triggered (Put). Lambda is triggered (for example, uploading something to DynamoDB). If the processing of the event fails, lambda will retry between 0-2 times (configurable). Lambda handles the retry logic
3) Event Source Mappings
   - Typically used on Streams or Queues which do NOT support event generations to invoke Lambda (ex. Kinesis, DynamoDB Streams, SQS)
   - Event Source Mapping - poles a stream and receives Event Source Batching
     - Event Batches are sent to Lambda.
       - You need to make sure that the batch has enough time to process (15 Min max)
   - #### Permissions from the Lambda Execution role are used by the Event Source Mapping to interact with the event source
     - Even though the Lambda itself does not read from the service, it requires the Lambda execution roll to do this
   - SQS Queues or SNS topics can be used for any discarded failed event batches (Dead Letter Queue)

>Video 5
#### Lambda Versions
- Lambda functions have versions - v1, v2, v3...
- A version is the code + the configuration of the lambda functions
- The version is immutable - It never changes once it is published and has it's own Amazon Resource Name
- $Latest points at the latest version
- Aliases (DEV, STAGE, PROD) point at a version (this can be changed)

#### Lambda Start-up Time
- When a lambda function is invoked,
  - A Lambda function needs to be set up. 
  - An execution context is the environment a lambda function runs in. 
    - A cold start is a full creation and configuration including function code downloading
  - If a future invocation is done somewhat soon after it is already invoked, an execution context can be re-used. 
    - With a warm start, the same execution context is reused. A new event is passed in but the execution context setup and creation can be skipped
  - Lambda can reuse an execution context but has to assume it cannot. 
    - If used infrequently, execution contexts will be removed. 
    - Concurrent executions will use multiple (potentially new) contexts
  - Provisioned concurrency can be used. AWS will create and keep X contexts warm and ready to use. This improves start speed
    - You can use in-memory data from the instance if it is a warm start, but you CANNOT assume you can, so you need to assume you CANNOT access the data.
      - An efficient thing to do is check if you can use the data, otherwise default to assuming its not available. 

>Video 6
#### CloudWatch Events and EventBridge
- EventBridge is replacing CloudWatch Events (FYI)
  - EventBridge = CloudWatch Events V2
- If X happens, or at Y times(s) (CRON), do z
- Uses a default Event Bus for the account
  - In CloudWatch Events, this is the only bus available (implicit)
  - EventBridge can have additional event busses
- Rules match incomming events or schedules (for CRON)
- Route the events to 1+ targets (ex. Lambda)

#### How EventBridge Works
- When an event occurs (example, EC2 event changes) - an event is sent through the Event Bus
- EventBridge reads the Event Bus and checks to see if the event pattern rule or schedule rule is triggered, which triggers a target (ex. Lambda)
- Events in the event bus are just JSON objects

>Video 7
#### Serverless Architecture
#### What is Serverless?
- Serverless is not 1 single thing
- You manage few, if any servers - low overhead
- Applications are a collection of small and specialized functions
  - Applications run in stateless and ephemeral environments
  - Applications are billed based on duration it is run
- Everything is Event-Driven - Consumption is ONLY when it is being used
- FaaS is used where possible for compute functionality
- Use Managed Services where possible 
  - Using already made services (such as S3 or DynamoDB) instead of making your own service, if available 

>Video 8
#### Simple Notification Service 
- Publish/Subscriber messaging service
- Public AWS Service - Network connectivity with Public Endpoint
  - Coordinates the sending and delivery of messages
- SNS Topics are the base entity of SNS - Permissions and Configurations
- A Publisher sends messages to a Topic
- Topics have Subscribers, which receive messages
  - ex. HTTPs, Email(-JSON), SQS, Mobile Push, SMS Messages, Lambda
  - SNS used across AWS for notifications 
    - ex. CloudWatch and CloudFormation
- You can apply filters to SNS so only relevant messages get delivered from a topic to a subscriber
- Can use Fanout architecture 
- Offers 
  - Delivery Status (including HTTP, Lambda, SQS)
  - Delivery Retries - Reliable delivery
  - Regional Resilient service (HA and Scalable)
  - Server Side Encryption (SSE)
  - Cross-Account via TOPIC Policy

>Video 9
#### Step Functions - A serverless state machine service
- While you can chain Lambdas together, it gets messy at scale
- Step Functions allow you to create State Machines using Lambda
- Step Machine Maximum Duration - 1 year
- 2 types of workflows
  1) Standard Workflow - 1 year max
  2) Express Workflow - 5 minute max
- Started via API Gateway, IOT Rules, EventBridge, Lambda, etc.
- Amazon States Language (ASL) - JSON Template
- Uses IAM Roles for permissions
- Examples of States that define actions: 
  - Succeed and fail
  - Wait
  - Choice 
  - Parallel 
    - Multiple actions done based on one thing
  - Map
    - A list of things
  - Task
    - A single unit of work that defines a workload 
      - ex. Lambda, Batch, DynamoDB, ECS, SNS, SQS, Glue, SageMaker, EMR, Step Functions

>Video 10
#### API Gateway
- Service that allows you to create and manage APIs
- Acts as an Endpoint/Entry-point for an application
- Sits between application and integrations (services)
- Highly available, scalable, handles authorization, throttling, caching, CORS, transformations, OpenAPI spec, direct integration with AWS services and much more
- Public service 
- Can connect to services/endpoints in AWS OR On-Premises
- Can handle HTTP APIs, REST APIs and WebSocket APIs
- API Gateway is an intermediary between clients and integrations
- Phases:
  1) Request 
     - Authorizes, Validates and Transforms the request
  2) Integrations 
     - Backend does it's compute
  3) Response
     - Transform, Prepare and Return
- CloudWatch logs can store and manage full stage request and response logs. CloudWatch can store metrics for client and integrations sides
- API Gateway Cache can be used to reduce the number of calls made to backend integrations and improve client performance 

#### API Gateway - Authentication
- Example: Cognito can be used to integrate authentication with existing providers (ex. Google) which provides an authentication token to the client
  - The token can then be passed to the API Gateway
  - API Gateway can validate the token with Cognito and validate the connections

#### API Gateway - Endpoint Types
1) Edge Optimized
  - Routed to the nearest CloudFront POP
2) Regional - Clients in the same Region
  - You get a regional endpoint that clients can connect to
3) Private - Endpoint accessible only within a VPC via interface endpoint

#### API Gateway - Stages
- APIs are deployed to stages, each stage has one deployment
  - Example: A Production and a Development endpoint, which have separate isolated stages that point to different integrations
- Stages can be enabled for canary developments
  - If done, deployments are made to the canary and NOT the stage
  - Stages enabled for canary deployments can be configured so a specific percentage of traffic is sent to the canary. This can be adjusted over time, or the canary can be promoted to make it the new base "stage"
  - Note: this is what was done in the new Loyalty Widgets work

#### API Gateway Error Codes
- 4xx - Client Error - Invalid Request on CLIENT SIDE
  - 400 - Bad request - GEneric
  - 403 - Access Denied - Authorizer denied - WAF Filtered
  - 429 - API Gateway can throttle - this means you have exceeded the amount
- 5xxx - Server Error - Valid Request, error on SERVER/BACKEND SIDE
  - 502 - Bad Gateway Exception - Bad output returned by server
  - 503 - Service Unavailable - Backing endpoint offline? Major service issue
  - 504 - Integration failure/Timeout error - 29s limit exceeded

#### API Gateway Caching
- CAche is defined per stage within API Gateway
- Cache TTL default is 300 seconds
- Configurable min 0 and max 3600s
- Can be encrypted
- Cache size 500MB to 237GB
- Calls are only made to the backend integration if the request is a Cache Miss
- Reduces load and cost and improves performance

>Video 11, 12 , 13 and 14
#### Simple Queue Service (SQS)
- Public, Fully MAnaged, Highly Available Queues
  - 2 Types
    1) Standard
        - Order does not matter, faster
        - Can scale better (you can have multiple streams of messages where order does not matter)
        - At-last-once delivery - there could be on occasion more than 1 copy of a message
        - Best-Effort-Ordering - No rigid preservation of message order
    2) FIFO
       - Order matters
       - 3000 messages per second with batching, or up to 300 messages without
       - FIFO must have a .fifo suffix
       - Exactly-once processing - duplicates are removed
       - Message order is STRICTLY Preserved
- Messages can be up to 256KB in size 
  - A link to larger data
- Received messages are hidden with a VisibilityTimeout
  - After the timeout, the message will reappear in the queue to be reprocessed again
  - Or the message is deleted
- Dead-Letter Queues 
  - Can be set up for problem messages
- ASG can be scaled based on the queue size and Lambdas can be built off of the queue
- Billed based on requests
- 1 request = 1-10 messages at a time up to 64KB total
  - Short (Immediate) vs Long (waitTimeSeconds) Polling
    - To reduce costs (costs per request)
  - Encryption at rest (KMS) AND in transit
- Queue Policy will configure how the resource can be accessed
  - Can configure external access as an option

#### Delay Queues 
- Allow you to define delays that apply to messages in queue for your consumers
- Visibility Timeouts 
  - Time to process the message before we assume the message failed to process
  - Only triggered then the message is received by the consumer
  - Message is not reachable while it is in the visibility timeout
  - Default is 30 seconds. Can be between 0 and 12 hours
    - ChangeMessageVisibility changes this value
  - Set on Queue or Per-Message
- Delay Queue
  - A delay queue has DelaySeconds set
  - It will be invisible for Delay Seconds
  - NOT AVAILABLE IN FIFO Queues
  - Delay is between 0 to 15 minutes

#### Dead Letter Queues
- Designed to help manage errors that happen while you use an SQS queue
- If the message has failed multiple times (that you set), the message can be sent to the dead letter queue so it does not stay in the queue
  - redrive policy - specifies the source queue, the dead-letter queue and the conditions where messages will be moved from one to the other
    - Define maxRecieveCount
  - Then RecieveCount > maxRecieveCount & message is not deleted, it's move to DLQ
- The queue will expire based on the original Enqueue timestamp, as the initial enqueue timestamp remains unchanged. 
  - The retention period of a dead-letter queue is generally longer than the source queue 

>Video 15
#### Amazon Kinesis Data Streams
- Kinesis is a scalable streaming service 
  - Meant for large data streaming
- Producers send data into a kinesis stream
- Streams can scale from low to near infinite data rates
- Public service and highly available by design
- Streams store a 24-hour moving window of data
  - Storage can increase to a maximum of 365 days for a cost
- Multiple consumers can access the data from the moving window
- Uses a shard architecture 
  - Shards are defined by size
    - 1MB Ingestion
    - 2MB consumption
  - The size of the stream is based on shard size
  - Kinesis data records (1MB) are stored across shards, which can be accessed based on the Kinesis Stream window (that is defined by 24h - 1 year)
  - Kinesis Data Fire Hose can be attached to a Kinesis stream (for immediate analysis) and this can be sent to S3 (for later analysis, say for big data analysis)

#### SQS vs Kinesis
- SQS is for 1 production group, 1 consumption group
- SQS Queues are used for Decoupling and Asynchronous communications (splitting up your application)
- SQS has no persistence of messages and no rolling window
- Kinesis is designed for huge scale data ingestion
- Kinesis is for multiple consumers
- Kinesis has a rolling window
- Kinesis is for Data ingestion, analytics, monitoring, app clicks

>Video 16
#### Kinesis - Data Firehose
- Kinesis alone does NOT provide a way to persist data nor a way to deliver data
  - It only provides a rolling window to access data within the rolling window
- Data Firehose is a fully managed service to load data for data lakes, data stores and analytic services
- Firehose has Automatically scaling 
  - Fully serverless and resilient
- NEAR real time delivery (~60 seconds). NOT REAL TIME
- Supports transformation of data on the fly (with Lambda)
- Billing based on volume through

#### Data Firehose Delivery Targets
1) HTTP
2) splunk
3) Redshift
4) ElasticSearch
5) S3 Destination Bucket

- Kinesis streams are realtime (~200ms)
- Producers can send records to data streams or send directly at firehose.
  - Firehose can also read from a data stream as a consumer
- Firehose offers NEAR REALTIME DELIVERY
  - Delivery when buffer size in MB (1) fills, or buffer interval in seconds (60) passes
- You can transform via Lambda
  - Lambda transformation function blueprint 
    - transforms data and returns it to the firehose
  - At the same time, a backup bucket in S3 is sent the unprocessed data
  - Then the processed data is sent to the consumers
- The only difference in delivery is Redshift
  - Redshift uses an intermediate S3 bucket and then copies from S3 into redshift

>Video 17
#### Kinesis - Data Analytics 
- Data Analytics provides Real Time Processing of data using Structured Query Language (SQL)
  - Adjusts data from input and outputs the new structured data
- Ingests data from Kinesis Data Streams OR Firehose
  - Can be from multiple sources and you can do something like, say, a join of data from multiple sources
  - Uses virtual tables in application
  - Outputs data after SQL processing
- After data is processed, it can be sent do destinations in real-time (or near real-time if Firehose is chosen to read from)
  - You can send to Firehose (and any of it's delivery options, S3, Redshift, ElasticSearch, Splunk) <- non-real time
  - AWS Lambda
  - Kinesis Data Streams
- Can also take in S3 sources
- Think of Data Analytics as a transformation application that transforms input data (from Kinesis Streams, Kinesis Firehose or S3) and outputs data into another Kinesis Stream or a Kinesis Firehose
- Not cheap

#### When do you use?
- Streaming data needing Real-Time SQL Processing
- Time-series analytics
  - Elections, E-sports
- Real-time dashboards 
  - Leaderboards for games
- Real-time metrics
  - Security and response teams

#### Kinesis - Video Streams
- Allows you to ingest live video data from producers
  - Security cameras, smartphones, cars, drones, time serialized audio, thermal, depth and radar data
- Consumers can access data frame-by-frame or as needed
- Can persist and encrypt (in-transit and at rest) data
- #### CANNOT ACCESS DIRECTLY VIA STORAGE, ONLY VIA API
- Integrates with other AWS services
  - ex. Rekognition and connect

>Video 18
#### Amazon Cognito
- Authentication, Authorization and user management for web/mobile apps
- Provides 2 uses
1) Provides User Pools - Sign-in and get a JSON Web Token (JWT)
   - Allows partner login. THIS IS DIFFERENT FROM ALLOWING AWS ACCESS
     - A joined sign in resource, helps coordinate with other providers
   - User directory management and profiles, sign-up and sign-in (Customizable web UI), MFA and other security features
2) Identity Pools - Allows you to offer access to Temporary AWS Credentials
   - Unauthenticated identities - Guess Users
   - Federated Identities - SWAP - Google, Facebook, Twitter, SAML2.0 and User Pool for short term AWS Credentials to access AWS resources
   - IAM Role integration

#### Cognito User Pools
- A JWT is used for confirm that the user has successfully authenticated with one of the partners identities successfully
  - JWT can be used to access self-managed server based resources
  - User pool token CANNOT be used to access modern AWS infrastructure
- Simplifies the management of Identity Tokens (JWT Tokens)
- #### Manage user log in and sign up and provides JWT Tokens

#### Cognito Identity Pools
- Identity pools work with user pools to assign IAM Roles to the user that has been authenticated and provided a JWT Token (by User Pools)
  - Swaps a JWT token with an AWS Token
  - Provides temporary AWS credentials and the user can now access AWS resources
- We can use Identity Federation to have this happen seamlessly 
- #### Swaps external Identity tokens for AWS Tokens
- #### Commonly used together with User Pools to manage external partner logins in 1 central service

>Video 17
#### AWS Glue
- A Serverless ETL (Extract, Transform and Load)
  - This is vs. a data pipeline (which can also do ETL) which uses a server (EMR)
- Moves and transforms data between source and destination
- Crawls data sources and generates the AWS Glue Data Catalog
- Sources:
  1) DStores: S3, RDS, JDBC compatible and DynamoDB
  2) Streams: Kinesis Data Stream and Apache Kafka
  3) Targets: S3, RDS, JDBC Databases

#### Data Catalog
- Persistent metadata about data sources in a region
- One catalog per region per account
- Data catalogs Avoids data silos
- Data catalogs are used by Amazon Athena, Redshift Spectrum, EMR and AWS Lake Formation
  - You then configure crawlers to look through data catalogs to collect data
- The data catalog can then be used by members to see data from various sources in the data catalog
- Glue extracts data in a "Glue Job" which are initiated manually or via events using EventBridge
- Data is loaded into S3, RDS and JDBC Compatible
- When resources are required, flue allocated from AWS warm pool to perform ETL processes
- When to use?
  - Serverless
  - An ETL and Data Catalog service all in 1
  - Ad-hoc or cost effective, pick Glue

>Video 18
#### Amazon MQ
- An SNS and SQS-like product in 1
- SQS and SNS are AWS services using AWS APIs
  - SNS provides Topics and SQS provides Queues
  - Public services, Highly scalable, AWS Integrated
- If a system is existing that is NOT SQS or SNS, Amazon MQ provides an open source message broker solutions
  - Think migration
- Based on Managed Apachie ActiveMQ
  - JMS API
  - Protocols such as AMQP, MQTT, OpenWire and STOMP
- Provides Queues and Topics
- One-to-one or one-to-many
- This is a single instance (Test, Dev, Cheap) or a HA Pair (Active/Stanyby)
- This is VPC Bases - NOT A PUBLIC SERVICE - Private Networking is required
- No AWS Native integration. 
  - Delivers activeMQ product which you manage
- #### An instance that runs the messaging service in your VPC
- #### To migrate your existing on-prem infra, you need to configure the connection via private VPC connections into AWS

#### When to use Amazon MQ
- Use SNS and SQS for most new implementations (default)
- SNS or SQS if AWS integration is required (logging, permissions encryption, service integration)
- Amazon MQ if you need to migrate from an existing system with little to no application change
- Amazon MQ if APIs such as JMS or protocols such as AMQP, MQTT, OpenWire and STOP are needed
- Remember you need private network access for Amazon MQ

>Video 19
#### Amazon AppFlow
- Fully-managed integration service
- Exchange data between applications (connectors) using flows
  - Sync data across applications
  - Aggregate data from different sources
- Public endpoints, but works with PrivateLink (privacy)
- AppFlow Custom Connector SDK (Build your own)
- Examples: Contact records from Salesforce into Redshift or Support Tickets from Zendesk into S3
- Connections store configurations and credentials to access the applications
  - Configure Source to Destination field mapping
  - Optional Data Transformation
  - Optional Configure filters and validation
- Connections can be reused across many flows - they are defined separately 

### Global Content Delivery and Optimization
>Video 1
#### Cloudfront Architecture
- Terms:
  - Origin - The source location of your content
    - S3 Origin or a Custom Origin
  - Distribution 
    - The "configuration" unit of CloudFront
  - Edge Locations
    - The local cache of your data
  - Regional Edge Cache 
    - Larger version of an edge location. Provides another layer of caching
- In general, workflow:
  1) User checks edge location cache
  2) If edge location cache miss, checks Regional edge cache
  3) If cache miss, fetch origin
  4) Return origin content to regional edge cache and edge location cache, and return to user
  5) All other users will get cache from edge location or regional cache
- Integrates with ACM (certificate manager) for HTTPS
- Upload direct to Origins. There are no read/write with CloudFront. 
  - Uploads are handled at origin and NOT at edge locations
- Distributions contain configurations when they are deployed to the edge location
  - Cache behaviors sit between the origins, and the origin->cache behavior is defined by this configuration
    - Example: How to route something if there is a cache miss
  - It will match patterns and then a behavior is used. Otherwise, it defaults

>Video 2
#### CloudFront Behaviors
- You select what path pattern to match, and then you can choose what to do with the options
  - example: 
    - which origin or origin groups to route to, 
    - cache behavior, 
    - response header policy, 
    - TTL settings, restricting viewer access (with trusted key groups or trusted signers, which will require tokens or signed URLs) etc.

>Video 3
#### CloudFront TTL and Invalidations
- When a cache is stored on the cache from the origin, the object is always returned to the user
- Once the Object expires (via TTL, Time to live) the origin checks the origin for the object version
  - If the object is the same as the one in the cache, a 304 is returned (not modified)
  - If the object is old, a 200 is returned and the object is updated to the new object
#### Exam Power Up
- The more cache hits, the lower the origin load
  - Default validity period (TTL) is 24 Hours
- You can set minimum TTL and Maximum TTL
- Headers:
  - Origin Header: Cache-Control max-age(seconds)
    - Setting this will set the TTL in CF for cache expiration
  - Origin Header: Cache-Control s-maxage(seconds)
    - Setting this will set the TTL in CF for cache expiration
  - Origin Header: Expires(Date and Time)
    - Setting a date and time that will set the TTL in CF for cache expiration
  - The Minimum and Maximum TTL values are the min and max that the origin headers will max or min at (if its more than the max, the max is set. If its less than the min, the min will be set)
  - Can be set using Custom Origin or S3 (Via Object Metadata)

#### Cache Invalidations
- Cache Invalidations are performed on a distribution
- Invalidations apply too all edge locations.
  - Invalidation takes time to complete
  - ex. /images/filename.jpg <- invalidate the file itself
  - ex. /images/filename* <- invalidate all files of filename*
  - ex. /images/* <- invalidate everything under images
  - ex. /* <- invalidate all files
  - Versioned file names would be a good idea to use (ex. wiskers1_v1.jpg vs wiskers1_v2.jpg) <- this prevents having to invalidate everything, which costs money

#### AWS Certificate Manager (ACM)
- HTTP - Simple and insecure
- HTTPS - SSL/TLS Layer of Encryption added to HTTP
  - Data is encrypted in transit
  - Certificates prove identities 
  - Chain of Trust - Signed by a trusted authority
- ACM lets you run a public or private Certificate Authority (CA)
- Private CA - Applications need to trust your private CA
- Public CA - Browsers trust a list of providers, which can trust other providers
- ACM can generate or import certificates
- If generated, it can automatically renew them
- If imported, YOU ARE RESPONSIBLE for renewal
- Certificates can be deployed out to supported services
- Supported AWS Services ONLY
  - eg. CloudFront and ALB... NOT EC2
- ACM is a regional service
- Certificates cannot leave the region they are generated of imported in
- #### To use a certificate with an ALB in ap-southeast-2, you NEED a cert in ACM in ap-southeast-2
- #### For Global Services such as CloudFront, operate as through within us-east-1 

>Video 4
#### CloudFront and SSL/TLS
- CloudFront Default Domain Name (CNAME)
  - ex. https://d111111abcdef8.cloudfront.net/
- SSL Supported by default (*.clouddfront.net cert)
- Alternative Domain Names (CNAMES) ex. cdn.catagram.... require certifications 
- To add your own name:
  - Verify Ownership (optionally HTTPS) using a matching certificate
  - Generate or import into ACM
    - #### INTO us-east-1
  - Options
    - HTTP or HTTPS
    - HTTP => HTTPS
    - HTTPS Only
  - Two SSL Connections: Viewer => CloudFront and Cloudfront => Origin
    - Both need to have valid public certificates (and intermediate certificates)
      - Self-signed certs WILL NOT WORK

#### CloudFront and SNI
- Historically, every SSL enabled site needed its own IP
- Encryption starts at the TCP connection (lower layer than HTTPs, level 7)
- With HTTPS, Host headers happen after that - Layer 7 // Application Layer
- SNI is a TLS extension, allowing a host to be included before a connection is made (which was impossible before this was added)
  - Resulting in many SSL Certs/Hosts using a shared IP
  - Old browsers don't support SNI
    - CF charges extra for dedicated IP
      - Dedicated IP at each CF edge location to support non-SNI Capable Browsers
  - Viewer Side 
    - Certificate issued by a trusted certificate authority (CA) such as Comodo, DigiCert or Symantic or ACM (us-east-1) MATCHES THE DNS NAME
  - Origin side
    - Origins need to have certificates issued by a trusted authority (CA) 
    - ALB can use ACM, others need to use an external generated certification. NO SELF SIGNED CERTS

>Video 5
#### Origin Types and Architecture
- You can use custom headers that only accept headers you configure for your custom Origin
- Custom Origins allow you to set custom ports for you to connect to (for your custom origin)
  - Can customize for 
    - HTTP port
    - HTTPS port
    - To match if you use both

>Video 6
#### Securing CloudFront and S3 using OAI (Origin Access Identity)
- How to deny access to your services behind a CloudFront Distribution where only CloudFront has access to the resource (or S3)
- This is applicable when you do not use the static website feature of S3
- For AWS Origins
  - An OAI is a type of identity
    - It can be associated with CloudFront Distributions
    - CloudFront "becomes" that OAI
    - The OAI can be used in S3 Bucket Policies
    - You can then Deny ALL but one or more OAIs (only allow access to CloudFront in this case)
  - A CloudFront distribution (so in this case, an edge location) will now be able to access the resource through an OAI allow
- What about custom origins? (Non-AWS origins)
  - Force HTTPS to be used when edge locations are accessed
  - Force HTTPS to be used to access the custom origin
  - Force a requirement for a custom header to be injected at the edge location and accept only a custom header via HTTPS at the origin 
    - Only the CloudFront would be able to know the custom header
- OR
  - Add AWS publishes the IP ranges of the edge locations
  - Add a Firewall to your Custom Origin to accept ONLY IP ranges of the CloudFront servers
  - Only CloudFront will be able to access the Origins

>Video 7
#### CloudFront Private Behaviours - Signed URLs and Cookies
- CloudFront can be run in 2 ways
  - Public
    - Open Access to objects
  - Private
    - #### Requests require signed cookies or a secure URL
- Multiple behaviours
  - each is public OR private
- Old way to access a private CloudFront
  - A CloudFront Key is created be an Account Root User
  - The Account is then added as a TRUSTED SIGNER
- The new way is by using
  - Trusted Key Group(s) added

#### Difference between signed URL's and signed cookies
- Signed URLs provide access to ONE OBJECT
  - Historically RTMP distributions could not use cookies
- Use URL's if your client does NOT support cookies
- Use Cookies provide access to a group of objects
- Use Cookies for groups of files/all files of a type (ex. all cat gifs)
- Use Cookies if maintaining application URL's is important

#### Example
- Application wants to get access to private pictures that require restricted access
1) The user application connects to CloudFront, which forwards the un-authenticated request to the API Gateway
2) Using ID Federation, the API Gateway adds identity authentication tokens with the request to a lambda sign-on function, which generates a cookie for the user. This cookie is returned to the user to access the private S3 bucket
3) The user receives the cookie, and sends another request with the cookie included back to CloudFront with a request for S3
4) CloudFront forwards the request along to S3 with the Cookie. The cookie is accepted (signed cookie) and pictures are returned from S3
- Note that the S3 bucket is also secured with one of the methods to prevent bypassing (likely OAI identity in the bucket policy)

>Video 8
#### Lambda at edge
- A feature that allows you to run lightweight lambda functions at edge locations
- Adjust data between the viewer and the origin
- Currently supports Node.js and Python
- Run in the AWS Public Space (NO VPC)
- Layers are NOT SUPPORTED
- Different limits vs normal lambda functions
- Can be placed between
  1) Between the view and CloudFront
     - Limits of: 128MB and 5 seconds
  2) Between CloudFront and the origin
     - Limits of: 128MB and 30 seconds
  3) Between the Origin and CloudFront
     - Limits of: 128MB and 30 seconds
  4) Between CloudFront and the viewer
     - Limits of: 128MB and 30 seconds
- Can be used for:
  - A/B Testing - Viewer Request (redirect by changing the URL)
  - Migration between different S3 origins - Origin Request - Redirect the origin request in a controlled way
  - Different objects based on device - Origin Request
  - Content by country - Origin Request

>Video 9
#### AWS Global Accelerator
- Optimizing data transfer between the user to the origin
- Starts with 2 anycast IP Addresses that a user connects to over the public network
  - Anycase - IP's allow a single IP to be in multiple locations. Routing moves traffic to the closest location
    - This means that the user will be routed to the nearest edge location with anycast IP addresses, which will then be placed on the AWS network
- From an edge location, data transits globally across the AWS global backbone network. Much faster
- Moves the AWS network closer to the customer
- Connections enter at edge using anycast IP addresses
- Transits over AWS Backbone to 1+ locations
- Global Accelerator is for NON HTTP/S (TCP/UDP)
  - Difference from CloudFront
- Global Accelerator does NOT CACHE information
- Global Accelerator does NOT do anything with HTTPS - Cannot use layer 7 stuff (security) like CloudFront. 
  - Network speed only

### Advanced VPC Networking
>Video 1
#### VPC Flow Logs
- Capture metadata (DOES NOT CAPTURE CONTENTS in a VPC)
- Flow logs can be captured (downwards) by:
  - Attached to a VPC 
    - All ENIs in that VPC
  - Attached to Subnets
    - All ENIs in that Subnet
  - To ENIs directly
- Flow Logs are NOT REAL TIME
- Log destinations are required
  - S3 or CloudWatch Logs
  - Or Athena for querying (ad hoq SQL-like querying engine)

>Video 2
#### Egress-Only Internet Gateway
- With IPv4 addresses are private or public
  - NAT allows for private IPs to access the public networks
    - NAT does NOT allow externally initiated connections IN
- With IPv6 all addresses are public
- Internet Gateway (IPv6) allows all IPs IN AND OUT
- Egress-Only is outbound ONLY for IPv6
- Egress-Only Gateways are Highly Available by default across all AZs in the region - Scales are required
  - Same as the normal Internet Gatewats
- Default IPv6 Route ::'0 added to the Route Table with eigw-id as the target

>Video 3
#### VPC Endpoints (Gateway)
- Provide private access to S3 and DynamoDB
  - Allow a private resource in a private only VPC to access a public service
  - Normally you would need some infrastructure with public access to access these public services
- Prefix List is added to the route table and uses the Gateway Endpoint as the target
  - The Gateway Endpoint acts as a destination and uses the Gateway Endpoint instead of the Internet Gateway
- Highly Available across all AZs in a region by default 
- Does NOT go into another subnet. It can be connected in the same subnet as the source
- You can only access the Gateway Endpoint from within the VPC
  - You use Subnet Lists and the VPC Router to point directly to the Gateway Endpoint, which is Highly Available in the VPC 
  - You cannot access the Gateway Endpoint from outside the VPC since it uses the Prefix List and is Subnet Associated
- Endpoint policy is used to control what can access the endpoint
- Gateway Endpoints are regional. They CANNOT access cross-regions
- Prevents Leaky Buckets - S3 Buckets can be set to private only by allowing access ONLY from a specific gateway endpoint
  - You implicitly deny everything EXCEPT the gateway endpoint

#### Typically
- A private only instance would usually go through a NATGW, then go through the VPC router and then go through the Internet Gateway to say, S3
- With a VPC Endpoint Gateway, the private instance has a Prefix List on the Subnet List that points to the VPC Gateway Endpoint (via the VPC Router), which points directly to the public resource, say, S3
  - Does not go through the public internet, nor does it go through a different Subnet

>Video 4
#### VPC Endpoints (Interface)
- Provide private access to AWS Puiblic Services
  - Historically, we used Interface Endpoints for anything NOT S3 and DynamoDB... until recently, as S3 is now supported!
- Added to specific subnets as an ENI. 
  - THIS IS NOT HIGHLY AVAILABLE
  - To be highly available, you need to add one endpoint per subnet per AZ used in the VPC
- Network access controlled via Security Groups
- Endpoint Policies - Restrict what can be done with the endpoint
  - TCP and IPv4 ONLY
  - #### Uses PrivateLink
    - Allows 3rd party applications to be deployed directly into your VPC
- Endpoint provides a NEW service endpoint DNS
  - Ex. vpce-123-xyz.sns.us-east-1.vpce.amazonaws.com
    - Endpoint Regional DNS
    - Endpoint Zonal DNS
  - Applications can optionally use a new service endpoint DNS or
    - PrivateDNS overrides the default DNS for the service
- If the service is in a public subnet, it will go via the public endpoint name via the Internet Gateway (public DNS name)
- If the service is in a private subnet, an interface endpoint will be added in the public zone and connect to the service via the interface endpoint (an ENI configured for a specific service) <- as if the service is injected into the VPC, never goes to the public internet
- If the service uses a private DNS, it will ALWAYS resolve/go through the interface endpoint and it will be routed via the Interface Endpoint

#### Exam Powerup
- Gateway Endpoints
  - Uses prefix lists and route tables
    - Never requires changes to the app
    - The app thinks its accessing services directly
    - Is Highly Available
    - Only S3 or DynamoDB
- Interface Endpoints
  - Uses DNS and Interface Endpoints to access services
  - Can use public DNS or private DNS to access the service
    - public DNS
      - will resolve to the service via Internet Gateway if a public service
      - will resolve to the Interface Endpoint (ENI) if in a private subnet
    - private DNS
      - will always resolve to the Interface Endpoint if 
  - Is NOT highly available
    - Requires an Interface Endpoint in every AZ to be HA (ENI)
  - Anything not DynamoDB

>Video 5
#### VPC Peering
- Lets you create a Direct encrypted network link between 2 VPCs
  - ONLY 2 (LIMIT)
- Works same/cross-region and same/cross-account
- Optional - Public Hostnames resolve to the private IPs for that instance
- Same region Security Groups can be referenced by peer Security Groups
- VPC peering is NOT transitive peering
  - Peer A, B and C will need 3 peers - A->B, A->C, B->C
- Routing configuration is needed
  - Security Groups and NACLs can filter
- Route tables at both sides of the peering connections are needed, directing traffic flow from the remote CIDR at the peer gateway object
- VPC Peering connections CANNOT be created where there is overlap in the VPC CIDRs
  - Ideally NEVER use the same address ranges in multiple VPCs
- VPC Peering communication is encrypted and transits over the AWS Global Network - Fast data transfer

### Hybrid Environments and Migration 
>Video 1
#### Border Gateway Protocol (BGP)
- BGP is a routing protocol
  - How a router figures out what the optimized path is
  - A router will exchange a table with the destinations it knows about and the associated ASPATH with that network
    - ASPATH - The fastest path
    - ASPATH will outline what ASN numbers are associated with the paths
      - ASN is the unique number assigned to the specific router network
  - By Default BGP uses the shortest path to the target
- Made up of Autonomous Systems (AS)
  - Routers controlled by 1 entity
  - AS is a network in BGP
- ASN - Unique and allocated by IANA (0-65534), 64512-65534 are private
- BGP operates over tcp/179 - its reliable
- BGP is NOT automatic, peering is a manual configuration
- BGP is a path-vector protocol 
  - It exchanges the best path to a destination between peers
- iBGP
  - Internal BGP - Routing WITHIN an AS
- eBGP
  - External BGP - Routing BETWEEN AS's
- A BGP does not take into account performance, it looks for the shortest length
  - AS Path Prepending - Can be used to artificially extend the path between source to destination by adding "dummy AS'"

>Video 2
#### IPSEC VPN Fundamentals
- IPSEC is a group of protocols that sets up secure tunnels (IPSec Tunnels) across insecure networks
- Connects between 2 peers (local and remove)
- Provides 
  - Authentication
  - Encryption
- Any data inside tunnels is encrypted 
  - secure connection over an insecure network
- NOTE:
  - Symmetric Encryption is fast but its hard to exchange keys securely
  - Asymmetric encryption is slow but with a public and private key, it can swap information without worrying about exchanging secret keys
- IPSEC has 2 main phases
  - IKE Phase 1 (Slow and heavy)
    1) Authenticate - Pre-shared key (password) and certificate
       - Certificate or pre-shared key is authenticated by each side
    2) Using Asymmetric encryption to agree on and create a shared symmetric key
       - Each side creates a DH private ke and derives a public key.
       - The public keys are exchanged over the public internet
       - Each side takes their private key and the remote peer's public key and independently generate an identical Diffie-Hellman Key (with math magic)
       - Using the DH Key the peers exchange key material and agreements
    3) IKE SA Created (Phase 1 tunnel)
       - Each side generates a Symmetrical key using the DH Key and exchanged material
  - IKE Phase 2 (Fast and Agile)
    1) Use the keys agreed on in phase 1
       - At this point both sides have a DH Key and symmetric private keys and an established security association (Phase 1 Tunnel), now we want to get the VPN up and running
       - Symmetrical key is used to encrypt and decrypt agreements and pass more key material
    2) Agree encryption method and the keys used for bulk data transfer
       - The best shared encryption and integrity methods are communicated and agreed upon
       - DH Key and Exchanged Key Material are used to create a symmetrical IPSEC Key
       - IPSEC Key is used for bulk encryption and decryption of interesting traffic
    3) Create IPSEC SA (Phase 2 Tunnel, architecturally running over phase 1)

#### Policy based VPNs vs Route-Based VPNs
- Policy Based VPNs
  - Rule sets match traffic 
    - a Pair of Security Associations
  - Different rules/security settings
- Route Based VPNs
  - Target matching (prefix)
  - Matches a single pair of Security Associations

#### Policy Based VPN
- You can run many Phase 2 tunnels on top of phase 1 tunnels, as each policy has a Security Association pair and unique IPSEC Keys for each
- Harder to set up

#### Route-Based VPNs
- You can only run 1 Phase 2 tunnel on top of the phase 1 tunnel, has a single Security Association pair and a single IPSEC Keys
- Easier to set up

>Video 3
#### AWS Site-to-Site VPN
- A logical connection between a VPC and on-premises network encrypted using IPSec, running over the private internet
- Fully Highly Available - if you design and implement it correctly
- Quick to provision (Less than 1 hour)
- Create a Virtual Private Gateway (VGW)
  - What you target on a route table, what you associate with in your VPC
  - The VPG has multiple endpoints in the AWS public zone that allow connections
- Create a Customer Gateway
  - A logical configuration in AWS or a physical device the logical configuration represents
  - Configure the CGW with the VPG IP address so it can connect with the VGW
- VPN Connection between VGW and CGW is the VPN
  - Require the Customer Gateway Router IP and associate it with the Virtual Private Gateway
  - Create a VPN connection that the endpoints use which tunnel to the CGW
- Note that the single point of failure is the CGW (the customer gateway)
  - To resolve this, you can add another on-premises CGW at another IP address
  - You would need to add another IP connection from the VGW to the new CGW
    - Note: This creates new VPC endpoints that will connect to the CGW and allow for failure of the CGW

#### Static vs Dynamic VPN (Uses BGP)
- Static VPN
  - Static routes are added to the route tables
  - Networks for remote side statically configured on the VPN connection
  - No load balancing and multi connection failover
- Dynamic VPN
  - Boarder Gateway Protocol (BGP) is configured on both the customer and AWS side using (ASN). Networks are exchanged via BGP
  - Multiple VPN connections provide HA and traffic distribution
  - Routes for remote side added to route tables as static routes
    - Route propagation (if enabled) means routes are added to Route Tables automatically

#### VPN Key Considerations
- VPNs have Speed Limitations - 1.25Gbps
- VPN latency considerations - Works over public internet, can be inconsistent
- Cost 
  - AWS Hourly cost
  - GB out cost
  - Data cap (on premises)
- Speed of setup - hours (entire thing is software configuration)
- Can be used as a backup for Direct Connect (DX)
- Can be used at the same time/with Direct Connect (DX)

>Video 4
#### AWS Direct Connect (DX)
- A physical connection (1, 10 or 100Gbps)
- Business premises -> DX Location -> AWS Region
- You order Port Allocation at a DX Location
  - You need to arrange physical connections to the DX Location and your physical server location
- Port Hourly Cost and Outbound Data Transfer
- Provisioning time - Physical cables and no resilience 
- Low and consistent latency + high speeds (physical connections or server connections)
- Can be used to access AWS Private Services (VPCs) and AWS Public Services - Without internet
- DX Locations are typically not owned by AWS
  - It is typically a large regional data center where AWS have space and equipment rented
  - You might also have customer or comms partner cages, which the customer or Partner DX Router is located
    - If you do not have optional infrastructure at the DX location, you can use a partners location to connect to
  - You cross connect AWS port to Customer/Partner Port Link at the DX location, which then connects to your on site server

>Video 5
#### Direct Connect Resilience
- AWS to DX location is assumed to be highly available. It has redundant high speed connections
- By default, a single cross-connect links a DX port with a customer or provider router in the data center
  - Using multiple AWS DX routers and customer DX routers, you can provision multiple DX cross-connect links
- A DX is extended from the DX location (customer or provider DX router) to a customer premises
  - Using multiple customer DX routers, you can provision multiple connections (make sure different independent cables) to multiple customer premises routers (in different buildings, need redundancies all the way through)

>Video 6
#### AWS Transit Gateway (TGW) 
- Network Transit Hub - To connect VPCs to on premises networks
  - Significantly reduces network complexity
  - Single network object - Highly Available and Scalable
- Attachments to other network types
  - VPC, Site-to-Site VPN and Direct Connect Gateway
- For example:
  - Having multiple VPCs and a customer connections, you would need
    - peer-to-peer connections for EACH VPC (many)
    - Customer Gate Way connections for each VPC
      - Might want 2+ Customer Gateways, which means more connections
    - Scales poorly
  - A transit gateway uses a side-side gateway router
    - Instead of the VPC connecting to the CGW, the Transit Gateway is used as the attachment that connects the VPCs to the Customer Gateways
    - Allows you to remove Peer-to-Peer connections as the Transit Gateways can be used instead
    - Can be used to peer Transit Gateways to other Transit Gateways (provides global connectivity)
    - Can connect to Direct Connect Gateway (to on premises networks)

#### Exam Power Ups
- Transit Gateways Support transitive routing
  - Compare with Peer-to-Peer connect
- Can be used for global networks
- Share between aaccounts using AWS RAM
- Peer with different regions
  - Same or cross account
- Less complex networks can be done with Transit Gateways

>Video 7
#### Storage Gateway - Volume
- Runs as a Virtual Machine (or hardware appliance)
  - Acts as a bridge between storage that is on premises and in AWS
- Presents storage using iSCSI(local storage), NFS (Linux) or SMB (Windows)
- Integrates with EBS, S3 and Glacier within AWS
- Used for:
  - Migrations
  - Extensions
  - Storage Tiering
  - DR
  - Replacement of Backup Systems
- For the exam, you need to pick the right mode

#### In general - In Volume Storage Mode <- Backups, DR or Migration
- Data is stored locally
- AWS is used to copy volumes into S3 (uses S3 for backups)
- Servers run with local disks
  - Interact with Network Attached Storage using iSCSI as Raw Block Devices
- Storage Gateway is run on a VM and connects to Servers W/Local disks
  - #### In Volume Storage Mode: Storage Gateway has local storage that stores data locally
  - Uses an upload buffer (local storage in VM) 
  - Uses the local storage to push data to the Storage Gateway Endpoint in AWS
    - Local storage has snapshots taken and snapshots are stored in EBS Snapshots
    - EBS Snapshots can then be provisioned into EBS Volumes
- Great for "Full Disk" backup of servers
- Assists with disaster recovery
  - Creates EBS Volumes in AWS
- DOES NOT IMPROVE DATACENTER CAPACITY
  - Main copy of data is stored on the gateway

#### In general - In Volume Cached Mode <- Database Extension
- Data is stored in AWS
- Cache is stored on-premises
- Servers run with local disks
    - Interact with Network Attached Storage using iSCSI as Raw Block Devices
- Storage Gateway is run on a VM and connects to Servers W/Local disks
    - #### In Volume Cached Mode: Storage Gateway has local cache. ALL PRIMARY DATA IS STORED IN S3
    - Primary storage in S3 is AWS Managed
      - The only data stored locally is cached data - data that is accessed frequently
    - In this case, AWS Storage appears to be on-premises but it is actually in AWS
      - You can use AWS for database extension architecture 

>Video 8
#### Storage Gateway - Tape - VTL Mode
- Uses virtual tape drives that use S3 run in a way that appears like tapes to migrate a tape library into AWS
  - Presents a media changer and tape drive as if it is a physical tape system
  - The Virtual Tape Library (VTL) uses S3 and acts as the tape library
  - The Tape Shelf (VTS) stored in Glacier
- Storage Gateway in VTL mode has an upload buffer and local cache that it uses to upload to the VTL and VTS 

>Video 9
#### Storage Gateway - File Mode
- Bridges on-premises file storage and S3
- Require Mount Points (shares) available via NFS or SMB
  - Maps directly into S3 bucket
  - Files stored into a mount point, they are visible as objects in an S3 bucket
- Read and write caching ensure LAN-like performance
- File Gateway runs in on-premises and saves files in on-premises file-shares
  - Buckets are linked to the file-share (maps between the file name and the bucket name)
    - This is how you can build flat object storage from a file-storage system
  - Primary data is held in S3
    - You can integrate AWS services into the S3 storage
  - 10 bucket shares per file gateway
- Capable of building unique hybrid-architecture 
  - You can have multiple on-premises locations running File-gateway and it can share the same S3 buckets
    - You can use "NotifyWhenUploaded" API to notify other gateways when objects are changed (it does not propogate to different fileshares automatically)
    - Object lock is not supported - Use read only mode on other shares or tightly control file access for security
  - You can create multiple buckets in various regions to replicate the fileshare S3 bucket and impliment cross-region replication
  - You can configure lifecycle moves in S3 to move files into various object storage classes to automatically move files to save costs

>Video 10
#### Snowball/Edge/Snowmobile
- Ordered from AWS
  - With data, empty, return
  - Empty, fill with data, upload to AWS
- Snowball
  - Ordered from AWS
  - Encrypted with KMS
  - 50TB ot 80TB
  - 10TB to 10PB economical range (multiple devices)
  - Multiple devices on premises
  - Storage only, NO compute
- Snowball Edge
  - Contains Compute
  - Larger capacity than Snowball
  - Faster upload than Snowball
  - Versions:
    - Storage Optimized (with EC2)
    - Compute Optimized
    - Compute with GPU
  - Ideal for remote sites or where data processing on ingestion is needed
- Snowmobile 
  - A truck that allows for large data transfer
  - Single location where 10+PB is required
  - Store up to 100PB of storage per Snowmobile

>Video 11
#### AWS Directory Service
- Directory (in general)
  - Stores objects (ex. Users, Groups, Computers, Servers, File Shares) with a Structure (domain/tree)
  - Multiple trees can be grouped into a forest
  - Commonly used  in Windows Environments
  - Sign in to multiple devices with the same username/password provides centralized management for assets
  - Microsoft Active Directory Domain Services (AD DS)
    - AD DS Most Popular
    - Open source Alternative (SAMBA)
- AWS Directory Service
  - AWS Managed implementation of AD DS
  - Runs within a VPC
  - To implement High Availability, you will need to deploy AW DS into multiple AZ
  - Some AWS Services REQUIRE a directory (ex. Amazon Workspace)
  - AWS Managed can be isolated 
  - or integrated with existing on-premises systems
  - or act as a proxy back to on-premises
- #### Simple AD Mode
  - Standalone directory which uses Samba 4
  - Support up to 500 users for small and 5000 users for large
  - Integrates with AWS services
    - EC2 intances can join SimpleAD and Workspaces can use it for logins and management
    - Not designed to integrate with any existing on-premises directory systems such as Microsoft AD
- #### AWS Managed Microsoft AD
  - Amazon Workspaces can connect with Microsoft AF Mode, which connects to on-premises directory systems
    - Primary running location is in AWS
      - Trust relationships can be created between AWS and on-premises directory services
      - Resilient if the VPN Fails
        - Services in AWS will still be able to access the local directory running in Microsoft Directory Service
    - Supports applications which require Microsoft Active Directory 
      - Simple schema or schema updates
- #### AD Connector
  - If you had 1 product that requires Active directory, but you don't want to set up an entire Microsoft Directory on AWS
    - You can use AD Connector to VPN to point back to your on-premises directory
  - Primary directory is located on-premises
  - If private connectivity fails, the AD Proxy will NOT function
    - inturruption of services

#### When to use which Mode
- Simple AD
  - The default. Simple requirements. A directory in AWS
- Microsoft AD 
  - Applications in AWS which need MS AD DS, or you need to Trust AD DS
  - Note: This is a full on Microsoft AD in AWS, exists on AWS
- AD Connect 
  - Use AWS Services which needs a directory WITHOUT storing any directory info in the cloud
  - Proxy your on-premises directory
  - Note: Directory runs on-premises and NOT in AWS

>Video 12
#### AWS DataSync
- Data Transfer Service INTO and OUT OF AWS
  - Service that manages data migrations from end-to-end
- Migrations, Data Processing Transfers, Archival/Cost Effective Storage or DR/BC
- This project is designed to work at Huge scale
- Keeps Metadata (ex. Permissions, Timestampes)
- Built in data validation
- Features
  - Scalable - 10Gbps per agent (~100 TB per day)
  - Bandwidth Limiters (avoid link saturation)
  - Incremental and scheduled transfer options
  - Compression and encryption
  - Automatic recovery from transit errors
  - AWS Service integration 
    - S3, EFS, FSx
  - Pay as you use per GB of data moved

#### Example
- SAN/NAS Storage is looking to transfer to AWS
- A DataSync Agent runs on a virtualization platform such as VMWare and communicate with the AWS DataSync Endpoint
  - Communicates with on-premises storage with NFS/SMB
  - Encryption in transit to AWS with TLS
  - Schedules can be set to ensure transfer of data occurs during or avoiding specific time windows
  - Customer impacts can be minimized by setting bandwidth limiters
- Data enters the DataSync Endpoint and locations define the source or destination for the sync of data TO or From AWS
  - ex. S3, EFS, FSx, NFS, SMB

#### DataSync Components
- Task
  - A Job within DAtaSync, defines what is being synced, how quickly, from and to where
- Agent 
  - Software used to read or write to on-premises data stores using NFS or SMB
- Location 
  - Every task has 2 location From and To
    - ex. Network File System (NFS), Server Message Block (SMB), Amazon EFS, Amazon FSx and Amazon S3

>Video 13
#### FSx for Windows File Server
- AWS Support for Windows Environments
- Fully managed native Windows file servers/shares
- Designed for integration with Windows environments
  - Can integrate with Directory Service or Self-Managed AD
- Can be configured with Single or Multi-AZ within a VPC
- On-Demand and Scheduled backups
- Files are accessible using VPC, Peering, VPN, Direct Connect
  - For exam, think about file systems or file sharing in Windows
- FSx can integrate with either On-Premises or in AWS

#### Key Features and Benefits
- VSS - User-Driven Restores
  - Users in workspace can view and restore versions of files
- FSx provides a Native file system accessible over SMB (SMB = Windows)
- Uses Windows permission Model
- Supports DFS (Distributed File System)
  - Scale-out file systems in Windows environments (scale out file structure)
- Managed - no file server admin
- Integrates with Directory Services and YOUR OWN Directory service (on premises)

>Video 14
#### FSx for Lustre
- Managed Lustre - Designed for HPC - LINUX Clients (POSIX)
  - Machine learning, Big data, Financial Modelling
- 100's GB/s throughput and sub milliseconds latency
- Deployment types - Persistent or Scratch
  - Scratch
    - Highly optimized for short term
      - No replication and fast
  - Persistent
    - Long term, High Availability, Self Healing
- Accessible over VPN or Direct Connect
- FSx - Where data lives while processing occurs
- Objects reside in the repository 
  - S3
- Data is "Lazy Loaded" from S3 (S3 Linked Repository) into the file system as needed
- Data can be exported back to S3 using hsm_archive
- Metadata is stored on Metadata Targets (MST)
- Objects are stored on object storage targets (OSTs) (1.17TiB)
- Baseline performance based on size
- Size - Minimum 1.2TiB, then increments of 2.4TiB
- For Scratch 
  - Base 200MB/s per TiB of storage
- For Persistent 
  - 50MB/s, 100Mb/s and 200MB/s per TiB of storage
  - Burst up to 1,300MB/S per TiB (credit system)

#### Key Features and Benefits
- Scratch is designed for pure performance
  - Short Term or Temp workloads
- No High Availability
- No Replication
- Larger file systems means more servers, more disks and more chance of failure
- Persistent has replication within ONE AZ ONLY
- Auto-heals when hardware failure occurs
- You can backup to S3 with both products

>Vedio 15
#### AWS Transfer Family
- Managed file transfer service
  - Supports transferring TO and FROM S3 and EFS
- Provides managed "Servers" which support protocols
- Protocols that are support
  - File Transfer Protocol (FTP) - Unencrypted file transfer
  - File Transfer Protocol Secure (FTPS) - File transfer with TLS encryption
  - Secure Shell (SSH) File Transfer Protocol (SFTP) - File transfer over SSH
  - Applicability Statement 2 (AS2) - Structured B2B Data
- Identities
  - Service Managed
  - Directory Service
  - Custom (Lambda/APIGW)
- Managed File Transfer Workflows (MFTW) - Serverless File Workflow Engine
- 3 Different Endpoint types to connect AWS to
  1) Public
     - Dynamic IP (Can Change)
     - Managed by AWS (use DNS)
     - Cannot control access via IP Lists
  2) VPC Internet
     - Using Direct Connect (DX) or VPN
     - Uses Security Groups and NACL for control
        - SFTP
        - FTPS
  3) VPC Internal
     - Using Direct Connect (DX) or VPN
     - Uses Security Groups and NACL for control
        - SFTP
        - FTP
        - FTPS
- Multi-AZ - Resilient and scalable
- Cost: Provisioned Server per Hour $ + Data Transferred $
- FTP and FTPS - Directory Service or Custom IDP only
- FTP - VPC Only (cannot be public)
- AS2 VPC Internet/Internal Only
- Used if you need to access S3/EFS, but with existing protocols
- Used if iIntegrating with existing workflows
- Used if using MFTW (Managed File Transfer Workflow feature) to create new ones

### Security, Deployment and Operations
>Video 1
#### AWS Secrets Manager
- It does share functionality with parameter store
- Designed specifically for secrets (Passwords, API Keys, etc)
  - Usable via console, CLI, API or SDK's 
    - Integrates with applications
  - Supports automatic rotation
    - This uses Lambda
  - Directly integrates with some AWS Products
    - RDS
- Between Parameter store vs Secret Manager
  - Secret manager is specifically for secrets 
  - Parameter is used for storing string and CAN store secrets
  - The main difference is Secret Manager can integrate Lambda functions
    - Lambda can be used to update passwords automatically
    - Lambda can be used to rotate secrets and sync the new passwords with products such as RDS
- Secrets are encrypted with KMS

>Video 2
#### Application (Layer 7) Firewalls
- Normal Firewall (Layer 3,4)
  - Firewall sees packets, segments, IPs and ports
    - Sees 2 way flow of information
      - Request and response
- Normal Firewall (Layer 3,4 and 5)
  - Capability for request and response to be considered as part of a session 
    - Reduces admin overhead and allows for more contextual security
- Layer 7 Firewall
  - Capable of seeing content contained in the packets
  - Understand layer 7 protocols (HTTP) 
    - Headers, DNS, RATE, Content etc...
  - Encryption HTTPS terminated (decrypted) on the layer 7 firewall
  - New encryption Layer 7 connection between the Firewall and the back end
  - Data at L7 can be inspected and blocked, replaced or tagged (adult, spam, off topic, etc.)
  - Layer 7 Firewalls can identify normal or abnormal requests
    - Protocol specific attacks
  - Able to identify, block and adjust applications (ex. Facebook)
  - Layer 7 Firewall keeps all Layer 3,4 and 5 features but adds functionality

>Video 3
#### Web Application Firewall (WAF)
- Protects web applications 
  - CloudFront, Application Load Balancer, AppSync, API Gateway
- Configured in the Web ACL
  - Use Rules within Rule Groups (within WEB ACL)
  - You can manually update Rules
  - You can use EventBridge and Lambda to update rules (ex. IP list parser)
- Example WAF Architecture (Firewall loop)
  - WAF can output logs to S3 every 5 min
  - CloudWatch Logs can read and feed into Firehose, which enters another bucket
  - Lambda event driven processing of logs or event subscription
  - Trigger Lambda updates to the WEB ACL

#### WEBACL
- WEBACL Default Action (Allow or Block) - Non matching
- Resource Type - CloudFront or Regional Service
  - Protects API, API GW, AppSync... pick a region
- Add Rule Groups or Rules
  - Rules are processed in order
  - Rules are processed using WCU
    - Web ACL Capacity Units (WCU) - Default to 1500
      - Can be increased with a support ticket
    - WEBACLs are associated with resources (this takes time)
      - Adjusting a WEBACL takes less time than associating one
    - Only 1 WEBACL can be attached to a resource, but many resources can use a WEBACL

#### Rule Groups
- Rule Groups contain rules
- Rule Groups DO NOT have default actions, that is defined when groups or rules are added to the WEBACLs
  - Managed (AWS or Marketplace), yours, Service Owned (ex. Shield and Firewall Manager)
- Rule Groups can be referenced by multiple WEBACL
- Have a WCU capacity (defined upfront with the 1500 capacity)

#### WAF Rules
- Rules:
  - Type
    - Regular 
      - Designed to match if something occurs
    - Rate-Based
      - Designed to match if something occurs at a defined rate
  - Statement
    - What to match (Regular)
    - Count all (Rate-Based)
    - What and Count (Rate-Based)
      - Statements can check Origin country, IP, Label, Header, Cookies, Query Parameters, URI Paths, Query String, Body (first 8192 bytes ONLY), HTTP Method
      - Matches
        - Single
        - And
        - Or
        - Not
  - Action 
    - Allow
      - Regular only
    - Block
      - Stops process
    - Count
      - Allows process to continue
    - Captcha
      - Allows process to continue
    - Extra additions on actions
      - Custom Response (x-amzn-waf-...)
        - Used to handle something and you know its from WAF
      - Label
        - How to handle something, WAF will NOT handle the request, something else does (follow-up rules)
        - Does not stop processing at the WEBACL

#### Pricing
- WEBACL - Monthly $5/Month (these can be reused)
- Rule on WEBACL - Monthly $1/Month
- Request per WEBACL - Monthly ($0.60/1 Million requests)
- Intelligent Threat Mitigation
- Bot Control - ($10/Month) and ($1/1 Million requests)
- Captcha - ($0.40/1000 challenge attempts)
- Fraud Control/Account Takeover - ($10/Month and $1/1000 login attempts)
- Marketplace Rule Groups - Extra

>Video 4
#### AWS Shield
- Provide protection against DDOS
- AWS Shield Standard
  - Free
- AWS Shield Advanced
  - Cost
- Network Volumetric Attacks (L3) - Saturate Capacity
  - Volume attack
- Network Protocol Attacks (L4) - TCP SYN Flood
  - Leave Connections open, prevent new ones
  - L3 and L4 attacks CAN be combined 
- Application Layer Attacks (L7) 
  - Web request floods
    - ex. query.php?search=all_the_things

#### AWS Shield Standard
- Free for AWS customers
- Protection at the perimeter
  - region/VPC or the AWS edge
- Common Network (L3) or Transport layer (L4) attack protection
- Best protection using R34, CloudFront, AWS Global Accelerator

#### AWS Shield Advanced
- $3000 per month per organization, 1 year lock-in + data (OUT)/Month
- Protects CloudFront, R53, Global Accelerator, Anything associated with EIPs (ex. EC2), ALBs, CLBs, NLBs
- Not automatic - Must be explicitly enabled in Shield Advanced or AWS Firewall Manager Shield Advanced Policy
- Cost protection (ex. EC2 scaling) for unmitigated attacks
- Proactive engagement and AWS Shield response team (SRT)
- WAF Integration - includes basic AWD WAF fees for web ACLs, rules and web requests
- Application Layer (L7) DDOS protection (uses WAD)
- Real time visibility of DDOS events and attacks
- Health-based detection - application specific health checks, used by the Proactive Engagement Team
- Protection groups

>Video 5
#### CloudHSM
- An appliance that handles Keys, similar to KMS
- KMS is
  - A Key Management Service
  - It is operated by AWS (Shared service)
    - This can be a problem for security
    - AWS does have access to the keys
- CloudHSM are physical devices that are deployed in a AWS CloudHSM VPC associated with your AZ and injected into the AZ you use in a VPC (works in clusters)
- HSMs keep keys and policies in sync when nodes are added or removed
- AWS Provision HSM but have no access to where the security materials are located (can only update the devices themselves)
    
#### Exam Power Up
- HSM is a Hardware Security Module
- CloudHSM is a True Single Tenant Hardware Security Module (HSM)
- AWS provisioned ONLY
  - The customer fully manages the HSM
- #### CloudHSM is Fully FIPS 140-2 Level 3 (KMS is L2 Overall, some L3)
- CloudHSM is not accessible with IAM
  - Cloud HSM is accessible with Industry Standard APIs
    - PKCS#11, Java Cryptography Extensions (JCE), Microsoft CryptoNG (CNG) Libraries
  - KMS can use CloudHSM as a custom key store. CloudHSM integrates with KMS

#### CloudHSM Use Cases
- No native AWS integration 
  - ex. No S3 SSE
- Offload the SSL/TLS processing for Web Services (cryptographically done in HSM, whereas KMS does it in the instance)
- Enable Transparent Data Encryption (TDE) for Oracle Databases (Oracle encryption into the database itself by the engine)
- Protect the private keys for an issuing certificate authority (CA)
- Anything that uses industry standard key access methods or requires high levels of security, use CloudHSM

>Video 6
#### AWS Config
- 2 Main jobs
  1) Record configuration changes over time on resources
     - Every time a resource is updated, the pre change and post change information is logged
  2) Auditing of changes, compliance with standards
     - It does NOT prevent the configuration changes from happening, there is NO protection. It just tracks
- Regional service
  - Supports cross-region and account aggregation
- Changes can generate SNS notifications and near-realtime events via EventBridge and Lambda
  - Can use EventBridge and Lambda to fix config changed automatically
- Requires you to enable the product, which stores configuration changes (Configuration Item, CI) into a config bucket
- Resources are evaluated against Config Rules - either AWS managed or custom (using Lambda)

>Video 7
#### Amazon Macie <- S3 Product
- A Data Security and DAta Privacy Service for S3
- Discover, Monitor and Protect data 
- Automated discovery of data
  - ex. Private Identifiable Information, Finance, Private Health Information, etc.
- Managed Data Identifiers - Built-in - ML/Patterns
- Custom Data Identifiers - Proprietary - Regex Based
- Integrations - With Security Hub and "finding events" to EventBridge
- Centrally Managed
  - Can be managed via AWS Org or one Macie account inviting another
- Schedule Discovery Jobs, which Amazon Macie uses along with data identifiers (mnanaged or custom) to detect and classify data in S3.
  - Finding events can then be delivered to integrations

#### Amazon Macie - Identifiers
- Managed Data Identifiers - Maintained by AWS
  - AWS provided list of common sensitive data types that Maice can use out of the box
    - Growing list of common sensitive data types, such as credentials, finances, health, personal data
- Custom Data Identifiers - Created by you
  - Regex - defines a pattern to match in data (ex. [A-Z]-\d{8})
  - Maximum Match Distance - how close keywords are to regex patterns
  - Ignore Words - If regex match contains ignore words, ignore

#### Amazon Macie - Findings
- Produce 2 types of findings
  1) Policy Findings
     - A finding that reduces the security of a bucket after Macie is enabled (ex. changing permissions for public access after Macie is enabled)
  2) Sensitive Data Findings
     - Something triggered when an Identifier shown above is matched

>Video 8
#### Amazon Inspector - EC2 Instances, Containers and OS
- Scans EC2 Instances and instance OOS
- Scans Containers
- Finds vulnerabilities and deviations against best practices
  - Lengths - 15 min, 1 hour, 8/12 hours or 1 day
- Provides report of findings ordered by priority
- Network Assessment (Agentless)
- Network and Host Assessment (Agent)
- Rules packages determine what is checked
- Network Reachability (no agent required)
  - Agents can provide additional OS visibility
  - Check reachability end-to-end. EC2, ALB, DX, ELB, ENU, IGW, ACLs, RTs, SGs, Subnets, VPCs, VGWs and VPC Peering
  - RecognizePortWithListener, RecognizePortNoListener, RecognizePortNoAgent
  - UnrecognizedPortWithListener
- Packages (Host assessments, Agent Required)
  - Common vulnerabilities and exposures (CVE)
  - Center for Internet Security (CIS) Benchmarks
  - Security Best Practices for Amazon Inspector

>Video 9
#### Amazon Guard Duty - AWS Account Security
- Continuous security monitoring service
- Analyses supported data sources 
- Uses AI/ML plus threat intelligence feeds
- Identifies unexpected and unauthorized activity as it occurs on your account
  - Notify or event-driven protection/remediation
- Supports multiple accounts (Master and member accounts)

### NoSQL Databases and DynamoDB
>Video 1
#### DynamoDB
- DynamoDB
  - NoSQL Public Database-as-a-Service (DBaaS) - Key/Value and Document Database
    - No self-managed servers or infrastructure
  - Scaling options
    - Manual/Automatic procisioned performance In/Out
    - On-Demand
  - Highly Resilient
    - Across AZs and optionally Globally
  - Really really fast, SSD Based (single digit millisecond)
  - Backups, point-in-time recover, encryption at rest
  - Event-driven integration (Events when data changes)
- A Table is a grouping of Items with the same Primary Key
  - A table can have an infinate number of items in it
  - A Primary Key can be 2 types
    1) Simple (Partition)
    2) Composite (Partition and Sort mix) - Primary Key
  - Each item MUST have a unique value for the Primary Key and Sort Key
    - Can have non, all, mixture or different attributes (DDB has no rigid attribute schema)
    - Item max size - 400KB
  - Capacity (speed) is set on a table
    - Writes - 1 WCU = 1KB/s
    - Reads - 1RCU = 4KB/s
- On-Demand Backups
  - Full copy of a table is retained until removed
  - Can be restored in the same or different region
    - With or without indexes
    - Adjust encryption settings
- Point-in-time recovery (PITR)
  - Not enabled by default
  - Continuous record of changes allows replay to any point in the window (35 day recovery window)
  - 1 second granularity

#### Exam Power Ups
- NoSQL <- Preference DynamoDB in the exam
- Relational Data <- Generally NOT DynamoDB
- Key/Value <- Preference DynbamDb in the exam
- Access via console, CLI, API <- "NoSQL"
  - True SQL is not supported, only "SQL-Like" queries
- Billed based RCU, WCU, Storage and Features

>Video 2 and 3
#### DynamoDB Operations, Consistency and Performance
#### Reading and Writing
- Types: 
  - On-Demand
    - Unknown load, unpredictable, low admin
    - Price per million Read and Write units
  - Provisioned
    - Read Capacity Unit (RCU) and Write Capacity Units (WCU) based on a table
    - Every operation consumes AT LEAST 1 RCU/WCU
    - 1 RCU is 1x4KB read operation per second
    - 1 WCU is 1x1KB write operation per second
    - Every table has a RCU and WCU burst pool (300 seconds)
#### Query and Scan
- Query
  - Query accepts a single Partition Key value and optionally a Sort Key or range
  - Capacity consumed is the size of ALL RETURNED ITEMS. 
    - Further filtering discards data, CAPACITY IS STILL CONSUMED
  - Can ONLY query on PK or PK and SK combo
- Scan
  - Most flexible and also most inefficient 
  - Scan moves through a table consuming the capacity of EVERY ITEM. 
    - You have complete control on what data is selected, any attributes can be used and any filters applied
    - SCAN consumes capacity for EVERY ITEM it scans through
#### Consistency Model
- How DynamoDB is either eventually consistent or strongly consistent
- DynamoDB saves data into separate nodes
  - Leader (storage) Node
    - DynamoDB directs the write at the leader storage node, which is elected from the 3 storage nodes
    - The Leader node replicated data to the other nodes, typically finishing within a few milliseconds
  - Storage Node
    - Data is pushed from the leader node to storage nodes
- Strongly Consistent Reads
  - Connects to the leader node to get the most up-to-date copy of the data
  - Use the leader node if you cannot tolerate data being slightly out of sync
- Eventually Consistent Reads
  - DynamoDB directs the user to one of the nodes. Since you could have a potential to have old reads (before the data is propagated) this is eventually consistent
  - 50% of the cost vs strongly consistent
#### WCU Calculation
- If you need to store 10 items per second, with a 2.5K average size per item
  - Calculate WCU per item and round up (Item Size/1KB)
  - Multiply by average number of seconds
#### RCU Calculation
- If you need to store 10 items per second, with a 2.5K average size per item
    - Calculate WCU per item and round up (Item Size/4KB)
    - Multiply by average number of seconds
      - Note: Eventually consistent reads are 50% of strongly consistent reads, so half the cost of Strongly consistent read costs

>Video 4
#### DynamoDB Local and Global Secondary Indexes
- Indexes in DybamoDB
  - Query is the most efficient operation in DDB
  - Query can only work on 1 PK Value at a time
    - And optionally a single, or range of SK values
  - Indexes are alternative views on table data
    - Local Secondary Index use a different SK (LSI) 
    - Global Secondary Indexes use a Different PK and SK (GSI)
  - Ability on indexes to choose some or all of the base table attributes (projections)
- Local Secondary Indexes is an alternative view of a table - Think of like attached to main cluster/leader
  - LSI MUST be created with a table
  - 5 LSI's per base table
  - Alternative SK on the table
  - Shares the RCU and WCU with the table
  - Attributes - ALL, KEYS_ONLY and INCLUDE
  - LSI's are an alternative view on the base table data using the same PK and different SK
  - Indexes are sparse, only items which have a value in the index alternative sort key are added to the index
    - You can filter stuff to show up in the index
- Global Secondary Indexes - Think of like a new read node
  - Can be created at any time
  - Default limit of 20 per base table
  - Alternative PK and SK
  - GSIs have their OWN RCU and WCU allocations
  - Attributes - ALL, KEYS_ONLY and INCLUDE
    - GIS's are an alternative view on the base table with alternative PK and SK. 
    - They have their own RCU and WCU and can be created ay ANY TIME
    - GSI's are sparce; Only Items which have values in the new PK and optional SK are added
    - GSI's are ALWAYS eventually consistent, replication between base and GSI is Asynchronous
#### LSI and GSI Considerations
- Be careful with Projections (KEYS_ONLY, INCLUDE, ALL)
  - You will use all the Capacity Units as you project
  - Queries on attributes NOT projected are expensive
- Use GSIs as default. LSI only use when STRONG CONSISTENCY is required
- Use indexes for alternative access patterns (teams that need to access data but in a different perspective)

>Video 5
#### DynammoDB Streams and Triggers
- Stream Concepts
  - Stream: A Time ordered list of ITEM CHANGES in a table
  - 24 Hour rolling window
  - Enabled on a per table basis
  - Records Inserts, Updates and Deletes
  - Different view types influence what is in the stream
- 4 View types a stream can be configured with
  1) KEYS_ONLY
     - Records Keys Only (PK and SK)
  2) NEW_IMAGE
     - Records Entire Image AFTER the change
  3) ONLD_IMAGE
     - Records Entire Image BEFORE the change
  4) NEW_AND_OLD_IMAGES
     - Records Entire Images, both the NEW and OLD Item (pre and post) <- can include blank if a new item is entered
- Streams are the basis of triggers
  - Triggers: Item changes generate an event
    - Events contain the data which changed
    - An action is taken using this data
  - AWS can use Streams together with Lambda to take actions
    - ex. Reporting and Analytics
    - ex. Aggregation, Messaging or Notifications
- Workflow
  1) Item change occurs in a table with streams enabled
  2) Stream Record is added into stream
  3) Lambda function is invoked when stream event occurs. Function is passed the VIEW data as an event.

>Video 6
#### DynamoDB Global Tables
- Global Tables:
  - Provide Multi-master cross-region replication
    - Tables are created in multiple regions and added to the same global table (becoming replica tables)
  - Global tables use Last Writer Wins for conflict resolution
  - Multi-master - Read and writes can occur in any regions
    - Generally sub-second replication between regions
    - Need to select regions where the global tables will be located
  - Strongly consistent reads ONLY in the same region as writes
  - Global eventual consistency
  - Provides Global HA and Global DR/BC
  - Application needs to be able to tolerate drawbacks

>Video 7
#### DynamoDB Accelerator (DAX)
- Traditional Caches vs DAX
  - Traditional Cache:
    1) Application checks cache for data - A cache miss occurs when cache does not contain data
    2) Data is loaded from database with a separate operation and SDK
    3) Cache is updated with retrieved data. Subsequenmt queries will load data from cache as a cache hit
  - DAX <- Dax stores cache in the cluster
    1) Application uses DAX SDK and makes a single call for data which is returned by DAX
    2) DAX either returns the data from it's cache or retrieves it from the Database and then acches it
  - Using DAX removes complexity for the app developer and allows for tighter integration
- How DAX Works
  - DAX runs in an AZ in a VPC
  - DAX replicates in Read Replicas in other AZ for HA
  - The APP uses the DAX SDK to connect to the DAX cluster accessible via an endpoint. 
    - Abstracts the cache and Database (DDB) away from the APP
#### DAX Considerations
- Primary NODE (writes) and Replicas (read)
- Nodes are Highly Available - Primary failure will result in an election
- In-memory cache - Scaling (Much faster reads, reduced costs)
- Scales UP and Scales OUT (Bigger and more units)
- Supports write-through
- DAX is deployed WITHIN your VPC as a CLUSTER- NOT A PUBLIC SERVER
- Your application MUST tolerate eventually consistent reads
- Caching with DDB - Assume DAX

>Video 8
#### DynamoDB TTL (Time to Live)
- Lets you define a timestamp for automatic delete of items in a table
- When TTL is enabled on a table, a specific attribute is selected for TTL (uses UCT time)
- A Per-partition process periodically runs, checking the current time (in seconds since epoch) to the value in the TTL attribute
  - Items where the TTL attribute < current time, items are set to expired
- Another per-partition background process scans for expired items and removes them from the table and indexes and delete is added to streams if enabled

>Video 9
#### Amazon Athena
- Athena is:
  - A serverless interactive querying service
  - Ad-hoc queries on data
    - You pay for data consumed
  - Uses Schema-on-read - Table-like translations
    - You can query non-database data
  - Schema translates data into a relational-like when reading
  - Original data NEVER changed - remains on S3
  - Output can be sent to other services
  - Tables are defined in advanced in a data catalog and data is projected through when read
    - This allows SQL-like queries on data without transforming the source data
#### When to use Athena
- Queries where Loading/Transformation is NOT desired (in advanced)
- Occasional/Ad-hoc queries on data in S3
- Serverless querying scenarios - Cost conscious
- Querying AWS logs - VPC Flow Logs, CloudTrail, ELB Logs, Cost Reports, etc.
- AWS Glue Data Catalog and Web Server Logs
  - With Athena Federated Query (other Data Sources)

>Video 10
#### ElastiCache <- Redis
- In-memory databases (high performance) 
- Options:
  - Managed Redis (as a service)
  - Managed Memcached (as a service)
- Can be used to cache data - for Read Heavy workload with low latency requirements
- Reduced database workloads (expensive)
- Can be used to store session data (stateless servers)
- Requires Application Changes

#### Redis vs Memcached
- Redis
  - Supports Advanced Data Structures
  - Multi-AZ
  - Replication (Scale Reads)
  - Supports Backup and Restore
  - Transactions
    - Better for consistency
- Memcached
  - Simple data structures
  - No replication
  - Multiple Nodes (Sharding)
  - Does NOT support Backups
  - Multi-threaded by design
    - Better for performance

>Video 11
#### Amazon Redshift
- Redshift Architecture
  - Petabyte-scale Data Warehouse
  - OLAP (Column based) and NOT OLTP (Row/transaction)
  - Pay as you use (similar structure to RDS)
  - 2 special features
    1) Direct Query S3 using Redshift Spectrum
       - Query S3 without loading into DB
    2) Direct Query other DBs using Federated Query
       - Query other DB without loading into DB
  - Integrates with AWS tooling such as Quciksight
  - SQL-like interface JDBC/ODBC connections
- Server based (NOT Serverless)
- Runs in One AZ in a VPC
  - Uses a Cluster Architecture in the one AZ
- Leader Node - Query input, planning and aggregation
- Compute Node - Performing queries of data
  - Nodes have slices that distribute compute and work together with the leader node
- Has VPC Security, IAM permissions, KMS at rest Encryption, CloudWatch monitoring (like any VPC service)
- Redshift Enhanced VPC Routing - VPC Networking (can use any VPC networking and services) - Need to enable
- Can configure snapshots into S3
- Can send snapshots to another AWS region
- Can integrate with Firehose streams, DMS to migrate data into Redshift, Copy from DunamoDB, Load or unload from S3 or read from applications using JDBC/ODBC standard connections

>Video 12
#### Redshift Resilience and Recovery
- Note: Redshift runs in one VPC in 1 Az, but there are ways to increase resilience 
- Can utilize S3 for backups
  - Automatic backups (every 8 hours) or 5GB of data and by default have a 1 day retention (up to 35 days)
  - Manual snaps taken at any time (require manual delete)
- Can move data backups to other AZs
- You can configure snapshots to be copied to another region for data recovery with a separate configuration retention period

### Machine Learning
#### Amazon Comprehend - NLP tool
- Natural Language Processing
- Input: Document (text)
- Output: Entities, Phrases, Language, PII, Sentiments
- Pre-trained models or custom
- Real-time analysis for small workloads
- Async jobs for large workloads
- Console and CLI - Interactive or use APIs to build into apps
#### Amazon Kendra - Intelligent search service (delivering data to a user)
- Designed to mimic interacting with a human
- Supports various question types
  - Factoid (who, what, where)
  - Descriptive
  - Keyword - Kendra helps determine intent
- Kendra connects with your back end to deliver data
- Index - Searchable data organized in an efficient way
- Data Source - Where your data lives, Kendra connects and indexes from the location
  - Can be S3, Confulence, Google Workspace, etc.
- Synchronizes with index based on schedule
- Documents - Structured (FAQs), unstructured (HTML, PDFs, etc)
- Integrates with AWS services (IAM, Identity Center, etc.)
#### Amazon Lex - Interactive Chat Bot Creation (think Alexa, powers Alexa)
- Text or Voice conversational interfaces
- Powers Alexa
- Automatic Speech Recognition (ASR) - Speech to text
- Natural Language Understanding (NLU) - Intent
- Build understanding into your application
- Scales, Integrates, Quick to deploy, Pay as you go pricing
- Bots, Voice assistants, Q&A, etc.
  - Interact with Bots in 1+ languages
- Lex is something you are going to develop into an application
#### Amazon Polly - Converts text to "life-like" speech
- Takes text in a single language and outputs speech in the same languge 
  - NO TRANSLATIONS
- Allows for control of HOW speech should be generated with Speech Synthesis Markup Language
  - Stuff like emphasis, pronounciation, etc.
#### Amazon Rekognition - Deep learning image and video analysis
- Identify objects, people, text, activities, content moderation, face detection, face analysis, face comparison, pathing and more
- Per image or per minute pricing
- Integrates with application and event-driven
- Can analyze live video streams 
  - with Kinesis Video Streams
#### Amazon Textract - Detect and analyze tect contained in an input document
- Input - JPEG, PNG, PDF or TIFF
- Output - Extracted text, structure and analysis
- Most documents - Synchronous 
- Large Documents - Asynchronous
- Pay per use
- Use Cases
  - Detection of text and relationship between text
  - Generates metadata
  - Document analysis or Receipt analysis or Identity documents
#### Amazon Transcribe - Automnatic Speech Recognition (ASR) and output texts
- Inout - Audio
- Output - Text
- Language customization, Filters for privacy, Audience appropriate language, Speaker identification
- Custom vocabularies and language models
- Pay per second
- Use Cases:
  - Full text indxing of audio - allow searching
  - Meeting notes
  - Subtitles/captions and transcripts
  - Call analytics 
  - Integration with other apps/AWS ML services
#### Amazon Translate - Translates text (ML based)
- Translates text from native language to another language
- Encoder reads source and semantic representation (meaning) is gathered, and the decoder reads the meaning and writes to the target language
- Use Cases:
  - Multilingual user experience
  - Meeting notes, posts, communications, articles
  - Emails, in game chat, customer support
  - Translate incoming data
  - Language independence for other AWS services
    - Comprehend, transcribe, polly, data stored in S3, RDS, DDB
  - Commonly integrates with other services/Apps/Platforms
#### Amazon Forecast 101 - Forecasting time-series data
- Examples: Retail demand, supply chain, staffing, traffic, etc.
- Import historical and related data
  - Creates relationships between the data
- Output - Forecast and forecast explain-ability
- Web console (visualization), CLI, APIs, Python SDK
#### Amazon Fraud Detector - Fully managed Fraud Detection service
- Imports data and detects fraudulent behavior on activity
- Upload historic data, choose model type
  - Online fraud - little historic data (ex. new account)
  - Transaction fraud - Transaction history, identifying suspect payments
  - Account takeover - Identifying phishing or another social based attack
  - Things are scored and Rule/Decision logic allows you to react to a score based on business activity
#### Amazon Sage Maker - AI and ML model tool
- Fully managed machine learning (ML) service
- Fetch, clean, prepare, train, evaluate, deploy, monitor/collect
- Sage Maker Studio - Build, train, debug and monitor models - IDE for ML lifecycle
- Sagemaker Domain - EFS Volume, Users, Apps, Policies, VPCs, ... isolation
- Containers - Docker containers deployed to ML EC2 instances - ML environments (OS, Libraries, Tooling)
- Hosting - Deploy endpoints for your models
- Sagemaker has no cost, but the resources to create models do (complex pricing)

### AWS Local Zones
- Infrastructure is located in AZs in a region. This region has instances running in infrastructure that is not necessarily in a close geographical area
- Local zones can be found in the name, for example, us-west-2a vs us-west-2-las-1 with us-west-2-lax-1a or us-west-2-lax-1b
- Local zones are created by expanding the VPC to include local zones 
- Local zones support DX
- Connections to local zone resources offer really low latency 
- Some thing in local zones behave like parent region resources (ex EBS Snapshots will appear in the parent region AZ)

#### Key Points
- 1 Zone - No built in resilience
  - Think of them like an AZ, but near your location
- They are physically close to you, latency is lower
- Not all products support local zones
  - Many operate with limitations
- DX to a local zone IS supported (extreme performance)
- Utilizes parent regions for some services (ex. EBS Snapshots will go TO parent region)
- Use when you need the HIGHEST performance

## Notes from Tutorial Dojo
#### AWS Proton
- Allows you to deploy any serverless or container-based application with increased efficiency, consistency and control. You can define inferastructure standards and effective confinuous delivery pipelines for your organization
  - Proton breaks fomn the infrastructure into environment and service (infrastructure as code templates)
  - Basically, make components and attach to your services as needed
- From TD:
  - In AWS Proton administrators define standard infrastructure that is used across development teams and applications. However, development teams might need to include additional resources for their specific use cases, like Amazon Simple Queue Service (Amazon SQS) queues or Amazon DynamoDB tables. These application-specific resources might change frequently, particularly during early application development. Maintaining these frequent changes in administrator-authored templates might be hard to manage and scale—administrators would need to maintain many more templates without real administrator added value. The alternative—letting application developers author templates for their applications—isn’t ideal either, because it takes away administrators’ ability to standardize the main architecture components, like AWS Fargate tasks. This is where components come in.
  - With a component, a developer can add supplemental resources to their application, above and beyond what administrators defined in environment and service templates. The developer then attaches the component to a service instance. AWS Proton provisions infrastructure resources defined by the component just like it provisions resources for environments and service instances.

#### AWS SNS
- You can filter SNS at the SUB level (in SQS) by enabling a filter policy and assigning topics to the JSON files in the messages
  - Example - Assigning car insurance as "type:car" and then on the sub, having a filter for car
- You can create 1 SNS topic and use the filters instead of using multiple SNS and SQS with the default option to listen to all    

#### Simple Workflow Service (SWF) 
- Service that makes it easy to coordinate work across distributed application components using visual workflows
- You define state machines that describe your workflow as a series of steps, their relationships and their inputs and their input and outputs

#### Networking
- AAAA records - IPv6
- A - IPv4 Records
- ALIAS - Points a domain to a resource, AWS SPECIFIC <- similar to CNAME but for AWS 
- CNAME - Points a DNS Query to a different DNS Record

#### VPC Tips
- For launching specific VPC subnets, you MUST specify a range of IPv4 addresses for the VPC in the form of a CIDR block
  - Each subnet MUST reside entirely within the AZ
  - The IPv6 (and CIDR block) is an option
  - ALL Subnets NEED A IPv4 CIDR RANGE!

#### AWS Big Data
- Amazon EMR - a managed cluster platform that simplifies running big data framework (such as Hadoop) to process data

#### Security Groups vs NACL
- Security Groups - Protect EC2 instances (firewall)
  - Support allow rules only
- NACL - Subnet protection (firewall)
  - Supports allow and deny rules

#### Elastic IP vs Static IP
- Static IPs
  - IP Addresses that do not change
  - Are public IP addresses
- Elastic IPs
  - Similar to Static IPs where
    - They are not changing
    - Are publically routable
  - EXCEPT
    - They are AWS IP addresses in a pool
    - Associated with the region and CANNOT leave the region
    - Can be assigned to EC2 instances (and Network Load Balancers)

#### Fargate
- By default, Fargate tasks are given a minimum of 20 Gb of free ephemeral storage
  - Task - What Fargate is running (The EC2 in Fargate) - Use definitions to configure tasks

#### Gateway Endpoints vs Interface Endpoints
- Gateway Endpoint
  - Connects your private VPC to public AWS services
  - Does NOT work for on premises
  - Free
- Interface Endpoints (Private Link)
  - Connects your private VPC OR on premises network to public AWS services
  - Costs Money

#### Customer Gateways 
- Used to connect Site-to-Site VPN in your on-premise network

#### RDS
- RDS Storage Auto Scaling - Automatically scales storage capacity in response to growing database workloads with zero downtime

#### Serverless Databases
- You cannot change instances classes from Provisioned to Serverless

#### SSH
- Layer 7 Secure Access Protocol
- Uses Port 22

#### Transit Gateway
- Allows you to consolidate and control an organizations entire AWS routing configuration in 1 place
  - No need to VPN Peer-to-Peer and worry about multiple customer gateways

#### AWS Backup
- Service that allows you to back-up your application data across AWS services in the AWS cloud.
- Centralized backup service to protect AWS Storage Volumes, Databases and File Systems
- Lets you define a Point In Time Recovery
- Note that Automatic backups provided in EDS is only for 35 days

#### S3 Glacier
- Expedited Retrievals - Allows you to auickly access data when there is an occasional urgent request
- Provisioned Capacity - Ensures you have retrieval capacity for expedited retrievals when you need it

#### EC2
- EBS volumes are automatically deleted when an instance terminates UNLESS you set DeleteOnTermination to false
- You are limited to running On-Demand instances per your vCPU-based On-Demand instance limit
  - Limit per region
- Limit of 20 reserved instance purchases
  - Limit per region
- Hibernation mode
  - EC2 mode (along with stop, running etc.) where you only pay for the EBS volumes and Elastic IP address attached. No hourly charge
  - It is not possible to enable or disable hibernation for an instance after it has launched

#### S3
- Note: Encrypting on EBS and then coping data to S3 does NOT mean the data is encrypted when it is in S3! You need to encrypt the S3 Bucket you store the data in
  - S3 does NOT use EBS to store the data, hence the encryption issue

#### EBS
- If you encrypt your EBS, it just means the data is encrypted while in EBS. If it moves out of EBS (say into S3), you NEED to encrypt S3, or client side encrypt all data

### Elastic Fabric Adapters
- A network device that you can attach to your EC2 instance to accelerate high performance compute and machine learning applications

#### AWS MQ
- Message service for AWS - Public Event and Public Message application (Mass email updates or marketing)

### Kinesis Data Analysis
- The full fancy version of Kinesis that does data transformation between multiple sources and shit
- Not to be confused with Kinesis Data Streams - Just regular Kinesis

#### AppSync 
- Manages data access from one or more sources or microservices with a single network request
- Unified API

#### Amazon EKS Anywhere
- K8s service in Amazon that allows customers to create and operate K8s clusters on customer managed infrastructure

#### Secrets Manager vs Parameter Store
- Secrets manager costs money, Parameter store is free
- If you are storing mostly parameters, use parameter store
- Secret manager can rotate secrets automatically

#### Network ACL and Rules
- When you create an outbound rule for a request, you MUST include the ephemeral ports that you will be requesting to. These are (32768 - 65535)
  - ex. Response to anywhere, outbound rule is TCP connection on port 32768 - 65535 to destination 0.0.0.0/0 (ephemeral ports to anywhere)

#### R53
- Active-Active Failover
  - Use this failover configuration when you want all your resources to be available the majority of the time. Will detect unhealthy endpoints and not include them for responses
  - All records have the same name, same type (A or AAA) and same routing policy (such as weighted or latency)
- Active-Passive Failover
  - Use this failover when you want a primary resource or group of resources to be available the majority of the time and want a secondary resource or group to be on standby in case of failure

#### CloudFront
- SNI Custom SSL relies on the SNI extension of the Transport Layer Security protocol, allowing multiple domains to serve SSL traffic over the same IP address using the hostname which the view is trying to access
  - Requires binding certificates to the Application Load Balancer, and the ALB will choose the optimal TSL certificate using Server Name Indication (SNI)
- ex. The catagram vs dogogram example in Cantril

