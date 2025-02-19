# AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY
Starting Off Your AWS Cloud Project
There are few requirements that must be met before you begin:

Properly configure your AWS account and Organization Unit Watch How To Do This Here https://youtu.be/9PQYCc_20-Q 
Create an AWS Master account. (Also known as Root Account)
Within the Root account, create a sub-account and name it DevOps. (You will need another email address to complete this) 1.1. In the AWS Management Console, navigate to AWS Organizations.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20an%20aws%20organization%20account.PNG)

Create a Sub-Account Named DevOps:
Navigate to Add an AWS Account > Fill in the name (DevOps).

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20an%20aws%20account.PNG)

Create an AWS Organization Unit (OU) Named Dev:
From the AWS Organization page, click on 'Root' > Action > Create new.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20dev%20ou%20account%20in%20the%20root.PNG)

Move the DevOps Account into the Dev OU:
Select the account to move, then click Action > Move.
Select the OU to move the account to and click Move AWS account.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/move%20devops%20into%20dev.PNG)

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/move%20devops%20into%20dev2.PNG)

Login to the Newly Created AWS Account:
Use the new email address to access the DevOps account.
You are going to be asked to input a password for the account.
Click on "Forget Password" and then click on "Continue".
Follow the instructions to Create/Reset the password for this new account. Voila! You are now logged in.
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/logging%20in%20to%20the%20devops%20aws%20account.PNG)

Domain Registration and Hosted Zone Setup
Register a Free Domain:
Use cloudns https://https//www.cloudns.net/  or Freenom https://www.freenom.com/ to register a free domain for your fictitious company - though freenom no longer offers free domains.
I used cloudns, so you can register a free domain name by:
Click on Create Zone, -> _Free Zone_
Type in the name of your choice
Click on "Create"
And Voila, you've gotten a domain name registered.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20free%20dns%20name.PNG)

Create a Hosted Zone in AWS "DevOps" account and Map It to Your Domain:
Navigate to Route 53 and select get started then select Create Hosted Zone.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/createing%20hosted%20zone%20in%20devops%20account.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20hosted%20zone%202.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20hosted%20zone%203.PNG)

Map the Hosted Zone to Your Domain:
Copy the Name Server (NS) values from AWS.
Update the default NS values in your domain registrar with the values from AWS.
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/updating%20the%20NS%20aws%20server%20on%20my%20cloudns.PNG)

The next step is to get a certificate from AWS Certificate Manager. The reason we are creating a certificate first is because when creating ALB we need to select a certificate.

Click on request a Cert > Request public cert > Next
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/request%20a%20certificate%20on%20aws.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/request%20a%20certificate%20on%20aws2.PNG)

In the domain name, we are going to use a wild card i.e(*.). should in case we want to have another name or subdomain, the WILDCARD will make sure that any name before the domain name is attached to the certificate. e.g  x.taiwo.ip-ddns.com
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/request%20a%20certificate%20on%20aws3.PNG)

NOTE: Because we are using DNS verification is going to automatically write to the Rout53 to confirm

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/request%20a%20certificate%20on%20aws4.PNG)
Click on "Create records in Route 53" -> Create records
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20records%20for%20certificate.PNG)
copy the CNAME name and the CNAME value generated by Acm, then Go to Cloudns, Click on Dns records -> CNAME -> Add new record -> type: CNAME. Paste the CNAME name and value to validate the record for AWS to issue the certificate. (For now, the Certificate status will remain pending unitl the record is validated by the Domain provider)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/adding%20our%20aws%20cname%20value%20and%20name%20on%20cloudns.PNG)

NOTE : As you proceed with configuration, ensure that all resources are appropriately tagged, for example:

Project: Give your project a name Project-15

Environment: dev

Automated:No (If you create a recource using an automation tool, it would be Yes)
SET UP A VIRTUAL PRIVATE NETWORK (VPC)
Set Up a Virtual Private Network (VPC) Always make reference to the architectural diagram and ensure that your configuration is aligned with it.

Create a [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20vpc.PNG)

Enable DNS hosting by clicking on action > edit vpc setting
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/enable%20dns%20setting.PNG)

Create subnets as shown in the architecture On the left panel menu of the VPC UI, click on Subnet > Create Subnet.
VPC: 10.0.0.0/16

