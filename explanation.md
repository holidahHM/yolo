## 1. Choice of Base Image
 The base image used to build the containers is `node:16-alpine3.16`. It is derived from the Alpine Linux distribution, making it lightweight and compact. 
 Used 
 1. Client:`node:16-alpine3.16`
 2. Backend: `node:16-alpine3.16`
 3.Mongo : `mongo:5.0 `
       

## 2. Dockerfile directives used in the creation and running of each container.
 I used two Dockerfiles. One for the Client and the other one for the Backend.

**Client Dockerfile**

```
# Build stage
FROM node:16-alpine3.16 as build-stage

# Set the working directory inside the container
WORKDIR /client

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies and clears the npm cache and removes any temporary files
RUN npm install --only=production && \
    npm cache clean --force && \
    rm -rf /tmp/*

# Copy the rest of the application code
COPY . .

# Build the application and  remove development dependencies
RUN npm run build && \
    npm prune --production

# Production stage
FROM node:16-alpine3.16 as production-stage

WORKDIR /client

# Copy only the necessary files from the build stage
COPY --from=build-stage /client/build ./build
COPY --from=build-stage /client/public ./public
COPY --from=build-stage /client/src ./src
COPY --from=build-stage /client/package*.json ./

# Set the environment variable for the app
ENV NODE_ENV=production

# Expose the port used by the app
EXPOSE 3000

# Prune the node_modules directory to remove development dependencies and clears the npm cache and removes any temporary files


# Start the application
CMD ["npm", "start"]

```
**Backend Dockerfile**

```
# Set base image
FROM node:16-alpine3.16

# Set the working directory
WORKDIR /backend

# Copy package.json and package-lock.json to the container
COPY package*.json ./

# Install dependencies and clears the npm cache and removes any temporary files
RUN npm install --only=production && \
    npm cache clean --force && \
    rm -rf /tmp/*

# Copy the rest of the application code
COPY . .

# Set the environment variable for the app
ENV NODE_ENV=production

# Expose the port used by the app
EXPOSE 5000

# Prune the node_modules directory to remove development dependencies and clears the npm cache and removes any temporary files
RUN npm prune --production && \
    npm cache clean --force && \
    rm -rf /tmp/*

# Start the application
CMD ["npm", "start"]

```

## 3. Docker Compose Networking
The (docker-compose.yml) defines the networking configuration for the project. It includes the allocation of application ports. The relevant sections are as follows:


```
services:
  backend:
    # ...
    ports:
      - "5000:5000"
    networks:
      - yolo_default

  client:
    # ...
    ports:
      - "3000:3000"
    networks:
      - yolo_default
  
  mongodb:
    # ...
    ports:
      - "27017:27017"
    networks:
      - yolo_default

networks:
  yolo_default:
    driver: bridge
```
In this configuration, the backend container is mapped to port 5000 of the host, the client container is mapped to port 3000 of the host, and mongodb container is mapped to port 27017 of the host. All containers are connected to the yolo-network bridge network.


## 4.  Explanation of How Inventory and Playbook Work Together
Hosts Definition:

The inventory file lists the hosts that Ansible will manage. In this case, there are three hosts (client, backend, and mongo), each with specific SSH connection details and private key files.

Playbook Configuration:
The playbook is configured to run on all hosts defined in the inventory file:

hosts: all: This directive specifies that the playbook should run on all hosts listed in the inventory file.
become: true: This setting ensures that the tasks in the playbook will be executed with elevated privileges (e.g., using sudo).

name: A descriptive name for the playbook, "Dockerize and Run YOLO E-Commerce App."

become: Enables privilege escalation, allowing the playbook to perform tasks that require elevated permissions.



Roles Configuration:
The playbook uses three roles:

frontend-deployment: This role is responsible for deploying the frontend of the YOLO e-commerce app.

setup-mongodb: This role sets up MongoDB, which is likely used as the database for the application.

backend-deployment: This role is responsible for deploying the backend of the YOLO e-commerce app.

## 5. Roles

Initialization: Ansible reads the inventory.yml file to get the list of hosts and their connection details.

Playbook Execution:

Connects to each host using the provided SSH details (ansible_host, ansible_port, ansible_user, ansible_private_key_file).

Executes the tasks defined in the roles frontend-deployment, setup-mongodb, and backend-deployment on all hosts.

Privilege Escalation: The become: true directive ensures that tasks requiring elevated privileges are executed as sudo.

## 6. Explanation of Vars.Yml
vars_files: Specifies an external file containing variables. Using an external file helps keep the playbook clean and makes managing variables easier. Here, variables are stored in roles/frontend-deployment/vars/vars.yml.

General Variables
app_name: The name of the application.
docker_network: The name of the Docker network to be used.

