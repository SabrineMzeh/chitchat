pipeline {
 agent any
 
  environment {
    dockerregistry = 'https://registry.hub.docker.com'
    registry = 'sys7/chitchat'
    githuburl = 'SabrineMzeh/chitchat'
    registryCredential = 'dockerHub'
    dockerImage = ''
  }

  tools {nodejs "node"}
 
  stages {
    
    stage('Clone git repo') {
      steps {
         git 'https://github.com/' + githuburl
      }
    }
    stage('Initialize'){
     steps{
        def dockerHome = tool 'myDocker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
    }
    stage('Build image') {
          steps{
            script {
               dockerImage = docker.build registry
            }
          }
        }
   
  }
}
