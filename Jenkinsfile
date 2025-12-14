// Triggering Jenkins scan

pipeline {
    agent any

    environment {
        APP_NAME = "jenkins_demo_app"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    def tag = BRANCH_NAME.replaceAll('/', '-')
                    sh "docker build -t ${APP_NAME}:${tag} ."
                }
            }
        }
    }
}