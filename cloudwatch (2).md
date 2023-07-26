Instructions
This is a technical assessment to test your presentation, Linux and AWS skills.  

You will be required to do a demonstration on Friday, your allocated time so take your time and prepare.  

The shared file contains all the instructions on how to go about it, you can use other external resources including AWS documentation to make the demo successful.  

You will have 20 minutes to do the demonstration so prepare yourself in advance.  

 
Beginning of Assessment  

# Hands-on CW-01 : Setting Logging with Cloudwatch 

Purpose of the this hands-on training is to create ClouWatch agent.

- configure Logging with Agent.


## Outline


## STEP 1 : Create a EC2 Instance

-Go to EC2 menu using AWS console

- Launch an Instance
- Configuration of instance.

```text
AMI             : Amazon Linux 2
Instance Type   : t2.micro
Tag             :
    Key         : Name
    Value       : Cloudwatch_Log
Security Group ---> SHH --->  anywhere
```
- Set user data.

```bash
#! /bin/bash
yum update -y
amazon-linux-extras install nginx1.12
chkconfig nginx on
cd /usr/share/nginx/html
chmod o+w /usr/share/nginx/html
rm index.html
wget https://raw.githubusercontent.com/muleif/route-53/Master/index.html
wget https://raw.githubusercontent.com/muleif/route-53/Master/bob.jpg
service nginx start
```

## STEP 2 : Create an IAM role granting cloudwatch access to the instance and attach

- Role Name :eMcloudwatchlog  
- Role Description: eM Cloudwatch EC2 logs access role
- permissions; CloudWatchAgentServerPolicy, administratoraccess



## STEP 3:  Install and Configure the CloudWatch Logs Agent

- Go to the terminal and connect to the Instance named "Cloudwatch_Log"

- install cloudwatch log agent with following command:
```bash
sudo yum install -y awslogs
sudo systemctl start awslogsd
sudo systemctl enable awslogsd.service
```
- go to the Cloudwatch menu and select Log groups on left hand pane

- show  the newly created  "/var/log/messages"  log group ---> show the newly created "log streams"


## STEP 4: Configure Nginx logs

- go to the terminal and connect to the EC2 Instance named "Cloudwatch_Log" with ssh

- go to the "awslogs" 
-cd /etc/awslogs


- use the root account to edit the awslogs.conf
- sudo nano awslogs.conf


- at the bottom of the page you'll see the following comments:
```bash
[/var/log/messages]
datetime_format = %b %d %H:%M:%S
file = /var/log/messages
buffer_duration = 5000
log_stream_name = {instance_id}
initial_position = start_of_file
log_group_name = /var/log/messages
```

Add the following after the above seen above: 


```bash

[/var/log/nginx/access.log]
datetime_format = %b %d %H:%M:%S
file = /var/log/nginx/access.log
buffer_duration = 5000
log_stream_name = {instance_id}
initial_position = start_of_file
log_group_name = AccessLog

[/var/log/nginx/error.log]
datetime_format = %b %d %H:%M:%S
file = /var/log/nginx/error.log
buffer_duration = 5000
log_stream_name = {instance_id}
initial_position = start_of_file
log_group_name = ErrorLog
```

- save the file and close
```bash
```

- to activate the new configuration, stop and start the "awslogsd".
sudo service awslogsd stop
sudo systemctl disable awslogsd.service
sudo service awslogsd start
sudo systemctl enable awslogsd.service


## access the instance

- Go to the EC2 instance and grab the public IP address. And paste it to the browser. Their logs will be sent to the cloudwatch logs part.

- go to the Cloudwatch logs group again 

- click the created log group named "AccessLog" and "ErrorLog" ---> show the newly created "log streams"





