pipeline {
    agent any

    tools {
        maven 'M1'       
        jdk 'jdk17'            
    }

    environment {
        SONARQUBE = 'SonarQubeServer'  
    }

    stages {
        stage('Cloner le projet') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/samibenarab/spring-boot-project'
            }
        }

        stage('Compiler le projet') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Construire image Docker') {
            steps {
                sh 'docker build -t springboot-app .'
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès'
        }
        failure {
            echo 'Une erreur est survenue'
        }
    }
}
