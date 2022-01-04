pipeline {
    environment {
       registry = "arunakilan/docker-demo"
       registryCredential = 'docker'
       dockerImage = ''
        
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
           
    }
    post {
        success {
            mail to:"s.arunkumar413@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: job '${env.JOB_NAME} [${env.BUILD_NUMBER}] check console output at ${env.BUID_URL} "build successfully."
        }
        failure {
            mail to:"s.arunkumar413@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
        }
        always{
            emailext (
          subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
          recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        )
        }
    }
}