Public Subnet 1: 10.0.1.0/24 in Zone A
Public Subnet 2: 10.0.2.0/24 in Zone B
Private Subnet 1: 10.0.3.0/24 in Zone A
Private Subnet 2: 10.0.4.0/24 in Zone B
Private Subnet 3: 10.0.5.0/24 in Zone A
Private Subnet 4: 10.0.6.0/24 in Zone B
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20subnet1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20subnet2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20subnet3.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20subnet4.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20subnet5.PNG)
![(screenshot all vpc)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/all%20subnet%20for%20vpc1%20created.PNG)

Do not forget to edit all public subnet to enable auto-assign public ipv4 address (as shown in the screenshot below):
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/enable%20auto%20assign%20ipv4%20on%20all%20subnet.PNG)

Route Tables Configuration
Navigate to VPC -> Route Tables -> Create route table, then create the table as follows:

Create and Associate Route Tables:
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/navigate%20to%20route%20tables.PNG)

Enter the following details:

Name tag: Provide a name for your route table Public Route Table
VPC: Select the VPC you created earlier.
Click Create route table.
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20route%20table%20for%20public%20subnets.PNG)

Associate Route Table with Public Subnets

Select the Public Route Table from the list.
Click on the Subnet associations tab.
Click Edit subnet associations.
In the Select subnets section, check the boxes for Public Subnet 1 and Public Subnet 2.
Click Save associations.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/associating%20our%20public%20subnet%20to%20route%20table.PNG)

Create a Private route table and associate it with private subnets
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20private%20route%20table.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/associating%20our%20private%20subnets%20to%20route%20table.PNG)

Route Tables
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/our%20route%20table.PNG)

Create an [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
Click on Internet Gateways on the left-hand side > Create internet gateway > Enter a name for your internet gateway > Create internet gateway
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20internet%20gateway.PNG)

Attach Internet Gateway to VPC: Select the internet gateway you just created > Click the Actions dropdown, then select Attach to VPC >Select the VPC you created earlier >Click Attach internet gateway.
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/attach%20vpc1%20to%20internet%20gateway.PNG)

Link Public Route Table to the Internet Gateway

Go to VPC -> Route Tables -> Public-RT -> Actions -> Edit Routes -> Add route

Since we want to route traffic to the internet from the public subnet (public route table), the record we are going to add will have its Destination field value to be 0.0.0.0/0, and its target field would be the id of the internet gateway we just created
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/editing%20route%20to%20be%20accessible%20over%20the%20internet%20and%20using%20the%20internet%20gateway%20we%20created.PNG)

Create 3 [Elastic IPs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) one for Nat getway and the other 2 will be used by Bastion hosts
Create Elastic IP to configured with the NAT gateway. The NAT gateway enables connection from the public subnet to private subnet and it needs a static ip to make this happen. VPC > Elastic IP addresses > Allocate Elastic IP address - add a name tag and click on allocate

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creat%203%20elastoic%20ip%20adress.PNG)

Create a Nat Gateway and assign one of the Elastic IPs the other 2 will be used by [Bastion hosts](https://aws.amazon.com/solutions/implementations/linux-bastion/)
Create a Nat Gateway and assign the Elastic IPs Click on VPC > NAT gateways > Create NAT gateway

Select a Public Subnet Connection Type: Public Allocate Elastic IP
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20natgateway%20and%20attched%20the%20ip%20adress.PNG)

We Will also associate our private route table with this NAT

We can do this by: Navigating to VPC -> Route Tables -> Private-RT -> Actions -> Edite Routes -> (see screenshot for configurations)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/edit%20route%20of%20private%20oute%20to%20the%20nat%20gateway.PNG)

### Create a Security Group for:
Nginx Servers: Access to Nginx should only be allowed from a Application Load balancer (ALB). At this point, we have not created a load balancer, therefore we will update the rules later. For now, just create it and put some dummy records as a place holder.

Bastion Servers: Access to the Bastion servers should be allowed only from workstations that need to SSH into the bastion servers. Hence, you can use your workstation public IP address. To get this information, simply go to your terminal and type curl [www.canhazip.com](http://www.canhazip.com/)

Application Load Balancer: ALB will be available from the Internet give it both HTTPS and HTTP from anywhere 0.0.0.0/0

Webservers : Access to Webservers should only be allowed from the Nginx servers. Since we do not have the servers created yet, just put some dummy records as a place holder, we will update it later.

Data Layer : Access to the Data layer, which is comprised of Amazon Relational Database Service (RDS) and Amazon Elastic File System (EFS) must be carefully desinged – only webservers should be able to connect to RDS, while Nginx and Webservers will have access to EFS Mountpoint.
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20application%20load%20balancer%20port.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Bastion%20Servers%20security%20group.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Nginx%20Servers%20security%20group.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Security%20group%20for%20Internal%20ALB.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Security%20group%20for%20Webservers.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Security%20group%20for%20Data%20Layer.PNG)

