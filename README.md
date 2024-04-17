# EC2 Region Migration Using AWS MGN

## Introduction
This document provides a step-by-step guide to migrating an Amazon EC2 instance from the Oregon region to the Mumbai region using AWS Migration Hub and AWS Server Migration Service (MGN).

## Overview of AWS Services Used
1. AWS Migration Hub (MGN): Coordinates the replication and migration of EC2 instances across AWS regions.
2. AWS Server Migration Service (MGN): Facilitates the actual replication of EC2 instances.

## Prerequisites
- AWS Account: Ensure that you have an AWS account with necessary permissions for using AWS Migration Hub and AWS MGN.

## Migration Process
Step 1: Preparing the Source EC2 Instance
- Stop Unnecessary Services: Turn off services that are not needed to reduce the complexity of the migration.

- Check Storage:
 - SSH into your EC2 instance.
 - Use the command df -h / to check if there is at least 3GB of available storage. If not, proceed to add more volume.

- Add Volume if Necessary:
 - Navigate to the EC2 instance console.
 - Select your instance, click on storage, and then on the volume ID.
 - Choose "Modify Volume" to increase the size by at least 3GB.
 - Follow system-specific guidance to reflect the change within the instance.

Step 2: AWS Replication Agent Setup
- Create IAM User:
 - Log into the AWS Management Console.
 - Go to the IAM dashboard and create a new IAM user with programmatic access.
 - Attach the necessary permissions policy (e.g., AWSMigrationHubFullAccess).
 - Obtain the Access Key ID and Secret Access Key for this user.

Step 3: Installing AWS Replication Agents
- Linux-based Setup:
 - SSH into the source EC2 instance.
 - Download and install the AWS replication agent:
```
sudo wget -O ./aws-replication-installer-init https://aws-application-migration-service-ap-south-1.s3.ap-south-1.amazonaws.com/latest/linux/aws-replication-installer-init
sudo chmod +x aws-replication-installer-init
sudo ./aws-replication-installer-init --region ap-south-1 --aws-access-key-id [ACCESS_KEY] --aws-secret-access-key [SECRET_KEY] --no-prompt
```

Step 4: Configuring Launch Settings
- Adjust EC2 Launch Template:
- In the AWS MGN console, select the instance you want to replicate.
- Modify launch settings to align with your requirements (e.g., subnet, security group, instance type).

Step 5: Launching Test Instances
- Test Instance Preparation:
- Ensure the source server is "Healthy" and "Ready for testing".
- Launch test instances from the AWS MGN console to validate the configuration.

Step 6: Launch the Cutover Instances
- Initiate Cutover:
 - Confirm the source server is "Ready for cutover".
 - Launch the cutover instance, which will create a new production instance in the target region.
Step 7: Finalize Cutover
- Complete Migration:
 - After re-testing the cutover instance, finalize the migration by setting the migration lifecycle status to "Cutover complete".
 - Archive the source servers in the AWS MGN console.

## Cleanup
- Post-migration, delete any unnecessary resources including temporary instances and IAM users to avoid additional charges.
