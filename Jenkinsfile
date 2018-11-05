pipeline {
  agent any

  tools {nodejs "nodeJS"}

  stages {
    stage('Install') {
      steps {
        git 'https://github.com/zlote/api_test/'
        sh 'npm install'
        sh 'npm run api-test'
        script {
            currentBuild.displayName = "Test dostępności serwisu"
        }
      }
    }
  }

   post {
        always{
            publishHTML (target: [
                  allowMissing: false,
                  alwaysLinkToLastBuild: false,
                  keepAll: true,
                  reportDir: 'coverage',
                  reportFiles: 'index.html',
                  reportName: "RCov Report"
                ])
        }


    }
}