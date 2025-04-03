pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'http://sonar:9000'
        DOCKER_IMAGE = 'YOUR_DOCKER_HUB_USERNAME/java-app:latest'
    }

    stages{
        stage('Checkout'){
            steps{
                git 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }

        stage('Test'){
            steps{
                script{
                    docker.image('openjdk:11-jdk').inside{
                        sh 'mvn test'
                    }
                }
            }
        }

        stage('Static Code Analysis'){
            steps{
                script{
                    docker.image('openjdk:8-jdk').inside{
                        sh 'mvn sonar:sonar -Dsonar.host.url=${SONARQUBE_SERVER}'
                    }
                }
            }
        }

        stage('Build Docker Image'){
            steps{
                script{
                    sh 'docker build -t ${DOCKER_IMAGE}.'
                }
            }
        }

        stage('Push to Docker Hub'){
            steps{
                script{
                    sh 'docker login -u YOUR_DOCKER_HUB_USERNAME -p YOUR_PASSWORD'
                    sh 'docker push ${DOCKER_IMAGE}'
                }
            }
        }

        stage('Deploy to Kubernetes'){
            steps{
                script{
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}