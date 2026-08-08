# AWS-EC2-Migration
Documentation to migrate an EC2 instance to another region


AWS documentation

Create a VPC in the first region, in this case us-east-1.
Ensure that the VPC has an internet gateway as well as private and public subnets
![image_alt](https://github.com/regular-options/AWS-EC2-Migration/blob/316000f4645992710a9c978f4f2c3ea171d636ed/AWS%201.png)


Create an EC2 instance, I have used Amazon AMI. The instance MUST HAVE at least 3gb of space available in the main filesystem AND 500m in the /tmp directory. In short use t2.small not t2.micro
You can check the storage using the following command: “df -h /” “df -h /tmp”
Ensure that you have enabled http and ssh for inbound traffic as this is a web server it must be available to the PUBLIC then enable it to be given a public IP
install httpd, enable and start the service. Create a index.html in the /var/www/htrml directory

 Create a secondary VPC in a different region, one that you will be migrating the instance to. Use a different subnet to prevent confusion

In the second region in this case it should be us-west-1 go to the application migration service section (MGN)
Click Get started
Under Source Servers click add server 
Select linux or windows depending on your instance in this case select linux
Select replicate all disks
In a new tab under the AWS console create a new IAM user. 
Give the user console access
Name the user MigrateUser, and provide the user with a password. 
Assign the user the following policy “AWSApplicationMigrationAgentInstallationPolicy”
Once the user is created click the user
In the top right under summary select “create access key”
Select other 
Now retrieve the keys both the access key and the secret access key.
Now go back the MGN tab enter the access key and secret access key of “MigrateUser”


Now connect to your instance in AWS
Paste the download installer command in the instance
It should be installed in the present working directory
Then paste the second command into the instance
It will take a little bit of time but the final output should be as follows


If you receive any errors refer to this link:
https://docs.aws.amazon.com/mgn/latest/ug/Troubleshooting-Agent-Issues.html#Error-Installation-Failed 
After this step the instance will be added to the active source servers console

Once the source server is listed and is “Healthy”
Go under the settings
Change the replication template to reflect what you want the instance to be like
Under the launch template you can modify the launch template ENABLE public IP under advanced settings
Save it and then under action make it the default version
Under MGN 
After all the replication is done and it is “ready for testing”
Click Test and cutover
Launch test instances 
Instances will be launched with the name AWS application, do not terminate them 
A job ID will be created under the migration dashboard, you can view the progress of the job as it goes on
This will take a while
After the test in progress is finished
Select in test and cutover launch cut over instances
Wait again
Finally select Finalize cutover
Now go to the EC2 instance in your migration region
The instance should be named the IP address that it had in the first region
Connect to it 
It should have the same configuration as the previous instance 
Congrats you have migrated an instance to another region



