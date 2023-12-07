pipeline {

    agent any

    stages {

        stage('Init') {

            steps {

                sh '''
                docker network create jenk-network || echo "Network Already Exists"
                docker stop flask-app || echo "flask-app Not Running"
                docker stop nginx || echo "nginx Not Running"
                docker rm flask-app || echo "flask-app Not Running"
                docker rm nginx || echo "nginx Not Running"
                '''

            }

        }

        stage('Build') {

            steps {

                sh '''
                docker build -t docker.io/haideralam/flask-jenk:latest .
                docker build -t nginx-jenk ./nginx
                '''

            }

        }

        stage('Push') {

            steps {

                sh '''
                docker push haideralam/flask-jenk
                docker push haider/nginx-jenk
                '''

            }

        }

        stage('Deploy') {

            steps {

                sh '''
                docker run -d --name flask-app --network jenk-network flask-jenk
                docker run -d -p 80:80 --name nginx --network jenk-network nginx-jenk
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