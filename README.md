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

1. **GitHub Setup**:
   ```bash
   # Clone the repository
   git clone https://github.com/tesnim01/MS_arch.git
   
   # Create and switch to dev branch
   git checkout -b dev
   ```

2. **Docker Hub Setup**:
   - Create a repository named `mimo009/ms_demo_cicd`
   - Configure Docker Hub credentials in Jenkins

3. **Jenkins Setup**:
   - Create a new pipeline job named "ms_demo_cicd"
   - Configure:
     - Repository URL: `git@github.com:tesnim01/MS_arch.git`
     - Branch: `dev`
     - Script Path: `Jenkinsfile`
   - Add credentials:
     - `sonar-token`: Your SonarQube token
     - `dockerhub-credentials`: Your Docker Hub credentials

4. **SonarQube Setup**:
   - Ensure SonarQube is running on port 9000
   - Configure SonarQube token in Jenkins credentials

## Service Endpoints

- Eureka Server: http://localhost:8761
- Gateway Service: http://localhost:8080
- Microservice1: http://localhost:8081
- Microservice2: http://localhost:8082
- SonarQube: http://localhost:9000

## CICD Pipeline

The project uses a single Jenkins pipeline that:
1. Builds all services in parallel
2. Runs SonarQube analysis on all services
3. Builds Docker images for all services
4. Pushes images to Docker Hub with service-specific tags
5. Deploys all services using Docker Compose

## Docker Images

Docker images are available at:
- mimo009/ms_demo_cicd:eureka-service-{BUILD_NUMBER}
- mimo009/ms_demo_cicd:gateway-service-{BUILD_NUMBER}
- mimo009/ms_demo_cicd:microservice1-{BUILD_NUMBER}
- mimo009/ms_demo_cicd:microservice2-{BUILD_NUMBER}

## Development Workflow

1. Make changes in the `dev` branch
2. Push changes to GitHub
3. Jenkins automatically:
   - Builds the code
   - Runs tests
   - Performs SonarQube analysis
   - Builds and pushes Docker images
   - Deploys the services

## Contributing

1. Create a feature branch from `dev`
2. Make your changes
3. Push to your feature branch
4. Create a Pull Request to `dev` 