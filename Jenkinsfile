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
        git 'https://github.com/ticmagicians/gbweb'
      }
    }
    stage('EC2 de-tag TG') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Code Deploy') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('QA Sign-off') {
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
    stage('EC2 add to TG') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
       }
    }
  }
}
