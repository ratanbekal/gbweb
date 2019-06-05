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
          aws elbv2 deregister-targets --target-group-arn arn:aws:elasticloadbalancing:ca-central-1:228804139688:loadbalancer/app/GB-TIC-LB1/3be466df58af6fa4  --targets Id=$instanceid
        }
      }
    }
    stage('Code Deploy') {
      steps{
         script {
            sh 
          }
        }
      }
    }
    stage('QA Sign-off') {
      steps{
         script {
            timeout(time:5, unit:'DAYS') {
            input message:'Approve deployment?'
            }
          }
        }
      }
    }
    stage('EC2 add to TG') {
      steps{
        aws elbv2 register-targets --target-group-arn arn:aws:elasticloadbalancing:ca-central-1:228804139688:loadbalancer/app/GB-TIC-LB1/3be466df58af6fa4 --targets Id=$instanceid
       }
    }
  }
}
