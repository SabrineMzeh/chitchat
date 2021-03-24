pipeline {
 agent {
  label 'jenkins-agent'
 }
 
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
   stage('Remove image') {
          steps{
            sh  'docker rmi $registry'
          }
        }
  
    
   
   stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(
          kubeconfigId: 'kube',
          configs: 'k8s.yaml',
          enableConfigSubstitution: true
        )
        }
      }
    }
  }
}
