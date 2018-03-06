#!/usr/bin/env groovy

pipeline {
    notifyStarted()
    agent {
        docker {
            image 'node'
            args '-u root'
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'npm test'
            }
        }
    }
}

 def notifyStarted() {
   // send to Slack 
   slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})") 
   // send to HipChat 
   hipchatSend (color: 'YELLOW', notify: true, 
       message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})" 
     ) 
   
   // send to email
   emailext ( 
       subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'", 
       body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
         <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
       recipientProviders: [[$class: 'DevelopersRecipientProvider']]
     )
 }
