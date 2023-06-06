pipeline {

environment {
  registry = "jjinwen/flask_app"
  registryCredentials = "docker"
  cluster_name = "skillstorm"
}

  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Git') {
      steps {
        git(url: 'https://github.com/jemain1987/flasking.git', branch: 'main')
      }
    }

    
  
    stage('Build stage') {
      steps {
        script {
          dockerImage = docker.build(registry)
        }
      }
    }
    
    stage('Deployment stage') {
      steps{
        script {
          docker.withRegistry('', registrycredentials) {
            dockerImage.push()
          }
        }
      }
    }

  }
}