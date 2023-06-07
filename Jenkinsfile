pipeline {

environment {
  registry = "jjinwen/flask_app"
  registryCredentials = "docker"
  cluster_name = "skillstorm"
  namespace = "jjinwen"
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
    stage('Kubernetes') {
      steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS', secretKeyVariable: "AWS_SECRET_ACCESS_KEY")]) {
          sh "aws eks --region us-east-1 update-kubeconfig --name ${cluster_name}"
          script {
            try {
              sh "kubectl create namespace ${namespace}"
            }
            catch (Exception e) {
              echo "Error / namespace already created"
            }
          }
          sh "kubectl apply -f deployment.yaml -n ${namespace}"
          sh "kubectl -n ${namespace} rollout restart deployment jjinwenflask"
        }
      }
    }
  }
}    