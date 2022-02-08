# Kubernetes project

This part includes a standalone version Kubernetes process, and it will achieve the following requirements


-   Able to view your application code, Dockerfile, Kubernetes manifests and monitoring configuration.
    
-   Able to deploy the application using kubectl commands
    
-   Able to view application deployment status using kubectl commands
    
-   Able to successfully load/display the application in a web browser
    
-   Able to view/display pod CPU and RAM usage over time, on a graph.
    
-   CPU and RAM monitoring for the Kubernetes PODs.
    
      
-   Host Docker image on [dockerhub.com](http://dockerhub.com)
    
-   Deploy application with Helm
    
-   Install a Kubernetes Dashboard
    

## Development environment

MacOS Monterey v 12.1

## Step 1. Docker

Install docker [https://download.docker.com/mac/edge/Docker.dmg](https://download.docker.com/mac/edge/Docker.dmg)

## Step 2. Kubernetes

Download and Install Kubernetes [https://github.com/gotok8s/k8s-docker-desktop-for-mac](https://github.com/gotok8s/k8s-docker-desktop-for-mac)

## Step 3. minikube

Install and run minikube

![Alt text](/solution_standalone/screenshots/step3.png?raw=true "Optional Title")

The reason why we need to use minikube is that it can provide the better tracking functions for k8s by

**minikube status**

**minikube ip (get the ip addresses)**

**minikube dashboard (GUI for checking status of Cluster )**

## Step 4. Web service

Create a basic web application by node JS, and the app will host a web server listening on localhost port 3000. Once the server receives a request, it will return a whale graph

## Step 5. dockerfile and dockerhub

Create a dockerhub repository

Create a dockerfile

Create a docker image by 

**docker build . -t erichsieh84/solution_standalone**

Push it to dockerhub with a tag latest by

**docker push erichsieh84/solution_standalone:latest**

![Alt text](/solution_standalone/screenshots/step5.png?raw=true "Optional Title")

All your files and folders are presented as a tree in the file explorer. You can switch from one to another by clicking a file in the tree.

## Step 6. Pod

Now, we can write a yaml file to specify the requirements for a Pod and run a container on it.

Create a Pod by 

**kubectl create -f standalone-demo.yaml**

Then we need to map the local port 3000 to pod port 3000

**kubectl port-forward kubernetes-demo-pod 3000:3000**

![Alt text](/solution_standalone/screenshots/step6.png?raw=true "Optional Title")

## Step 7. Service

We can also create a service managing a group of Pods (how they can be connected and accessed)

Create a service yaml for the service detail

Create a service by kubectl create -f service.yaml

Check if the service is running by kubectl get services

Once the service is created, we can access to the web service by Kubernetes Cluster public IP address (obtained by minikube ip) plus nodePort

Or Cluster IP : port

![Alt text](/solution_standalone/screenshots/step7.png?raw=true "Optional Title")

## Step 8. Deployment

Now, we can create a deployment for multiple replica Pods to handle failures or updates (Zero Downtime Rollout )

Create a deployment by **kubectl create -f deployment.yaml**

Check if it's running by kubectl get deploy and kubectl get pods

### Zero Downtime Rollout

Now, we can change our services without downtime

**kubectl edit deployments my-deployment (change the port from 3000 to 3001)**

When we made changes to our services, it k8s will create the same amount of Pods to replace with the old ones

![Alt text](/solution_standalone/screenshots/step8.png?raw=true "Optional Title")

We can also check history by 

**kubectl rollout history deployment my-deployment**

and rollback to any version by 

**kubectl rollout undo deploy my-deployment --to-revision=2**



## Step 9. Ingress

For better mapping Service port to Node port, we apply Ingress and reverse-proxy to set up a same port number to receive the request.

Then, it can forward the requests based on the hostname or pathname.

Assume that we have two web services, docker_blue and docker_red listening on the same port 3000, and we are going to use Ingress

to reroute the requests to the proper servers

Create a new deployment with two container deployment_ingress.yaml

**kubectl create -f deployment_ingress.yaml**

Create a new service service_ingress.yaml

Enable Ingress

**minikube addons enable ingress**

## Step 10. Dashboard - CPU/Memory

We can use minikube dashboard to track all the CPU and Memory usage

![Alt text](/solution_standalone/screenshots/dashboard.png?raw=true "Optional Title")

We can also track them by using k9s

![Alt text](/solution_standalone/screenshots/k9s.png?raw=true "Optional Title")

## Step 11. Helm

To better manage our services and fast deployment. We can use Helm to group all yaml file into Chart
brew install kubernetes-helm

helm create helm-demo
