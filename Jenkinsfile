pipeline {

    agent any

    stages {
        stage('Build') {
            steps {
                timestamps {
                    echo 'Building...'
                    slackSend channel: "#dot-releases",
                              color: "#4BB453",
                              message: "Building ${env.BRANCH_NAME} branch of ${env.JOB_NAME}"
                    sh '`which python3.4` -m venv ./venv'
                    sh 'source ./venv/bin/activate && `which python3.4` -m pip install --upgrade pip'
                }
            }
            post {
                success {
                    slackSend channel: '#dot-releases',
                              color: '#4BB453',
                              message: "Built branch ${env.JOB_NAME}"
                }
                failure {
                    slackSend channel: '#dot-releases',
                              color: '#FF0000',
                              message: "Build failed for branch ${env.JOB_NAME}"
                }
            }
        }
        stage('Test') {
            steps {
                timestamps {
                    echo 'Testing...'
                    slackSend channel: "#dot-releases",
                              color: "#4BB453",
                              message: "Testing branch ${env.JOB_NAME}"
                    sh 'source ./venv/bin/activate && `which python3.4` -m pip install --requirement test-requirements.txt'
                    sh 'tox -e pycodestyle,py27,py34'
                }
            }
            post {
                success {
                    slackSend channel: '#dot-releases',
                              color: '#4BB453',
                              message: "Testing branch ${env.JOB_NAME} success!"
                }
                failure {
                    slackSend channel: '#dot-releases',
                              color: '#FF0000',
                              message: "Testing branch ${env.JOB_NAME} failed!"
                }
            }
        }
    }
}
