pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub') // Set in Jenkins
        IMAGE_NAME = 'obieh/my-node-app'
    }
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/obieh/CICD-Pipeline-with-JDK.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $IMAGE_NAME'
            }
        }
        stage('Deploy to K3s') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
