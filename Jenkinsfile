pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'git submodule update --init --recursive'
        sh 'hugo -s . -d public/'
      }
    }
  }
}
