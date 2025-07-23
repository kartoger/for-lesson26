pipeline {
    agent any
    environment {
        CHANGED = sh(script: "git diff --name-only HEAD~1 HEAD", returnStdout: true).trim()
    }
    stages {
        stage('Build & Test Parallel') {
            parallel {
                stage('HelloWorld') {
                    when {
                        expression { return env.CHANGED.contains('HelloWorld') }
                    }
                    steps {
                        dir('simple-java-maven-app') {
                            sh 'mvn clean package'
                        }
                    }
                }
                stage('HelloJenkins') {
                    when {
                        expression { return env.CHANGED.contains('HelloJenkins') }
                    }
                    steps {
                        dir('simple-java-maven-app-Jenkins') {
                            sh 'mvn clean package'
                        }
                    }
                }
                stage('HelloDevops') {
                    when {
                        expression { return env.CHANGED.contains('HelloDevops') }
                    }
                    steps {
                        dir('simple-java-maven-app-Devops') {
                            sh 'mvn clean package'
                        }
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploy only changed apps"
            }
        }
    }
}
