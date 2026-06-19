pipeline {
    agent any
    tools {
        maven 'Maven-3.9'
    }
    environment {
        registry = '630006583899.dkr.ecr.us-east-1.amazonaws.com/devops_repository'
        registryCredential = 'jenkins-ecr'
        dockerImage = ''
        // ⚠️ IMPORTANT : On va injecter les credentials AWS dans l'environnement
        AWS_DEFAULT_REGION = 'us-east-1'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MOHAMED-lab2025/-helloworld_pipeline.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    // On utilise le credential Jenkins pour le login ECR
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', 
                                      credentialsId: 'jenkins-ecr', 
                                      accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                                      secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        sh '''
                            aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${registry}
                        '''
                    }
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    dockerImage.push()
                }
            }
        }
    }
}
