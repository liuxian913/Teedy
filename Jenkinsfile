pipeline {
    agent any

    tools {
        jdk 'JDK11'
        maven 'Maven'
    }

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('PMD Code Check') {
            steps {
                bat 'mvn pmd:pmd'
            }
            post {
                always {
                    recordIssues(
                        tools: [pmd(pattern: '**/pmd.xml')],
                        allowMissingResults: false
                    )
                }
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/surefire-reports/*.xml'
                }
            }
        }

        stage('JavaDoc') {
            steps {
                bat 'mvn javadoc:jar'
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '**/*.jar, **/site/*.html, **/javadoc/*.jar', fingerprint: true
        }
    }
}
