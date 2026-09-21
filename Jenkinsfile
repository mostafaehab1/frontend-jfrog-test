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
            steps {
                sh 'npm run build'
            }
        }

        stage('Check Dist') {
            steps {
                sh 'ls -lah dist'
            }
        }
    }
}
