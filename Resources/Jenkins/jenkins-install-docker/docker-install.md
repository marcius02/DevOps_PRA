# Jenkins Pipeline with Docker

## Add dockerfile to root/project

https://spring.io/guides/gs/spring-boot-docker

## Volumes

```bash
docker run --name jenkins-blueocean --restart=on-failure --detach  
--network jenkins --env DOCKER_HOST=tcp://docker:2376  
--env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1  
--publish 8080:8080 --publish 50000:50000  
--volume jenkins-data:/var/jenkins_home  
--volume jenkins-docker-certs:/certs/client:ro  
myjenkins-blueocean:2.462.3-1
```

## Tools

- Go to **Manage Jenkins** > **Configure System**.
- Scroll down to the **Docker** section and add your Docker host details if needed. If you're using the default local Docker setup, you may not need to change anything.

## Plugins

Install the Docker plugin and Docker Pipeline plugin in Jenkins

## Set variables (optional)

1. **Set Environment Variables**:
   
   - Still in **Manage Jenkins** > **Configure System**, add an environment variable for Docker if needed:
     
     - Variable Name: `PATH+EXTRA`
     - Value: `/usr/local/bin` (or wherever your Docker binary is located).

## Credentials

To add Docker Hub credentials to Jenkins:

1. Go to "Manage Jenkins" > "Manage Credentials"

2. Click on "Jenkins" under "Stores scoped to Jenkins"

3. Click "Global credentials (unrestricted)"

4. Click "Add Credentials"

5. Fill out the form:
   
   - Kind: Username with password
   - Scope: Global
   - Username: Your Docker Hub username
   - Password: Your Docker Hub access token (not your account password)
   - ID: dockerhub_id (or another memorable ID)
   - Description: Docker Hub credentials

## Permissions (optional)

Ensure the Jenkins user has permissions to access the Docker daemon:

text

`sudo usermod -aG docker jenkins`
