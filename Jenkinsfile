pipeline {
  agent any

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
