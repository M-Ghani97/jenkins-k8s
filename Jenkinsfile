pipeline {
  agent any
  environment {
    registry = "localhost:5000/node-test-app_jenkins"
    dockerImage = ""
  }
  stages {
    stage('Poll SCM') {
      steps {
        cleanWs()
        checkout scm
      }
    }
    stage('Build Image') {
      steps {
        script {
          dockerImage = docker.build("${registry}:${BUILD_NUMBER}", "node-test") 
        }
      }
    }
    stage('Push Image') {
      steps {
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          sh 'sudo kubectl create -f webapp.yaml'
          sh 'sudo kubectl get all'
        }
      }
    }
  }
}