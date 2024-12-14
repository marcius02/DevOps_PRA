# Summary of Jenkins Pipelines

<mark>Pipe 1: Basic Java Application Deployment</mark>

- **Agent**: Any
- **Tools**: JDK 21, Maven 3
- **Stages**:
  - **Checkout**: Clones the repository from GitHub.
  - **Build**: Executes `mvn clean package` to build the application.
  - **Archive**: Archives the built JAR files.
  - **Execute**: Runs the application on port 8088 using `java -jar`.

<mark>Pipe 2: Docker Image Build and Push</mark>

- **Agent**: Any
- **Environment Variables**:
  - `DOCKERHUB_CREDENTIALS`: Docker Hub credentials.
  - `IMAGE_NAME`: Docker image name (`w3564/bookspageable`).
  - `IMAGE_TAG`: Build number as image tag.
- **Tools**: JDK 21, Maven 3
- **Stages**:
  - **Checkout**: Clones the repository from GitHub.
  - **Build**: Executes `mvn clean package`.
  - **Build Docker Image**: Builds a Docker image using the Maven artifact.
  - **Push to DockerHub**: Pushes the built image to Docker Hub.

<mark>Pipe 3: ECR Deployment for Frontend</mark>

- **Agent**: Any
- **Environment Variables**:
  - `DOCKERHUB_IMAGE`: Docker Hub image name.
  - `ECR_REGISTRY`: AWS ECR public registry.
  - `ECR_REPOSITORY`: ECR repository name.
  - `IMAGE_TAG`: Build number as image tag.
- **Stages**:
  - **Pull from DockerHub**: Pulls the latest frontend image from Docker Hub.
  - **Tag for ECR**: Tags the image for ECR deployment.
  - **Push to ECR**: Pushes the tagged image to AWS ECR.

<mark>Pipe 4: Combined Docker and ECR Deployment</mark>

- **Agent**: Any
- **Environment Variables**:
  - `DOCKERHUB_CREDENTIALS`: Docker Hub credentials.
  - `IMAGE_NAME`: Docker image name (`w3564/bookspageable`).
  - `IMAGE_TAG`: Build number as image tag.
  - `ECR_REGISTRY`: AWS ECR public registry.
  - `ECR_REPOSITORY`: ECR repository name.
- **Tools**: JDK 21, Maven 3
- **Stages**:
  - **Checkout**: Clones the repository from GitHub.
  - **Build**: Executes `mvn clean package`.
  - **Build Docker Image**: Builds a Docker image using the Maven artifact.
  - **Push to DockerHub**: Pushes the built image to Docker Hub.
  - **Push to ECR**: Tags and pushes the image to AWS ECR.
