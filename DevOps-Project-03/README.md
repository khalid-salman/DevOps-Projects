# Deploy a Django Application on AWS Using ECS and ECR

![AWS](https://imgur.com/wLMcRHS.jpg)

**This guide walks you through deploying a Django-based application on AWS using Elastic Container Service (ECS) and Elastic Container Registry (ECR). We'll start by creating a Docker image of the application, pushing it to ECR, and then deploying it on AWS using ECS. Finally, we'll verify that the application is running correctly using Django’s built-in web server.**

## Prerequisites

- Basic knowledge of **Django**
- Familiarity with **Docker**
- An **AWS Account**
- A creative mindset is always helpful! 😃

## Django Web Framework

**Django** is a high-level Python web framework that promotes rapid development and clean, pragmatic design. It is open-source, has a vibrant community, excellent documentation, and offers both free and paid support options. Django uses HTML/CSS/JavaScript for the frontend and Python for the backend.

## Understanding Docker and Containers

![Docker](https://imgur.com/raGErLx.png)

### Docker Workflow

**Docker** is an open platform for developing, shipping, and running applications. It virtualizes the operating system of the host machine, allowing applications to run in isolated environments called **containers**. A container is a runnable instance of a Docker image. You can create, start, stop, move, or delete containers using the Docker API or CLI. Containers can be connected to networks, have storage attached, and even be used to create new Docker images.

## What is AWS Elastic Container Registry (ECR)?

**Amazon Elastic Container Registry (ECR)** is a managed container image registry service. It allows you to push, pull, and manage Docker images using the Docker CLI or other preferred clients. ECR provides a secure, scalable, and reliable registry for your Docker images.

### Steps to Use ECR

1. **Create a Dockerfile**: Add a `Dockerfile` to your Django application. This file contains the commands needed to build the Docker image.

2. **Build the Docker Image**: Use the following command to create a Docker image named `django-app:version-1`.

   ```bash
   docker build -t hello-world-django-app:version-1 .
   ```

3. **Verify the Docker Image**: Check if the Docker image was created successfully using:

   ```bash
   docker images | grep hello-world-django-app
   ```

4. **Create a Repository on AWS ECR**:
   - Open the AWS Management Console and search for ECR.
   - Click on **Create Repository**.
   - Choose between **Private** or **Public** visibility. Private repositories are managed by IAM and repository policies.
   - Name your repository and enable the **Scan on Push** option to identify software vulnerabilities in your container images.

5. **Push the Docker Image to ECR**:
   - Authenticate your Docker client to the ECR registry. Use the following commands to export your AWS credentials:

     ```bash
     export AWS_ACCESS_KEY_ID=******
     export AWS_SECRET_ACCESS_KEY=******
     ```

   - Log in to your AWS account using:

     ```bash
     aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
     ```

   - Tag your Docker image with the ECR registry and repository name:

     ```bash
     docker tag 480903dd8 aws_account_id.dkr.ecr.region.amazonaws.com/hello-world-django-app
     ```

   - Push the Docker image to ECR:

     ```bash
     docker push aws_account_id.dkr.ecr.region.amazonaws.com/hello-world-django-app
     ```

## What is AWS Elastic Container Service (ECS)?

**Amazon Elastic Container Service (ECS)** is a scalable container management service that supports Docker containers. It allows you to run applications on a managed cluster of Amazon EC2 instances. ECS simplifies the deployment, operation, and scaling of applications by handling cluster management. You can launch and stop Docker-enabled applications, query cluster logs, and use features like security groups, Elastic Load Balancer, EBS volumes, and IAM roles.

### Steps to Use ECS

1. **Create a Cluster**:
   - Open the AWS Management Console and search for ECS.
   - Click on **Create Cluster** and configure the cluster settings, such as region, network configuration, and CloudWatch Container Insights.

2. **Launch an EC2 Instance**:
   - Configure the cluster by setting up network configurations, Auto Scaling groups, and other necessary settings. Note that some configurations cannot be changed after the cluster is created.

3. **Create a Service**:
   - Define how your ECS service will run by specifying parameters like cluster, launch type, and task definition.

4. **Create a Task Definition**:
   - Define the task that will run your Docker containers. Specify the ECR repository, container settings, and port mappings.

5. **Run the Task**:
   - Trigger the task to launch the EC2 instance. You can verify the instance status in the EC2 console.

## Congratulations! 🎉

**You have successfully deployed your Django application on AWS using ECS and ECR.**

To verify the deployment, navigate to the public DNS of the instance in your browser and check if the Django application is running correctly.

### Additional Considerations for Production

When deploying a Django application in a production environment, consider the following factors:

- **Security**: Implement proper security measures to protect your application.
- **Monitoring**: Set up monitoring tools to track application performance.
- **Load Balancing**: Use load balancers to distribute traffic efficiently.
- **Recovery Plans**: Have backup and recovery plans in place.

For a more streamlined deployment process, you can also use **AWS Elastic Beanstalk**, which simplifies the deployment of Django applications.

**Happy Learning!**

# Hit the Star! ⭐

**If you find this repository helpful for learning, please give it a star. Thank you!**

#### Author by [Khalid Salman](https://github.com/khalid-salman)