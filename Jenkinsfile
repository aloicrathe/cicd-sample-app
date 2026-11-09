pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }
    stages {
        stage('Preparation') {
            steps {
                sh 'rm -rf tempdir'
                catchError(buildResult: 'SUCCESS') {
                    sh 'docker stop samplerunning'
                    sh 'docker rm samplerunning'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'bash ./sample-app.sh'
            }
        }
        stage('Results') {
            steps {
                sleep 3
                sh 'curl http://172.17.0.3:5050/ | grep "You are calling me from 172.17.0.2"'
            }
        }
    }
}
