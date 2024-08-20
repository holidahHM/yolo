# Overview
This project involved the containerization and deployment of a full-stack yolo application using Docker.


# Requirements
Install the docker engine here:
- [Docker](https://docs.docker.com/engine/install/) 
- Docker Compose (if applicable)
- Other tools or dependencies


 ### Creating the Network

To create the network, use the following command:

```bash
docker 'network create yolo_default'
```

## How to launch the application 
### Method 1 (faster)
- NOTE: This method does not require cloning of this repository

- Navigate to the launch_app folder and copy the contents of the [docker-compose.yaml] in the root of the project
- On your local machine, navigate to your desired directory and create
  a docker-compose.yml file, paste the contents into it and save

  `touch docker-compose.yml`

- Launch the application using docker compose up

  `docker compose up`

### Method 2
- NOTE: This requires cloning of this whole repository

- Clone this repository to your local machine

  `git clone https://github.com/holidahHM/yolo`

- Navigate to the root directory of your cloned repository

  `cd yolo`

- Launch the application using the docker compose command

  `docker compose up`
-Run the front end:
   `npm start`

- Install dependancies on npm
`export NODE_OPTIONS=--openssl-legacy-provider`

- Run the backend
`node server`


## Access the application on your browser using the following URL
 `http://localhost:3000/`



## How to stop the application
- Navigate back to your terminal and press "ctrl+c" 


 ## The Docker images used in this application are sourced from this repository

https://hub.docker.com/repositories/holidahmwangi23

![Description of image](image.png)



## How to terminate the application completely
 `docker compose down`

 # YOLO E-commerce App Ansible Deployment

## Overview

This repository contains an Ansible setup for deploying and managing the YOLO e-commerce app. The setup includes an inventory file defining the target hosts and an Ansible playbook that orchestrates the deployment process.

## Inventory File

The inventory file is located at `~/yolo/inventory.yml` and defines the hosts for deployment:



## Hosts
client: The frontend server.
backend: The backend server.
mongo: The MongoDB server

## Playbook
The Ansible playbook is located in playbook.yml. It is designed to dockerize and run the YOLO e-commerce app. The playbook performs the following tasks on roles:

frontend-deployment: Deploys the frontend of the YOLO e-commerce app.
setup-mongodb: Configures MongoDB.
backend-deployment: Deploys the backend of the YOLO e-commerce app.

## Configuration
ansible.cfg
Ensure that the ansible.cfg file is correctly set up to point to the inventory file and include necessary configurations:

## To run the playbook:
sh
`ansible-playbook playbook.yml`



## File Paths
Ensure all file paths in the inventory and ansible.cfg are accurate and accessible from your machine.

## Usage
To run the Ansible playbook:

Ensure that the Vagrant VMs are up and running.

Verify that the inventory file and playbook are correctly configured.

Execute the playbook with the following command:  "ansible-playbook -i /home/holidah/yolo/inventory.yml playbook.yml"



# YOLO E-commerce Kubernetes Orchestration
# Minikube Setup Guide

This guide provides step-by-step instructions to install and start Minikube on different operating systems: Linux, macOS, and Windows.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
  - [Linux](#linux)
  - [macOS](#macos)
  - [Windows](#windows)
- [Post-Installation Steps](#post-installation-steps)
  - [Verify Installation](#verify-installation)
  - [Access Kubernetes Dashboard](#access-kubernetes-dashboard)
  - [Stopping and Deleting Minikube](#stopping-and-deleting-minikube)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)

## Prerequisites
- A compatible hypervisor or container runtime (e.g., Docker).
- Administrative privileges on your machine.
- Internet connection to download Minikube and its dependencies.

## Installation

### Linux

1. **Install Dependencies**:

   ```bash
   `sudo apt-get update`
   sudo apt-get install -y curl apt-transport-https
   ```

2. **Install Minikube:**

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
3. **Start Minikube:**

```bash
minikube start --driver=docker
```
4. **Verify Installation**
Check if Minikube is running correctly:

```bash
minikube status
```

5. **Access Kubernetes Dashboard**
To access the Kubernetes dashboard, use:
```bash
minikube dashboard
```

6. **Stopping and Deleting Minikube**
Stop Minikube:

```bash
minikube stop
```

7. **Delete the Minikube cluster:**

```bash
minikube delete
```

8. **Troubleshooting**
If you encounter issues, consider the following:

-Ensure Docker or another hypervisor is running and configured correctly.
-Verify your system meets the Minikube requirements.
-Consult the Minikube troubleshooting guide.

##  INSTALL kubectl

Update apt package index, then install kubectl:

``` bash
sudo apt-get update`
sudo apt-get install -y kubectl
```

Kubernetes Deployment
Deploying
To deploy your Kubernetes resources, save each configuration to a YAML file, then apply them in the following order:

```bash
kubectl apply -f network.yaml
kubectl apply -f mongo-deployment.yaml
kubectl apply -f backend-deployment.yaml
kubectl apply -f client-deployment.yaml
```

## StatefulSets##
In Kubernetes, StatefulSets are used for applications that require persistent storage and stable network identities, such as databases.

**Key Concepts:**
Stateful Applications: Applications that maintain state across restarts and need persistent storage.

Stable Network Identity: Ensures each pod has a stable, unique network identity.

Ordered Deployment and Scaling: Useful for databases that need to start in a specific order.

Persistent Storage: Retains storage across pod restarts or rescheduling.

For your MongoDB deployment, which is a stateful application, you should use a StatefulSet instead of a Deployment.

## Inspect Pods and Deployments##
To ensure that all your pods are running as expected, run:

```bash
kubectl get pods
```
To list all deployments in the current namespace:

```bash
kubectl get deployments
```

## Verify Deployments##
Check the status of your deployments, pods, and services to ensure everything is running correctly:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```


## Accessing Your Project##
Expose Your Service Using a LoadBalancer
If you have configured your services as LoadBalancer types, Kubernetes will provision an external IP address for you to access your services.

Check the External IP:

```bash
kubectl get svc 
```

Look for the EXTERNAL-IP column under your client-service or any other service you want to access. It may take a few minutes for the IP address to be assigned.

Access the Service
Once you have the EXTERNAL-IP, you can access your application by navigating to the IP address in your web browser:

plaintext

http://<EXTERNAL-IP>

**Additional Notes**
Kubernetes on Cloud: For production environments, consider using managed Kubernetes services like Amazon EKS, Google Kubernetes Engine (GKE), or Azure Kubernetes Service (AKS).

WSL2 on Windows: Using WSL2 on Windows provides a Linux-based development environment, which is beneficial for running Linux containers and Kubernetes locally.