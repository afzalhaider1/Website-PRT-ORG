pipeline {
    agent { label 'k8s' }

    environment {
        IMAGE = "afzalhaider1/prt-app:latest"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/afzalhaider1/Website-PRT-ORG.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t $IMAGE .'
            }
        }

        // Removed Push to DockerHub stage

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deploy.yml'
                sh 'kubectl apply -f svc.yml'
            }
        }
    }
}
