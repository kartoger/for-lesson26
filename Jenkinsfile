pipeline {
    agent any
    environment {
        CHANGED_FILES = ""
    }

    stages {
        stage('Detect Changes') {
            steps {
                script {
                    CHANGED_FILES = sh(
                        script: "git fetch origin ${env.BRANCH_NAME} && git diff --name-only origin/${env.BRANCH_NAME} HEAD",
                        returnStdout: true
                    ).trim()
                    echo "Changed files:\n${CHANGED_FILES}"
                }
            }
        }

        stage('Build & Test') {
            parallel {
                stage('HelloWorld') {
                    when { expression { CHANGED_FILES.readLines().any { it.startsWith('simple-java-maven-app/') } } }
                    steps {
                        dir('simple-java-maven-app') { sh 'mvn clean package' }
                    }
                }
                stage('HelloJenkins') {
                    when { expression { CHANGED_FILES.readLines().any { it.startsWith('simple-java-maven-app-Jenkins/') } } }
                    steps {
                        dir('simple-java-maven-app-Jenkins') { sh 'mvn clean package' }
                    }
                }
                stage('HelloDevops') {
                    when { expression { CHANGED_FILES.readLines().any { it.startsWith('simple-java-maven-app-Devops/') } } }
                    steps {
                        dir('simple-java-maven-app-Devops') { sh 'mvn clean package' }
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
