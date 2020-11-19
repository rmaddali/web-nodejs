pipeline {
    agent {
        kubernetes{
            label 'jenkins-slave'
        }

    }
    stages {
    stage('Build and compile packages ') {
             steps{
                 sh 'echo Scanning image ---'

             }
             }
     stage('Build Image') {
          steps{
        sh 'sudo docker build -t octankwebapp/web-nodejs --network host .'


          }
    }
         stage('Scan image') {
          steps{
              sh 'echo Scanning image ---'

          }
    }
        stage('Tag image') {
          steps{
         sh 'sudo docker tag octankwebapp/web-nodejs:latest 889393155762.dkr.ecr.us-west-2.amazonaws.com/octankwebapp/web-nodejs:latest'
              sh 'sudo aws ecr get-login-password --region us-west-2 | sudo docker login --username AWS --password-stdin 889393155762.dkr.ecr.us-west-2.amazonaws.com'


          }
    }
        stage('Push Image') {
          steps{
         sh 'sudo docker push 889393155762.dkr.ecr.us-west-2.amazonaws.com/octankwebapp/web-nodejs:latest'
          }
    }
    stage('Get Pods') {
          steps{
            sh 'ls'
            sh 'sudo kubectl get pods'
          }

    }
     stage('K8s Deploy to kubernetes') {
          steps{
              script {
           kubernetesDeploy(configs: "kubernetes/deployment.yaml", kubeconfigId: "k8-cred", deleteResource:true)
          kubernetesDeploy(configs: "kubernetes/deployment.yaml", kubeconfigId: "k8-cred")

          }
          }
}
}
}