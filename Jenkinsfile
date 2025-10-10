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
                            def gradleFile = 'app/build.gradle.kts'

                            def content = readFile(gradleFile)

                            def pattern = /versionCode\s*=\s*(\d+)/

                            def currentVersionCode = -1
                            def found = false

                            content.eachMatch(pattern) { match ->
                                currentVersionCode = match[1].toInteger()
                                found = true
                                return
                            }

                            if (found) {
                                def newVersionCode = currentVersionCode + 1

                                echo "Current versionCode: ${currentVersionCode}"
                                echo "New versionCode: ${newVersionCode}"

                                def newVersionString = "versionCode = ${newVersionCode}"

                                def newContent = content.replaceFirst(pattern, newVersionString)

                                writeFile(file: gradleFile, text: newContent)

                                env.NEW_ANDROID_VERSION_CODE = newVersionCode
                            } else {
                                error("Could not find 'versionCode' in ${gradleFile} using the pattern: ${pattern}. Check file format.")
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

        stage('Commit Version Change') {
                    steps {
                        // This step is CRITICAL to save the incremented version back to the repository.
                        // ⚠️ IMPORTANT: The '[ci skip]' tag in the commit message is used
                        // to prevent this commit from triggering a new Jenkins build,
                        // avoiding an infinite build loop.
                        //sh 'git config user.email "bankarprashant17@gmail.commit"'
                        //sh 'git config user.name "bankarprashant"'
                        sh 'git add app/build.gradle.kts'
                        //sh 'git commit -m \"CI: Auto-increment versionCode ${env.NEW_ANDROID_VERSION_CODE} [ci skip]\"'
                        sh 'git commit -m \"CI: Auto-increment versionCode [ci skip]\"'
                        withCredentials([string(credentialsId: 'github-credentials', variable: 'GITHUB_TOKEN')]) {
                            sh "git push https://bankarprashant:\$GITHUB_TOKEN@github.com/JenkinsDemo.git HEAD"
                        }
                        //sh 'git push origin HEAD'
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