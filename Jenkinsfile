#!/usr/bin/env groovy

pipeline {
    
    agent {
        docker {
            image 'node'
            args '-u root'
        }
    }

    stages {
        stage('Build') {
            slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
            steps {
                echo 'Building...'
                sh 'npm install'
            }
        }
        stage('Test') {
            slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
            steps {
                echo 'Testing...'
                sh 'npm test'
            }
        }
    }
}
