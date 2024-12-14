To resolve the **permission denied error and start the Jenkins container with the correct rights** to execute Docker commands, follow these steps:

1. Stop the current Jenkins container if it's running:

```bash
docker stop jenkins2
```

2. Remove the existing container:

```bash
docker rm jenkins2
```

3. Start a new Jenkins container with the Docker socket mounted and the correct permissions:

```bash
docker run -d --name jenkins2 -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -u root \
  jenkins/jenkins:lts
```

4. After the container starts, exec into it:

```bash
docker exec -it jenkins2 bash
```

5. Inside the container, add the jenkins user to the docker group:

```bash
groupadd -f docker
usermod -aG docker jenkins
```

6. Change the ownership of the Docker socket:

```bash
chown root:docker /var/run/docker.sock
```

7. Exit the container and restart it or <mark>wait and do it at end, when docker is installed:</mark>

```bash
exit
docker restart jenkins2
```

8. To install Docker within the Jenkins container using bash, you can execute the following commands:
   
   ```bash
   #!/bin/bash
   apt-get update
   apt-get install -y \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
   curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   echo \
     "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
     $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
   apt-get update
   apt-get install -y docker-ce docker-ce-cli containerd.io
   usermod -aG docker jenkins
   ```
   
   After running these commands, restart the Jenkins service to apply the changes. This script updates the system, adds the Docker repository, installs Docker, and adds the Jenkins user to the Docker group.
   
   
   
   
