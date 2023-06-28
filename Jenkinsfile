pipeline {
    agent any

    environment {
        registry = "728183567244.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Pushpalathasaparapu/docker-spring-boot-helm.git']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 728183567244.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker push 728183567244.dkr.ecr.us-east-1.amazonaws.com/myrepo:latest"
                    
                }
            }
        }
        
        stage ("Helm package") {
            steps {
                    sh "helm package mychart"
                }
            }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade myrelease-21 mychart-0.1.0.tgz"
                }
            }
    }
}
