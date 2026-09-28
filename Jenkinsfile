pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                dir('backend') {
                    sh './mvnw clean package -DskipTests'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'sudo docker build -t appgestion-backend:latest ./backend'
            }
        }

        stage('Deploy') {
            steps {
                sh 'sudo docker compose up -d'
            }
        }
    }

    post {
        success {
            echo 'Deployment réussi !'
        }

        failure {
            echo 'Pipeline échoué.'
        }
    }
}
