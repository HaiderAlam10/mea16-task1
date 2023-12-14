pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                sh '''
                docker build -t haideralam/flask-jenk:latest -t haideralam/flask-jenk:v${BUILD_NUMBER} .
                '''
            }
        }

        stage('Push') {
            steps {
                sh '''
                docker push haideralam/flask-jenk:latest
                docker push haideralam/flask-jenk:v${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./kubernetes
                kubectl rollout restart deployment/flask-deployment
                kubectl rollout restart deployment/nginx-deployment
                '''
            }
        }

        stage('Clean') {
            steps {
                sh '''
                docker system prune -f
                '''
            }
        }
    }
}