Security Best Practices:

Least Privilege: Only open necessary ports and restrict access using specific IP ranges.
Regular Audits: Periodically review and update security group rules to adhere to best practices.
Network Segmentation: Isolate different layers of the application to minimize the impact of potential breaches.
⚠️ Common Issues:

Port Conflicts: Ensure no overlapping or conflicting port rules exist.
Unrestricted Access: Avoid using 0.0.0.0/0 except for public-facing services.
Incorrect Associations: Verify that security groups are correctly associated with their respective resources.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/all%20our%20security%20group%20created.PNG)


### Amazon Elastic File System (EFS) Setup
Overview
Amazon EFS provides scalable and fully managed Network File System (NFS) storage, ideal for shared file storage across multiple EC2 instances.

Step-by-Step Setup
Create an EFS Filesystem:

Create an EFS filesystem: Navigate to; Amazon EFS -> Create file system:
Give the Elastic File System (EFS) a name, _ Choose the VPC (Virtual Private Cloud) to deploy the EFS in,
Click on "Create".
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20efs%20on%20aws.PNG)

Create an EFS mount target per AZ in the VPC, associate it with both subnets dedicated for data layer.
NB: The amazon EFS becomes available in any subnet we specify in our mount target, and as such, we will specify that this efs be created in the private webserver subnets so that all the resources in that subnet will have the ability to mount to the file system.

click on Network > manage 
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/adding%20mount%20to%20efs%20file%20system.PNG)
Associate the Security groups created earlier for data layer. (See the above picture for how this has been accomplished)

Create Efs Access points:

This is necessary to specify where the webservers would be situated/mounted on, therefore, we are going to be creating 2 mount points. One for Wordpress, the other for Tooling.

Access Point for Wordpress server:
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/wordpress%20access%20entry1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/wordpress%20access%20entry2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/wordpress%20access%20entry3.PNG)

Access Point for Tooling Webserver:
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/tooling%20accessentry.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/tooling%20accessentry1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/tooling%20accessentry2.PNG)

Overview of Access Entry
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/overview%20of%20access%20entry.PNG)

### Amazon Relational Database Service (RDS) Configuration
Overview
Amazon RDS offers a managed relational database service, simplifying database setup, operation, and scaling.

Step-by-Step Setup
Create a KMS Key for Encryption:
Navigate to: KMS -> Customer managed keys -> Create key and follow the configurations in the screenshot:
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/configuring%20KMS.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/configuring%20KMS1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/configuring%20KMS2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/configuring%20KMS3.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/configuring%20KMS4.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/configuring%20KMS5.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/configuring%20KMS6.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/configuring%20KMS%20done.PNG)

Create an RDS Subnet Group:
To begin, we will start by preparing the subnets needed for setting up rds. Navigate to: RDS -> Subnet groups -> Create DB subnet group, then replicate the following configurations:
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20db%20subnet%20for%20RDS.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20db%20subnet%20for%20RDS1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/creating%20db%20subnet%20for%20RDS2.PNG)

Launch an RDS Instance:

Navigate to: RDS -> Create database, and follow the configurations in the screenshots below:

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL.PNG)

To fulfil our architectural structure/diagram, we will need to select either Dev/Test or Production Sample Template. Also, to minimize AWS cost, we can select the Do not create a standby instance option under Availability & durability sample template (The production template will enable Multi-AZ deployment)

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL1.PNG)

Now, Configure other settings accordingly {Leave instance configuration section as is} (For test purposes, most of the default settings are cool to be left as is). In the real world, we would need to size the database appropriately. we would need to get some information about the usage. If it is a highly transactional database that grows at 10GB weekly, we must bear that in mind while configuring the initial storage allocation, storage autoscaling, and maximum storage threshold. {user: admin, pass: G*****&}
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL2.PNG)

Configure VPC and security (ensure the database is not available from the Internet)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL3.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL4.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL5.PNG)

Configure backups and retention
Encrypt the database using the KMS key created earlier
Enable CloudWatch monitoring and export Error and Slow Query logs (for production, and also Audit).

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL6.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL7.PNG)

RDS Created

