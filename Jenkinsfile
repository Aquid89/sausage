pipeline {
    agent any

    triggers {
        pollSCM('H/50 * * * *') // Проверка репозитория каждые ~50 минут
    }

    tools {
        maven 'maven-3.8.7'  // Maven
        jdk 'jdk16'          // JDK
        nodejs 'node-15'     // NodeJS
    }

    stages {
        stage('Build & Test backend') {
              steps {
                dir("backend") {
               sh 'mvn package'
             }
    }
    post {
        success {
            junit 'target/surefire-reports/**/*.xml'
        }
    }
        }

        stage('Build frontend') {
            steps {
                dir("frontend") { // Переходим в папку frontend
                    sh 'npm install'   // Установка зависимостей
                    sh 'npm run build' // Сборка фронтенда
                }
            }
        }

        stage('Save artifacts') {
            steps {
                archiveArtifacts(artifacts: 'backend/target/sausage-store-0.0.1-SNAPSHOT.jar')
                archiveArtifacts(artifacts: 'frontend/dist/frontend/*')
            }
        }
    }
}
