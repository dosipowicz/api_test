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
        success {
            slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${currentBuild.description} ' (${env.BUILD_URL})")
        }

        failure {
            slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${currentBuild.description} ' (${env.BUILD_URL})")
        }
    }
}