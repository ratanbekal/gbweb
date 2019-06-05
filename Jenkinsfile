#!groovy 

node { 

Environment Variables 
env.instance_id = "${instance_id}" 
echo "${env.instance_id}" 

withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'green_berets', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){ 
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
          aws elbv2 deregister-targets --target-group-arn 'arn:aws:elasticloadbalancing:ca-central-1:228804139688:loadbalancer/app/GB-TIC-LB1/3be466df58af6fa4'  --targets Id=$instanceid
        }
      }
    }
    stage('Code Deploy') {
      steps{
         script {
            echo " Code Deploy" 
          }
        }
      }
    }
    stage('QA Sign-off') {
      steps{
         script {
            timeout(time:5, unit:'DAYS') {
            input message:'QA Sign-off ?'
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
