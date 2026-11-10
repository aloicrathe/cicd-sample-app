pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }
    stages {
        stage('Preparation') {
            steps {
                sh 'rm -rf tempdir'
                sh 'docker rm -f samplerunning || true'
            }
        }
        stage('Build & deploy staging') {
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
        stage('Deploy production') {
            steps {
                sshagent(credentials: ['dmz010-deploy']) {
                    withEnv(['DOCKER_HOST=ssh://deploy@192.0.2.10']) {
                        sh 'rm -rf tempdir'
                        sh 'docker rm -f samplerunning || true'
                        sh 'bash ./sample-app.sh'
                    }
                }
            }
        }
    }
}
