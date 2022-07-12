-: Install Kubernetes
The simplest way to install Kubernetes is to use Docker Desktop for Windows or Docker Desktop for Mac. This comes with the kubectl kubernetes command line tool.
The next recommended way is to use a cloud shell environment like AWS Cloud9 or Google Cloud Shell. These cloud environments dramatically simplify issues that crop up on a laptop or workstation. You can follow the native packagement guide from the official documentation here.

A more advanced method for experts could be to directly download the latest binary. HINT: You probably don't want to use this method shown in the video unless you are an expert and should use an easier method above. The expert commands are shown below.

OS X install latest kubectl release:

curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl"
Linux install latest kubectl release:

curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

Curl command
The curl command the instructor uses in the video for Mac is as follows:

curl -LO https://storage.googleapis.com/kubernetes-release/release/$\(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt\)/bin/darwin/amd64/kubectl
Note that the escape characters (\) in the above may need to be removed for the command to work on your system depending on your settings.

If the above command doesn't work on your machine, look at the links above for the most up-to-date documentation.

Checking for virtualization
The other command used is:

sysctl -a | grep machdep.cpu.features

Q: What is an easy method of installing Kubernetes?
Using the Docker application

https://kubernetes.io/
https://docs.docker.com/
https://kubernetes.io/docs/tutorials/hello-minikube/


-: Overview of Kubernetes
https://github.com/kubernetes/kubernetes

What is Kubernetes? It is an open source orchestration system for containers developed by Google and open sourced in 2014. Kubernetes is a useful tool for working with containerized applications. Given our previous work with Docker containers and containerizing an app, working with Kubernetes is the next logical step. Kubernetes was born out of the lessons learned in scaling containerized apps at Google, and is used for automating deployment, scaling and managing such containerized applications.

What are the Benefits of using Kubernetes?
Kubernetes is the standard for container orchestration. All major cloud providers support Kubernetes. Amazon through Amazon EKS, Google through Google Kubernetes Engine GKE and Microsoft through Azure Kubernetes Service (AKS).

Kubernetes is also a framework for running distributed systems at "planet scale". Google uses it to run billions of containers a week.

A few of the Capabilities of Kubernetes include:

High availability architecture
Auto-scaling
Rich Ecosystem
Service discovery
Container health management
Secrets and configuration management
The downside of these features is the high complexity and learning curve of Kubernetes. You can read more about the features of Kubernetes through the official documentation.

https://kubernetes.io/docs/home/

What are the Basics of Kubernetes?
The core operations involved in Kubernetes include creating a Kubernetes Cluster, deploying an application into the cluster, exposing an application ports, scaling an application and updating an application.

What is the Kubernetes (Cluster) Architecture?
The core of Kubernetes is the cluster. Containers run in the cluster. The core components of the cluster include a cluster master and nodes. Inside nodes there is another hierarchy. This is shown in the diagram. A Kubernetes node can contain multiple pods, which in turn can contain multiple containers and/or volumes.

How do you Set Up a Kubernetes Cluster?
There are two main methods:

Set up a local cluster (preferably with Docker Desktop)
Provision a cloud cluster:
Amazon through Amazon EKS
Google through Google Kubernetes Engine GKE
Microsoft through Azure Kubernetes Service (AKS).
If you are using Docker and have enabled kubernetes then you already have a standalone Kubernetes server running. This would be the recommended way to get started with Kubernetes clusters.

How do you Launch Containers in a Kubernetes Cluster?
Now that you have Kubernetes running via Docker desktop how do you launch a container? One of the easiest ways is via the docker stack deploy --compose-file command.

https://docs.docker.com/desktop/kubernetes/

The yaml example file looks like the following:
version: '3.3'

services:
  web:
    image: dockersamples/k8s-wordsmith-web
    ports:
     - "80:80"

  words:
    image: dockersamples/k8s-wordsmith-api
    deploy:
      replicas: 5
      endpoint_mode: dnsrr
      resources:
        limits:
          memory: 50M
        reservations:
          memory: 50M

  db:
    image: dockersamples/k8s-wordsmith-db
This could be deployed with the following command:

docker stack deploy --namespace my-app --compose-file /path/to/docker-compose.yml mystack

Demo
You can follow the demo yourself, or read through a quick primer beforehand:
https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/

Create a cluster
https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-interactive/

minikube version

minikube start

kubectl version

kubectl cluster-info

kubectl get nodes

