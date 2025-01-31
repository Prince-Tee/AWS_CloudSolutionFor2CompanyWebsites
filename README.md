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
