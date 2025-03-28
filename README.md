# Microservices Demo with CICD

This project demonstrates a microservices architecture with Continuous Integration and Continuous Deployment (CICD) using:
- Spring Boot
- Spring Cloud
- Jenkins
- Docker
- SonarQube

## Project Structure

The project consists of the following microservices:
- **Eureka Service**: Service Discovery Server
- **Gateway Service**: API Gateway
- **Microservice1**: First microservice
- **Microservice2**: Second microservice

## Prerequisites

- Java 17
- Maven
- Docker
- Jenkins
- SonarQube

## Setup Instructions

1. Clone the repository:
```bash
git clone https://github.com/mimo009/ms_demo_cicd.git
```

2. Build the services:
```bash
mvn clean package
```

3. Run the services using Docker Compose:
```bash
docker-compose up -d
```

## Service Endpoints

- Eureka Server: http://localhost:8761
- Gateway Service: http://localhost:8080
- Microservice1: http://localhost:8081
- Microservice2: http://localhost:8082
- SonarQube: http://localhost:9000

## CICD Pipeline

The project uses Jenkins for Continuous Integration and Deployment:
1. Code is pushed to GitHub
2. Jenkins builds the project
3. SonarQube performs code analysis
4. Docker images are built and pushed to Docker Hub
5. Services are deployed using Docker Compose

## Docker Images

Docker images are available at:
- mimo009/ms_demo_cicd/eureka-service
- mimo009/ms_demo_cicd/gateway-service
- mimo009/ms_demo_cicd/microservice1
- mimo009/ms_demo_cicd/microservice2

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request 