#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }
        stage('test') {
            steps {
                script {
                    echo "Testing the application..."
                }
            }
        }
        stage('deploy') {
            steps {
                sshagent(['ec2-server-key']) {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                            ssh -o StrictHostKeyChecking=no ec2-user@13.201.50.244 "
                                docker login -u $DOCKER_USER -p $DOCKER_PASS &&
                                docker rm -f demo-app || true &&
                                docker run --name demo-app -p 3080:3080 -d saifullahkhann/demo-app:jma-1.0
                            "
                        '''
                    }
                }
            }
        }
    }
}
