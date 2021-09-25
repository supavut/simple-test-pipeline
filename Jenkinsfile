def lastSuccessfulBuild(passedBuilds, build) {
  if ((build != null) && (build.result != 'SUCCESS')) {
      passedBuilds.add(build)
      lastSuccessfulBuild(passedBuilds, build.getPreviousBuild())
   }
}

@NonCPS
def getChangeLog(passedBuilds) {
    def log = ""
    for (int x = 0; x < passedBuilds.size(); x++) {
        def currentBuild = passedBuilds[x];
        def changeLogSets = currentBuild.rawBuild.changeSets
        for (int i = 0; i < changeLogSets.size(); i++) {
            def entries = changeLogSets[i].items
            for (int j = 0; j < entries.length; j++) {
                def entry = entries[j]
                log += "* ${entry.msg} by ${entry.author} \n"
            }
        }
    }
    return log;
  }

def notifyLINE(status) {
    def token = "96T74l9EORhZ8z0pt7BTikVJEWmJ4DxLnlnrP3y38Xe"
    def jobName = env.JOB_NAME +' '+env.GIT_BRANCH
    def buildNo = env.BUILD_NUMBER

    def url = 'https://notify-api.line.me/api/notify'
    def message = "${jobName} Build #${buildNo} ${status} \r\n"
    sh "echo ${message}"

    def changes = "Changes:\n"
       passedBuilds = []

      lastSuccessfulBuild(passedBuilds, currentBuild);

      def changeLog = getChangeLog(passedBuilds)
      echo "changeLog ${changeLog}"
    // sh "curl ${url} -H 'Authorization: Bearer ${token}' -F 'message=${message}'"
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
        notifyLINE("succeed")
    }
    failure{
        notifyLINE("failed")
    }
  }

}
