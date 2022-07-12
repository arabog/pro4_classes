Creating an EKS Cluster
Before we move further, note that there are multiple ways to create/delete an AWS EKS cluster, such as:

EKS web-console
CloudFormation console
EKSCTL utility - Recommended way
Create an EKS Cluster using the EKSCTL utility
1. Create command
Let's use eksctl and kubectl utilities to create a Kubernetes cluster in the AWS EKS. Run the following command to create the EKS cluster:

eksctl create cluster --name eksctl-demo --region=us-east-1 [--profile <profile-name>]
OR:
eksctl create cluster --name eksctl-demo

The command above will take a few minutes to execute, and create an EKS cluster "eksctl-demo" in the "us-east-1" region with:

One node group containing two m5.large nodes

Two subnets in separate availability zones

Two separate CloudFormation stacks - eksctl-eksctl-demo-cluster for the cluster, and eksctl-eksctl-demo-nodegroup-ng-7f9854c3 for the initial nodegroup

For more information on creating and managing clusters, refer here.
https://eksctl.io/usage/creating-and-managing-clusters/

View progress
Go to the CloudFormation console to view progress. If you don’t see any progress, be sure that you are viewing clusters in the same region that they are being created. For example, if eksctl is using region us-east-2, you’ll need to set the region to US East (Ohio) in the dropdown menu in the upper right of the console.

In case of issues, you can try:

eksctl utils describe-stacks --region=us-east-2 --cluster=eksctl-demo [--profile <profile-name>]

View details
Once the status is CREATE_COMPLETE in the CloudFormation web-console, fetching the details of the newly created cluster using:
eksctl get cluster --name=eksctl-demo --region=us-east-2 [--profile <profile-name>]
Go back to the CloudFormation web console and select the eksctl-eksctl-demo-cluster stack. Select the tab Template, this shows you the Cloudformation template that eksctl command used to create your EKS cluster. The Cloudformation template describes all of the resources and connections between them that are needed for a cluster. We will discuss Cloudformation templates more later in the course.
Click the View in Designer button. This will give you a visual representation of the resources that make up the stack created. Close and leave the designer

You can also check the health of your clusters nodes using the command:

kubectl get nodes  [--profile <profile-name>]
Ask yourself: Did you see the difference in the usage of eksctl vs kubectl? The former is used to create/delete/edit a cluster, whereas the latter is used to interact with the cluster.

4. Deploy a Sample App
Let's deploy a sample Python "Hello World" app to the kubernetes cluster.

# Assuming you have already cloned the course repo as
git clone git clone https://github.com/udacity/DevOps_Microservices.git
# Move to the exercise folder if you want to write Dockerfile from scratch
cd DevOps_Microservices/Lesson-3-Containerization/python-helloworld
Once you are in the right folder, run these commands and read the inline comments carefully.

# Ensure Docker Desktop is running locally
docker --version
# Build an image using the Dockerfile in the current directory
docker build -t python-helloworld .
docker images
# Run a container
docker run -d -p 5000:5000 python-helloworld
# Check the output at http://localhost:5000/ or http://0.0.0.0:5000/ or http://127.0.0.1:5000/
docker ps
# Now, stop the container.
# Tag locally before pushing to the Dockerhub
# We have used a sample Dockerhub profile /sudkul
# Replace sudkul/ with your Dockerhub profile
docker tag python-helloworld sudkul/python-helloworld:v1.0.0
docker images
# Log into the Dockerhub from your local terminal
docker login
# Replace sudkul/ with your Dockerhub profile
docker push sudkul/python-helloworld:v1.0.0
# Check the image in your Dockerhub online at https://hub.docker.com/repository/docker/sudkul/python-helloworld

Once your Docker image is publicly available, you can deploy it to the kubernetes cluster.

# Assuming the Kubernetes cluster is ready
kubectl get nodes
# Deploy an App from the Dockerhub to the Kubernetes Cluster
kubectl create deploy python-helloworld --image=sudkul/python-helloworld:v1.0.0
# See the status
kubectl get deploy,rs,svc,pods
# Port forward 
kubectl port-forward pod/python-helloworld-84857d9565-2598m --address 0.0.0.0 5000:5000

Access the app locally at http://127.0.0.1:5000/

