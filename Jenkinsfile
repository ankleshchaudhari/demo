pipeline {
    agent any

    tools {
        nodejs "nodejs-25"
    }

    environment {
        APP_NAME = "react-demo"
        DEPLOY_DIR = "C:\\deploy\\react-demo"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/ankleshchaudhari/demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Publish Test Results') {
            steps {
                junit 'reports/junit.xml'
            }
        }

        stage('Build App') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Create Artifact') {
            steps {
                bat 'powershell Compress-Archive -Path dist\\* -DestinationPath %APP_NAME%.zip'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '*.zip', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                bat """
                if not exist %DEPLOY_DIR% mkdir %DEPLOY_DIR%
                xcopy /E /I /Y dist\\* %DEPLOY_DIR%
                """
            }
        }
    }

    post {
        success {
            echo "CI/CD SUCCESS"
        }
        failure {
            echo "BUILD FAILED"
        }
    }
}
