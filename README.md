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
Problem (1) Solution: Install the necessary software on the machines using a configuration management tool
In this project, we need to install the “Java”, “Jenkins”, “Ansible”, “Docker” & “Python” on “Master” machine while install the “Java” & “Docker” on both the slaves. 
We don’t need to create a third job as “build” here. Only “test” & “prod” will do the jobs.
A. Create the Three Instances Named as Master, Slave1 & Slave2.
Step 1: Go to “AWS Console > EC2”.
 
Step 2: Click on “Instances”.
 
Step 3: Click on “Launch Instance”.
 
Step 4: Choose “Name” as “Master” in “Name and tags”.
 
Step 5: Choose “AMI” as “Ubuntu” with “Ubuntu Server 22.04 LTS (HVM), SSD Volume Type”.
 
Step 6: Choose “Instance type” as “t2.medium” for “Master” machine.
 
Step 7: Choose “key pair (login)” as “Demo”.
 
Step 8: In “Network Settings”, choose “Select existing security group” as “launch-wizard-9”.
 
Step 9: Click on “Launch Instance”.
 
Step 10: Click on “Hyperlink”.
 
Step 11: Click on “Instances” in “Breadcrumb Section”.
 
Step 12: “Master” will be in “Running” State. Again, click on “Launch Instances” to create the “Slaves” Machine on “t2.micro”.
 
Step 13: Choose “Name” as “Slave” in “Name and tags”.
 

Step 14: Choose “AMI” as “Ubuntu” with “Ubuntu Server 22.04 LTS (HVM), SSD Volume Type”.
 
Step 15: Choose “Instance type” as “t2.micro” for “Slave” machine.
 
Step 16: Choose “key pair (login)” as “Demo”.
 
Step 17: In “Network Settings”, choose “Select existing security group” as “launch-wizard-9”.
 
Step 18: Choose “Number of Instances” as “2”. Click on “Launch Instance”.
 
Step 19: Click on “Hyperlink”.
 
Step 20: Click on “Instances” in “Breadcrumb Section”.
 
Step 21: “Slaves” will be in “Running” State. 
 
Step 22: Rename both the “Slaves”. First one as “Slave1” & second one as “Slave2”.
 
Both “Master” & “Slaves” machine is ready now.
B. Install “Ansible” Over Master Machine using “install.sh” file
Step 1: Choose the “Master” machine & click on “Connect”.
 
Step 2: Click on “Connect”.
 
Step 3: Run this command to create an “install.sh” file: sudo nano install.sh. Press “enter” from the keyboard.
 
Step 4: Paste these commands in “install.sh” file.
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
 
Do “CTRL+X” to “exit” & Press “Y” to save the file. Press “enter” from the keyboard to exit the file.
Step 5: Run the “install.sh” file using the command: bash install.sh & press “enter” from the keyboard.
 
Step 6: Commands will be started successfully to execute. Press “Y” for updating the machine & Ansible will start installing. Check the installation using “ansible –version” command. 
 
With Ansible, Python will automatically have installed also.
C. Create the keys to “Master” instance & Paste to “Slave1” & “Slave2”
Step 1: Run this command: ssh-keygen to create the “public” & “private” key.
Press “enter” three times.
“Public” & “Private” key has been successfully generated.
 
Step 2: Run this command to land inside the .ssh directory: cd .ssh/
 
Step 3: Run this command: “cat id_rsa.pub” to copy the content.
 
Step 4: Go to “Slave1” & run this command “cd .ssh/” to land inside the .ssh directory.
 
Step 5: Run this command to open the “authorized_keys” file: vi authorized_keys
 
Step 6: Paste the public key content in “authorized_keys” file. Press “ESC” & type :wq! To quit & save the file.
 
File will be successfully saved with master public key content.
Step 7: Go to “Slave2” & run this command “cd .ssh/” to land inside the .ssh directory.
 
Step 8: Run this command to open the “authorized_keys” file: vi authorized_keys
 
