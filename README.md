Project – Capstone I

You have been hired as a Sr. DevOps Engineer in Abode Software. They want to implement DevOps Lifecycle in their company. You have been asked to implement this lifecycle as fast as possible. Abode Software is a product-based company and their product is available on this GitHub link. 
https://github.com/hshar/website.git 

Following are the specifications of the lifecycle: 

1. Install the necessary software on the machines using a configuration management tool 
2. Git workflow has to be implemented 
3. Code Build should automatically be triggered once a commit is made to master branch or develop branch. 
a. If a commit is made to master branch, test and push to prod 
b. If a commit is made to develop branch, just test the product, do not push to prod 
4. The code should be containerized with the help of a Dockerfile. The Dockerfile should be built every time there is a push to GitHub. Use the following pre-built container for your application: hshar/webapp The code should reside in '/var/www/html' 
5. The above tasks should be defined in a Jenkins Pipeline with the following jobs: 
a. Job1: build 
b. Job2: test 
c. Job3: prod

For detailed solution, click on this link: https://medium.com/devops-guides/devops-capstone-project-implementing-a-devops-lifecycle-on-a-website-using-docker-jenkins-only-ab4d6ee908f2

# Project 1 Solution:

Problem (1) Solution: Install the necessary software on the machines using a configuration management tool

In this project, we need to install the “Java”, “Jenkins”, “Ansible”, “Docker” & “Python” on “Master” machine while install the “Java” & “Docker” on both the slaves. 

We don’t need to create a third job as “build” here. Only “test” & “prod” will do the jobs.

A. Create Three Instances as Master, Slave1 & Slave2.

B. Install “Ansible” Over Master Machine using “install.sh” file


C. Run the “install.sh” file using the command: bash install.sh
 

D. Create the keys to “Master” instance & Paste to “Slave1” & “Slave2”


E. Put the “Slave IP’s” into the “Ansible Host File”

 
F. Run this ansible command to ping the machines: ansible -m ping all.
Type “Yes” & your “slaves” will be successfully pinged.
 

G. Create an yaml File to run “master.sh” & “slave.sh” File to Install Much Needed Tools on Instances

H. Run this command to create an “ans.yaml” file to execute the “master.sh” & “slave.sh” through “Ansible”. Command: sudo nano ans.yaml 
 

I. Create “master.sh” & “slave.sh” File to Install Much Needed Tools on Instances

 
J. Type this command to execute the “master.sh” & “slave.sh” using “ans.yaml” file.
ansible-playbook ans.yaml 

All the mentioned tools in “master” & “slaves” has been successfully installed, if status is “Ok”.
 
 
K. Verified All Tools Have Been Successfully Installed on Master & Slaves or Not


L. Setup Jenkins Dashboard Over “Master” Server 

 
M. Add “Slave1” in “Jenkins”

N. Add “Slave2” in “Jenkins”

O. Fork the Given Repository 
 
P. Clone the Given Repository Over “Master” Node

Q. Create a Webhook for Trigger the Jobs

R. Create a Dockerfile & Push it into “Master” Branch
 
S. Create a “Test” Job 
 
T. Create a “Prod” Job 
 
Put these commands to build a container on port 80:

sudo  docker build . -t finalrelease

sudo docker run -itd -p 80:80 finalrelease
 
U. Do change in "index.html" file & create a job again automatically. 

V. Do changes in docker file also in "Jenkins" also.

sudo docker rm -f $(sudo docker ps -a -q)

sudo  docker build . -t finalrelease

sudo docker run -itd -p 80:80 finalrelease

W. Your container will be successfully created everytime & website will be successfully deploed.

X. Copy the Slave IP & Paste it into browser address bar & a webpage will be successfully shown.



