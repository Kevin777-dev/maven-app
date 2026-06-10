pipeline {
    agent any
    tools {
        maven 'Maven-3' 
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                     credentialsId: 'github',
                    url: 'https://github.com/Kevin777-dev/maven-app.git'
            }
        }
        stage('Build the application') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('UnitTestExecution'){
            steps{
                bat 'mvn test'
            }
        }
        stage('Build the dockerimage'){
            steps{
                bat 'docker build -t arkkevin/tpjenkins:1.0 .'            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpass',
                                        variable: 'dockerHubPass')]) {
                    bat 'docker login -u arkkevin -p %dockerHubPass%'
                    bat 'docker push arkkevin/tpjenkins:1.0'
                }
            }
        }
    }

    post {
        failure {
            emailext(
                subject: " Build #${BUILD_NUMBER} a échoué !",
                body: """
                    Bonjour,

                    Le build Jenkins #${BUILD_NUMBER} a échoué.

                    Projet   : ${JOB_NAME}
                    Build    : #${BUILD_NUMBER}
                    Statut   : FAILURE 
                    Lien     : ${BUILD_URL}

                    Merci de vérifier les logs.
                """,
                to: 'adrianina59@gmail.com',
                recipientProviders: [requestor()]
            )
        }
        success {
            emailext(
                subject: " Build #${BUILD_NUMBER} réussi !",
                body: """
                    Salut,

                    Le build Jenkins #${BUILD_NUMBER} a réussi.

                    Projet   : ${JOB_NAME}
                    Build    : #${BUILD_NUMBER}
                    Statut   : SUCCESS 
                    Lien     : ${BUILD_URL}
                """,
                to: 'adrianina59@gmail.com',
                recipientProviders: [requestor()]
            )
        }
    }
}
