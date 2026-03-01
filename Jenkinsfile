pipeline {
    agent any

    tools {
        jdk 'jdk17'
    }

    environment {
        GRADLE_OPTS = "-Dorg.gradle.daemon=false"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-ssh-key',
                    url: 'git@github.com:your-org/gradle-junit-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: false,
                  testResults: 'build/test-results/test/*.xml'

            archiveArtifacts artifacts: 'build/libs/*.jar',
                             fingerprint: true
        }

        success {
            echo 'Build SUCCESS'
        }

        failure {
            echo 'Build FAILED'
        }
    }
}