pipeline {
    agent any

    tools {
        maven 'Maven 3'  // Ensure Maven is installed and configured in Jenkins
        jdk 'JDK 17'     // Ensure JDK 17 or your preferred JDK version is installed in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Generate Test Report') {
            steps {
                publishHTML(target: [
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'target/surefire-reports',
                    reportFiles: 'index.html',
                    reportName: 'TestNG Report'
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/surefire-reports/*.xml', allowEmptyArchive: true
            junit 'target/surefire-reports/*.xml'
        }
        failure {
            mail to: 'your-email@example.com',
                 subject: "Build ${env.BUILD_NUMBER} Failed",
                 body: "The Jenkins build ${env.BUILD_NUMBER} failed. Please check the console output."
        }
    }
}
