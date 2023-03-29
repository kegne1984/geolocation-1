pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
        registry = "979327759024.dkr.ecr.us-east-1.amazonaws.com/geolocation_ecr_rep",
        registryCredential = 'jenkins-ecr'
        dockerimage = ''
    }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/kegne1984/geolocation-1.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
          // Building Docker images
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry 
                }
            }
        }
        // Uploading Docker images into AWS ECR
        stage('Pushing to ECR') {
            steps{
                script {
                    sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 979327759024.dkr.ecr.us-east-2.amazonaws.com'
                    sh 'docker push 979327759024.dkr.ecr.us-east-2.amazonaws.com/my-docker-repo:latest'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}