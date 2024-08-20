pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'nodejs', type: 'NodeJS' 
        PATH = "${env.NODEJS_HOME}/bin:${env.PATH}"
        S3_BUCKET = 'sk-jenkins-angular'  // Replace with your actual S3 bucket name
        AWS_CREDENTIALS_ID = 'f5d36311-dcee-4c9d-bd4f-62ca3bc759e2'  // Replace with your AWS credentials ID in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: '55221542-684c-42b9-83e6-9691c039263c', url: 'https://github.com/MalikSayyed/Jenkinsangular.git'  // Replace with your GitHub credentials ID, username, and repository
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('D:\\ATM\\my-angular-app') { // Change to the directory where your Angular app is installed
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                dir('D:\\ATM\\my-angular-app') { // Change to the directory where your Angular app is installed
                    sh 'npm run build --prod'
                }
            }
        }

        stage('Upload to S3') {
            steps {
                withAWS(credentials: AWS_CREDENTIALS_ID) {
                    s3Upload(bucket: S3_BUCKET, path: '', workingDir: 'D:\\ATM\\my-angular-app\\dist\\my-angular-app', includePathPattern: '**/*')  // Adjust the working directory to point to the Angular build output
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
