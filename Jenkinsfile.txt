pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/taimoor-543/ml-ci-cd-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t taimooraliata/ml-ci-cd-project .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub', url: '']) {
                    sh 'docker tag taimooraliata/ml-ci-cd-project taimooraliata/ml-ci-cd-project:latest'
                    sh 'docker push taimooraliata/ml-ci-cd-project:latest'
                }
            }
        }
        stage('Deploy Application') {
            steps {
                sh 'docker run -d -p 5000:5000 taimooraliata/ml-ci-cd-project:latest'
            }
        }
    }

    post {
        success {
            mail to: 'admin@example.com',
                 subject: 'Deployment Successful',
                 body: 'The application has been successfully deployed!'
        }
    }
}
