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
    
    stage('kubernetes') {
      steps {
        withcredentials([aws(accesskeyvariable: 'AWS_ACCESS_KEY_ID', credential: 'AWS', secretkeyvariable: "AWS_SECRET_ACCESS_KEY")]) {
            // some block
            sh "aws eks --region us-east-1 update-kubeconfig --name $(cluster_name)"
            scripy {
              try {
                sh "kubectl create namespace $(namespave)"
              }
              catch (Exception e) {
                echo "Error / namespace already created"
              }
              }
          }
        
           sh "kubectl apply -f ./deployment.yaml -n $(namespace)"
          sh "kubectl -n $(namespace) rollout restart deployment flaskcontainer"
        
        }
      }
    }
    }

  }
}