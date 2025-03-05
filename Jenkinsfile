pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/taimoor-543/MLOps-Assignment-1.git', branch: 'master'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                sh 'docker build -t taimooraliata/ml-app .'
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo 'Logging into Docker Hub...'
                sh 'docker login -u taimooraliata -p $DOCKER_PASSWORD'

                echo 'Tagging and Pushing Docker Image...'
                sh 'docker tag taimooraliata/ml-app taimooraliata/ml-app:latest'
                sh 'docker push taimooraliata/ml-app:latest'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying Application...'
                sh 'docker run -d -p 5000:5000 taimooraliata/ml-app'
            }
        }
    }

    post {
        success {
            emailext(
                subject: "✅ Jenkins Build Successful: ${env.JOB_NAME}",
                body: "The build ${env.BUILD_NUMBER} for ${env.JOB_NAME} has completed successfully! 🎉",
                to: "taimoorwastaken@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "❌ Jenkins Build Failed: ${env.JOB_NAME}",
                body: "The build ${env.BUILD_NUMBER} for ${env.JOB_NAME} has failed. Please check Jenkins logs.",
                to: "taimoorwastaken@gmail.com"
            )
        }
    }
}
