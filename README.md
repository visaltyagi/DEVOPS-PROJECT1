# project1
Devops Capstone Project 1

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

# Project 1 Solution:

Step 1: Install the necessary software on the machines using a configuration management tool

a. Create the Three Instances Named as Master, Slave1 & Slave2.

b. Install “Ansible” Over Master Machine using “install.sh” file

sudo apt update

sudo apt install software-properties-common

sudo add-apt-repository --yes --update ppa:ansible/ansible

sudo apt install ansible -y

Use these commands to install "Ansible" over "Master" Machine.

c. Create the keys to “Master” instance & Paste to “Slave1” & “Slave2”

d. Create an yaml File to run “master.sh” & “slave.sh” File to Install Much Needed Tools on Instances  

Run the ansible script to install the tools over here.

e. Verified All Tools Have Been Successfully Installed on Master & Slaves or Not

f. Setup Jenkins Dashboard Over “Master” Server 

g. Add “Slave2” in “Jenkins”

h. Fork the Given Repository over "GitHub account": https://github.com/hshar/website.git 

i. Clone the Given Repository Over “Master” Node: git clone https://github.com/hshar/website.git 

j. Create a “Develop” Branch & Push it into “GitHub” Repository

  git branch
  git checkout develop
  git checkout master
  git push --all  to push the commits

k. Create a Webhook for Trigger the Jobs

l. Create a Dockerfile & Push it into “Master” Branch
   FROM ubuntu
   RUN apt-get update
   RUN apt-get install apache2 -y
   ADD  . /var/www/html
   ENTRYPOINT apachectl -D FOREGROUND
   
   Push the dockerfile in "Github Repository".
   git add .
   git commit -m "commiting dockerfile"
   git push --all
   
m. Create a “Test” Job in "Jenkins"
n. Create a “Prod” Job 

   Add build steps here: 
   sudo  docker build . -t finalrelease
   sudo docker run -itd -p 80:80 finalrelease

o. Do change in index.html file in "Github repository".
p. Create a build in "Prod" job.
q. Paste the IP Address in browser & website has been successfully shown.
   

  

