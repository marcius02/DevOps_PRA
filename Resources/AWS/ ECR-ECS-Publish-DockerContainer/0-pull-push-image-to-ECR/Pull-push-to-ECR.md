# Pull image and push to ECR

## CloudShell AWS

**What is AWS CloudShell?**

> AWS CloudShell is a browser-based, pre-authenticated shell that provides immediate access to AWS tools and resources directly from the AWS Management Console. It eliminates the need for local CLI configuration and provides a consistent, secure environment for managing AWS services.

**Key Features**

- 🌐 Browser-based terminal
- 🔒 Pre-authenticated with your AWS credentials
- 💻 Pre-installed AWS CLI, SDKs, and developer tools
- 📂 1 GB persistent storage
- 🌍 Available in multiple regions
- 🔧 Supports Bash, PowerShell, and Z shell

### Create a cloudshell

To create and connect to AWS CloudShell, follow these steps:

1. Sign in to the` AWS Management Console` at [cloudshell](console.aws.amazon.com).

2. Launch AWS `CloudShell`:
   
   - Click on the <mark>AWS CloudShell icon in the navigation bar</mark>, or
   - Type "cloudshell" in the search box and select the CloudShell option.

3. Select a Region:
   
   - Use the "Select a Region" menu to choose a supported AWS Region.

4. Choose a shell:
   
   - By default,<mark> you'll be in Bash</mark>.
   
   - To switch shells, type the shell name at the command prompt:
     
     ```bash
     bash    # for Bash
     pwsh    # for PowerShell
     zsh     # for Z shell
     ```

5. Start using CloudShell:
   
   - You now have access to a pre-configured environment with AWS CLI, SDKs, and other tools
   - You have <mark>1 GB of persistent storage in your home directory</mark>.

Remember that files in persistent storage are only accessible in the same AWS Region.

## Pull and Push

These commands allow you to transfer an image<mark> from DockerHub to Amazon ECR,</mark> enabling seamless integration with AWS services.

1. Pull a Docker image from DockerHub using `docker pull`.
2. Authenticate to Amazon ECR using AWS CLI and Docker login.
3. Tag the pulled image for ECR using `docker tag`.
4. Push the tagged image to ECR using `docker push`.



1. <mark>Pull the image from DockerHub:</mark>
   
   ```bash
   docker pull w3564/books:latest
   ```
   
   This command downloads the specified image from DockerHub. It's useful when you want to use or modify an existing image.

2. <mark>Authenticate</mark> to Amazon ECR:
   
   ```bash
   aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws
   ```
   
   This step is necessary to gain access to your ECR repository. It uses the AWS CLI to obtain an authentication token, which is then passed to Docker for login.

3. Tag the Docker image for ECR:
   
   ```bash
   docker tag w3564/books:latest public.ecr.aws/s4x3q8t5/books/books:latest
   ```
   
   Tagging is required to associate your local image with the ECR repository. It helps in version control and identification of images.

4. <mark>Push the image to ECR:</mark>
   
   ```bash
   docker push public.ecr.aws/s4x3q8t5/books/books:latest
   ```
   
   This command uploads your tagged image to the specified ECR repository.

## Why use ECR?

1. **Integration with AWS services**: ECR integrates seamlessly with other AWS services like ECS and EKS, making it easier to deploy containerized applications.

2. **Security: ECR** provides secure, private repositories with access controls through IAM.

3. **Scalability**: As part of AWS infrastructure, ECR can handle large-scale deployments efficiently.

4. **Redundancy**: By pushing to both DockerHub and ECR, you create redundancy in your image storage, ensuring availability.
