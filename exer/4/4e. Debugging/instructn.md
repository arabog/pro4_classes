As with coding itself, once you have launched your app with Kubernetes, it's likely you will need to do some debugging to get everything working properly. Here, you'll do some debugging with an example app to build your skills with Kubernetes.

Take a look at this sample bash script that shows how the kubectl commands would be run in the project root directory. You will want to change the dockerpath to your container name and DockerHub username.

#!/usr/bin/env bash

dockerpath="noahgift/flasksklearn"

# Run in Docker Hub container with kubernetes
kubectl run flaskskearlndemo\
    --generator=run-pod/v1\
    --image=$dockerpath\
    --port=80 --labels app=flaskskearlndemo

# List kubernetes pods
kubectl get pods

# Forward the container port to host
kubectl port-forward flaskskearlndemo 8000:80
Instructions - Pod Issues
Let's say you have deployed a Kubernetes app, but have the pod does not seem to be running.

First, use kubectl get pods to check the names of your running pods. You may notice the pod with an issue is shown as in a Pending status instead of Running.
Using the NAME of the specific pod from step 1, use kubectl describe pod {POD NAME} to get more information about that pod.

From the output of the above command, search until you find the Events header. This should give you a Reason and Message related to the failure, such as FailedScheduling. An issue like this could be due to the necessary resources not being available for the pod, such as CPU limits.

From what we have seen before, kubectl scale could be used in such a situation to correctly scale up and provide the necessary resources for our Pending pod. On the next page, you'll get to see an automated way to scale up your apps which improves on the manual functionality of kubectl scale

Instructions - Node Issues
In this case, consider a Kubernetes app where the pod is working, but behaving strangely. Alternatively, you may have noticed an issue where no pod will schedule onto a particular node. In this case, there is likely an issue with the specific node that needs to be debugged. While the overall process is fairly similar to debugging issues, the syntax of commands is slightly different, so let's walk through these.

First, use kubectl get nodes to check the names of the available nodes. You may notice the node with an issue is shown as in a NotReady status instead of Ready.
Using the NAME of the specific node from step 1, use kubectl describe node {NODE NAME} to get more information about that node.
The outputs here can vary quite a bit, but the issue could be caused by a disconnection from the network, some other negative Event, too high of resource usage, etc.

