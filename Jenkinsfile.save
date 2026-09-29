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

        emailext(
            to: 'maya.zahrouni@gmail.com',
            subject: "SUCCESS - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """
Bonjour,

Le pipeline Jenkins s'est terminé avec succès.

Projet : ${env.JOB_NAME}
Build : #${env.BUILD_NUMBER}
Statut : SUCCESS

URL du build :
${env.BUILD_URL}

Le déploiement Docker a été effectué avec succès.

Cordialement,
Jenkins
"""
        )
    }

    failure {
        echo 'Pipeline échoué.'

        emailext(
            to: 'maya.zahrouni@gmail.com',
            subject: "FAILURE - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """
Bonjour,

Le pipeline Jenkins a échoué.

Projet : ${env.JOB_NAME}
Build : #${env.BUILD_NUMBER}
Statut : FAILURE

Consultez les logs :
${env.BUILD_URL}

Cordialement,
Jenkins
"""
        )
    }
}
