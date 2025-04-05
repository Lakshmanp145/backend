pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
        //retry(1)
    }
    environment {
        DEBUG = 'true'
        appVersion = '' // this will become global, we can use across pipeline
    }
    
    stages {
        stage('Read the version') {
            steps {
                script{
                   def packageJson = readJSON file: 'package.json'
                   appVersion = packageJson.version
                   echo "App version: ${appVersion}"
                }
            }
        }

        stage('approval'){
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps{
                echo "Hello, ${PERSON}, nice to meet you. "
            }
        }
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

