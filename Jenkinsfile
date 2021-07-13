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
                    def dockerCmd = 'docker run -p 80:80 -d clementleeky/my-repo'
                    sshagent(['ec2-server-key']) {  // SSH to EC2 Server
                       sh "ssh -o StrictHostKeyChecking=no ec2-user@13.250.60.54 ${dockerCmd}"
                    }
                }
            }
        }
    }
}