![(Sreenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20RDS%20MYSQL8.PNG)

### Compute Resources Setup
Base Server Configuration
NB: To create the Autoscaling Groups, we need Launch Templates and Load Balancers. The Launch Templates requires AMI and Userdata while the Load balancer requires Target Group

Provision EC2 Instances for NGINX, Bastion, and Webservers:

Navigate to: EC2 -> Launch an instance, and create the ec2 instances with the specifications above (see screenshots for more context):
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20ec2%20instance%20with%20centos.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20ec2%20instance%20with%20centos1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20ec2%20instance%20with%20centos2.PNG)

💡 Information: Repeat the above steps to create instances for webservers and nginx servers
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/launch%20ec2%20instance%20with%20centos3.PNG)

Software Installation and Configuration
Install Essential Software on All Servers: Make sure you install the essential softwares these in each of the above servers by running the following command;

sudo yum update -y
 sudo yum install -y   wget   vim   python3   telnet   htop   epel-release   dnf-utils
sudo yum install -y \
  openssl-devel \
  cargo \
  mysql \
  git
sudo systemctl start chronyd
sudo systemctl enable chronyd
💡 Tip: You can automate software installation using User Data scripts or configuration management tools like Ansible.

I am going to do this manually in this illustration, and I will begin with the Bastion Server:

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Install%20Essential%20Software%20on%20All%20Servers.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Install%20Essential%20Software%20on%20All%20Servers1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Install%20Essential%20Software%20on%20All%20Servers2.PNG)

Note: You have to also repeat the above steps for the Nginx and Webserver instances
Note: since we configured a Bastion Host with open SSH access, restricted SSH on NIGINX and WEBSERVER EC2 instances to only allow connections from the Bastion, I transferred the PEM key to the Bastion (or you can also use SSH agent forwarding for best security pratice), and successfully accessed the Nginx and webserver ec2 instance from inside our Bastion servers via SSH

NGINX Specific Configuration
Set SELinux Policies for NGINX:

sudo setsebool -P httpd_can_network_connect=1
sudo setsebool -P httpd_can_network_connect_db=1
sudo setsebool -P httpd_execmem=1
sudo setsebool -P httpd_use_nfs=1

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/SELinux%20Policies%20for%20NGINX.PNG)
💡 Best Practice: Regularly audit SELinux policies to maintain security without hindering functionality.

Amazon EFS Utilities Installation
Install EFS Utilities for Mounting EFS:

git clone https://github.com/aws/efs-utils
cd efs-utils
sudo yum install -y make rpm-build
sudo yum install -y linux-headers-$(uname -r)
sudo make rpm
sudo yum install -y ./build/amazon-efs-utils*rpm
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/EFS%20Utilities%20for%20Mounting%20EFS.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/EFS%20Utilities%20for%20Mounting%20EFS1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/EFS%20Utilities%20for%20Mounting%20EFS2.PNG)

Note: Repeat the above steps for the Webservers Instance as well

💡 Tip: Ensure EFS utilities are up-to-date to support the latest features and security patches.

### SSL Certificate Setup
We are using an ACM (Amazon Certificate Manager) certificate for both our external and internal load balancers. But for some reasons, we might want to add a self-signed certificate:

Compliance Requirements:

Certain industry regulations and standards (e.g., PCI DSS, HIPAA) require end-to-end encryption, including between internal load balancers and backend servers (within a private network). Defense in Depth:

Adding another layer of security by encrypting traffic between internal load balancers and web servers can provide additional protection. When generating the certificate, In the common name, enter the private IPv4 dns of the instance (for Webserver and Nginx). We use the certificate by specifying the path to the file kos.crt and kos.key in the nginx configuration.

Generate Self-Signed Certificates for NGINX and Apache:

For NGINX:

sudo mkdir /etc/ssl/private
sudo chmod 700 /etc/ssl/private
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/kos.key \
  -out /etc/ssl/certs/kos.crt
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048

Common Issues:

Certificate Mismatch: Ensure certificate paths in configurations are correct.
SELinux Denials: Verify SELinux policies allow SSL operations.
Firewall Restrictions: Confirm that SSL ports (443) are open in security groups.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Generate%20Self-Signed%20Certificates%20for%20NGINX.PNG)

