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

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_registry', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                        docker tag ${APP_NAME}:${BRANCH_NAME.replaceAll('/', '-')} $USER/${APP_NAME}:${BRANCH_NAME.replaceAll('/', '-')}
                        docker push $USER/${APP_NAME}:${BRANCH_NAME.replaceAll('/', '-')}
                    """
                }
            }
        }
    }
}