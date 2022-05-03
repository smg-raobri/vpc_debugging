# S24 AWS Small VPC Deployment Guide

This Guide is aimed at anyone, wanting to deploy a medium VPC (with up to ~ 250 IP Addresses) into their AWS Account.

To do so, download the code and store it into an existing repository , or clone this repository into a new one, and follow the steps noted below

## 1 Preparation
### Secrets
After clong the repository, the following Secrets need to be submitted into your newly created, or existing Repository:

 1. AWS_ACCESS_KEY_ID 
 2. AWS_SECRET_ACCESS_KEY 
 3. VPC_AUTOMATOR_SNS_TOPIC_ARN 
 4. TGW_HUB_ACCOUNTID 

The AWS Access Key and Secret Key are used to connect to and authorize the AWS Account

The VPC Automator SNS Topic currently needs to be obtained through ITDC - It's an output from the VPC Automator Stack and only visible there. 

The TGW Hub AccountId is the AccountId from the S24 Network Master Account, contact ITDC to recieve that ID.

### Github Actions Variables

The Github Actions files can mostly be left as they are. 
There are some variables in the "parameter-overrides" Section at the end of the yml file of which some have adjustable values.

 - vpccidrtype=medium_test,


This value defines, that a medium VPC, with one of the Test-Environment IP-Ranges is deployed. If you want to deploy a Production-Environment VPC, change this value to "medium_prod"

 - primarystack=true,


This enables some exports in the CloudFormation Stack. leave this value as is.
For the additional VPC, this is set to false, as the export names are fixed and there may only be one export with the same name in each Account (and Region).

 - requestsnstopic=${{ secrets.VPC_AUTOMATOR_SNS_TOPIC_ARN }},


This Secret is used to ping the VPC Automator which is then used to obtain a predefined CIDR Range. Leave this value as is.

 - tgwassociation=Isolated,


The tgwassociation controls which Route Table of the Transit Gateway is used. If you want to connect to the Scout24 on-premise Datacenter, leave this value. If connectivity to other AWS Accounts is needed. Can be one of (On-Premises, Isolated, Flat, Infrastructure) 


 - tgwpropagation=On-premises


This will add the routes TO this VPC to the On-Premises Route Table so that Request coming from On-Premises can reach the VPC

## 2 Deploy Main VPC
After setting up the Secrets, you are all set do now deploy your VPC.
Go to the Actions Tab in the Repository and manually run the "Deploy VPC and Connect to TGW" Action.
This will connect your AWS Account to the Transit Gateway Network and Set up your Accounts Network.

## 3 Deploy Additional VPC if needed
If you need more than one VPC within your Account, simply go to Actions and run the "Deploy Additional VPC" Action.
This will deploy a 2nd VPC into your desired Account
