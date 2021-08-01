pipeline {
    agent { label "master" }
    environment {
        ECR_REGISTRY = "563287996287.dkr.ecr.eu-west-1.amazonaws.com"
        APP_REPO_NAME= "techpro-repo/to-do-app"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build --force-rm -t "563287996287.dkr.ecr.eu-west-1.amazonaws.com/techpro-repo/to-do-app:latest" .'
                sh 'docker image ls'
            }
        }
        stage('Push Image to ECR Repo') {
            steps {
                
                sh 'aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 563287996287.dkr.ecr.eu-west-1.amazonaws.com'
                sh 'docker push "563287996287.dkr.ecr.eu-west-1.amazonaws.com/techpro-repo/to-do-app:latest"'
            }
        }
        stage('Deploy') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin "563287996287.dkr.ecr.eu-west-1.amazonaws.com"'
                sh 'docker build -t techpro-repo/to-do-app .'
                sh 'docker run --name todo -dp 80:3000 "563287996287.dkr.ecr.eu-west-1.amazonaws.com/techpro-repo/to-do-app"'
            }
        }
    }
    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}