More on Kubernetes
To go more in-depth with Kubernetes, we also suggest going through the remaining starter tutorials on the Kubernetes website:

Deploy An App
https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/

Explore Your App
https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-intro/

Expose Your App Publicly
https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/

Scale Your App
https://kubernetes.io/docs/tutorials/kubernetes-basics/scale/scale-intro/

Update Your App
https://kubernetes.io/docs/tutorials/kubernetes-basics/scale/scale-intro/

Q: What problem does Kubernetes solve?
Managing and running container clusters
Kubernetes came out of Google as they needed a system to schedule jobs at scale.

Q: How can you get a list of deployed apps running on Kubernetes?
kubectl get deployments

Q: If you had one Kubernetes app test-app running, which of the following would manually scale your app up to 6 application instances?
kubectl scale deployments/test-app --replicas=6

Additional References
Here is a list of links to concepts in Kubernetes:

kubectl: https://kubernetes.io/docs/tasks/tools/

Pods: https://kubernetes.io/docs/concepts/workloads/pods/

Containers: https://kubernetes.io/docs/concepts/containers/

Clusters: https://kubernetes.io/docs/tutorials/clusters/

-: Installing eksctl
eksctl
The AWS EKS service is available through AWS CLI. Creating an AWS EKS cluster is a multi-step process, however, involving many resources. AWS and Weaveworks collaborated to create the eksctl CLI utility to simplify cluster creation.

The eksctl is a simple CLI utility for creating clusters on Amazon EKS. This utility that uses the services of AWS CloudFormation internally.

AWS CloudFormation is an AWS service for creating, managing, and configuring ANY resource on the AWS cloud using a YAML/JSON script. In the script file, you can define the properties of the resource you want to create in the cloud.

If you are creating a simple cluster, the eksctl utility eliminates the need to write a CloudFormation script. In the case of an advanced cluster with desired configuration, you may have to write a minimal YAML script.

EKSCTL greatly simplifies EKS cluster creation, by auto-generating the necessary AWS resources in your default region, such as:

VPC - a virtual network defined by a range of IP addresses allocated to you.
Subnets - a subnet is a subset of the VPC (IP addresses) in your desired availability zone (data center)
Nodegroups - logical group of worker nodes. Note that each node is a Virtual Machine (VM)
Deciding the AMI and Type of the worker nodes. AMI defines the underlying OS, the default is Linux with supporting packages. Type defines the hardware capacity.
Kubernetes API endpoint
Let's learn how to install the eksctl CLI utility.

eksctl Installation
Latest Linux:
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin


If you face any error due to ownership permission, you can change the ownership of those directories to your user.

sudo chown -R $(whoami) /usr/local/<directory_name>

For more installation information see the installation instructions here.
https://eksctl.io/introduction/#installation

Q: eksctl is a command line tool designed to simplify
Creating EKS clusters
Eksctl is a tool specifically designed to simplify the EKS cluster creation process

How it works?
Create a basic cluster
Once you have installed EKSCTL utility, you can create a basic cluster with just one command:

eksctl create cluster
A cluster will be created with default parameters, such as:

An auto-generated name
Two m5.large worker nodes. Recall that the worker nodes are the virtual machines, and the m5.large type defines that each VM will have 2 vCPUs, 8 GiB memory, and up to 10 Gbps network bandwidth.
Use the Linux AMIs as the underlying machine image
Your default region
A dedicated VPC
However, you can specify all the above details explicitly, for example:

eksctl create cluster --name myCluster --nodes=4

Create an advanced cluster

If you want to specify a detailed configuration, then you may have to write all the configuration in a separate YAML file, and pass it along with the command:

eksctl create cluster --config-file=<path>
We will not discuss the option above to use configuration files, however, you can have a look at an example here. https://eksctl.io/

List the details
List the details about the existing cluster(s)
eksctl get cluster [--name=<name>][--region=<region>]

Delete a cluster
Run a simple command, it will delete the cluster as well as all the associated resources.
eksctl delete cluster --name=<name> [--region=<region>]
There are many more options while creating a cluster, such as defining the auto-scaling size (min-max number of nodes), and SSH access to the nodes.

-: Installing kubectl
kubectl
The kubectl is a command-line tool for interacting with Kubernetes cluster. You can use this command-line utility to deploy the application and communicate with the Kubernetes cluster, such as checking the status of the pods. Mostly the Docker Desktop installation will also install the kubectl utility.