### Load Balancer Configuration
External Application Load Balancer (ALB)
Please: Before continuing with this, [create AMI's of the instances you've just created](https://github.com/Kosenuel/DevOps_CloudEngr-StegHub/tree/main/15.Cloud-Solution(AWS)-For-2-Websites-Owned-by-a-Company#create-amis-from-ec2-instances) and [Setup Target Groups](https://github.com/Kosenuel/DevOps_CloudEngr-StegHub/tree/main/15.Cloud-Solution(AWS)-For-2-Websites-Owned-by-a-Company#configure-target-groups). This would facilitate the creation of our Application Load Balancer (ALB).

Configure External ALB to Route Traffic to NGINX:

Now, notice that we have configured our Nginx EC2 Instances to have configurations that accepts incoming traffic only from the Load Balancers, No request should go directly to Nginx servers. With this kind of setup, we will benefit from intelligent routing of requests from the ALB to Nginx servers across the 2 Availability Zones. We will also be able to [offload](https://avinetworks.com/glossary/ssl-offload/) SSL/TLS certificates on the ALB instead of Nginx. Therefore, Nginx will be able to perform faster since it will not require extra compute resources to validate certificates for every request. So in a nutshell/recap, this is what we are going to do:

Create an Internet facing ALB
Ensure that it listens on HTTPS protocol (TCP port 443)
Ensure the ALB is created within the appropriate VPC | AZ | Subnets
Choose the Certificate from ACM
Select Security Group
Select or create target group Nginx Instances as the target group

Navigate to EC2 -> Load balancers -> Create load balancer -> Application load balancer and apply the configurations like so:
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Create%20an%20Internet%20facing%20ALB.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Create%20an%20Internet%20facing%20ALB1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Create%20an%20Internet%20facing%20ALB2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Create%20an%20Internet%20facing%20ALB3.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Create%20an%20Internet%20facing%20ALB4.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Create%20an%20Internet%20facing%20ALB5.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Create%20an%20Internet%20facing%20ALB6.PNG)


### Internal Application Load Balancer (ALB) for Webservers
Since the webservers are configured for auto-scaling, there is going to be a problem if servers get dynamically scalled out or in. Nginx will not know about the new IP addresses, or the ones that get removed. Hence, Nginx will not know where to direct the traffic.

To solve this problem, we must use a load balancer. But this time, it will be an internal load balancer. Not Internet facing since the webservers are within a private subnet, and we do not want direct access to them.

Create an Internal ALB
Ensure that it listens on HTTPS protocol (TCP port 443)
Ensure the ALB is created within the appropriate VPC | AZ | Subnets
Choose the Certificate from ACM
Select Security Group
Select or create target group webserver Instances as the target group
Ensure that health check passes for the target group
Note: This process must be repeated for both WordPress and Tooling websites.

Configure Internal ALB to Route Traffic to Webservers:

Ensure you are Navigated to EC2 -> Load balancers -> Create load balancer -> Application load balancer, then apply the configurations like so:
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configure%20Internal%20ALB.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configure%20Internal%20ALB1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configure%20Internal%20ALB2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configure%20Internal%20ALB3.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configure%20Internal%20ALB4.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configure%20Internal%20ALB5.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configure%20Internal%20ALB6.PNG)

💡 Tip: Use host-based routing to direct traffic to the appropriate or created target groups based on the requested hostname.

The default target configured on the listener while creating the internal load balancer is configured to forward traffic to wordpress on port 443. Now, we need to create a rule to route traffic to tooling as well.

1. Select internal load balancer from the list of load balancers created: Navigate to EC2 -> Load balancers -> "your internal load balancer"

Choose the load balancer where you want to add the rule.
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20a%20rule%20to%20route%20traffic%20to%20tooling.PNG)

2. Listeners Tab:

Click on the Listeners tab.
Select the listener - HTTPS:443 - and click Manage rules -> Add rule -> Add Condition -> Host header type. After configuring, click "next"

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20a%20rule%20to%20route%20traffic%20to%20tooling1.PNG)

4. Configure the Rule:

Choose the appropriate or create target group for the hostname.
Select the priority for the rule.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20a%20rule%20to%20route%20traffic%20to%20tooling2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20a%20rule%20to%20route%20traffic%20to%20tooling3.PNG)

3. Add Rules:

Review and create rule.
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20a%20rule%20to%20route%20traffic%20to%20tooling4.PNG)

⚠️ Common Issues:

SSL Certificate Errors: Ensure ACM certificates are correctly associated with ALBs.
Routing Misconfigurations: Verify host header conditions match the intended subdomains.
Security Group Restrictions: Confirm ALBs can communicate with their target groups.
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/create%20a%20rule%20to%20route%20traffic%20to%20tooling5.PNG)

Configure Listener Rules for Internal ALB:
The default target is WordPress. Add rules to route tooling.kosenuel.ip-ddns.com to the Tooling target group.

