pipeline {
    agent any
    environment {
        IMAGE_NAME = "holyday-mission"
        REGISTRY = "idannadler"  // שנה לשם המשתמש שלך ב-Docker Hub
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling from GitHub'
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Test') {
            steps {
                bat 'npm test || echo "no tests defined"'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
            }
        }
        stage('Tag Image') {
            steps {
                bat "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${REGISTRY}/${IMAGE_NAME}:latest"
            }
        }
        stage('Push to Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    bat "docker push ${REGISTRY}/${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                bat 'docker-compose up -d --build'
            }
        }
    }
}