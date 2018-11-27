import groovy.json.JsonSlurper
                    def parseJsonText(String json) {
                      def object = new JsonSlurper().parseText(json)
                      if(object instanceof groovy.json.internal.LazyMap) {
                          return new HashMap<>(object)
                      }
                      return object
                    }
pipeline {
  agent any

  tools {nodejs "nodeJS"}
    environment {
        salename = 'null'
    }
  stages {
    stage('get sales'){
        when {
             expression { params.SALE == null }
        }
        steps{
         script {
                def patchOrg = """
                        {"scope": "[{id, section}]"}
                    """
                    def response = httpRequest consoleLogResponseBody: false, acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON', httpMode: 'GET', url: "https://www.zlotewyprzedaze.pl/api/rest/catalog/sales", customHeaders: [[name: 'LOGIN-HASH', value: '0000000']], requestBody: patchOrg
                    def jsonSlupper=new JsonSlurper()

                    def json = parseJsonText(response.content)

                    def sales = [:]
                    def i=0
                    json.each {
                        i++
                        if(it.section!="future"){
                            sales.put(i,it.id)
                        }
                    }
                    json=null
                    jsonSlupper=null
                    response=null

                    //def branches = [:]
                    sales.each {
                    //        branches["branch${it.value}"] = {
                         build job: "zwtesty", propagate: false, wait: true, parameters: [[$class: 'StringParameterValue', name: 'SALE', value: "${it.value}"]]
                    //     }
                    }

                    //parallel branches

                }
        }
    }
    stage('test sale') {
        when {
             expression { params.SALE != null }
        }
        steps {
            sh 'printenv'
            sh "npm install --save-dev"
            script{
                currentBuild.displayName = "Test kampanii ${params.SALE}"
                //echo "testing1 ${params.SALE}"
            }
            try{
            sh "npm run test-sale -s -- --global-var 'sale_id=${params.SALE}' -r cli,html --reporter-html-export reports/newman.html --reporter-html-template template-default.hbs"
}cach(Exception e){
echo e.toString()
}
            step([$class: 'LogParserPublisher',
                    failBuildOnError: true,
                    parsingRulesPath: '/rules/rule1',
                    unstableOnWarning: true,
                    useProjectRule: false])

        }
    }}
   post {
   unstable{
    echo currentBuild.currentResult;
   }
        failure {
            archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: true,
                reportDir: 'reports',
                reportFiles: 'newman.html',
                reportName: 'HTML Report',
                reportTitles: 'Raport'])

                //emailext([
                //    attachLog: true,
                //    body: '${FILE,path="reports/newman.html"}',
                ///    mimeType: 'text/html',
                //    subject: currentBuild.displayName,
                //    to: 'campaign_check@zlotewyprzedaze.pl'
                //    ])

               //slackSend (color: 'danger', message: " ${currentBuild.displayName}: ${currentBuild.currentResult}  (${env.BUILD_URL}HTML_20Report/)")
        }
    }
}