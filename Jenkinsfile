pipeline {
    agent {
        label 'remote-builder'
    }
    
    environment {
        DOCKER_REPO = 'mimo009/ms_demo_cicd'
        SONAR_TOKEN = credentials('sonar-token')
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        SONAR_HOST_URL = 'http://localhost:9000'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Test') {
            steps {
                parallel(
                    'Eureka Service': {
                        dir('eureka-service') {
                            sh 'mvn clean package -DskipTests'
                        }
                    },
                    'Gateway Service': {
                        dir('gateway-service') {
                            sh 'mvn clean package -DskipTests'
                        }
                    },
                    'Microservice1': {
                        dir('microservice1') {
                            sh 'mvn clean package -DskipTests'
                        }
                    },
                    'Microservice2': {
                        dir('microservice2') {
                            sh 'mvn clean package -DskipTests'
                        }
                    }
                )
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                parallel(
                    'Eureka Service': {
                        dir('eureka-service') {
                            sh """
                                mvn sonar:sonar \
                                -Dsonar.host.url=${SONAR_HOST_URL} \
                                -Dsonar.login=${SONAR_TOKEN} \
                                -Dsonar.projectKey=eureka-service \
                                -Dsonar.projectName=eureka-service \
                                -Dsonar.projectVersion=1.0 \
                                -Dsonar.sources=src/main/java \
                                -Dsonar.tests=src/test/java \
                                -Dsonar.sourceEncoding=UTF-8
                            """
                        }
                    },
                    'Gateway Service': {
                        dir('gateway-service') {
                            sh """
                                mvn sonar:sonar \
                                -Dsonar.host.url=${SONAR_HOST_URL} \
                                -Dsonar.login=${SONAR_TOKEN} \
                                -Dsonar.projectKey=gateway-service \
                                -Dsonar.projectName=gateway-service \
                                -Dsonar.projectVersion=1.0 \
                                -Dsonar.sources=src/main/java \
                                -Dsonar.tests=src/test/java \
                                -Dsonar.sourceEncoding=UTF-8
                            """
                        }
                    },
                    'Microservice1': {
                        dir('microservice1') {
                            sh """
                                mvn sonar:sonar \
                                -Dsonar.host.url=${SONAR_HOST_URL} \
                                -Dsonar.login=${SONAR_TOKEN} \
                                -Dsonar.projectKey=microservice1 \
                                -Dsonar.projectName=microservice1 \
                                -Dsonar.projectVersion=1.0 \
                                -Dsonar.sources=src/main/java \
                                -Dsonar.tests=src/test/java \
                                -Dsonar.sourceEncoding=UTF-8
                            """
                        }
                    },
                    'Microservice2': {
                        dir('microservice2') {
                            sh """
                                mvn sonar:sonar \
                                -Dsonar.host.url=${SONAR_HOST_URL} \
                                -Dsonar.login=${SONAR_TOKEN} \
                                -Dsonar.projectKey=microservice2 \
                                -Dsonar.projectName=microservice2 \
                                -Dsonar.projectVersion=1.0 \
                                -Dsonar.sources=src/main/java \
                                -Dsonar.tests=src/test/java \
                                -Dsonar.sourceEncoding=UTF-8
                            """
                        }
                    }
                )
            }
        }
        
        stage('Build Docker Images') {
            steps {
                parallel(
                    'Eureka Service': {
                        dir('eureka-service') {
                            script {
                                docker.build("${DOCKER_REPO}:eureka-service-${BUILD_NUMBER}")
                            }
                        }
                    },
                    'Gateway Service': {
                        dir('gateway-service') {
                            script {
                                docker.build("${DOCKER_REPO}:gateway-service-${BUILD_NUMBER}")
                            }
                        }
                    },
                    'Microservice1': {
                        dir('microservice1') {
                            script {
                                docker.build("${DOCKER_REPO}:microservice1-${BUILD_NUMBER}")
                            }
                        }
                    },
                    'Microservice2': {
                        dir('microservice2') {
                            script {
                                docker.build("${DOCKER_REPO}:microservice2-${BUILD_NUMBER}")
                            }
                        }
                    }
                )
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                               usernameVariable: 'DOCKER_USERNAME', 
                                               passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    sh "docker push ${DOCKER_REPO}:eureka-service-${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_REPO}:gateway-service-${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_REPO}:microservice1-${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_REPO}:microservice2-${BUILD_NUMBER}"
                }
            }
        }
        
        stage('Deploy') {
            steps {
                // First, start Eureka server
                dir('eureka-service') {
                    sh 'docker-compose up -d'
                    // Wait for Eureka server to be healthy
                    sh 'sleep 60'
                }
                
                // Then start other services sequentially
                dir('gateway-service') {
                    sh 'docker-compose up -d'
                }
                
                dir('microservice1') {
                    sh 'docker-compose up -d'
                }
                
                dir('microservice2') {
                    sh 'docker-compose up -d'
                }
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
} 