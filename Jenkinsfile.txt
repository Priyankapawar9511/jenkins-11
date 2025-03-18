pipeline {
    agent any

    environment {
        NODEJS_HOME = tool 'nodejs'
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Priyankapawar9511/jenkins-11.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Add deployment script here (e.g., copy to server, upload to S3)
            }
        }
    }
}
