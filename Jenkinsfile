pipeline {

  environment {
    registry = "gopiguru1988/docker"
    registry_mysql = "gopiguru1988/docker/mysql"
    registryCredential="gopiguru1988"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/mgsgoms/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          
           docker.withRegistry( '', registryCredential ){
          
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "gopiguru1988/docker/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "gopiguru1988/docker/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
