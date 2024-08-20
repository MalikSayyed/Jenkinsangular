pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'ls'
                sh 'npm install'
                sh 'echo n | ng analytics off'
                sh 'ng build'
                sh 'cd dist && ls'
                sh 'cd dist/angular-tour-of-heroes/browser && ls'
            }
        }
        stage('S3 Upload') {
            steps {
                withAWS(region: 'us-east-1', credentials: '67edb076-88d2-4173-94be-01df20752475') {
                    sh 'ls -la'
                    sh 'aws s3 cp dist/angular-tour-of-heroes/browser/. s3://sk-jenkins-angular/ --recursive'
                }
            }
        }
    }
}