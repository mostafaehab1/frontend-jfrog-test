@Library('jen-shared-lib@main') _


pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Read Release') {
            steps {
                script {
                    RELEASE = readFile('RELEASE').trim()

                    if (!RELEASE) {
                        error 'RELEASE file is empty'
                    }

                    echo "Current release: ${RELEASE}"
                }
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

        stage('Generate Build ID') {
            steps {
                script {
                    def timestamp = sh(
                        script: 'date -u +%Y%m%d-%H%M%S',
                        returnStdout: true
                    ).trim()

                    BUILD_ID = "${RELEASE}-${timestamp}-b${env.BUILD_NUMBER}"

                    echo "Build ID: ${BUILD_ID}"
                }
            }
        }

        stage('Upload Frontend') {
            steps {
                uploadFrontend(
                    distDir: 'dist',
                    repository: 'key',
                    release: RELEASE,
                    buildId: BUILD_ID
                )
            }
        }
    }
}
