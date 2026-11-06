# Jenkins + Kubernetes: Application Deployment and Connection Verification

## About the Project

This project is a hands-on implementation of how Jenkins can work with Kubernetes to automate application deployment and verify cluster connectivity.

The main idea was simple: instead of manually deploying an application every time the code changes, I wanted to understand how Jenkins could automate the process and how Kubernetes could manage the running application.

I built a simple web application using HTML and Nginx, containerized it using Docker, and deployed it to a local Kubernetes cluster using Minikube. I also configured Jenkins to connect to Kubernetes through `kubectl` and verify that the cluster and its resources were accessible.

## What I Implemented

- Created a simple web application using HTML.
- Used Docker to package the application into a container image.
- Pushed the Docker image to Docker Hub.
- Created a Kubernetes Deployment to run multiple replicas of the application.
- Exposed the application using a Kubernetes NodePort Service.
- Configured Jenkins to execute Kubernetes commands.
- Connected Jenkins to the Minikube cluster using `kubectl`.
- Verified the connection by checking cluster information, nodes, pods, deployments, and services.
- Created a Jenkins pipeline to automate the build and deployment process.

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Jenkins | CI/CD pipeline automation |
| Kubernetes | Container orchestration |
| Minikube | Running a local Kubernetes cluster |
| Docker | Building container images |
| Docker Hub | Storing Docker images |
| kubectl | Communicating with Kubernetes |
| Git & GitHub | Source code management |
| HTML & Nginx | Sample web application |

## How the Project Works

1. The application source code is stored in GitHub.
2. Jenkins retrieves the project from the GitHub repository.
3. Jenkins builds a Docker image using the Dockerfile.
4. The image is pushed to Docker Hub.
5. Jenkins uses `kubectl` to apply the Kubernetes configuration files.
6. Kubernetes creates the required application pods.
7. A NodePort Service exposes the application so it can be accessed through a browser.
8. A separate Jenkins pipeline checks the Kubernetes connection and verifies the cluster resources.

## Project Architecture

```text
        GitHub Repository
                |
                v
          Jenkins Pipeline
                |
         Build Docker Image
                |
                v
           Docker Hub
                |
                v
       Kubernetes Deployment
                |
          +-----+-----+
          |           |
        Pod 1       Pod 2
          |           |
          +-----+-----+
                |
                v
       Kubernetes Service
                |
                v
        Web Application
```

## Kubernetes Connection Verification

I created an additional Jenkins pipeline specifically to verify the connection between Jenkins and Kubernetes.

The pipeline runs commands such as:

```bash
kubectl cluster-info
kubectl get nodes -o wide
kubectl get pods --all-namespaces
kubectl get deployments --all-namespaces
kubectl get services --all-namespaces
```

These checks help confirm that Jenkins can communicate with the Kubernetes API server and retrieve information about the cluster and its resources.

## Running the Project Locally

### Prerequisites

Make sure you have installed:

- Docker Desktop
- Minikube
- kubectl
- Jenkins
- Git

### Step 1: Start Minikube

```bash
minikube start --driver=docker
```

### Step 2: Check the Kubernetes Cluster

```bash
kubectl get nodes
```

### Step 3: Apply the Kubernetes Deployment

```bash
kubectl apply -f deployment.yaml
```

### Step 4: Create the Kubernetes Service

```bash
kubectl apply -f service.yaml
```

### Step 5: Verify the Resources

```bash
kubectl get pods
kubectl get deployments
kubectl get services
```

### Step 6: Open the Application

```bash
minikube service jenkins-k8s-service
```

This opens the application in your browser.

## What I Learned

Working on this project helped me understand how different DevOps tools work together in a practical environment.

Some of the important things I learned include:

- How to create and manage Docker images.
- How Jenkins pipelines automate repetitive deployment tasks.
- How Kubernetes Deployments manage multiple application replicas.
- How Services expose applications running inside Kubernetes.
- How Jenkins can communicate with a Kubernetes cluster using `kubectl`.
- How to verify cluster connectivity and troubleshoot deployment issues.
- How GitHub, Jenkins, Docker, and Kubernetes fit into a CI/CD workflow.

I also faced a few setup and configuration issues during development, which helped me get more comfortable working with command-line tools and troubleshooting.

## Final Result

Successfully built and deployed a containerized web application to a local Kubernetes cluster using Jenkins and Docker.

The Jenkins pipelines completed successfully, the application was accessible through the Kubernetes Service, and the additional verification pipeline confirmed that Jenkins could communicate with the Kubernetes cluster.

This project gave me practical exposure to CI/CD, containerization, Kubernetes deployments, and cluster connectivity.

## Author

**Sai Prashanth**

GitHub: [sair85765](https://github.com/sair85765)