### Compute Resources Setup
Provision EC2 Instances for NGINX, Bastion, and Webservers
```bash:path/to/configuration-script.sh
#!/bin/bash
# Example User Data Script
sudo yum update -y
sudo yum install -y nginx
# Additional installation commands
```
### user Data Configuration
Navigate to: EC2 -> Lauch Templates -> Create Launch Template, and follow the configurations you see in the screenshot below:

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Created%20Lauch%20Template%20for%20Nginx.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Created%20Lauch%20Template%20for%20Nginx1.PNG)
![(Screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Created%20Lauch%20Template%20for%20Nginx2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Created%20Lauch%20Template%20for%20Nginx3.PNG)

Repeat the above steps to create the Bastion-Template, the only thing you will have to do differently when configuring the Bastion-Template Launch Template is to set the AMI to Bastion AMI and configure the User data field differently as shown in the screenshot below:

![screen](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Created%20Lauch%20Template%20for%20Bastion.PNG)
WordPress User Data:

Before we jump into configuring the templates for our webserver instances (Wordpress and Tooling), we have to do some home-keeping. The webserver instances needs to communicate with the database and efs, and as such we need to configure the database and make it ready to attend to such requests it is about to get without spewing out errors.

Navigate to your terminal, connect to the bastion instance, and jump from there to your webserver instance. Now log into your RDS database like so:
mysql -h <the end point of your rds instance> -u admin (or the name of your configured user) -p 
Create some databases (wordpressdb and toolingdb) as shown in the screenshot:

![screen](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Creating%20Databases.PNG)
Now create a user and grant this user access to the databases

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Creating%20Databases1.PNG)

Now we are done with setting up the foundation, now Lets Create AMIs from EC2 Instances and configure our remaining Launch Template 

### Create AMIs from EC2 Instances
Create an AMI for Bastion Hosts:

Navigate to `EC2 -> instances -> right-click on an instance and select Image and templates -> Create image (see screenshot for context)

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Bastion%20AMI.PNG)

Create an AMI for NGINX Servers:

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/NGINX%20AMI.PNG)

Create an AMI for Webservers:

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/weberver%20AMI.PNG)

View All AMIs:

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/all%20AMIs.PNG)


now it's time for configuring the Launch Template for Wordpress. Navigate to EC2 -> Launch templates -> Create launch template. Follow the steps we observed in creating the preivious lauch template, only that the AMI to use here would be your Webserver-AMI and not the Nginx-AMI we used in the 1st launch template created. Also, the user data configuration script would greatly differ this time around to look like this:

```
#!/bin/bash

sudo mount -t efs -o tls,accesspoint=fsap-095eef60db83a501b fs-0dbd247de4bf9b340:/ /var/www/

yum install -y httpd
systemctl start httpd
systemctl enable httpd

yum module reset php -y
yum module enable php:remi-7.4 -y

yum install -y php php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd php-fpm php-json wget

systemctl start php-fpm
systemctl enable php-fpm

wget http://wordpress.org/latest.tar.gz

tar xzvf latest.tar.gz
rm -rf latest.tar.gz

