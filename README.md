# Skill-Test-1: Microservices-Task: Containerize three services: user-service | product-service | gateway-service

## Containerize a microservices-based application using Node.js, Docker, and Docker Compose.
Microservices Task - Dockerized Node.js Project

## Overview

This project demonstrates a microservices architecture using Node.js, Docker, and Docker Compose.
It includes three services:

## Service	Description	Port
user-service	Manages user data	3000
product-service	Manages product data	3001
gateway-service	API Gateway routing requests to backend services	3003

All services are containerized using Docker and connected via a shared network (microservices-net).

Folder Structure

Microservices-Task/

│── docker-compose.yml

└── microservices/

    ├── user-service/
    
    │   ├── Dockerfile
    
    │   └── app.js
    
    ├── product-service/
    
    │   ├── Dockerfile
    
    │   └── app.js
    
    └── gateway-service/
    
        ├── Dockerfile
        
        └── app.js

Prerequisites

Docker Desktop
 installed and running

Docker Compose (comes with Docker Desktop)

Node.js (for building Docker images if needed locally)

Dockerfile Overview

Each microservice contains a Dockerfile:

Example structure:

# Base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy project files
COPY . .

# Expose service port
EXPOSE 3000  # Change per service

# Start the application
CMD ["node", "index.js"]


Each service exposes its own port (user-service:3000, product-service:3001, gateway-service:3003).

---

## Docker Compose

docker-compose.yml defines all services, their build paths, ports, and network:

Network: microservices-net (bridge network)

Depends_on: Ensures gateway starts after backend services.

Run all services together using Docker Compose.

Commands
1. Build images
docker compose build

2. Start services
docker compose up -d

3. Stop services
docker compose down

4. View logs
docker compose logs -f gateway-service

5. Test APIs

Users API:

http://localhost:3000/users


Products API:

http://localhost:3001/products

Gateway routes:

http://localhost:3003/api/users
http://localhost:3003/api/products

Healthcheck (Optional)

Verify microservices connectivity via gateway:

curl http://localhost:3003/api/users
curl http://localhost:3003/api/products
curl http://localhost:3003/api/orders


Returns JSON if services are healthy.

---

The requirement to create network for containers to run the gateway microservice to communicate with other services as containers are network-isolated:

it is required in case of containers are created with multiple Dockerfile for communicate between containers

commands
# Make sure network exists (safe to run even if already created)
docker network create microservices-net

# Run user-service
docker run -d --name user-service --network microservices-net -p 3000:3000 user-service

# Run product-service
docker run -d --name product-service --network microservices-net -p 3001:3001 product-service

# Run order-service
docker run -d --name order-service --network microservices-net -p 3002:3002 order-service

# Run gateway-service
docker run -d --name gateway-service --network microservices-net -p 3003:3003 gateway-service

---



# Testing the services locally after containerization

Steps:
---

## Services and Endpoints

### **User Service**
- **Base URL:** `http://localhost:3000`
- **Endpoints:**
  - **List Users:**  
    ```
    curl http://localhost:3000/users
    ```
    Or open in your browser: [http://localhost:3000/users](http://localhost:3000/users)

---

### **Product Service**
- **Base URL:** `http://localhost:3001`
- **Endpoints:**
  - **List Products:**  
    ```
    curl http://localhost:3001/products
    ```
    Or open in your browser: [http://localhost:3001/products](http://localhost:3001/products)

---

### **Order Service**
- **Base URL:** `http://localhost:3002`
- **Endpoints:**
  - **List Orders:**  
    ```
    curl http://localhost:3002/orders
    ```
    Or open in your browser: [http://localhost:3002/orders](http://localhost:3002/orders)

---

### **Gateway Service**
- **Base URL:** `http://localhost:3003/api`
- **Endpoints:**
  - **Users:**  
    ```
    curl http://localhost:3003/api/users
    ```
  - **Products:**  
    ```
    curl http://localhost:3003/api/products
    ```
----

## Screenshots:

from creation of containers from Dockerfile to docker-compose.yml (multi-container creation) along with network and step-by-step methods

user-service docker image
<img width="1542" height="913" alt="building user-service docker image" src="https://github.com/user-attachments/assets/bd8b0d1e-4cee-42b8-b73d-6062525a5ec1" />

docker run user-service
<img width="1205" height="262" alt="docker run for user-service" src="https://github.com/user-attachments/assets/8935e785-59dc-42c9-8a31-3970a2f77754" />

Container running locally:
<img width="1916" height="861" alt="running container locally for user-service" src="https://github.com/user-attachments/assets/060786c9-3cb8-475d-b25d-1fd7e94109ae" />

building product-service
<img width="1532" height="948" alt="building product-service docker image" src="https://github.com/user-attachments/assets/b1c48c88-4bf4-4344-a401-18d2672726fa" />

Running product-service
<img width="1233" height="258" alt="running container for product service " src="https://github.com/user-attachments/assets/e5e2e693-9bfd-44d8-aa65-22f7f27b75d5" />

running container locally
<img width="1862" height="717" alt="running container locally for product-service" src="https://github.com/user-attachments/assets/c09534d1-6af6-4e35-bd63-f752e8266810" />

building gateway service
<img width="1527" height="943" alt="building gateway-service image" src="https://github.com/user-attachments/assets/2d4d0ee7-b4df-41a6-860c-3ac0658fdb22" />

running container locally for gateway
<img width="1916" height="728" alt="running container locally for gateway-api" src="https://github.com/user-attachments/assets/e242b0a5-6afb-4581-be24-33948062188e" />

created docker network for containers
<img width="1186" height="52" alt="created network for docker containers" src="https://github.com/user-attachments/assets/403fa0aa-8edd-4260-b99a-745500a65c55" />

docker network with connected containers
<img width="1186" height="400" alt="docker network with connect containers" src="https://github.com/user-attachments/assets/3fd78c90-5844-4f3f-ba08-2f774d5ac4a0" />

gateway container running locally for user-service
<img width="1918" height="761" alt="running container for gateway-service-users" src="https://github.com/user-attachments/assets/ca8d43c2-79c6-4b47-b3ef-f86647530f4d" />

network inspect 
<img width="1186" height="692" alt="docker inspect for microservices containers" src="https://github.com/user-attachments/assets/c18c2ba4-c80e-4e2d-a2b7-f9a26f5b1cbe" />

running docker compose
<img width="1562" height="952" alt="running docker-compose" src="https://github.com/user-attachments/assets/231b3b3b-6d7c-4ac7-9185-851dfad32eda" />

building docker compose
<img width="1503" height="952" alt="building docker compose" src="https://github.com/user-attachments/assets/321c5653-dc2d-4c74-9a65-48d05532cd2c" />

Testing other microservices from gateway service
<img width="1213" height="901" alt="testing microservices from gateway service" src="https://github.com/user-attachments/assets/2f7a356c-81bd-48e9-b11d-cbe818a3fa44" />

---

-Rahul Sharma
