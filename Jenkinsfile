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

        stage('SonarQube Analysis') {
            steps {
                dir('backend') {
                    withSonarQubeEnv('SonarQube') {
                        sh './mvnw sonar:sonar -Dsonar.projectKey=mayazahrouni-5ia1-appgestion'
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t mayazahrouni_5ia1_appgestiondesprojets:latest ./backend'
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
