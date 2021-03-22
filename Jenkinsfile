pipeline {
 
  environment {
    dockerregistry = 'https://registry.hub.docker.com'
    dockerhuburl = 'sys7/chitchat'
    githuburl = 'SabrineMzeh/chitchat'
    dockerhubcrd = 'dockerHub'
    dockerImage = ''
  }
 
 agent any
 
  tools {nodejs "node"}
 
  stages {
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
 
    stage('Clone git repo') {
      steps {
         git 'https://github.com/' + githuburl
      }
    }
   stage('Build image') {
          steps{
            script {
              dockerImage = docker.build(dockerhuburl + ":$BUILD_NUMBER")
            }
          }
        }
    stage('Deploy image') {
      steps{
        script {
          docker.withRegistry(dockerregistry, dockerHub ) {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
          }
        }
      }
    }
    
    stage('Test image') {
      steps {
        sh 'docker run -i ' + dockerhuburl + ':$BUILD_NUMBER npm test'
      }
    }
 
    
 
    stage('Remove image') {
      steps{
        sh "docker rmi $dockerhuburl:$BUILD_NUMBER"
      }
    }
 
    stage('Deploy k8s') {
      steps {
        kubernetesDeploy(
          kubeconfigId: 'k8s',
          configs: 'k8s.yaml',
          enableConfigSubstitution: true
        )
      }
    }
  }
}
