pipeline {
 agent any
 
  environment {
    dockerregistry = 'https://registry.hub.docker.com'
    registry = 'sys7/chitchat'
    githuburl = 'SabrineMzeh/chitchat'
    registryCredential = 'dockerHub'
    dockerImage = ''
  }

  tools {nodejs "nodejs"}
 
  stages {
    
    stage('Clone git repo') {
      steps {
         git 'https://github.com/' + githuburl
      }
    }
   stage('Build image') {
          steps{
            script {
              dockerImage = docker.build registry
            }
          }
        }
   stage('Deploy image') {
      steps{
        script {
          docker.withRegistry(dockerregistry, registryCredential ) {
           dockerImage.push()
          }
        }
      }
    }
   
   stage('Install Node.js dependencies') {
            steps {
                sh 'apk add nodejs'
                sh 'echo $PATH'
                sh 'npm install'
            }
        }
 
        stage('Test App') {
            steps {
                sh 'npm test'
            }
        }
    
   
   
  }
}
