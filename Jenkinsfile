pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/prajwal9399/Boardgame.git'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build JAR') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t boardgame:latest .'
            }
        }
        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f boardgame-container || true'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d --name boardgame-container -p 8081:8080 boardgame:latest'
            }
        }
        stage('Verify') {
            steps {
                sh 'sleep 10 && docker ps | grep boardgame-container'
            }
        }
    }
}
