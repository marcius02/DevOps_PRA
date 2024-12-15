# Summary of How to Integrate Jenkins With GitHub



> To integrate Jenkins with GitHub, bringing together these powerful tools 
> for an efficient and automated CI CD pipeline we need a webhook in Github and configure the pipeline.



## Prerequisites

1. Create an Ubuntu 20.04 EC2 Instance on your AWS Account, If you don’t have an AWS Account, you can create a free tier account.  
   Or
   create an Ubuntu 20.04 machine on a cloud platform of your preference. 
   Keep in mind that the steps for software installation and accessing the 
   machine may vary in this scenario.
2. Open ports 22, 80, and 8080 for incoming connections on the designated EC2 Instance or chosen machine.
3. Create a mandatory GitHub account.
4. Install Jenkins, Java, Node.js, and Git on the specified EC2 Instance or machine.
5. Initialize Jenkins with the Initial Password during the setup process.
6. Install Docker for containerized sample application deployment.
7. Create a GitHub Repository by cloning sample code from a reference repository, and create a Personal Access Token.

## Integrate Jenkins With GitHub

1. Configure a Jenkins GitHub Webhook within the GitHub Repository for seamless integration with Jenkins.
2. Create a Jenkins Pipeline, integrating the GitHub Repository into the pipeline structure.
3. Validate the Jenkins Pipeline’s auto-trigger feature by committing changes to the connected GitHub Repository.

### Links

[Jenkins with GitHub: How to Integrate Them](https://www.clickittech.com/devops/jenkins-with-github/)


