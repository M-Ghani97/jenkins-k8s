pipeline {
  agent any
  environment {
    registry = "localhost:5000/node-test-app_jenkins"
    dockerImage = ""
    Deploy = true
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
      when {
        expression { Deploy = true }
      }
      steps {
        script {
          sh 'sudo kubectl create -f webapp.yaml'
          sh 'sudo kubectl get all'
        }
      }
    }
    stage('Update') {
      script {
        sh "sudo kubectl set image deployment.apps/test-deployment node-app-container=${registry}:${BUILD_NUMBER}"
      }
    }
  }
}