Step 9: Paste the public key content in “authorized_keys” file. Press “ESC” & type :wq! To quit & save the file.
 
 
File will be successfully saved with master public key content.	
D. Put the “Slave IP’s” into the “Ansible Host File”
Step 1: Go to “Master Machine” & type this command: sudo nano /etc/ansible/hosts to open the host file.
 
Step 2: Paste these commands in host file:
slave1 ansible_host=172.31.36.248
slave2 ansible_host=172.31.47.61
 
Do “CTRL+X” to “exit” & Press “Y” to save the file. Press “enter” from the keyboard to exit the file.
Step 3: Run this ansible command to ping the machines: ansible -m ping all.
Type “Yes” & your “slave1” is successfully pinged.
 
Again type “yes” & second slave machine (slave2) has been successfully pinged.
E. Create an yaml File to run “master.sh” & “slave.sh” File to Install Much Needed Tools on Instances
Step 1: Run this command to create an “ans.yaml” file to execute the “master.sh” & “slave.sh” through “Ansible”. Command: sudo nano ans.yaml 
 
Step 2: Paste this script in “ans.yaml” file.
- name: installing tools on master
  hosts: localhost
  become: true
  tasks: 
  - name: executing master.sh script
    script: master.sh
- name: installing tools on slaves
  hosts: slave1
  become: true
  tasks: 
  - name: executing slave.sh script
    script: slave.sh
- name: installing tools on slaves
  hosts: slave2
  become: true
  tasks: 
  - name: executing slave.sh script
    script: slave.sh	  
 
Do “CTRL+X” to “exit” & Press “Y” to save the file. Press “enter” from the keyboard to exit the file.
F. Create “master.sh” & “slave.sh” File to Install Much Needed Tools on Instances
Step 1: Create a “master.sh” file & paste the below commands here to install the tools such as “Jenkins”, “Docker”, & “Java”. Use this command: sudo nano master.sh
 
Press “enter” from the keyboard to paste the content. 
Step 2: Paste this content to the “master.sh” file.
sudo apt-get update
sudo apt-get install fontconfig openjdk-17-jre
sudo apt-get install docker.io -y
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
 
Do “CTRL+X” to “exit” & Press “Y” to save the file. Press “enter” from the keyboard to exit the file.
Step 3: Create a “slave.sh” file for installing the much-needed tools on “Slaves” server.
 
Press “Enter” from the keyboard to open the file.
Step 6: Paste the command here:
sudo apt-get update
sudo apt-get install default-jdk -y
sudo apt-get install default-jre -y
sudo apt-get install docker.io –y
 
Do “CTRL+X” to “exit” & Press “Y” to save the file. Press “enter” from the keyboard to exit the file.
Step 7: Type this command to execute the “master.sh” & “slave.sh” using “ans.yaml” file.
ansible-playbook ans.yaml 
All the mentioned tools in “master” & “slaves” has been successfully installed, if status is “Ok”.
 
 
G. Verified All Tools Have Been Successfully Installed on Master & Slaves or Not
Step 1: Go to “Master”, type these commands to check that Jenkins, Docker & Java has been successfully installed or not.
docker –version to check the “docker” version.
jenkins –version to check the “jenkins” version.
java –version to check the “java” version.
 
All tools have been successfully installed on “Master” machine.
Step 2: Go to “Slave1”, type these commands to check that Docker & Java has been successfully installed or not.
docker –version to check the “docker” version.
java –version to check the “java” version.
 
All tools have been successfully installed on “Slave1” machine.
Step 3: Go to “Slave2”, type these commands to check that Docker & Java has been successfully installed or not.
docker –version to check the “docker” version.
java –version to check the “java” version.
 
All tools have been successfully installed on “Slave1” machine.
H. Setup Jenkins Dashboard Over “Master” Server 
Step 1: Put the URL : http://3.110.160.39:8080/. Press “enter” from the keyboard. Jenkins Dashboard will be opened.
 
