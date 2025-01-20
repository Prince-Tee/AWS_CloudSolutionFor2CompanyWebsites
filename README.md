AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY
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