Frontend Variables
frontend_image: Docker image for the frontend.
frontend_container_name: Container name for the frontend.
frontend_port: Internal port used by the frontend application.
frontend_host_port: Port on the host machine to map to the frontend container.

Backend Variables
backend_image: Docker image for the backend.
backend_container_name: Container name for the backend.
backend_port: Internal port used by the backend application.
backend_host_port: Port on the host machine to map to the backend container.

MongoDB Variables
mongodb_image: Docker image for MongoDB.
mongodb_container_name: Container name for MongoDB.
mongodb_port: Internal port used by MongoDB.
mongodb_host_port: Port on the host machine to map to the MongoDB container.
mongodb_data_dir: Directory for MongoDB data.

## 7. To run the playbook:
sh
`ansible-playbook playbook.yml`




To achieve the task the following git workflow was used:

1. Fork the repository from the original repository.
2. Clone the repo: `https://github.com/holidahHM/yolo.git`
3. Create a .gitignore file to exclude unnecessary     files and directories from version control.
4. Committed the changes:
`git commit -m "Installed vagrant"`
5. Updated Vagrant file:
`git add docker-compose.yml`
6. Committed the changes:
`git commit -m "downgraded docker compose yml"`
7. Committed the changes:
`git commit -m "update the main.yml file for both frontend and backend"`
8. Updated role folder:
`Updated roles main.yml files`
9. Committed the changes:
`git commit -m "updated the hosts on inventory.yml"`
10. Pushed the files to github:
`git push `


# . Summary
Your setup uses an Ansible inventory file to define the connection details for multiple hosts and a playbook to deploy and configure a YOLO e-commerce app. 

The playbook will run on all hosts specified in the inventory, applying roles to deploy the frontend and backend components and set up MongoDB.

Install Dependencies: Each role installs necessary dependencies, such as git, docker.io, and docker-compose.

Clone Repositories: The frontend-deployment and backend-deployment roles clone their respective repositories from GitHub.

Run Services: Both roles use docker-compose to build and run Docker containers for the frontend and backend services.

Setup MongoDB: The setup-mongodb role installs and starts MongoDB.
Make sure to replace https://github.com/yourusername/yolo-frontend.git and https://github.com/yourusername/yolo-backend.git with the actual URLs of your repositories.

To run the playbook:

sh
ansible-playbook playbook.yml

This setup should provision your Vagrant VMs, clone the necessary repositories, and run the YOLO e-commerce application so it can be verified in the browser.

If you need to adjust or troubleshoot this setup, ensure that the inventory file correctly reflects the hosts' details and that the playbook roles are correctly implemented.

####

## KUBERNERTES ORCHESTRAION

## Step I: INSTALL Minikube

## 1. Installation on Linux
**a. Install Dependencies**
Update the package index:

```bash
sudo apt-get update
```

Install required dependencies:

```bash
sudo apt-get install -y curl apt-transport-https
```

**b. Install Minikube**
Download and install Minikube:
``` bash
`curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`
`sudo install minikube-linux-amd64 /usr/local/bin/minikube`
```

**c. Start Minikube**
Start Minikube with a driver of your choice (e.g., docker):
```bash
minikube start --driver=docker
```

2. Installation on macOS
**a. Install Dependencies**
Ensure Homebrew is installed, then update Homebrew:
```bash
brew update
```

**b. Install Minikube**
Install Minikube using Homebrew:

```bash
brew install minikube
```

**c. Start Minikube**
Start Minikube:
```bash
minikube start --driver=docker
```

3. Installation on Windows
**a. Install Dependencies**
Download and install Chocolatey if not already installed.

**b. Install Minikube**
Use Chocolatey to install Minikube:
```bash
choco install minikube
```

**c. Start Minikube**
Open Command Prompt or PowerShell as an Administrator and start Minikube:
`choco install minikube`
`minikube start --driver=docker`

4. Common Post-Installation Steps
**a. Verify Installation**
Check the Minikube status to ensure it's running:

``` bash
minikube status
```

**b. Access Kubernetes Dashboard**
To access the Kubernetes dashboard, run:
```bash
minikube dashboard
```

**c. Stopping and Deleting Minikube**
Stop Minikube:

``` bash
minikube stop
```

Delete the Minikube cluster:

```bash
minikube delete
```

These steps should help you get started with Minikube on your preferred operating system.

## STEP 2 : INSTALL kubectl

Update apt package index, then install kubectl:

``` bash
sudo apt-get update`
sudo apt-get install -y kubectl
```

### Additional Notes

- **Kubernetes on Cloud**: For deploying Kubernetes in a production environment, consider using managed Kubernetes services like Amazon EKS, Google Kubernetes Engine (GKE), or Azure Kubernetes Service (AKS).
- **WSL2 on Windows**: Using WSL2 on Windows provides a Linux-based development environment, which is beneficial for running Linux containers and Kubernetes locally.
