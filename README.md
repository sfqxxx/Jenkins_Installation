# Jenkins Installation on AWS Ubuntu Instance

This tutorial helps you install Jenkins on an AWS Ubuntu instance.
---
## Prerequisites
- AWS Account
- Basic knowledge of AWS EC2
- SSH client (PuTTY or Command Prompt)
---
## Step 1: Create Ubuntu Instance in AWS

1. Launch a new **Ubuntu** instance in the AWS EC2 console
2. Select an appropriate instance type (t2.micro for testing)

![AWS Instance Creation](https://github.com/user-attachments/assets/2cad837e-7f11-46e4-9be2-2be2fc96399c)
---
## Step 2: Configure Instance Settings
### Key Pair Selection
- Select an existing key pair or create a new one

### Network Settings
- **Important:** Properly configure the Security Group
- For experimentation, you can open all ports (not recommended for production)

![Security Group Configuration](https://github.com/user-attachments/assets/75ac4971-cfc0-43ed-bd61-bc7bcacb2d54)

### Launch Instance
- Review settings and launch the instance

---

## Step 3: Connect to Instance

Connect to your instance using SSH. For detailed connection instructions, refer to the connection tutorial in this repository.

---

## Step 4: Install Java (Prerequisites)

Jenkins requires Java to run. Install OpenJDK 17:

```bash
# Update package list
sudo apt update

# Install OpenJDK 17
sudo apt install openjdk-17-jre

# Verify Java installation
java -version
```

---

## Step 5: Install Jenkins

Run the following commands to install Jenkins:

```bash
# Add Jenkins repository key
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add Jenkins repository
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update package list
sudo apt-get update

# Install Jenkins
sudo apt-get install jenkins
```

---

## Step 6: Configure Security Group for Jenkins

1. Go to **EC2 > Instances**
2. Click on your instance
3. In the bottom tabs, click on **Security**
4. Click on **Security groups**
5. **Add inbound rule:**
   - **Type:** Custom TCP
   - **Port:** 8080
   - **Source:** 0.0.0.0/0 (for testing only)

![Security Group Rules](https://github.com/user-attachments/assets/5405c61f-06ab-46e3-93c8-5659b436297d)

> **Security Note:** For production, restrict the source to specific IP addresses instead of allowing all traffic (0.0.0.0/0).

---

## Step 7: Access Jenkins Web Interface

### Login URL
```
http://YOUR_INSTANCE_PUBLIC_IP:8080
```

**Example:** `http://54.81.161.129:8080`

### Get Initial Admin Password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password and paste it in the Jenkins setup wizard.

![Jenkins Login](https://github.com/user-attachments/assets/5ecafc4b-9e5d-4842-8dcb-ea45acc53990)

---

## Step 8: Complete Jenkins Setup

### Install Plugins
1. Click **"Install suggested plugins"**
2. Wait for plugins to install

![Plugin Installation](https://github.com/user-attachments/assets/5041ba12-4926-4b24-bc3b-9ecb654b291c)

### Create Admin User
1. Fill in the admin user details
2. Click **"Save and Continue"**

![Create Admin User](https://github.com/user-attachments/assets/f189b54f-9322-4db6-8b78-999434f34fa7)

### Success!
Jenkins installation is complete and ready to use.

![Jenkins Success](https://github.com/user-attachments/assets/f9d1bb11-b016-4c41-b9dd-4db5df611f23)

---

## Step 9: Install Docker Pipeline Plugin

### Install Plugin
1. Log in to Jenkins
2. Go to **Manage Jenkins > Manage Plugins**
3. In the **Available** tab, search for **"Docker Pipeline"**
4. Select the plugin and click **Install**
5. **Restart Jenkins** after installation

![Plugin Management](https://github.com/user-attachments/assets/1582610e-5ef6-49c3-9050-cf0b5e242bfa)

![Docker Plugin](https://github.com/user-attachments/assets/74b3a7f9-16af-461b-baac-d5d81330b694)

---

## Step 10: Docker Slave Configuration

### Install Docker

```bash
# Update package list
sudo apt update

# Install Docker
sudo apt install docker.io
```

### Configure Docker Permissions

```bash
# Switch to root user
sudo su -

# Add Jenkins user to docker group
usermod -aG docker jenkins

# Add Ubuntu user to docker group
usermod -aG docker ubuntu

# Restart Docker service
systemctl restart docker
```

### Restart Jenkins
After completing the above steps, restart Jenkins:

```
http://YOUR_INSTANCE_PUBLIC_IP:8080/restart
```

---

## Verification

- ✅ Jenkins is accessible via web browser
- ✅ Docker Pipeline plugin is installed
- ✅ Docker is configured for Jenkins user
- ✅ Security group allows traffic on port 8080

**The Docker agent configuration is now successful!**

---

## Security Recommendations

### For Production Use:
- **Restrict Security Group:** Only allow specific IP addresses
- **Use HTTPS:** Configure SSL/TLS certificates
- **Regular Updates:** Keep Jenkins and plugins updated
- **Backup Configuration:** Regularly backup Jenkins configuration

### Clean Up Test Environment:
- Remove the "All Traffic" inbound rule
- Replace with specific TCP port 8080 rule
- Consider using a VPN or bastion host for access

---

## Troubleshooting

### Common Issues:

**Cannot access Jenkins web interface:**
- Check security group rules
- Verify Jenkins service is running: `sudo systemctl status jenkins`
- Check if port 8080 is listening: `sudo netstat -tlnp | grep 8080`

**Permission denied errors with Docker:**
- Ensure user is in docker group: `groups $USER`
- Restart Jenkins service: `sudo systemctl restart jenkins`

**Plugin installation fails:**
- Check internet connectivity from instance
- Verify Jenkins has proper permissions

---

## Next Steps

Now that Jenkins is installed, you can:
- Create your first pipeline
- Connect to Git repositories
- Set up build automation
- Configure deployment pipelines

For more advanced tutorials, check the other guides in this repository!
