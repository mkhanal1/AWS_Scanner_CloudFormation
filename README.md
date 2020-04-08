# add_aws_Scanner
CloudFormation Template to deploy Qualys scanner in your selected subnet

# License
_**THIS SCRIPT IS PROVIDED TO YOU "AS IS."  TO THE EXTENT PERMITTED BY LAW, QUALYS HEREBY DISCLAIMS ALL WARRANTIES AND LIABILITY FOR THE PROVISION OR USE OF THIS SCRIPT.  IN NO EVENT SHALL THESE SCRIPTS BE DEEMED TO BE CLOUD SERVICES AS PROVIDED BY QUALYS**_

# Usage:
Use CloudFormation Template to create a Qualys EC2 Scanner using the AMI Qualys Virtual Scanner Appliance (Pre-Authorized Scanning) hardware virtual machine (HVM) and associated Security Group. To run the script you will need to supply credentials for the Qualys user name and password for Qualys API Access.

## Input Parameters: 

* UserName: Default: {supply_Qualys_user_name} ...

* Password: Default: {supply_Qualys_user_password}

* BaseUrl: Url of the Qualys Server  Default: https://qualysapi.qg2.apps.qualys.com 

* ScannerName: The name you want to give to your scanner appliance

* InstanceType:Qualys Scanner instance size Default: t2.medium

* Subnets: The subnet in which the scanner will be launched


## The script spins off following resources:

1. A Lambda Function:
    The Lambda function will run a Qualys API to create a virtual scanner and return the personalization code. The code is supplied as user input data when launching a Qualys EC2 scanner appliance. It also returns the latest AMI code for the image of the current region.

2. An EC2 Scanner using the AMI Qualys Virtual Scanner Appliance (Pre-Authorized Scanning) HVM:
    After completion of execution of Lambda function, the scanner appliance is launched using the input parameters InstanceType and the subnet. The Personalization code obtained from Lambda is passed as userdata input during the process to activate the appliance.

3. A security group:
    A security group with inbound rules as SSH from anywhere is created. A default outbound rule allowing access to any protocol will also be created. This could be modified as per organization's policy.

## Note
The CloudFormation template finds the AMI Id using `ec2 describe-images --filters "Name=name,Values=*1b8af947-aa54-4852-9da6-282428ba2f46*" ` and then does a quick sort to find the latest AMI ID based on the creation date.

You can filter based on `product-code`. Product code for Qualys Scanner is 1mp9h4zd2ze4biqif5schqeyu
