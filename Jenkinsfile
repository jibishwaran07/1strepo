pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                git url: 'https://github.com/jibishwaran07/1strepo.git', branch: 'main'
            }
        }

        stage('Show Workspace Files') {
            steps {
                sh '''
                echo "========= WORKSPACE CONTENTS ========="
                ls -l
                '''
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh '''
                echo "========= STARTING SONARQUBE SCAN ========="

                sonar-scanner \
                  -Dsonar.projectKey=test \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.login=sqp_fe2535d44ac6990cff748ea021bf72aa2899e0d9
                '''
            }
        }
    }

    post {
        success {
            echo '✅ SonarQube scan completed successfully'
        }
        failure {
            echo '❌ SonarQube scan failed'
        }
    }
}