Before you head for the installation, check if you already have a kubectl installed using:

# Make sure Docker Desktop is running
kubectl version --client

If do not have it installed, the installation instructions for kubectl can be found here.
https://kubernetes.io/docs/tasks/tools/

Q: Kubectl can be used to
Kubectl is a tool for interacting with a kubernetes cluster and it’s components 
such as pods and services

Kubectl can be used to interact with pods and services
Eksctl is designed to easily set up an EKS cluster.

-: Monitoring, Logging and Debugging with Kubernetes
We covered Prometheus more in-depth in the previous course, so we'll focus more on how it integrates with Kubernetes here.

The Prometheus Operator helps to integrate Prometheus monitoring directly with Kubernetes. There is also the kube-prometheus package which includes additional helpful components (including the Prometheus Operator) for monitoring, as well as the Prometheus Adapter.

https://github.com/prometheus-operator/prometheus-operator
https://github.com/kubernetes-sigs/prometheus-adapter

Prometheus is not the only monitoring tool available for Kubernetes though - if you are interested in other options as well, check out this link.
https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-usage-monitoring/

On the next page, you'll get to perform an exercise to get monitoring set up with Prometheus, as well as think through designing a monitoring system of your own.


Reference
https://prometheus.io/
https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/

-: Autoscaling with CPU or Memory
If you did the Scaling Demo earlier, you already saw one way to scale your apps:
https://kubernetes.io/docs/tutorials/kubernetes-basics/scale/scale-intro/

kubectl scale {deployment name} --replicas={desired number of replicas}

The Horizontal Pod Autoscaler does this work for you.
https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

One of the "killer" features of Kubernetes is the ability to set up auto-scaling via the Horizontal Pod Autoscaler. How does this work? The Kubernetes HPA (Horizontal Pod Autoscaler) will automatically scale the number of pods (remember they can contain multiple containers) in a replication controller, deployment or replica set. The scaling is based on CPU utilization, memory or custom metrics defined in the Kubernetes Metrics Server.


There is a Docker article "Kubernetes autoscaling in UCP" that is a good guide to go deeper on this topic and experiment with it yourself.

The Horizontal Pod Autoscaler built into Kubernetes is incredibly useful for expanding the number of Pods available based on processing or memory needs. The underlying algorithm itself, somewhat simplified, is as follows:

newNumPods = ceil(currentNumPods * (currentMetric / desiredMetric))

This means, if by some metric, we are currently at 2.5X our desired metric level, we will scale up our number of pods by 2.5X, rounded up to the nearest one pod.

Q: If we currently have 27 pods, and a desired metric value of 137, how many pods will scale to we have if the metric currently has a value of 1,337?
1,337 / 137 = 9.7591, and multiplying that by 27 pods = 263.496. Since we always round up with ceil, we get 264 pods.

https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/

-: Summary
Why Kubernetes?
There are many compelling reasons to use Kubernetes. Let's summarize them:

Created, Used and Open Sourced by Google
High Availability Architecture
Auto-Scaling is Built In
Rich Ecosystem
Service Discovery
Container Health Management
Secrets and Configuration Management
Another advantage is that Kubernetes is cloud agnostic and it could be a solution for companies that are willing to take on the additional complexity to protect against "vendor lockin".

Key Terms:
Kubernetes
Kubernetes is an open-source system for automating the operations of containerized applications. Google created it and opened-sourced it in 2014.

Amazon EKS
Amazon EKS is a managed Kubernetes service created by Amazon.

Google GKE
Google GKE is a managed Kubernetes service created by Google.

Azure Kubernetes Service AKS
Azure Kubernetes Service is a managed Kubernetes service created by Google.

YAML
YAML is a human-readable serialization format often used in configuration systems. It is easily portable to JSON format.

Kubernetes Pods
A Kubernetes pod is a group of one or more containers.

Kubernetes Containers
A Kubernetes container is a Docker image that deploys into a Kubernetes cluster.

Kubernetes Clusters
A Kubernetes cluster is a deployment of Kubernetes that contains the entire ecosystem of Kubernetes components, including nodes, pods, the API, and containers.

Prometheus
Prometheus is an open-source monitoring system with an efficient time-series database.

Logging
Logging is a process of creating messages about the running state of a software application.

Autoscaling
Autoscaling is the process of scaling load up or down automatically based on how many resources the nodes are using.

https://noahgift.github.io/cloud-data-analysis-at-scale/topics/key-terms










