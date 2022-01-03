pipeline {
  agent any
  environment {
    registry = "arunakilan/docker-demo"
    dockerImage = ''
    tag = "v1"
    
  }
  stages {
    stage('checkout') {
      steps {
        git credentialsId: '516bc367-3bd2-481d-80ba-a590a3404bd1', url: 'https://github.com/ArunAkil/alpine-docker.git'
      }
    }
    stage('build docker image') {
      steps {
        script {
          dockerImage = docker build registry + tag
        }
      }
    }
  }
}
