pipeline {
    agent any

    environment {
        APP_NAME = "indie-gems-portal"
        DOCKER_IMAGE = "${APP_NAME}:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Naveen1-6/Indie_Gems_Portal.git'
            }
        }

        stage('Build with Maven') {
            steps {
                // 👇 Run Maven from the repo root (where pom.xml exists)
                sh 'ls -l'    // helpful for debugging
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f ${APP_NAME} || true
                docker run -d --name ${APP_NAME} -p 8088:8080 ${DOCKER_IMAGE}
                '''
            }
        }
    }
}
    