Step 2: Paste the command sudo cat /var/lib/jenkins/secrets/initialAdminPassword to “Jenkins Master”. Press “enter” from the keyboard & a token will be provided.
Copy this token from here.
 
Step 3: Paste the token in “Dashboard” & click on “Continue”.
 
Step 4: Click on “Install Suggested Plugins”.
 
Step 5: It will started set up the needed “Jenkins” plugin.
 
Step 6: Put the following entries here:
Username: admin
Password: admin
Confirm Password: admin
 
Full name: admin
E-mail address: admin@admin.com
Click on “Save and continue”.
 
Step 7: Click on “Save and Finish”.
 
Step 8: Click on “Start using jenkins”.
 
Step 9: “Jenkins Dashboard” will be ready.
 
I. Add “Slave1” in “Jenkins”
Step 1: Go to “Jenkins Master” & click on “New Node”.
 
Step 2: Write “Node name” as “Slave1”. Choose “Type” as “Permanent Agent”.
 
Click on “Create”.
Step 3: Choose the following options here:
Name: Slave1
Description: This is the Slave1 node.
Number of executors: 1
Remote root directory: /home/ubuntu/jenkins
 
Label: Write “Slave1” here because “Jenkins” identifies the node using this feature.
Launch Method: Launch agents via SSH
Host: 172.31.36.248 (Slave1 Private IP)
Credentials: Click on “Add”. Again, click on “Jenkins”.
 
In “Add Credentials”, choose “kind” as “SSH Username with private key”. 
Scope: Global (Jenkins, nodes, items, all child items, etc) – Remain as it is.
ID & Description	: PEM
Username: ubuntu
 
In “Private Key”, choose “Enter Directly”. Click on “Add”.
 
Now, paste the. pem key content here. We have taken pem key as “Demo”.
 
 
 
Click on “Add”.
Step 4: Choose “Credentials” as “ubuntu (PEM)”. 
Choose “Host Key Verification Strategy” as “Non verifying Verification Startegy”. Click on “Save”.
 

Step 5: “Slave1 Node” has been successfully launched like this:
 
J. Add “Slave2” in “Jenkins”
Step 1: Go to “Jenkins Master” & click on “New Node”.
 
Step 2: Write “Node name” as “Slave2”. Choose “Type” as “Copy Existing Node”. In “Copy Existing Node”, choose “Slave1”.
 
Click on “Create”.
Step 3: Fill the following options here:
Description: This is a slave2 agent.
Labels: Slave2
 
Host: 172.31.47.61
Remain all entries are as it is.
Click on “Save”.
 
Step 4: The “Slave2” Node has been successfully launched.
 
“These are the complete steps to set up the tools & server using the configuration management tool “Ansible”.
Problem (2) Solution: Git workflow has to be implemented 
A. Fork the Given Repository
Step 1: First, fork the given repository to your “GitHub Account” to perform this assignment. 
 
Step 2: Login into your “Git Hub Account”. Click on “Signin”.
 
Step 3: Put your username & password here. Click on “Sign-in”.
 
Step 4: Now, “Fork” option will be shown. Click on “Fork”. It will automatically fetch “website” as “Repository name”.
 
Description: Jenkins Case Study Website
Remain as it is selected “Copy the master branch only”.
Step 5: Click on “Create fork”.
 
Step 6: It will take some time to fork the repository.
 
Step 7: Given “Repository [website]” has been successfully forked. 
 
B. Clone the Given Repository Over “Master” Node
Step 1: Go to “Instances”. Select the “Master” node & click on “Connect”.
 
Step 2: Again, click on “Connect”.
 
Step 3: Run this command to update the machine: sudo apt-get update.
 
Step 4: Now, clone the repository using this command: “git clone https://github.com/visaltyagi/website”. Press “Enter” from the keyboard.
 
Step 5: Do “ls” here. Go to “Website” directory using “cd website”.
You will land inside the “website” directory.
 
Step 6: Do “ls” here & your “images” directory & “index.html” file will be present here.
 
