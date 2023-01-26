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