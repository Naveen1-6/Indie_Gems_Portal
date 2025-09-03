pipeline {
    agent any

    environment {
        APP_NAME = "indie-gems-portal"
        APP_VERSION = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/Indie_Gems_Portal.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${APP_NAME}:${APP_VERSION} .'
            }
        }

        stage('Run Container') {
            steps {
                // Stop and remove if already exists
                sh '''
                if [ "$(docker ps -aq -f name=${APP_NAME})" ]; then
                    docker rm -f ${APP_NAME} || true
                fi
                docker run -d --name ${APP_NAME} -p 5151:8080 ${APP_NAME}:${APP_VERSION}
                '''
            }
        }
    }

