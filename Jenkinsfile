pipeline{
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/obieh/CICD-Pipeline-with-JDK.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t webapp:latest .'
            }
        }
        stage('Run on Kubernetes') {
            steps {
                sh '''
                kubectl delete deployment webapp || true
                kubectl create deployment webapp --image=webapp:latest
                kubectl expose deployment webapp --type=NodePort --port=80
                '''
            }
        }
    }

}