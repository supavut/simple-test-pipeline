
@NonCPS
def notifyLINE(status) {
    def token = "96T74l9EORhZ8z0pt7BTikVJEWmJ4DxLnlnrP3y38Xe"
    def jobName = env.JOB_NAME +' '+env.GIT_BRANCH
    def buildNo = env.BUILD_NUMBER

    def url = 'https://notify-api.line.me/api/notify'
    def message = "${jobName} Build #${buildNo} `${status}` \r\n"

    def changes = "Changes:\n"
     def changeLogSets = currentBuild.changeSets
     for (int i = 0; i < changeLogSets.size(); i++) {
         def entries = changeLogSets[i].items
         for (int j = 0; j < entries.length; j++) {
             def entry = entries[j]
//              echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
             changes += "\t by ${entry.author} : ${entry.msg} \r\n"
        }
     }
     httpRequest consoleLogResponseBody: true, contentType: 'APPLICATION_FORM', customHeaders: [[maskValue: false, name: 'Authorization', value: "Bearer ${token}"]], httpMode: 'POST', requestBody: "message=${message}${changes}", responseHandle: 'NONE', url: "${url}", wrapAsMultipart: false
     //echo "${changes}"

}

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
        notifyLINE("success")
    }
    failure{
        notifyLINE("failed")
    }
  }

}
