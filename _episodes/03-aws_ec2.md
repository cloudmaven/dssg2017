---
title: "Introduction to the AWS console & provisioning an EC2 instance"
teaching: 0
exercises: 30
questions:
- "How do I build a virtual machine in AWS?"
- "How do I use my virtual machine to transfer data to an S3 bucket"
objectives:
- "Learn to launch an AWS EC2 instance"
- "Learn to create an AWS S3 storage bucket"
- "Learn how to use the AWS CLI to transfer data from your AWS EC2 instance to the S3 storage bucket"
keypoints:
- We want to build a virtual machine on the AWS cloud and transfer some data into storage.
- We will learn about the different available storage mechanisms on AWS

---

### Prerequisites
A 2-factor authentication app (e.g. Authy ) 
Terminal or bash for Windows

### Create an IAM user
For security, you should not log in to your account using root credentials. Anything that you need 
to do with your AWS services can be achieved by creating an Administrator role through IAM. You 
only need root access for managing your account plans (upgrades or closing your account)

There are two parts to ensuring security of your account. One is enabling Multi-Factor Authentication 
to log on as root, and installing yourself as the first ~~All-Supreme Being~~ Administrator. 

More information is available [here](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html)

### Launch an AWS EC2 Instance
*This tutorial was adapted from the 
[AWS EC2 Instance Starter Guide](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) 
and https://cloudmaven.github.io/documentation/aws_ec2.html

Step 1: Launch an Instance

You can launch a Linux instance using the AWS Management Console as described in the following procedure. This 
tutorial is intended to help you launch your first instance quickly, so it doesn't cover all possible options. 
For more information about the advanced options, see Launching an Instance.

To launch an instance

Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
Participants on our accounts should use: https://uwcloudczar.signin.aws.amazon.com/console and log 
on with the given username and password. You will be prompted to change your password. 

From the console dashboard, choose Launch Instance.

The Choose an Amazon Machine Image (AMI) page displays a list of basic configurations, called Amazon Machine 
Images (AMIs), that serve as templates for your instance. We will be setting up an Ubuntu 16.04 virtual machine. 

![](/dssg2017/fig/01-aws-intro-0001.png)

On the Choose an Instance Type page, you can select the hardware configuration of your instance. 
Here, we will select the t2.small type

![](/dssg2017/fig/01-aws-intro-0002.png)

![](/dssg2017/fig/01-aws-intro-0003.png)

![](/dssg2017/fig/01-aws-intro-0004.png)


You can also add storage to your virtual machine. We will change the root directory to be 25GB, to account for 
installation of some packages. 

