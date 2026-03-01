pipeline {
    agent any

    tools {
        jdk 'JDK-9.0.4'
    }

    environment {
        GRADLE_OPTS = "-Dorg.gradle.daemon=false"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/kst171/jenkins-demo-app.git'
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
        junit 'build/test-results/test/*.xml'

        publishHTML(target: [
            allowMissing: false,
            alwaysLinkToLastBuild: true,
            keepAll: true,
            reportDir: 'build/reports/tests/test',
            reportFiles: 'index.html',
            reportName: 'JUnit HTML Report'
            ])
        }

        success {
            echo 'Build SUCCESS'
        }

        failure {
            echo 'Build FAILED'
        }
    }
}