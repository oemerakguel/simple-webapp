pipeline {
  agent any

  tools {
    nodejs 'node2430'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install & Build') {
      steps {
        sh 'npm ci'
        sh 'npm run build'
      }
    }
  }
}
