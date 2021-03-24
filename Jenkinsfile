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
            sh 'docker rmi $registry '
          }
        }
   stage('Test image') {
      steps {
        sh 'docker run -i ' + registry
      }
    }
   
   stage('Install Node.js dependencies') {
            steps {
                sh 'npm install'
            }
        }
 
        stage('Test App') {
            steps {
                sh 'npm test'
            }
        }
    
   stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(
          kubeconfigId: 'k8s',
          configs: 'k8s.yaml',
          enableConfigSubstitution: true
        )
        }
      }
    }
  }
}
