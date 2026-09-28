pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Build') {
            steps {
                dir('backend') {
                    sh './mvnw clean package -DskipTests'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t appgestion-backend:latest ./backend'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose up -d'
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
