pipeline {
    agent any
    
    environment {
        AWS_CREDENTIALS_ID = '8791eb07-fa98-4aba-a9bc-5127dab9373e' // Replace with your AWS credentials ID
        S3_BUCKET_NAME = 'sk-jenkins-angular' // Replace with your S3 bucket name
        DIST_DIR = 'dist/my-angular-app' // This is your Angular app's dist directory
    }
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/MalikSayyed/Jenkinsangular.git' // Replace with your Git repository URL
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Build Angular application
                    sh 'npm install'
                    sh 'ng build --prod'
                }
            }
        }
        
        stage('Deploy to S3') {
            steps {
                withAWS(credentials: AWS_CREDENTIALS_ID, region: 'us-east-1') { // Replace with your AWS region
                    script {
                        // Upload the built Angular application to S3
                        sh "aws s3 sync ${DIST_DIR} s3://${S3_BUCKET_NAME} --delete"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}