pipeline {
  environment {
    registry = "ratankb/node-app1"
    registryCredential = 'DockerHubID'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/ratanbekal/docker-app1'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Docker Repo Push') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('ECR Push') {
      steps{
         script {
            //configure registry
            docker.withRegistry('https://044661814431.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:aab5d928-39d8-4ce2-92af-ce9635a361e7') {
           
            //build image
            def customImage = docker.build("sample-nodejs-app1:latest")
             
            //push image
            customImage.push()
        }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
       }
    }
  }
}
