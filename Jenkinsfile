pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'git submodule update --init --recursive'
        sh 'hugo -s . -d public/'
      }
    }
    
    stage('Publish') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "cd5a702b-c61a-4822-b882-9d7de7e99f4e", keyFileVariable: "keyfile")]) {
          sh 'echo "put -r public/* tomm.yt/" | sftp -i ${keyfile} www@tomm.yt'
        }
      }
    }
  }

  post {
    success {
      cleanWs()
    }
  }
}