We won't be covering this in the workshop, but the directions on how to mount the additional storage (other than root) 
to your virtual machine is [here](https://cloudmaven.github.io/documentation/aws_ec2.html#mounting-the-attached-volume). 

![](/dssg2017/fig/01-aws-intro-0005.png)

You can add Tags to your EC2 to help with identify your resource for things like billing etc. For example, you can 
specify Machine Name, Owner, Group, etc. 
More info [here](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#tag-basics).

![](/dssg2017/fig/01-aws-intro-0006.png)


To avoid creating multiple security groups, we will all select the Cloud101 Security Group which will have 
Port 22 (ssh) and Port 80 (HTTP) open. 

![](/dssg2017/fig/01-aws-intro-0007.png)


On the Review Instance Launch page, choose Launch.

When prompted for a key pair, select "Create New Key Pair", enter a name for the key pair, and then 
choose Download Key Pair. 

This is the only chance for you to save the private key file, so be sure to download it. Save the private key 
file in a safe place. You'll need to provide the name of your key pair when you launch an instance and the 
corresponding private key each time you connect to the instance.

**Caution**
Don't select the Proceed without a key pair option. If you launch your instance without a key pair, then you can't 
connect to it.  When you are ready, select the acknowledgement check box, and then choose Launch Instances.

![](/dssg2017/fig/01-aws-intro-0008.png)

A confirmation page lets you know that your instance is launching. Choose View Instances to close the confirmation 
page and return to the console.

On the Instances screen, you can view the status of the launch. It takes a short time for an instance to launch. 
When you launch an instance, its initial state is pending. After the instance starts, its state changes to running 
and it receives a public DNS name. (If the Public DNS (IPv4) column is hidden, choose the Show/Hide icon in the 
top right corner of the page and then select Public DNS (IPv4).)

It can take a few minutes for the instance to be ready so that you can connect to it. Check that your instance has 
passed its status checks; you can view this information in the Status Checks column.


# Tag that resource!!!


Let's take a moment to discuss tagging...


### Create a s3 storage bucket

We will now ssh into the virtual machine, install miniconda and install the AWS CLI. 

The two things you need are your private key and your instance DNS or IP address. 

Open your Terminal or Bash Shell. Go to the location of where you downloaded your private key. We will need 
to change the permissions for the private key. 


```bash
$ cd ~/Downloads/keys #This is the directory I save my keys in
$ sudo chmod 400 privatekey.pem
$ ssh -i "privatekey.pem" ubuntu@ec2-35-160-75-232.us-west-2.compute.amazonaws.com
```

Once you've ssh-ed into your virtual machine, we will download and install miniconda. Amanda thinks the command
apt-get is useful but this is because she advocates using Ubuntu.  Rob on the other hand used the AWS Linux and 
therefore he uses the *yum* command. In what follows we present both of these commands as alternatives...


For AWS Linux: 

```bash
$ sudo yum update
```

For Ubuntu:

```bash
$ sudo apt-get update
```

And then we continue on either OS by installing the Miniconda package manager.  This in turn helps us get
Python running.

```
$ wget 'https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh'
$ bash Miniconda3-latest-Linux-x86_64.sh
```


Running the last command you should take the defaults; but the last question is worth mentioning: 
*"Do you wish the installer to prepend the Miniconda install location to PATH in your /home/ec2-user/.bashrc?"*


Answer 'yes' (not the default 'no'): This places Miniconda3 first in line when you run commands in the future.
You may need to log out and back in for this to take effect.  Now re-run the bash configuration script:


```bash
$ source ~/.bashrc
```

Go through the prompts. Next we will install the AWS-CLI

```bash
$ pip install --upgrade --user awscli
```

Now we can configure the AWS Command Line Interface (CLI). 


```bash
$ aws configure
```


When prompted, enter your Access Key ID and Secret Access Key.  These are found in your personal credentials
file, the file associated with your IAM User identity. You keep this file in a secure location (password-protected)
on your personal file system. Both of these values -- the Key ID and the Secret Key -- are strings of characters; 
easiest to copy and paste.

To continue: You are configuring the CLI on this EC2 instance to authenticate 'as you' when subsequently carrying 
out S3 actions like creating new buckets. That is our next task. Hence "you" must have IAM permissions that allow you 
to do those things.  There are two other questions in the **aws configure** dialog: Default region name and default 
output format (which might be json, text etc.).  For both of these questions press "Enter". 


***Heads up: If for some reason you find yourself unable to see S3 bucket listings in what follows: Delete all but 
the first line in the file ~/.aws/config.  That single line should read '[default]'.***


Once **aws configure** is complete we issue a series of 'two-part' CLI commands: First **aws** to invoke the AWS CLI, 
then **s3** to stipulate 'this command concerns AWS S3 storage'. Here are the commands, intended to list S3 buckets, 
create a bucket, download a file from an external S3 bucket and then upload it to your this new S3 bucket. 

```bash
$ aws s3 ls #list buckets
$ aws s3 mb s3://<bucket-name>  #make bucket
$ aws s3 ls s3://<bucket-name> #list bucket contents
```

Ok we left off here...

```bash
$ aws s3 cp s3://cloud101demo/Mean_Apr_ET.geojson . #download a file from a shared bucket
$ aws s3 sync s3://<bucket-name> s3://cloud101demo --acl public-read #sync your bucket with the cloud101 bucket and allow public read access
```

More info here: http://docs.aws.amazon.com/cli/latest/userguide/using-s3-commands.html
