pipeline {
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

    stage('Build ') {
      steps {
        sh 'docker build -t jjinwen/flask_app .'
      }
    }

    stage('Docker login') {
      steps {
        sh 'docker login -u jjinwen -p dckr_pat_tiZEfYI-VxX1cGNMHcwa1T6zbqA'
      }
    }

    stage('Docker Push') {
      steps {
        sh 'docker push jjinwen/flask_app '
      }
    }

  }
}