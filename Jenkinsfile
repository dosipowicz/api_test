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
        success {
            slackSend (color: '#008000', message: " ${currentBuild.displayName}: ${currentBuild.currentResult}  (${env.BUILD_URL})")
        }

        failure {
            slackSend (color: '#EC1414', message: " ${currentBuild.displayName}: ${currentBuild.currentResult}  (${env.BUILD_URL})")
        }


    }
}