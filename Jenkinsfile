pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials') // ID of your Docker Hub credentials in Jenkins
        IMAGE_TAG = "mahesh430/java-sample:latest" // Replace with your Docker Hub username and image name
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'develop', url: 'https://github.com/mahesh430/sample-java-application.git'
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Run Maven build and test
                    sh 'mvn clean package'
                }
            }
        }
   stage('SonarQube - SAST') {
      steps {
sh "mvn sonar:sonar -Dsonar.projectKey=java-app -Dsonar.host.url=http://18.118.115.177:9000 -Dsonar.login=5e0e4bffaa2c80ef0cf2cd208197d5afe3d04be3"
      }
   }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Building Docker Image
                    sh "docker build -t ${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login --username ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    // Pushing Image to Docker Hub
                    sh "docker push ${IMAGE_TAG}"
                }
            }
        }

    }

    // post {
    //     always {
    //         // Clean up Docker images
    //         sh "docker rmi ${IMAGE_TAG}"
    //     }
    // }
}

