pipeline {
    agent { label 'k8s' }

    environment {
        IMAGE = "prt-app:latest"
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
                sh 'sudo docker tag $IMAGE afzalhaider1/prt-app:latest'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'sudo docker push afzalhaider1/prt-app:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deploy.yml'
                sh 'kubectl apply -f svc.yml'
            }
        }
    }
}
