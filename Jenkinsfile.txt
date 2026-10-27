
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    environment {
        IMAGE_NAME = 'saiprashanth2006/jenkins-k8s-app'
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                checkout scm
                echo 'Source code downloaded successfully!'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
                echo 'Docker image built successfully!'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat '''
                        @echo off
                        echo %DOCKER_PASS%| docker login -u %DOCKER_USER% --password-stdin
                        docker push %IMAGE_NAME%:%BUILD_NUMBER%
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat '''
                    kubectl apply -f deployment.yaml

                    kubectl set image deployment/jenkins-k8s-app web-app=%IMAGE_NAME%:%BUILD_NUMBER%

                    kubectl rollout status deployment/jenkins-k8s-app --timeout=120s

                    kubectl apply -f service.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'kubectl get pods'
                bat 'kubectl get services'
                echo 'Application deployed successfully!'
            }
        }
    }
}
