pipeline {
    agent any

    environment {
        APP_NAME = "indie-gems-portal"
        APP_VERSION = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Naveen1-6/Indie_Gems_Portal.git'
            }
        }

    stage('Build with Maven') {
    steps {
        dir('Indie_Gems_Portal') {   
            sh 'mvn clean package -DskipTests'
        }
    }
}


        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${APP_NAME}:${APP_VERSION} .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    if [ "$(docker ps -aq -f name=${APP_NAME})" ]; then
                        docker rm -f ${APP_NAME} || true
                    fi
                    docker run -d --name ${APP_NAME} -p 6166:8080 ${APP_NAME}:${APP_VERSION}
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished. Current Docker containers:"
            sh 'docker ps -a'
        }
    }
}