Delete the cluster
You can delete the cluster either from the CloudFormation web-console, or by using the EKSCTL command. Choose any one option from below:

From the CloudFormation web-console, select your stack and choose delete from the actions menu
Delete using the EKSCTL:
eksctl delete cluster --region=us-east-2 --name=eksctl-demo [--profile <profile-name>]

Using the eksctl delete cluster command to delete the cluster and all the associated resources.

Q: Which command creates an EKS cluster with default values, including nodes and vpc?
eksctl create cluster

Eksctl is designed to simply cluster creation by creating not only the EKS control layer, but also resources the cluster needs to function such as nodes and networking.

Q: Which command would you use to get the status of the nodes in your cluster?
kubectl get nodes

Kubectl is used to communicate with a clusters control(master) system. It allows you to get the status of nodes, pods, and services

Q: How do you delete a cluster along with all of its associated resources?
Delete its stack in the CloudFormation console
Delete its stack using eksctl delete cluster

It is important when deleting a cluster to remove all of its associated resources, this can be done in the CloudFormation console or using the eksctl command line tool.

Creating a EKS Cluster: Key Points
eksctl makes it easy to create a cluster and all of the resources needed to use it
kubectl communicates the clusters master system and can be used to interact with nodes, pods, and services
When deleting a cluster, use eksctl delete or the CloudFormation console to avoid leaving dangling resources

Creating an EKS Cluster
Before we move further, note that there are multiple ways to create/delete an AWS EKS cluster, such as:

EKS web-console
CloudFormation console
EKSCTL utility - Recommended way
Create an EKS Cluster using the EKSCTL utility
1. Create command
Let's use eksctl and kubectl utilities to create a Kubernetes cluster in the AWS EKS. Run the following command to create the EKS cluster:

eksctl create cluster --name eksctl-demo --region=us-east-2 [--profile <profile-name>]
The command above will take a few minutes to execute, and create an EKS cluster "eksctl-demo" in the "us-east-1" region with:

One node group containing two m5.large nodes

Two subnets in separate availability zones

Two separate CloudFormation stacks - eksctl-eksctl-demo-cluster for the cluster, and eksctl-eksctl-demo-nodegroup-ng-7f9854c3 for the initial nodegroup

For more information on creating and managing clusters, refer here.

2. View progress
Go to the CloudFormation console to view progress. If you don’t see any progress, be sure that you are viewing clusters in the same region that they are being created. For example, if eksctl is using region us-east-2, you’ll need to set the region to US East (Ohio) in the dropdown menu in the upper right of the console.

In case of issues, you can try:

eksctl utils describe-stacks --region=us-east-2 --cluster=eksctl-demo [--profile <profile-name>]
Screenshot showing how to verify the region in the AWS web console
Verify the region in the web-console.

Screenshot showing how to verify the region in the command line with the output from creating the cluster with the eksctl command. The line starting with "creating EKS cluster" shows the cluster name as well as the region.
Verify the region shown in the command line

Screenshot of CloudFormation on the AWS web console showing stacks created with eksctl command
CloudFormation stacks created as a part of EKSCTL command

3. View details
Once the status is CREATE_COMPLETE in the CloudFormation web-console, fetching the details of the newly created cluster using:
eksctl get cluster --name=eksctl-demo --region=us-east-2 [--profile <profile-name>]
Go back to the CloudFormation web console and select the eksctl-eksctl-demo-cluster stack. Select the tab Template, this shows you the Cloudformation template that eksctl command used to create your EKS cluster. The Cloudformation template describes all of the resources and connections between them that are needed for a cluster. We will discuss Cloudformation templates more later in the course.
Click the View in Designer button. This will give you a visual representation of the resources that make up the stack created. Close and leave the designer.
Screenshot of web console showing template used by eksctl to create CloudFormation stack
EKSCTL internally uses the template above to create the CloudFormation stack.

You can also check the health of your clusters nodes using the command:

kubectl get nodes  [--profile <profile-name>]
Ask yourself: Did you see the difference in the usage of eksctl vs kubectl? The former is used to create/delete/edit a cluster, whereas the latter is used to interact with the cluster.

4. Deploy a Sample App
Let's deploy a sample Python "Hello World" app to the kubernetes cluster.

