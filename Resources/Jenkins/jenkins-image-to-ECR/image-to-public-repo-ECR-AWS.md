# Jenkins pipe: image push to ECR

To create a Jenkins pipeline stage for **pushing a Docker image to an AWS ECR public repository,** follow these steps:

1. **Set Up AWS Credentials in Jenkins**:
   
   - Go to `Manage Jenkins` > `Credentials` > `System` > `Global credentials` and add your <mark>AWS Access Key ID and Secret Access Key</mark>.

2. **Install Required Plugins**:
   
   - Install the Amazon ECR plugin from the Jenkins Plugin Manager to handle Docker authentication tokens.

3. **Install AWS CLI:**
   
   - Access the Jenkins container and once inside the container, run the following commands to install the AWS CLI, <mark>.after exiting and restarting</mark>.:

```groovy
      apt-get update
      apt-get install -y unzip curl
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      ./aws/install
      aws --version
```

  

5. **Pipeline Configuration**:
   
   - Use the following pipeline script:

```groovy
pipeline {
    environment {
        DOCKERHUB_IMAGE = 'w3564/books-frontend:latest'
        ECR_REGISTRY = 'public.ecr.aws/s4x3q8t5'
        ECR_REPOSITORY = 'books/books-frontend'
        IMAGE_TAG = "${BUILD_NUMBER}"
        AWS_REGION = 'us-east-1' // Public ECR repositories are only available in us-east-1
    }

    agent any

    stages {
        stage('Pull from DockerHub') {
            steps {
                sh "docker pull ${DOCKERHUB_IMAGE}"
            }
        }

        stage('Tag for ECR') {
            steps {
                sh "docker tag ${DOCKERHUB_IMAGE} ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                sh "docker tag ${DOCKERHUB_IMAGE} ${ECR_REGISTRY}/${ECR_REPOSITORY}:latest"
            }
        }

        stage('Push to ECR') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'awscredentials_id']]) {
                    sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                    sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:latest"
                }
            }
        }
    }

    post {
        always {
            sh "docker rmi ${DOCKERHUB_IMAGE}"
            sh "docker rmi ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
            sh "docker rmi ${ECR_REGISTRY}/${ECR_REPOSITORY}:latest"
        }
    }
}
```

<mark>Note</mark>:

- The `AWS_REGION` is set to <mark>'us-east-1'</mark> as public ECR repositories are only available in this region. Adjust if necessary.
- Make sure the <mark>'awscredentials_id' </mark>is correctly set up in your Jenkins credentials.
- The pipeline uses the AWS CLI for ECR authentication, so ensure it's installed on your Jenkins server.

**Summary**

This pipeline does the following:

1. Pulls the specified image from Docker Hub.
2. Tags the pulled image for the ECR repository.
3. Authenticates with ECR using the AWS credentials stored in Jenkins.
4. Pushes the tagged images to the ECR repository.
5. Cleans up by removing all the Docker images used in the process.
