pipeline {
    environment {
       registry = "arunakilan/docker-demo"
       registryCredential = 'docker'
       dockerImage = ''
        doError = '1'
    }
    agent any
       stages {
        stage('Cloning Git') {
            steps {
               git 'https://github.com/ArunAkil/alpine-docker.git'
            }
        }
        stage('build image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        }
                }
            }
        }
           stage('Error') {
               when{
                   expression{ doError == '1' }
               }
               steps{
                   echo "Failure"
                   error "failure test. It's work"
               }
           }
           stage('success') {
               when{
                   expression{ doError == '0' }
               }
               steps{
                   echo "ok"
               }
           }
    }
    post {
        always {
            echo 'I will always say Hello again!'
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
            
        }
    }
}

