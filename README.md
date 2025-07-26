**# Jenkins_Installation
This tutorial Helps you to install Jenkins in AWS Instance (Ubuntu)**

**1) Create Instance Ubuntu instance in the AWS.**
2) <img width="940" height="763" alt="image" src="https://github.com/user-attachments/assets/2cad837e-7f11-46e4-9be2-2be2fc96399c" />

**2) Select the Key Pair.
3) Network Settings ,Make sure you properly configured the Security Group.**
   <img width="940" height="699" alt="image" src="https://github.com/user-attachments/assets/75ac4971-cfc0-43ed-bd61-bc7bcacb2d54" />

   If you're just experimenting just open the ports and just play.
   Launch the instance.

**4) Connect the instance, You can check my repo for Connecting the instance. Detailed Guide is there.**

Install Jenkins.
Pre-Requisites:
•	Java (JDK)
Run the below commands to install Java and Jenkins
$ install Java

$sudo apt update
$sudo apt install openjdk-17-jre

Verify Java is Installed
$java -version

**5) Now, you can proceed with installing Jenkins**
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

**6) EC2 > Instances > Click on**
In the bottom tabs -> Click on Security
Security groups
Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed All traffic).
<img width="1617" height="374" alt="image" src="https://github.com/user-attachments/assets/5405c61f-06ab-46e3-93c8-5659b436297d" />

**Login to Jenkins using the below URL:**

http:// ip address:8080 
Eg : http://54.81.161.129:8080/

Note: If you are not interested in allowing All Traffic to your EC2 instance 
1. Delete the inbound traffic rule for your instance
2. Edit the inbound traffic rule to only allow custom TCP port 8080

After you login to Jenkins, 
- Run the command to copy the Jenkins Admin Password
- - sudo cat /var/lib/jenkins/secrets/initialAdminPassword
- - Enter the Administrator password

<img width="940" height="434" alt="image" src="https://github.com/user-attachments/assets/5ecafc4b-9e5d-4842-8dcb-ea45acc53990" />

**Click on Install Plugins**
<img width="940" height="434" alt="image" src="https://github.com/user-attachments/assets/5041ba12-4926-4b24-bc3b-9ecb654b291c" />

**Create Admin User,**
<img width="1980" height="1232" alt="image" src="https://github.com/user-attachments/assets/f189b54f-9322-4db6-8b78-999434f34fa7" />

**Jenkins Installation is Successful. You can now starting using the Jenkins**
<img width="1980" height="1232" alt="image" src="https://github.com/user-attachments/assets/f9d1bb11-b016-4c41-b9dd-4db5df611f23" />

**Install the Docker Pipeline plugin in Jenkins:**
•	Log in to Jenkins.
•	Go to Manage Jenkins > Manage Plugins.
<img width="1511" height="529" alt="image" src="https://github.com/user-attachments/assets/1582610e-5ef6-49c3-9050-cf0b5e242bfa" />
•	In the Available tab, search for "Docker Pipeline".
•	Select the plugin and click the Install button.
<img width="940" height="86" alt="image" src="https://github.com/user-attachments/assets/74b3a7f9-16af-461b-baac-d5d81330b694" />
•	Restart Jenkins after the plugin is installed.

**Docker Slave Configuration**

Run the below command to Install Docker

sudo apt update
sudo apt install docker.io
Grant Jenkins user and Ubuntu user permission to docker deamon.
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
Once you are done with the above steps, it is better to restart Jenkins.

http://<ec2-instance-public-ip>:8080/restart
The docker agent configuration is now successful.









