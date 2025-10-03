pipeline {
    agent any

    tools {
        maven 'maven-3.8.7'
        jdk 'jdk16'
        nodejs 'node-15'
    }

    stages {
        stage('Build & Test backend') {
            steps {
                dir("backend") { // папка backend из репозитория
                    sh 'mvn package' // Maven сам найдёт pom.xml здесь
                }
            }
            post {
                success {
                    junit 'backend/target/surefire-reports/**/*.xml'
                }
            }
        }

        stage('Build frontend') {
            steps {
                dir("frontend") {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Save artifacts') {
            steps {
                archiveArtifacts artifacts: 'backend/target/*.jar'
                archiveArtifacts artifacts: 'frontend/dist/**/*'
            }
        }
    }
}
