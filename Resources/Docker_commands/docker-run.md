

# docker run

```bash
docker run -d -p 8080:8080 -p 50000:50000
-v jenkins_home:/var/jenkins_home
-v /var/run/docker.sock:/var/run/docker.sock
--name jenkins
jenkins/jenkins:lts
```



This command creates and runs a new Jenkins container:

- `-d`: Runs the container in detached mode (in the background)
- `-p 8080:8080`: Maps port 8080 from the container to port 8080 on the host
- `-p 50000:50000`: Maps port 50000 from the container to port 50000 on the host
- `-v jenkins_home:/var/jenkins_home`: Creates a volume named jenkins_home and mounts it to /var/jenkins_home in the container
- `-v /var/run/docker.sock:/var/run/docker.sock`: Mounts the Docker socket from the host to the container
- `--name jenkins`: Names the container "jenkins"
- `jenkins/jenkins:lts`: Specifies the Jenkins LTS image to use
