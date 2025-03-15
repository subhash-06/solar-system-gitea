pipeline{
  agent any
  tools {
  nodejs 'node-js'
}


  stages{
    stage('Installing Dependency'){
      steps{
          sh 'npm install --no-audit'
        
      }
    }
    stage('parellel'){
      parallel{
              stage('audit check but not fixing'){
               steps{
                  sh 'npm audit '
                  
               }
              }
             stage('owasp' ){
               steps{
               dependencyCheck additionalArguments: '''
                 --scan './' 
                 --out './' 
                 --format 'ALL' 
                 --prettyPrint''', odcInstallation: 'owasp'
                   dependencyCheckPublisher pattern: 'dependency-check-report.xml', stopBuild: true, unstableTotalCritical: 2
                   publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: './', reportFiles: 'dependency-check-jenkins.html', reportName: 'HTML Report-Project', reportTitles: '', useWrapperFileDirectly: true])
                   
               }
             }
             
      }
    }
    stage('Unit Testing'){
               steps{
                 sh 'npm test'
                 sh 'echo hi'
                 junit allowEmptyResults: true, testResults: 'test-results.xml'

               }
             }
    stage("code coverage"){
              steps{
                catchError(buildResult: 'SUCCESS', message: 'Next time ....', stageResult: 'UNSTABLE') 
                  sh 'npm run coverage'
                
              }
          }
    
  }
}


































