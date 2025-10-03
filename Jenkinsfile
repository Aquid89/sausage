pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') // проверка репозитория каждые ~5 минут
    }

    tools {
        maven 'maven-3.8.7'
        jdk 'jdk16'
        nodejs 'node-15'
    }

    stages {
        stage('Build & Test backend') {
            steps {
                // Запуск Maven с указанием полного пути к pom.xml
                sh 'mvn -f $WORKSPACE@script/backend/pom.xml package'
            }
            post {
                success {
                    // Сбор результатов тестов
                    junit '$WORKSPACE@script/backend/target/surefire-reports/**/*.xml'
                }
            }
        }

        stage('Build frontend') {
            steps {
                dir("$WORKSPACE@script/frontend") {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Save artifacts') {
            steps {
                archiveArtifacts artifacts: '$WORKSPACE@script/backend/target/*.jar'
                archiveArtifacts artifacts: '$WORKSPACE@script/frontend/dist/**/*'
            }
        }
    }
}
