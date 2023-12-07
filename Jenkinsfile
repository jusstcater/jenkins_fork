pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '076892551558.dkr.ecr.us-west-2.amazonaws.com/jenkins_ecr_registry'
    //registryCredential = 'jenkins-role'
    region = 'us-west-2'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Hermann90/helloworld_jan_22.git'
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
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('docker login'){
            steps{
                script{
                    aws ecr get-login-password --region "${region}"| docker login --username AWS --password-stdin "${registry}"
                }
            }
        }

        stage('Deploy image') {
            steps{
                script{ 
                    
                        dockerImage.push()
                    
                }
            }
        }  
    }
}
