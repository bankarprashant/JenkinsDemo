pipeline {
    agent any

    stages {
        stage("Cleanup Workspace") {
            steps {
                echo 'Cleanup Workspace...'
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                echo "Checking out code for branch: ${env.BRANCH_NAME}"
                script {
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building Release version...'
                echo "Building Pull Request: ${env.CHANGE_ID}"
                echo "Source Branch: ${env.CHANGE_BRANCH}"
                echo "Target Branch: ${env.CHANGE_TARGET}"

                sh './gradlew clean assembleRelease --stacktrace'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!!!!'
        }

        success {
            echo 'Build successful!!'
        }

        failure {
            echo 'Build failed!!'
        }
    }
}