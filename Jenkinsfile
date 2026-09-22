@Library('jfrog-shared-lib@main') _

pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Build Frontend') {
            when {
                tag pattern: 'v*'
            }
            steps {
                sh 'npm run build'
            }
        }

        stage('Upload Frontend') {
            when {
                tag pattern: 'v*'
            }
            steps {
                uploadFrontend(
                    distDir: 'dist',
                    repository: 'key'
                )
            }
        }
    }
}
