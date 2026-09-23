@Library('jfrog-shared-lib@main') _

pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Determine Release') {
            steps {
                script {
                    sh 'git fetch --tags --force'

                    def lastReleaseTag = sh(
                        script: '''
                            git tag --list 'v[0-9]*.[0-9]*.[0-9]*' --sort=-version:refname | head -n 1
                        ''',
                        returnStdout: true
                    ).trim()

                    if (!lastReleaseTag) {
                        error 'No release tag found.'
                    }

                    RELEASE = lastReleaseTag

                    echo "Last release tag: ${RELEASE}"
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
