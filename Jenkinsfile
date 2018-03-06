#!/usr/bin/env groovy

// Based on https://jenkins.io/blog/2016/07/18/pipline-notifications/.
def notifyMessengers(String buildStatus = 'STARTED') {
    // Build status of null means successful.
    buildStatus = buildStatus ?: 'SUCCESS'
    // Replace encoded slashes.
    def decodedJobName = env.JOB_NAME.replaceAll("%2F", "/")

    def colorSlack
    def colorHipchat

    if (buildStatus == 'STARTED') {
        colorSlack = '#D4DADF'
        colorHipchat = 'GRAY'
    } else if (buildStatus == 'SUCCESS') {
        colorSlack = '#BDFFC3'
        colorHipchat = 'GREEN'
    } else if (buildStatus == 'UNSTABLE') {
        colorSlack = '#FFFE89'
        colorHipchat = 'YELLOW'
    } else {
        colorSlack = '#FF9FA1'
        colorHipchat = 'RED'
    }

    def msgSlack = "${buildStatus}: `${decodedJobName}` #${env.BUILD_NUMBER}: ${env.BUILD_URL}"
    def msgHipchat = "${buildStatus}: <code>${decodedJobName}</code> #${env.BUILD_NUMBER}: <a href=\"${env.BUILD_URL}\">${env.BUILD_URL}</a>"

    slackSend(color: colorSlack, message: msgSlack)
    hipchatSend(color: colorHipchat, message: msgHipchat)
}

node {
    try {
        notifyMessengers()

        // Existing build steps.
    } catch (e) {
        currentBuild.result = "FAILURE"
        throw e
    } finally {
        notifyMessengers(currentBuild.result)
    }
}

pipeline {

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
