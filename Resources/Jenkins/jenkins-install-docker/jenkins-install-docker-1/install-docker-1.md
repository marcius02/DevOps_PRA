Access the Jenkins container:

`docker exec -it jenkins bash`

Install Docker inside the container:

`curl https://get.docker.com/ > dockerinstall && chmod 777 dockerinstall && ./dockerinstall`

dd the Jenkins user to the Docker group:

`usermod -aG docker jenkins`

If you still encounter permission issues, you may need to change the permissions of the Docker socket:

`chmod 666 /var/run/docker.sock`

Restart the Jenkins container:

`docker restart jenkins2`

---------------

The installation process completed successfully, and Docker is now operational within the container. The system also provided recommendations for running Docker as a non-privileged user and warnings about API access security.

1. Essential packages were installed, including ca-certificates and curl.

2. Docker's GPG key was downloaded and added to the system's keyring.

3. Docker's repository was added to the system's sources list.

4. Docker packages were installed, including docker-ce, docker-ce-cli, containerd.io, and related tools.

5. Docker version information was displayed, confirming the installation:
   
   - Client version: 27.3.1
   - Server version: 27.2.0
+ sh -c DEBIAN_FRONTEND=noninteractive apt-get -y -qq install ca-certificates curl >/dev/null
+ sh -c install -m 0755 -d /etc/apt/keyrings
+ sh -c curl -fsSL "https://download.docker.com/linux/debian/gpg" -o /etc/apt/keyrings/docker.asc
+ sh -c chmod a+r /etc/apt/keyrings/docker.asc
+ sh -c echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian bookworm stable" > /etc/apt/sources.list.d/docker.list
+ sh -c apt-get -qq update >/dev/null
+ sh -c DEBIAN_FRONTEND=noninteractive apt-get -y -qq install docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-ce-rootless-extras docker-buildx-plugin >/dev/null
  debconf: delaying package configuration, since apt-utils is not installed
+ sh -c docker version
  Client: Docker Engine - Community
  Version:           27.3.1
  API version:       1.47
  Go version:        go1.22.7
  Git commit:        ce12230
  Built:             Fri Sep 20 11:41:11 2024
  OS/Arch:           linux/amd64
  Context:           default

Server:
 Engine:
  Version:          27.2.0
  API version:      1.47 (minimum version 1.24)
  Go version:       go1.21.13
  Git commit:       3ab5c7d
  Built:            Fri Sep  6 19:08:45 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.7.21
  GitCommit:        472731909fa34bd7bc9c087e4c27943f9835f111
 runc:
  Version:          1.1.13
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

================================================================================

To run Docker as a non-privileged user, consider setting up the
Docker daemon in rootless mode for your user:

    dockerd-rootless-setuptool.sh install

Visit https://docs.docker.com/go/rootless/ to learn about rootless mode.

To run the Docker daemon as a fully privileged service, but granting non-root
users access, refer to https://docs.docker.com/go/daemon-access/

WARNING: Access to the remote API on a privileged Docker daemon is equivalent
         to root access on the host. Refer to the 'Docker daemon attack surface'
         documentation for details: https://docs.docker.com/go/attack-surface/

================================================================================

root@e492324d654e:/#


pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_id')
        IMAGE_NAME = 'w3564/bookspageable'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    tools {
        maven 'M3'
        jdk 'JDK21'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/AlbertProfe/BooksPageable.git'
            }
        }
    
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it with both BUILD_NUMBER and latest
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} -t ${IMAGE_NAME}:latest ."
                }
            }
        }
    
        stage('Push to DockerHub') {
            steps {
                script {
                    // Login to DockerHub
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
    
                    // Push both the specific tag and the latest tag
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
    }
    
    post {
        always {
            // Logout from DockerHub and remove images
            sh "docker logout"
            sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
            sh "docker rmi ${IMAGE_NAME}:latest || true"
        }
    }

}
