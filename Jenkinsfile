pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
    registry = '365314850921.dkr.ecr.us-east-1.amazonaws.com/devop_repository'
    registryCredential = 'Jenkins-ecr'
    dockerimage = ''
  }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/siyah2021/geolocation.git'
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
        
        stage('Deploy Image') {
            steps{
                script { 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        //deploy the image that is in ECR to our EKS cluster
        stage ("Kube Deploy") {
            steps {
                withKubeConfig([credentialsId: 'eks_credential', serverUrl: '']) {
                 sh "kubectl apply -f eks_deploy_from_ecr.yaml"
                }
            }
        }
        } 
    }
}