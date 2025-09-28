pipeline {
    agent any

    stages {
        stage("Cleanup Workspace") {
            when {
                changeRequest()
            }
            steps {
                echo 'Cleanup Workspace...'
                cleanWs()
            }
        }

        stage('Checkout') {
            when {
                changeRequest()
            }
            steps {
                echo "Checking out code for branch: ${env.BRANCH_NAME}"
                script {
                    checkout scm
                }
            }
        }

        stage('Build') {
        when {
                        changeRequest()
                    }
            steps {
                echo 'Building Release version...'
                echo "Building Pull Request: ${env.CHANGE_ID}"
                echo "Source Branch: ${env.CHANGE_BRANCH}"
                echo "Target Branch: ${env.CHANGE_TARGET}"

                sh './gradlew clean assembleRelease --stacktrace'
            }
        }

        stage('Test') {
        when {
                        changeRequest()
                    }
            steps {
                echo 'Testing...'
            }
        }

        stage('Deploy') {
        when {
                        changeRequest()
                    }
            steps {
                echo 'Deploying...'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!!'
        }

        success {
            echo 'Build successful!!'
        }

        failure {
            echo 'Build failed!!'
        }
    }
}

