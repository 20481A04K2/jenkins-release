pipeline {
    agent any

    environment {
        // Default environment values (can be overridden by Jenkins or webhook)
        ORG = "${ORG ?: 'phyllo'}"
        ENVIRONMENT = "${ENVIRONMENT ?: 'development'}"
        BRANCH_NAME = "${env.GIT_BRANCH ?: 'release-tag'}"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo "🔄 Checking out source code..."
                checkout scm
                sh 'git fetch --tags'
                sh 'echo "✅ Current tag/release:" && git describe --tags --always'
            }
        }

        stage('Pre-Condition Check') {
            steps {
                script {
                    echo "🔍 Validating environment variables..."
                    if (!env.ORG?.trim()) {
                        error "❌ ORG is not defined!"
                    }
                    if (!env.ENVIRONMENT?.trim()) {
                        error "❌ ENVIRONMENT is not defined!"
                    }
                    if (!env.BRANCH_NAME?.trim()) {
                        error "❌ BRANCH_NAME is not defined!"
                    }
                    echo """
                    ✅ Pre-condition checks passed.
                    ORG: ${env.ORG}
                    ENVIRONMENT: ${env.ENVIRONMENT}
                    BRANCH/TAG: ${env.BRANCH_NAME}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline completed successfully for tag/release: ${env.BRANCH_NAME}"
        }
        failure {
            echo "⚠️ Pipeline failed. Please check logs."
        }
    }
}