C. Create a “Develop” Branch & Push it into “GitHub” Repository
Step 1: For creating a branch in git, use this command: “git branch develop”. Run this command to view the branch: “git branch”.
“develop” branch will be shown here.
 
Step 2: Now, run this command to check the “develop” branch: git checkout develop. Do “ls” here & all the files will be found here also.
 
Step 3: Now, switch to “master” branch using the “git checkout master” command.
 
Step 4: Run the command: “git push –all” to push all the commits inside the master.
 
Step 5: Now, go to your “GitHub Repository” & checkout the branches. “develop” branch will be successfully shown here.
 
D. Create a Webhook for Trigger the Jobs
Step 1: Go to “Settings”.
 
Step 2: Click on “Webhooks”.
 
Step 3: Click on “Add webhook”.
 
Step 4: Choose the following options here:
Payload URL: http://3.110.160.39:8080/github-webhook/
Which events would you like to trigger this webhook?: Send me everything
 
Click on “Add webhook”.
Step 5: Your webhook will be successfully created.
 
Problem (4) Solution: The code should be containerized with the help of a Dockerfile. The Dockerfile should be built every time there is a push to GitHub. Use the following pre-built container for your application: hshar/webapp The code should reside in '/var/www/html' 
Create a Dockerfile & Push it into “Master” Branch
Step 1: Run this command to create a “Dockerfile”: sudo nano DockerFile.
 
Step 2: Press “Enter” from the keyboard. Dockerfile will be opened & type the given command here:
FROM ubuntu
RUN apt-get update
RUN apt-get install apache2 -y
ADD  . /var/www/html
ENTRYPOINT apachectl -D FOREGROUND
 
Do “CTRL+X” to exit & for saving the file, type “Y” from the keyboard & press “Enter”. Your Dockerfile will be successfully saved & created.
Step 2: You can view the content using the cat command: “cat Dockerfile”.
 
Step 3: First, put this Dockerfile into “Website” Repository in “GitHub”.
Run these commands to insert the file into git:
git add .
git commit –m “Dockerfile”
 
Step 4: Now, “Dockerfile” is not inserted into “Master Branch”. Run this command: git branch to view the branch. 
Only “master” branch will be shown.
 
Step 5: After running the branch command, run “git checkout master” command to check-out the commits. Run this command: “git push --all” to publish all the commits.
Put your “GitHub Username” & “Password” Here.
Username: visaltyagi@gmail.com
Password: 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 (GitHub Token)
After doing “git push”, all the content is successfully pushed to “master” branch.
  
Step 6: Go to “GitHub” account & view the “master” branch. You will notice that “Dockerfile” has been successfully pushed to “master” branch.
 Step 7: If we do “ls” here, “Dockerfile” will be shown in “website” directory. 
 
Problem (5) Solution: The above tasks should be defined in a Jenkins Pipeline with the following jobs: 
a. Job1: build (Not needed here)
b. Job2: Test 
c. Job3: Prod
A. Create a “Test” Job 
Step 1: Click on “New Item”.
 
Step 2: Choose the following options here:
Enter an item name: Test
Option: Freestyle project
Click on “OK”.
 
Step 3: Choose “Description” as “For develop branch, don't publish it”.
 
Step 5: Go to “GitHub” account & click on Code. Go to “HTTPS” section & copy the link.
 
While Choose “Label Expression” as “Slave1”.
Step 6: In “Source Code Management”, choose “Git”. Put “Repo URL” in “Repository URL” section.
Choose “Credentials” as “none”.
 
Step 7: Choose “Branch” as “develop” in “Branch Specifier”.
 
Step 8: Choose “GitHub hook trigger for GITScm polling” in “Build Triggers”.
 
Step 9: Click on “Apply”. After click on “Apply”, choose “Save”.
 
Step 10: Your “Test Job” has been successfully created.
 
B. Create a “Prod” Job 
Step 1: Click on “New Item”.
 
Step 2: Choose the following options here:
Enter an item name: Prod
Option: Freestyle project
 
Step 3: Choose “Copy from” as “Test”. Click on “Ok”.
 
