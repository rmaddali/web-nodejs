pipeline {
  agent any

  tools {
    jdk 'jdk8'
    maven 'apache-maven36'
  }

 stages {
  stage('Deploy to K8s') {
        steps {
          withKubeConfig([credentialsId: 'kubernetes-config']) {
            sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"'
            sh 'chmod u+x ./kubectl'
            sh './kubectl version'
                                                               }
               }
                       }
         }
  }