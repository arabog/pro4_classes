Exercise
Create an EKS cluster and a node group comprising of two nodes.

Spoiler: If you face difficulties in this exercise, the following pages will explain how to run a single command to create an AWS EKS Kubernetes cluster.

Guided Instructions - Use the web-console
The exercise has three main parts:

Part 1. Prerequisites
Default VPC and subnets: A default VPC comprises a size /16 IPv4 CIDR block, providing up to 65,536 private IPv4 addresses. It automatically creates a size /20 default subnet in each Availability Zone. There are other components, such as default route tables, and internet gateway that are also created automatically. Go to the VPC service and check if you have a default VPC already set up in your account. (See the snapshot below). If no, refer to the Creating a default VPC instructions.

IAM role for the cluster: A role is a set of permissions that can be assigned to an entity. You need to create the roles that your cluster and the node group will assume. To create a new role, go to the IAM service → Roles, and follow the steps below.

Create the role that the cluster (control plane) will assume in order to manage AWS resources on your behalf.

Click on the Create role button to start the wizard.
Choose AWS service as the trusted entity.
Click on the EKS to see EKS use cases. (See the snapshot below)
Choose EKS - Cluster. It will allow access to other AWS service resources that are required to operate clusters managed by EKS. Click Next.
The needed policy, AmazonEKSClusterPolicy, will be selected. This policy provides Kubernetes the permissions it requires to manage resources on your behalf.
Click Next, and ignore the Tags.
Click Next, and name the role something like myEKSClusterRole

IAM role for the worker nodes: Create a new role, say myEKSWorkerNodeRole, that will give permissions to the kubelet running on the worker node to make calls to other APIs on your behalf. Follow the similar procedure as above, except choosing the EC2 use case instead of EKS. Also, in the Attach permissions policies section on the next page, search and choose the checkbox against the following policies:

AmazonEKSWorkerNodePolicy
AmazonEC2ContainerRegistryReadOnly
AmazonEKS_CNI_Policy

SSH key pair: AWS generates a pair of RSA encrypted public and private keys that help log into the EC2 instance (virtual machines). The public key is placed automatically on the EC2 instances, whereas you use the private key instead of a password to access your instances securely. To create the key pair, go to the EC2 service → Network & Security → Key Pairs option on the left navigation tree.
Click on the Create key pair button.
Provide a name of your choice and choose the desired format. A .pem format is used by Mac/Linux users, and a .ppk format is used by Windows users.
It will download the private key file to your local.

Part 2. Create an EKS Cluster
An EKS cluster comprises of:

A control plane consists of the nodes running the Kubernetes software, such as etcd and the Kubernetes API server. These components run in AWS-owned accounts.
A data plane made up of worker nodes. Worker nodes run in customer accounts.

Once your control plane is ready, you can attach a set of worker nodes to the control plane for running the pods. Let's create the control plane (EKS cluster) first

Navigate to the EKS service → Amazon EKS → Clusters option on the left navigation tree. Click on the Create cluster button to launch the wizard, and use the following details:

Cluster configuration - Provide a name of your choice, say myEKSCluster, and choose the default Kubernetes version. Ensure to have selected the IAM role you created above. The cluster, after assuming the role, will manage AWS resources on your behalf.

Specify networking - Choose the default VPC, subnets, and security group (firewall rules) you have in your account, and mark the cluster endpoints as publicly accessible.

Configure logging - Accept the defaults and create the cluster. It will take several minutes to create the cluster.

Part 3. Create a Node Group
A Node Group is a set of worker nodes (virtual machines) used to run the pods that your cluster will be serving.

Follow the steps below to create a Node Group and attach it to the Cluster:

Once your cluster shows an Active state, click on the cluster name to view more details.

Go to the Configuration → Compute section of your new cluster, and click on the Add Node Group button.

Node Group configuration: Provide a name, and attach the myEKSWorkerNodeRole IAM role to the Node Group. According to AWS,
The IAM Role will give permissions to the kubelet running on the node to make calls to other APIs on your behalf.

Node Group compute and scaling configuration: In this section, you choose the OS, (virtual) hardware configuration, and count of the worker nodes. Use the following values:
Field	            Value	                        Purpose
AMI type	        Amazon Linux 2 (AL2_x86_64)	    OS
Capacity type	    On-Demand	                    Instance purchasing option
Instance types	    t3.micro	                    2 vCPU, 1 GiB memory
Disk size	        20 GiB	---
Scaling configuration		
Min size	        2	                            Min number of nodes for scaling in.
Max size	        2	                            Max number of nodes for scaling out.
Desired size	    2	                            Initial count

Node Group network configuration: Choose the subnets selected earlier while creating the cluster. Choose the SSH key pair created in the prerequisites section above. Also, allow remote access from anywhere on the Internet.

Review and finish creating the Node Group.

Part 4. Clean up
Delete the Node Group. Explore how you'd do the deletion.
If you need help, refer to the instructions here.
Delete the cluster. Need help, again? Refer to the instructions.
Delete the custom IAM roles you have created in this exercise.

