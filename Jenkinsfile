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

        stage('Auto-Increment Version') {
                    steps {
                        script {
                            def gradleFile = 'app/build.gradle'

                            def content = readFile(gradleFile)

                            def pattern = /versionCode\s+(\d+)/

                            def matcher = content =~ pattern

                            if (matcher.find()) {
                                def currentVersionCode = matcher[0][1].toInteger()
                                def newVersionCode = currentVersionCode + 1

                                echo "Current versionCode: ${currentVersionCode}"
                                echo "New versionCode: ${newVersionCode}"

                                def newContent = content.replaceFirst(pattern, "versionCode ${newVersionCode}")

                                writeFile(file: gradleFile, text: newContent)

                                env.NEW_ANDROID_VERSION_CODE = newVersionCode
                            } else {
                                error("Could not find 'versionCode' in ${gradleFile}. Check file format.")
                            }
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