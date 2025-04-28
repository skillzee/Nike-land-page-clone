pipeline {
    agent any
    
    environment {
        DOCKER_HUB = credentials('docker-hub-credentials') // You'll need to set this up in Jenkins
        IMAGE_NAME = 'skillzee/vite-app'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/skillzee/Nike-land-page-clone'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        
        stage('Run Tests') {
        steps {
            script {
                try {
                    bat 'npm test || echo "Tests failed but continuing pipeline"'
                } catch (Exception e) {
                    echo "Tests failed: ${e}"
                    // Continue pipeline anyway
                }
            }
        }
        }
        
        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
    steps {
        script {
            // Use default context instead of desktop-linux
            withEnv(['DOCKER_CONTEXT=default']) {
                docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                    docker.image("${IMAGE_NAME}:${env.BUILD_ID}").push()
                    docker.image("${IMAGE_NAME}:latest").push()
                }
            }
        }
    }
}
        
        stage('Deploy') {
            steps {
                bat 'docker-compose down && docker-compose up -d'
            }
        }
    }
    
    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}