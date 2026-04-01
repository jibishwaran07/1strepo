pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Run Python Script') {
            steps {
                sh 'python3 python3.py'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh """
                    docker run --rm \
                      --network sonar-net \
                      -e SONAR_HOST_URL=http://sonarqube:9000 \
                      -e SONAR_LOGIN=sqp_bd9ca5a4f6e1c0c875e18fd72ae798058ded40de \
                      -v "\$(pwd):/usr/src" \
                      sonarsource/sonar-scanner-cli:5 \
                      -Dsonar.projectKey=Devops1 \
                      -Dsonar.sources=/usr/src \
                      -Dsonar.host.url=http://sonarqube:9000
                """
            }
        }

        stage('Quality Gate') {
            steps {
                echo "SonarQube analysis completed successfully."
            }
        }
    }
}
