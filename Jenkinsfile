pipeline {
    agent { 
        kubernetes{
            label 'jenkins-slave'
        }
        
    }
    stages {
        
     stage('Build and push image') {
          steps{
        sh 'sudo docker build -t octankwebapp/web-nodejs --network host .'
        sh 'sudo docker tag octankwebapp/web-nodejs:latest 889393155762.dkr.ecr.us-west-2.amazonaws.com/octankwebapp/web-nodejs:latest'
        sh 'sudo aws ecr get-login-password --region us-west-2 | sudo docker login --username AWS --password-stdin 889393155762.dkr.ecr.us-west-2.amazonaws.com'
        sh 'sudo docker push 889393155762.dkr.ecr.us-west-2.amazonaws.com/octankwebapp/web-nodejs:latest'
          }
    }
    stage('K8s Get Pods') {
          steps{
            sh 'ls'
            sh 'sudo kubectl delete deployment web-nodejs -n octank'
        sh 'sudo kubectl get pods'
          }
          
    }
     stage('K8s Deploy to kubernetes') {
          steps{
              script {
                  
          kubernetesDeploy(configs: "kubernetes/deployment.yaml", kubeconfigId: "k8-cred")
            
          }
          }
}
}
}