# Assuming you have already cloned the course repo as
git clone git clone https://github.com/udacity/DevOps_Microservices.git
# Move to the exercise folder if you want to write Dockerfile from scratch
cd DevOps_Microservices/Lesson-3-Containerization/python-helloworld
Once you are in the right folder, run these commands and read the inline comments carefully.

# Ensure Docker Desktop is running locally
docker --version
# Build an image using the Dockerfile in the current directory
docker build -t python-helloworld .
docker images
# Run a container
docker run -d -p 5000:5000 python-helloworld
# Check the output at http://localhost:5000/ or http://0.0.0.0:5000/ or http://127.0.0.1:5000/
docker ps
# Now, stop the container.
# Tag locally before pushing to the Dockerhub
# We have used a sample Dockerhub profile /sudkul
# Replace sudkul/ with your Dockerhub profile
docker tag python-helloworld sudkul/python-helloworld:v1.0.0
docker images
# Log into the Dockerhub from your local terminal
docker login
# Replace sudkul/ with your Dockerhub profile
docker push sudkul/python-helloworld:v1.0.0
# Check the image in your Dockerhub online at https://hub.docker.com/repository/docker/sudkul/python-helloworld
Once your Docker image is publicly available, you can deploy it to the kubernetes cluster.

# Assuming the Kubernetes cluster is ready
kubectl get nodes
# Deploy an App from the Dockerhub to the Kubernetes Cluster
kubectl create deploy python-helloworld --image=sudkul/python-helloworld:v1.0.0
# See the status
kubectl get deploy,rs,svc,pods
# Port forward 
kubectl port-forward pod/python-helloworld-84857d9565-2598m --address 0.0.0.0 5000:5000
Access the app locally at http://127.0.0.1:5000/

Note that the Kubernetes commands above are also valid if you are running the cluster locally.

Deploying an App to the Kubernetes cluster
Deploying an App to the Kubernetes cluster

View the output in your local browser at http://127.0.0.1:5000/
View the output in your local browser at http://127.0.0.1:5000/

5. Delete the cluster
You can delete the cluster either from the CloudFormation web-console, or by using the EKSCTL command. Choose any one option from below:

From the CloudFormation web-console, select your stack and choose delete from the actions menu
Delete using the EKSCTL:
eksctl delete cluster --region=us-east-2 --name=eksctl-demo [--profile <profile-name>]
Note: We recommend you shut down every resource (e.g., EC2 instances, CloudFormation stack, or any other hosted service) on the AWS cloud immediately after the usage; otherwise, you will be billed even if the resources are not in "actual" use.

Using the `eksctl delete cluster` command to delete the cluster and all the associated resources. 
Using the eksctl delete cluster command to delete the cluster and all the associated resources.

QUESTION 1 OF 3
Which command creates an EKS cluster with default values, including nodes and vpc?

eksctl create cluster



QUESTION 2 OF 3
Which command would you use to get the status of the nodes in your cluster?


kubectl get nodes


QUESTION 3 OF 3
How do you delete a cluster along with all of its associated resources?


Delete its stack in the CloudFormation console

Delete its stack using eksctl delete cluster

Delete Cluster Task
Delete the EKS Cluster

Task List

Creating a EKS Cluster: Key Points
eksctl makes it easy to create a cluster and all of the resources needed to use it
kubectl communicates the clusters master system and can be used to interact with nodes, pods, and services
When deleting a cluster, use eksctl delete or the CloudFormation console to avoid leaving dangling resources
Summary of the CLI tools
Now that you have now been introduced to three command-line tools that you can use to work with AWS EKS services:

AWSCLI: This tool allows you to interact with a wide variety of AWS services, not just EKS. Although there are aws commands to create or modify EKS services, this is a much more manual approach than using the other options.
eksctl: This command line tool allows you to run commands against a Kubernetes cluster. This is the best tool for creating or deleting clusters from the command line since it will take care of all associated resources for you.
kubectl: This tool is used to interact with an existing cluster, but can’t be used to create or delete a cluster.

Additional Resource
Further Usage of EKSCTL from eksctl.io. This documentation includes common commands for creating and managing clusters.
Official Amazon documentation for creating your Amazon EKS cluster and worker nodes

https://eksctl.io/usage/creating-and-managing-clusters/

https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

