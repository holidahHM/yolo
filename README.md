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