Step 4: In “General”, put description as “For master branch, publish it”.
 
Step 5: Choose “Label Expression” as “Slave2” in “Restrict where this project can be run”.
 

Step 6: Remain other settings as it is. Choose “Branch Specifier (blank for ‘any’) as “*/master”.
 
Step 7: Click on “Apply” & “Save”.
 
Step 8: “Prod” job has been successfully created.
 
Step 9: Click on “Configure” in “Prod”.
 
Step 10: Click on “Build Steps”.
 
Step 11: Click on “Add build step”.
 
Step 12: Click on “Execute Shell”.
 
Step 13: Put these commands to build a container on port 80:
sudo  docker build . -t finalrelease
sudo docker run -itd -p 80:80 finalrelease
 
Click on “Apply>Save”.
Problem (3) Solution: Code Build should automatically be triggered once a commit is made to master branch or develop branch. 
a. If a commit is made to master branch, test and push to prod 
b. If a commit is made to develop branch, just test the product, do not push to prod 
A. If a commit is made to master branch, test and push to prod 
1. Do change in index.html file
Step 1: Go to “index.html” in “GitHub” account.
 
Step 2: Click on “Edit” Icon & click on “Edit in place”.
 
Step 3: Now, we will replace “Intellipaat” as “Hi Intellipaat”.
 
  
Step 4: Click on “Commit changes”.
 
Step 5: Again, click on “Commit changes”.
 
Step 6: File has been successfully saved.
 
Step 7: Go to “Prod” job & “Build #1” has been successfully created.
 
Step 8: Go to “Console Output”. Container has been successfully created.
 
 
Step 9: Go to “Slave2” & Paste the “Public IP Address” in “Browser Address Bar” & Press “enter” from the keyboard.
 

" 
When we do commit in “index.html” file in “master” branch, we have changed the title from “Intellipaat” to “Hi Intellipaat”, which is successfully shown here. This means when we do commit in “master” branch, container has been successfully published.
Step 10: If you do the changes second time, paste this command “sudo docker rm -f $(sudo docker ps -a -q)” in “execute shell” before these commands:
sudo  docker build . -t finalrelease
sudo docker run -itd -p 80:80 finalrelease
Save the changes.
It will remove the container & again create a new container on the same port.
 
Step 11: Again, we go to “index.html” in “GitHub” account.
 
Step 12: Click on “Edit” Icon & click on “Edit in place”.
 Step 13: Now, we will replace “Hi Intellipaat” as “Intellipaat”.  
Step 14: Click on “Commit changes”.
 
Step 15: Again, click on “Commit changes”.
 
Step 16: File has been successfully saved.
 
Step 17: Again, go to “Prod” job & check the “Build #2”. Build has been successfully created. Click on “#2”.
 
Step 18: Go to “Console Output” & check container is successfully created or not.
 
Container has been successfully created.
Step 19: Again, refresh the browser. You will notice that “Hi, Intellipaat” has been successfully replaced with “Intellipaat” in title.
 
b. If a commit is made to develop branch, just test the product, do not push to prod 
Step 1: Now, we will go to “develop” branch in “GitHub”. Click on ‘+’ sign> Create new file.
k 
Step 2: Put “develop.txt” in “URL” & choose “Description” as “This is a develop file.”
Click on “Commit changes”.
 
Step 3: Again, click on “Commit changes”.
 
Step 4: “develop.txt” has been successfully created.
 
Step 5: Go to “Test” job.
 
Step 6: Click on “#2”.
 
Step 7: “develop.txt” has been successfully created.
 
Click on “Console Output”.
Step 8: “develop.txt” has been successfully created, while Build created.
 
Step 9: Go to “Slave1” & type “cd /home/ubuntu/jenkins/workspace/Test”. Press “enter” from the keyboard.
Do “ls” here. All the “develop” branch file has been shown here.
 
Here, we don’t have published the content & file has been successfully copied to “Slave1”.
-------------------------Capstone Project 1 Completed-----------------------



