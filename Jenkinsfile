#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('test') {
            steps {
                script {
                    echo "Testing the application..."
                }
            }
        }

        stage ('build') {
            steps {
               script {
                   echo "Building the application..."
               }
            }
        }

        stage ('deploy') {
            steps {
                script {
                    echo 'Deploying Docker Image to EC2 Server'
                    def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"
                    sshagent(['ec2-server-key']) {
                       sh "scp docker-compose.yaml ec2-user@13.250.60.54:/home/ec2-user"   
                       sh "ssh -o StrictHostKeyChecking=no ec2-user@13.250.60.54 ${dockerComposeCmd}"
                    }
                }
            }
        }
    }
}
