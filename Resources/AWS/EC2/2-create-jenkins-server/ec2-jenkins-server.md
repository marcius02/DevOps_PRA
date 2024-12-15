## Jenkins Server Setup on AWS EC2

### Prerequisites

- AWS Account
- Basic Linux knowledge
- EC2 instance access

### 1. Prepare EC2 Instance

```bash
# Update system packages
sudo yum update -y

# Ensure security group allows:
# - Port 22 (SSH)
# - Port 8080 (Jenkins)
```

### 2. Install Java Development Kit (JDK)

```bash
# Install OpenJDK 11
sudo amazon-linux-extras install java-openjdk11 -y

# Verify Java installation
java -version
```

### 3. Configure Jenkins Repository

```bash
# Download Jenkins repository configuration
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

# Import Jenkins GPG key for package verification
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

### 4. Install Supporting Packages

```bash
# Install fontconfig and additional Java dependencies
sudo yum install fontconfig java-11-openjdk -y
```

### 5. Install Jenkins

```bash
# Install Jenkins from configured repository
sudo yum install jenkins -y
```

### 6. Configure Jenkins Service

```bash
# Start Jenkins service
sudo systemctl start jenkins

# Enable Jenkins to start on system boot
sudo systemctl enable jenkins

# Check Jenkins service status
sudo systemctl status jenkins
```

### 7. Configure Firewall (Optional)

```bash
# Open Jenkins port if using firewalld
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload
```

### 8. Initial Jenkins Setup

```bash
# Retrieve initial admin password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### Access Jenkins

- URL: `http://<your-ec2-public-ip>:8080`
- Use initial admin password from previous step
- Follow setup wizard to complete installation

### Recommended Post-Installation Steps

1. Install suggested plugins
2. Create admin user
3. Configure global security
4. Set up authentication

### Troubleshooting

```bash
# Common troubleshooting commands
sudo systemctl status jenkins
sudo journalctl -u jenkins
```

### Security Recommendations

- Use strong, unique passwords
- Enable security groups
- Regularly update Jenkins and plugins
- Use SSH key authentication
- Implement role-based access control


