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
                echo "===== WORKSPACE FILES ====="
                ls -l
                '''
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh '''
                echo "===== STARTING SONAR SCAN ====="

                docker run --rm \
                  --network host \
                  -e SONAR_HOST_URL="http://localhost:9000" \
                  -e SONAR_LOGIN="sqp_464e44936e377a2a1d6e2f18d94498cd2eb1cb7d" \
                  -v "$(pwd):/usr/src" \
                  sonarsource/sonar-scanner-cli:5 \
                    -Dsonar.projectKey=1strepo \
                    -Dsonar.projectName=1strepo \
                    -Dsonar.sources=/usr/src \
                    -Dsonar.python.version=3.11 \
                    -Dsonar.host.url=http://localhost:9000
                '''
            }
        }
    }

    post {
        success {
            echo "✅ SonarQube Analysis Completed Successfully"
        }
        failure {
            echo "❌ SonarQube Analysis Failed"
        }
    }
}
