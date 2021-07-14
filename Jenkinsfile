#!/usr/bin/env groovy

pipeline {
    agent any
    environment {
        IMAGE_NAME = 'clementleeky/my-repo:1.0'
        DOCKER_PWD = 'clementleeky'
        DOCKER_USER = 'clementleeky'
    }

    stages {
        stage ('Build Docker Image') {
            steps {
               script {
                   echo "Building the application..."
                   sh "docker build -t ${IMAGE_NAME} ."
               }
            }
        }

        stage ('Login and Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: "${DOCKER_PWD}", usernameVariable: "${DOCKER_USER}")]) {
                        sh "docker login "
                        sh "docker push clementleeky/my-repo"
                    }
                }
            }
        }

        stage ('deploy') {
            steps {
                script {
                    echo 'Deploying Docker Image to EC2 Server'

                    def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                    def ec2-instance = "ec2-user@13.250.60.54"
                    
                    sshagent(['ec2-server-key']) {
                       sh "scp server-cmds.sh ${ec2-instance}@13.250.60.54:/home/ec2-user"
                       sh "scp docker-compose.yaml ${ec2-instance}@13.250.60.54:/home/ec2-user"   
                       sh "ssh -o StrictHostKeyChecking=no ${ec2-instance}@13.250.60.54 ${shellCmd}"
                    }
                }
            }
        }
    }
}
