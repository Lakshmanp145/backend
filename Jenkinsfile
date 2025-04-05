pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        //retry(1)
    }
    environment {
        DEBUG = 'true'
         appVersion = '' //this will become global, we can use across pipeline 
    }
  
    stages {
        stage('Read the version') {
            steps {
                  script {
                       def packegeJson = readJSON file: 'packege.json'
                       appVersion = packegeJson.version
                       echo "App version: ${AppVesion}"
                  }
               
            }
        }
        stage('Test') {
            steps {
                
                  sh 'echo "This id Test"'
                
            }
        }
        stage('Deploy') {
            when{
                expression { env.GIT_BRANCH == "origin/main"} 
            }
            steps {
                
                  sh 'echo "This is Deploy"'
                  //error 'pipeline failed'
            }
        }
        stage('Print params') {
            steps{
                echo "Hello ${params.PERSON}"
                echo "Biography: ${params.BIOGRAPHY}"
                echo "Toggle: ${params.TOGGLE}"
                echo "Choice: ${params.CHOICE}"
                echo "Password: ${params.PASSWORD}"
            }
        }
//         stage('approval'){
//             input {
//                 message "Should we continue?"
//                 ok "Yes, we should."
//                 submitter "alice,bob"
//                 parameters {
//                     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
//                 }
//             }
//             steps{
//                 echo "Hello, ${PERSON}, nice to meet you. "
//             }
//         }
    }
    post { 
        always { 
            echo 'This session runs always'
            deleteDir()
        }
        success { 
            echo 'This session runs when pipeline success'
        }
        failure { 
            echo 'This session runs when pipeline failure'
        }
    }
}

