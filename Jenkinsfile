pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    stages {
        stage ('build') {
            steps {
                sh 'mvn clean'
                sh 'mvn install'
                sh 'mvn package'
                
            }
        }
        stage ('Test') {
            steps {
                echo 'Test step'
            } 
            }
        }
        stage ('Deploy') {
            steps {
                echo 'Deploy step'
            }
        }
        stage ('Docker') {
            steps {
                echo 'Image step'
            }
        }
    }
}