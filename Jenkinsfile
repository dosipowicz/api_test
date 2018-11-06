import groovy.json.JsonSlurper
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
    stage('Test Sale'){
        steps{
            println "sale:_____"
        }
    }
    stage('Test') {
      steps {
        sh 'npm run api-test'
        script {
            currentBuild.displayName = "Test dostępności serwisu"
            def patchOrg = """
                {scope: "[{id, section}]"}
            """

            def response = httpRequest consoleLogResponseBody: false, acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON', httpMode: 'GET', url: "https://www.zlotewyprzedaze.pl/api/rest/catalog/sales", customHeaders: [[name: 'LOGIN-HASH', value: '0000000']], requestBody: patchOrg
            def json = new JsonSlurper().parseText(response.content)
println "sale: $response.content"

            for (rec in json) {
                 println "sale: $rec.name"
                 //sh "echo Hello ${rec.name}"
                 //jobs["$rec.name"]== newJob()
                 //sh 'newman run tests/test.postman_collection.json'
                 //slackSend (color: '#008000', message: " sprawdzam $rec.name")
            }
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