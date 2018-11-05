pipeline {
  agent any

  tools {nodejs "nodeJS"}

  stages {
    stage('Install') {
      steps {
        git 'https://github.com/zlote/api_test/'
        sh 'npm install'
        sh 'npm run api-test'
      }
    }
  }

   post {
          always {
              slackNotifier(currentBuild.currentResult)
              cleanWs()
          }
      }
}