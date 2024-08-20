pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'nodejs', type: 'NodeJS'
        PATH = "${env.NODEJS_HOME}/bin:${env.PATH}"
        S3_BUCKET = 'sk-jenkins-angular'  // Your actual S3 bucket name
        AWS_CREDENTIALS_ID = 'f5d36311-dcee-4c9d-bd4f-62ca3bc759e2'  // Your AWS credentials ID in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: '55221542-684c-42b9-83e6-9691c039263c', url: 'https://github.com/MalikSayyed/Jenkinsangular.git'  // GitHub credentials ID, username, and repository
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('D:\\ATM\\my-angular-app') { // Directory where your Angular app is installed
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                dir('D:\\ATM\\my-angular-app') { // Directory where your Angular app is installed
                    sh 'npm run build --prod'
                }
            }
        }

        stage('Upload to S3') {
            steps {
                withAWS(credentials: AWS_CREDENTIALS_ID) {
                    // Using AWS CLI to sync the build output to S3
                    dir('D:\\ATM\\my-angular-app\\dist\\my-angular-app') {  // Build output directory
                        sh 'aws s3 sync . s3://sk-jenkins-angular --delete'  // Sync the build output to S3
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}