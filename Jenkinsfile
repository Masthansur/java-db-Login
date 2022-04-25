pipeline {
    agent any
    parameters {
            choice choices: ['develop', 'release', 'master'], description: 'Please provide the branch name so that we can deploy', name: 'branch'
               }

    triggers {
        pollSCM 'H/2 * * * * '
            }
    tools {
            maven 'maven'
    }
    environment { 
        project_type = 'java'
        DEPLOY_TO = 'production'
    }
    stages {
        stage('git fetch') {
            steps {
                git branch: "${params.branch}", credentialsId: '4991f864-6845-456a-90df-f0f62edcdc79', url: 'https://github.com/Masthansur/java-db-Login.git'
            }
        }
         
        stage('Build the code') {
        when{
             environment name: 'project_type', value: 'java'
         }
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true clean package'
                echo "deploying to ${DEPLOY_TO} Env " 
                deploy adapters: [tomcat8(credentialsId: 'tomcat-cred', path: '', url: 'http://172.31.29.247:8080')], contextPath: 'login-release-branch', onFailure: false, war: '**/*.war'
            }
        }
 
    }
    post {
         always {
             echo "You can always see me"
         }
         success {

 

              echo "I am running because the job ran successfully"
              //sh '/usr/local/bin/aws s3 cp /jenkins-home/workspace/stage-tavisca/target/LoginRegisterApp.war s3://9am-weekend-jul/$JOB_NAME/${BUILD_NUMBER}/'
              

 

         }
         unstable {
              echo "Gear up ! The build is unstable. Try fix it"
         }
         failure {
             echo "OMG ! The build failed"
         }
     }
 
}
