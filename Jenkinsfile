
pipeline {
  agent any
  tools {
      git 'Default'
      nodejs "node12.12"
    }

  stages {

    stage('Prepare') {
        steps {
            sh "npm install -g yarn"
        }
    }

    // stage('Install') {
    //   steps {
    //       sh 'yarn install'
    //   }
    // }

    // stage('Build') {
    //   steps {
    //       sh 'node --max-old-space-size=4096 node_modules/@angular/cli/bin/ng build --prod'
    //   }
    // }
  }

  post{
      success{
          script {
              notifyLine.send 'SUCCESS'
          }
      }
      failure{
          script {
              notifyLine.send 'FAILED'
          }
      }
    }

}
