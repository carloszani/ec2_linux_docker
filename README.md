## ec2_linux_docker
Run a Linux 2 IAM with docker installed and all necessary permissions.

## Table of contents
* [Instruction](#Instructions)
* [Prerequisites](#Prerequisites)
* [Setup](#setup)

## Instructions
Follow the step by step to create a new ec2 instance on AWS with Git, Docker, Docker-Compose and Docker-Machine installed and configured.

## Prerequisites
- Have an AWS account with permission to create and run EC2 instances. (If you don't have it, follow the instructions <a target="_blank" rel="noopener noreferrer" href="https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start">link</a>.)
- A <b>valid credit card</b> for billing (<b>new accounts have 12 months of free use on AWS</b>).
- Basic knowledge of AWS, how to create EC2 instances and access remotely.

## Setup 

### Selecting an Amazon Linux 2 IAM (Free Tier eligible)
* In the example I'm using an Amazon 2 Linux IAM (Free Tier).
![Image of IAM](https://imgur.com/D9tcUlx.jpg)

* In Step3 of creating EC2, add the following UserData:

```shell
#!/bin/bash

echo "Updating IAM..."
yum update -y &&

echo "Installing docker and add user ec2 in the group to avoid to use sudo command in docker."
sudo amazon-linux-extras install docker &&
sudo service docker start &&
sudo usermod -a -G docker ec2-user &&

# Make docker auto-start
echo "Make docker auto-start..."
sudo chkconfig docker on &&

# Install GIT
echo "Installing git..."
sudo yum install -y git &&

# Get lastest docker-compose verion & fix permissions after download
echo "Getting lastest docker-compose...."
sudo curl -L  https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose &&
    sudo chmod +x /usr/local/bin/docker-compose &&

# Get lastest docker-machine version & fix permission after download
echo "Getting lastest docker-machine..."
sudo curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-machine &&
    sudo chmod +x /usr/local/bin/docker-machine &&

# Reboot to verify it all loads fine on its own.
echo "Rebooting the EC2..."
sudo reboot
```
![Image of UserData](https://imgur.com/cZjstXk.png)
