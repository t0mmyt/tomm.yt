pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'hugo -s . -d public/'
      }
    }
  }
}
