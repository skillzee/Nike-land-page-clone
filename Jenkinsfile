pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://your-repo-url.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    dockerImage = docker.build("your-dockerhub-username/vite-app")
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials-id') {
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}
