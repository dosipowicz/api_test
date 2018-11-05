pipeline {
  agent any

  tools {nodejs "nodeJS"}

  stages {
    stage('Install') {
      steps {
        git 'https://github.com/zlote/api_test/'
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {

        git 'https://github.com/zlote/api_test/'
        sh 'npm run api-test'
         echo "hello ${env.sales}"
        script {
            currentBuild.displayName = "Test dostępności serwisu"

            response = httpRequest acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON',
                      httpMode: 'GET',
                      requestBody: body, consoleLogResponseBody: true,
                      url: 'https://www.zlotewyprzedaze.pl/api/rest/catalog/sales',
                      validResponseContent: 'ok'
                    println "Sent a notification, got a $response response"
        }


      }
    }
  }

   //post {
   //     success {
            //slackSend (color: '#008000', message: " ${currentBuild.displayName}: ${currentBuild.currentResult}  (${env.BUILD_URL})")
   //     }

   //     failure {
            //slackSend (color: '#EC1414', message: " ${currentBuild.displayName}: ${currentBuild.currentResult}  (${env.BUILD_URL})")
   //     }


  //  }
}