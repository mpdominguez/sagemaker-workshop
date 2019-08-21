---
title: "Configure the Video Recommendation App"
chapter: false
weight: 5
---
## Finalise Django Framework Configuration

There are various components within the application that need some final configuration. The basics, such as the VPC, the Application Load Balancer, the Auto-Scaling Group and resultant EC2 images, are all good to go, but some configuration is necessary on the Django application framework that is hosting the application. This needs to be done by connecting into the instance using SSH.

There are many ways to connect to a remote machine using SSH. This Lab Guide will continue with using SSH at the command line on an Apple Mac computer - your own method for establishing a connection may be different, perhaps using an application such as Putty on a Windows-based computer, but once connected the instructions are the same regardless of your platform combination

In order to connect you need to have your downloaded key-pair from earlier in an accessible location. It also must not be publicly readable, so if you are on a Mac or Linux system you can fix this with the following command, remembering to replace myLabKey.pem with your key name!
```
$ chmod 400 myLabKey.pem
```

Go to the the EC2 console page, go to the Instances menu on the left, and find your one running instance. Select it, and make a note of (or copy) the IPv4 Public IP for your instance

Using the same methods as before, go to the Services drop-down in the console and navigate to the Amazon Personalize service in another tab, and select Dataset groups. You will see the dataset group that you created earlier, and click on the name of your dataset group.

![selectEC2details](/images/selectEC2details.png)

Go to your computer CLI, and navigate to the directory containing your key-pair. Issue the following command to connect via SSH, changing the key-pair filename and IP address as necessary, and you should see results similar to what follows. You will see a warning about the authenticity of the host then just enter yes at the prompt.
```
$ ssh -i myLabKey.pem ec2-user@3.87.13.157

The authenticity of host '3.87.13.157 (3.87.13.157)' cannot be established.
ECDSA key fingerprint is SHA256:hFLzWhKWXwSevk14ulMwyLJqM7LN7j3Yt5w7NcnNwow.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '3.87.13.157' (ECDSA) to the list of known hosts.

Last login: Sun May  5 20:51:33 2019 from 72-21-198-64.amazon.com

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
[ec2-user@ip-10-192-11-223 ~]$
```

Navigate to the root of the Django project, and configure the single-line run script to use the private IP address of the EC2 instance. This is on the previous EC2 details screen just underneath the Public IP - you will need this again, so keep it handy. Simply replace the IP address with yours, keeping the trailing :8000 - my server's private IP is 10.192.11.223.
```
$ cd personalize-video-recs/videorecs/
$ vi runmyserver
--- {editor screen} ---
python manage.py runserver 10.192.11.223:8000
```

Django only allows access via pre-defined source IP addresses. Naturally, these could be open to the internet, but they recommend only exposing it the instance private IP address (for internal calls) and to your front-end load balancer. You already have a reference to the private IP address, so you now need to extract the Load Balancer DNS entry. Go back to the EC2 console screen, but this time select Load Balancers on the left-hand menu; select your Application Load Balancer and in the details screen that comes up select the DNS name and store it for later.

![loadbalancerDNS](/images/loadbalancerDNS.png)

Whilst we're collecting data, move to the Amazon RDS service section of the console, select Databases from the left-hand menu and select the Lab database called amazonpersonalizelab from the list. In the details screen copy the DNS endpoint for the database and store it for later.


![rdsDNS](/images/rdsDNS.png)

Go back to your SSH session window. You now need to edit two entries in the file - one is called ALLOWED_HOSTS and the other is HOST entry in the DATABASES section. Edit the file, then in the editor window find the two relevant lines and edit them so that they look like that shown below, but with your IP and DNS entries
```
$ vi videorecs/settings.py
--- {ALLOWED_HOSTS line - server private IP and ALB DNS} ---

ALLOWED_HOSTS = ['10.192.11.151', 'TestS-Appli-ADS60FMCKPMG-1862985075.us-east-1.elb.amazonaws.com']

--- {DATABASES HOSTS line - RDS DNS} ---

        'HOST': 'amazonpersonalizelab.c0azewoaia5d.us-east-1.rds.amazonaws.com',
```
### [OPTIONAL STEP]

The RDS database is postgres, and we have included the pgcli tool with this deployment, as you may wish to look at the database schema structure, examine the data that was in the RDS snapshot, or potentially update this all after you have customised the Django application for your own needs. If you wish to use it then you need to edit the startup script for the utility to point to the RDS DNS entry. You also need to know the password, which you may have noticed in the settings.py file, and it's recPassw0rd
```
$ vi pgcli
--- {editor screen} ---
/home/ec2-user/.local/bin/pgcli -h amazonpersonalizelab.c0azewoaia5d.us-east-1.rds.amazonaws.com -u vidrecdemo -d videorec
```
