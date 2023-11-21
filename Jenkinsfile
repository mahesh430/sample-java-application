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
sh "mvn sonar:sonar \
  -Dsonar.projectKey=sample-app \
  -Dsonar.host.url=http://18.222.25.49:9000 \
  -Dsonar.login=dd859a7944049d3261946025f8455464a9db7ece"
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
stage('Trivy Scan') {
            steps {
                script {
                    // Scan the Docker image with Trivy
                    sh "trivy image --exit-code 1 --no-progress ${IMAGE_TAG}"
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

    post {
        always {
            // Clean up Docker images
            sh "docker rmi ${IMAGE_TAG}"
        }
    }
}

