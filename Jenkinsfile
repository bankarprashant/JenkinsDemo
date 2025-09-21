pipeline {
    agent any

    parameters {
        choice(name = "BUILD_TYPE", choices = "debug\nrelease", description = "Select the build type")
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                echo 'Cleanup Workspace...'
                cleanWs()
            }
        }

        stage('Clone') {
            steps {
            echo 'Clone...'
//                 git 'https://github.com/bankarprashant/JenkinsDemo.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building release APK...'
                    // `gradlew clean assembleRelease` cleans the project and builds
                    // the release APK. The `--stacktrace` option is useful for debugging failures.
//                     sh './gradlew clean assembleRelease --stacktrace'
                }
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
