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

```yaml
all:
  hosts:
    client:
      ansible_host: 127.0.0.1
      ansible_port: 2222
      ansible_user: vagrant
      ansible_private_key_file: /home/holidah/Downloads/DevopsClass/Dev06/yolo/.vagrant/machines/client/virtualbox/private_key
    backend:
      ansible_host: 127.0.0.1
      ansible_port: 2201
      ansible_user: vagrant
      ansible_private_key_file: /home/holidah/Downloads/DevopsClass/Dev06/yolo/.vagrant/machines/backend/virtualbox/private_key
    mongo:
      ansible_host: 127.0.0.1
      ansible_port: 2202
      ansible_user: vagrant
      ansible_private_key_file: /home/holidah/Downloads/DevopsClass/Dev06/yolo/.vagrant/machines/mongo/virtualbox/private_key

##Hosts
client: The frontend server.
backend: The backend server.
mongo: The MongoDB server

##Playbook
The Ansible playbook is located in playbook.yml. It is designed to dockerize and run the YOLO e-commerce app. The playbook performs the following tasks on roles:

frontend-deployment: Deploys the frontend of the YOLO e-commerce app.
setup-mongodb: Configures MongoDB.
backend-deployment: Deploys the backend of the YOLO e-commerce app.

##Configuration
ansible.cfg
Ensure that the ansible.cfg file is correctly set up to point to the inventory file and include necessary configurations:

##To run the playbook:
sh
`ansible-playbook playbook.yml --ask-vault-pass`

The vault pass is "H0llie?!"



##File Paths
Ensure all file paths in the inventory and ansible.cfg are accurate and accessible from your machine.

##Usage
To run the Ansible playbook:

Ensure that the Vagrant VMs are up and running.

Verify that the inventory file and playbook are correctly configured.

Execute the playbook with the following command:  "ansible-playbook -i /home/holidah/yolo/inventory.yml playbook.yml"

##Troubleshooting
Error: Unable to parse inventory file: Verify the inventory file path and format.
Error: Connection refused: Ensure that the VMs are up and accessible.