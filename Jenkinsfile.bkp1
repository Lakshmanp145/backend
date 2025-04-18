pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
        //retry(1)
    }
    parameters{
         booleanParam(name: 'deploy', defaultValue: false, description: 'User can select deploy or not')
    }
    environment {
        DEBUG = 'true'
        appVersion = '' // this will become global, we can use across pipeline
        region = 'us-east-1'
        account_id = '503561459301'
        project = 'expense'
        environment = 'prod'
        component = 'backend'
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
        stage('install dependendies'){
             steps {
                  sh 'npm install'

             }
        }
        stage('Docker build'){
            steps {
                withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                    sh """
                    aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${account_id}.dkr.ecr.us-east-1.amazonaws.com

                    docker build -t ${account_id}.dkr.ecr.${region}.amazonaws.com/${project}/${environment}/${component}:${appVersion} .

                    docker images

                    docker push ${account_id}.dkr.ecr.${region}.amazonaws.com/${project}/${environment}/${component}:${appVersion}

                    """
                }
            }
        }   
        stage('Deploy'){
            when {
                expression { params.deploy }
            }
            steps{
                build job: 'backend-cd', parameters: [
                    string(name: 'version', value: "$appVersion"),
                    string(name: 'ENVIRONMENT', value: "${environment}"),
                ], wait: true
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

