pipeline {
    agent any

    parameters {
            choice(
                name: 'BUILD_TYPE',
                choices: ['Debug', 'Release'],
                description: 'Select the build type'
            )
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
                git 'https://github.com/bankarprashant/JenkinsDemo.git'
            }
        }

        stage('Build') {
                    steps {
                        script {
                            def buildTask = ''
                            if (params.BUILD_TYPE == 'Debug') {
                                buildTask = 'assembleDebug'
                            } else if (params.BUILD_TYPE == 'Release') {
                                buildTask = 'assembleRelease'
                            } else {
                                error("Invalid build type selected: ${params.BUILD_TYPE}")
                            }
                            echo("Building ${params.BUILD_TYPE} version...")
                            sh "./gradlew clean ${buildTask} --stacktrace"
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