cp wordpress/wp-config-sample.php wordpress/wp-config.php
mkdir /var/www/html/
cp -R /wordpress/* /var/www/html/
cd /var/www/html/
touch healthstatus

sed -i "s/localhost/database-1.cqduk2gi2dkw.us-east-1.rds.amazonaws.com/g" wp-config.php
sed -i "s/username_here/Taiwo/g" wp-config.php
sed -i "s/password_here/Taiwoboy123/g" wp-config.php
sed -i "s/database_name_here/wordpressdb/g" wp-config.php

chcon -t httpd_sys_rw_content_t /var/www/html/ -R

systemctl restart httpd
```
⚠️ Important: The command for attaching the mountpoint (sudo mount -t efs ...) for your specific efs can be gotten from going to Amazon EFS -> Access Points -> Your Access Point -> Attach. And also, your rds endpoint value can be gotten from going to RDS -> Databases -> Your database name -> Connectivity & Security -> Endpoint & Port -> Endpoint
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configuring%20Launch%20Template%20for%20Wordpress.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configuring%20Launc%20Template%20for%20Wordpress1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configuring%20Launc%20Template%20for%20Wordpress2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Configuring%20Launc%20Template%20for%20Wordpress3.PNG)


### Tooling User Data:
```
#!/bin/bash

mkdir /var/www/

sudo mount -t efs -o tls,accesspoint=fsap-095eef60db83a501b fs-0dbd247de4bf9b340:/ /var/www/

yum install -y httpd
systemctl start httpd
systemctl enable httpd

yum module reset php -y
yum module enable php:remi-7.4 -y

yum install -y php php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd php-fpm php-json

systemctl start php-fpm
systemctl enable php-fpm

git clone https://github.com/Prince-Tee/tooling.git

mkdir /var/www/html
cp -R /tooling/html/*  /var/www/html/
cd /tooling

mysql -h database-1.cqduk2gi2dkw.us-east-1.rds.amazonaws.com -u admin -p toolingdb < tooling-db.sql

cd /var/www/html/
touch healthstatus

sed -i "s/$db = mysqli_connect('10.0.1.138', 'admin', 'admin', 'tooling');/$db = mysqli_connect('database-1.cqduk2gi2dkw.us-east-1.rds.amazonaws.com', 'Taiwo', 'Taiwoboy123', 'toolingdb');/g" functions.php

chcon -t httpd_sys_rw_content_t /var/www/html/ -R

mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup

systemctl restart httpd
```
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Launch%20tooling%20template.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Launch%20tooling%20template1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Launch%20tooling%20template2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Launch%20tooling%20template3.PNG)

### Auto Scaling Groups Configuration
 Set Up Auto Scaling Groups (ASGs)
Bastion Hosts ASG:

Config info:

Bastion-ASG Configuration:
  Name: Bastion-ASG
  Launch Template: Bastion-Launch-Template
  VPC: Production-VPC
  Subnets: Public-AZ1, Public-AZ2
  Desired Capacity: 2
  Min Size: 2
  Max Size: 4
  Scaling Policy:
    - Metric: CPU Utilization
      Threshold: 90%
      Adjustment: +1
  Health Checks:
    - Type: EC2
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name Bastion-ASG \
  --launch-template LaunchTemplateName=Bastion-Launch-Template,Version=1 \
  --vpc-zone-identifier "<public-subnet-az1-id>,<public-subnet-az2-id>" \
  --min-size 2 \
  --max-size 4 \
  --desired-capacity 2 \
  --health-check-type EC2 \
  --target-group-arns <bastion-tg-arn>

 Attach scaling policies as needed

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Bastion%20Hosts%20ASG.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/Bastion%20Hosts%20ASG1.PNG)

NGINX ASG:

Config info:

NGINX-ASG Configuration:
  Name: NGINX-ASG
  Launch Template: NGINX-Launch-Template
  VPC: Production-VPC
  Subnets: Public-AZ1, Public-AZ2
  Desired Capacity: 2
  Min Size: 2
  Max Size: 4
  Scaling Policy:
    - Metric: CPU Utilization
      Threshold: 90%
      Adjustment: +1
  Health Checks:
    - Type: ELB
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name NGINX-ASG \
  --launch-template LaunchTemplateName=NGINX-Launch-Template,Version=1 \
  --vpc-zone-identifier "<private-subnet-az1-id>,<private-subnet-az2-id>" \
  --min-size 2 \
  --max-size 4 \
  --desired-capacity 2 \
  --health-check-type ELB \
  --health-check-grace-period 300 \
  --target-group-arns <nginx-tg-arn>

![(Screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/nginx%20asg.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/nginx%20asg1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/nginx%20asg2.PNG)

WordPress ASG:

Config info:

WordPress-ASG Configuration:
  Name: WordPress-ASG
  Launch Template: WordPress-Launch-Template
  VPC: Production-VPC
  Subnets: Private-AZ1, Private-AZ2
  Desired Capacity: 2
  Min Size: 2
  Max Size: 4
  Scaling Policy:
    - Metric: CPU Utilization
      Threshold: 90%
      Adjustment: +1
  Health Checks:
    - Type: ELB
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name WordPress-ASG \
  --launch-template LaunchTemplateName=WordPress-Launch-Template,Version=1 \
  --vpc-zone-identifier "<private-subnet-az1-id>,<private-subnet-az2-id>" \
  --min-size 2 \
  --max-size 4 \
  --desired-capacity 2 \
  --health-check-type ELB \
  --health-check-grace-period 300 \
  --target-group-arns <wordpress-tg-arn>

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/WordPress%20ASG.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/WordPress%20ASG1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/WordPress%20ASG2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/WordPress%20ASG3.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/WordPress%20ASG4.PNG)

After the above following the above screenshots, the proceed to review and create the wordpress ASG

Tooling ASG:

Config info:

Tooling-ASG Configuration:
  Name: Tooling-ASG
  Launch Template: Tooling-Launch-Template
  VPC: Production-VPC
  Subnets: Private-AZ1, Private-AZ2
  Desired Capacity: 2
  Min Size: 2
  Max Size: 4
  Scaling Policy:
    - Metric: CPU Utilization
      Threshold: 90%
      Adjustment: +1
  Health Checks:
    - Type: ELB
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name Tooling-ASG \
  --launch-template LaunchTemplateName=Tooling-Launch-Template,Version=1 \
  --vpc-zone-identifier "<private-subnet-az1-id>,<private-subnet-az2-id>" \
  --min-size 2 \
  --max-size 4 \
  --desired-capacity 2 \
  --health-check-type ELB \
  --health-check-grace-period 300 \
  --target-group-arns <tooling-tg-arn>

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/tooling%20asg.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/tooling%20asg1.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/tooling%20asg2.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/tooling%20asg3.PNG)


### DNS Configuration with Route 53
You remembered when we registered a free domain with Cloudns and configured a hosted zone in Route53. But that is not all that needs to be done as far as DNS configuration is concerned.

We need to ensure that the main domain for the WordPress website can be reached, and the subdomain for Tooling website can also be reached using a browser.

Create other records such as CNAME, alias and A records.

NOTE: You can use either CNAME or alias records to achieve the same thing. But alias record has better functionality because it is a faster to resolve DNS record, and can coexist with other records on that name. Read here to get to know more about the differences.

Create an alias record for the root domain and direct its traffic to the ALB DNS name. Create an alias record for tooling.fncloud.dns-dynamic.net and direct its traffic to the ALB DNS name.

14. Configure DNS Records
Set Up DNS Records for Websites:

Config info:
```
DNS Records:
  - Name: wordpress.taiwo.ip-ddns.com
    Type: A
    Alias: Yes
    Alias Target: External-ALB DNS

  - Name: tooling.taiwo.ip-ddns.com
    Type: A
    Alias: Yes
    Alias Target: External-ALB DNS

  - Name: www.taiwo.ip-ddns.com
    Type: CNAME
    Value: taiwo.ip-ddns.com
aws route53 change-resource-record-sets \
  --hosted-zone-id <hosted-zone-id> \
  --change-batch file://dns-records.json
dns-records.json:

{
  "Changes": [
    {
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "wordpress.taiwo.ip-ddns.com",
        "Type": "A",
        "AliasTarget": {
          "HostedZoneId": "<ALB-hosted-zone-id>",
          "DNSName": "external-alb-dns-name",
          "EvaluateTargetHealth": false
        }
      }
    },
    {
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "tooling.taiwo.ip-ddns.com",
        "Type": "A",
        "AliasTarget": {
          "HostedZoneId": "<ALB-hosted-zone-id>",
          "DNSName": "external-alb-dns-name",
          "EvaluateTargetHealth": false
        }
      }
    },
    {
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "www.taiwo.ip-ddns.com",
        "Type": "CNAME",
        "TTL": 300,
        "ResourceRecords": [
          {
            "Value": "taiwo.ip-ddns.com"
          }
        ]
      }
    }
  ]
}
```
Navigate to Route53 -> Hosted Zones -> Your hosted zone name -> Create record

💡 Best Practices:

Use Alias Records: Prefer alias records over CNAMEs for root domains to optimize DNS resolution.
Implement Multi-AZ DNS Failover: Configure Route 53 health checks to route traffic only to healthy endpoints.
Consistent Naming Conventions: Maintain clear and consistent DNS naming for easier management.
⚠️ Common Issues:

Incorrect Hosted Zone IDs: Ensure that the AliasTarget HostedZoneId matches the ALB's hosted zone.
Propagation Delays: Allow time for DNS changes to propagate globally before verifying accessibility.

![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/our%20word%20press%20website.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/our%20website.PNG)
![(screenshot)](https://github.com/Prince-Tee/AWS_CloudSolutionFor2CompanyWebsites/blob/main/Sreenshots%20from%20my%20local%20env/our%20wbesite.PNG)

The end of Project 15
In this project We have just created a secured, scalable and cost-effective infrastructure to host 2 enterprise websites using various Cloud services from AWS. At this point, your infrastructure is ready to host real websites’ load. Since it is a pretty expensive infrastructure to keep it up and running, we are going to start using Infrastructure as Code tool Terraform to easily provision and destroy this set up.

### Conclusion
This AWS cloud solution effectively hosts two company-owned websites by utilizing solid architecture designs, automation, and security best practices. Once you have implemented multi-Availability Zone deployments, reverse proxy configurations with NGINX, and secure database management with Amazon RDS, the infrastructure ensures high availability, scalability, and security. Continuous monitoring and maintenance will further reinforce the reliability and performance of the deployed services, thereby positioning the company for sustained operational excellence.