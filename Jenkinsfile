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
                script {
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building Release version...'
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

