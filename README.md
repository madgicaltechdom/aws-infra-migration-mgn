# EC2 Region Migration Using AWS MGN

The second part of the migration involves moving an EC2 instance from one region to another using AWS Migration Hub and AWS Server Migration Service (MGN).

Step 1: Check EC2 Instance Storage

SSH to the EC2 instance and check available storage.

Run this command "df -h /" to check how much storage is available.

Ensure at least 3GB of available storage for the replication agent.

Follow the below steps if EC2 instance don't have 3GB of space available.

Step 2: Add Volume to EC2 Instance

Visit the AWS EC2 instance console.

Modify the volume by increasing its size by at least 3GB.

Upgrade the size in the EC2 instance accordingly.

Step 3: AWS Replication Agent Setup

Create an IAM user for the AWS replication agent via the AWS Management Console.

Obtain the Access Key ID and Secret Access Key for this user.

Installing AWS Replication Agents for Linux-Based Servers

SSH into the source EC2 instance in the source region.

Download and install the AWS replication agent installer for Linux.

sudo wget -O ./aws-replication-installer-init https://aws-application-migration-service-ap-south-1.s3.ap-south-1.amazonaws.com/latest/linux/aws-replication-installer-init

sudo chmod +x aws-replication-installer-init; sudo ./aws-replication-installer-init --region ap-south-1 --aws-access-key-id xxxxx --aws-secret-access-key xxxxxxxx --no-prompt

Verify the addition of the instance to the source servers list in the AWS MGN console.

Step 4: Configuring Launch Settings

The launch template settings are default. We need to make changes according to our requirements.

Follow the steps to make changes in the settings:

Click on the instance that you want to launch for testing.

Now, click on “Replication settings”. Then click on “Edit”.

Select the subnet as “Prod-VPC” (Choose your VPC).

Now, click on “Launch setting”.

Click on “Modify”, to change ec2 launch template settings.

Modify Amazon EC2 launch templates to place source servers in the appropriate subnets and security groups.

Select the respective subnet. (Choose “Prod-subnet-public-3”, as it is in ap-south-1c AZ. Our RDS is also in this AZ).

Choose the appropriate security group.

Enable or disable public IP assignments based on your server's requirements.

Select the type of instance as per your need.

Select the changes as per your needs and requirements and save that template with a specific description.

When the template version is created, click on its link.

Now, surely make that template version default. As when you launch the instance, it will pick the settings of the default version template.

Now, return to the MGN console and select the same source server.

Click on launch settings, and check whether changes are done or not.

Step 5: Launching Test Instance

Test Instance Preparation

Ensure that source servers are in a healthy state and ready for testing. Confirm the indicators in the MGN console display "Healthy" and "Ready for testing."

Select the source servers to launch as test instances.

Choose "Test and Cutover" from the top-right dropdown menu.

Select "Launch test instances" and confirm.

After launching test instances, go to the AWS EC2 instances console.

Check whether instances are created with the given settings.

Test the instances running smoke test cases for the servers.

If they are working fine you can move to the next step.

Step 6: Launch the Cutover Instances

Ensure source servers are "Healthy" and "Ready for cutover" in the MGN console.

Select the source servers for cutover and launch the cutover instances.

Click on “Test and cutover”, then click on “Ready for cutover”.

Now, click on “Launch cutover instance”.

It will terminate the instance created for testing and re-create a new instance. This instance is actually a production instance.

Re-test the newly created instance by running the same smoke test cases for your servers.

Step 7: Finalize Cutover

After, testing the instances you can finalize the migration process.

For this, select all the source servers where cutover is completed.

Click on “Test and cutover”, then click on “Finalize cutover”.

It will change the source servers' Migration Lifecycle status to "Cutover complete."

This terminates the Replication Server and halts data replication.

Now, move them to “Archived source servers”:

Go to the AWS MGN console.

Select all source servers.

Click on “Action”.

Then, select “Mark as archived”.
