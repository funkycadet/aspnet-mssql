pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Checkout source code from Git
                git 'https://github.com/funkycadet/aspnet-mssql.git'

                // Build the .NET application
                sh 'dotnet restore'
                sh 'dotnet build'
            }
        }

        stage('Test') {
            steps {
                // Run tests if necessary
                sh 'dotnet test'
            }
        }

        stage('Docker Build and Publish') {
            steps {
                // Build and push Docker image
                script {
                    def dockerImage

                    // Build the Docker image
                    dockerImage = docker.build('funkycadet/aspnet-mssql:${env.BUILD_NUMBER}')

                    // Push the Docker image to DockerHub
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials-id') {
                        dockerImage.push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment to DockerHub successful!'
        }
        failure {
            echo 'Build or deployment to DockerHub failed!'
        }
    }
}
