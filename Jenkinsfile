pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-southeast-1'
        ECR_REGISTRY = '812751451654.dkr.ecr.ap-southeast-1.amazonaws.com'
        ECR_REPOSITORY = 'boardgame-local'
        IMAGE_TAG = 'latest'
        CONTAINER_NAME = 'boardgame-container'
        HOST_PORT = '8081'
        CONTAINER_PORT = '8080'
    }

    stages {

        stage('Login to ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region ${AWS_REGION} | \
                    docker login --username AWS --password-stdin ${ECR_REGISTRY}
                '''
            }
        }

        stage('Pull Image from ECR') {
            steps {
                sh '''
                    docker pull ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker run -d \
                      --name ${CONTAINER_NAME} \
                      -p ${HOST_PORT}:${CONTAINER_PORT} \
                      ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    sleep 5
                    docker ps --filter name=${CONTAINER_NAME}
                '''
            }
        }
    }

    post {
        success {
            echo 'Boardgame deployment successful!'
        }

        failure {
            echo 'Boardgame deployment failed!'
        }
    }
}
