pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Python 3.11') {
            steps {
                sh """
                    sudo apt update
                    sudo apt install -y python3.11 python3.11-venv python3.11-dev
                """
            }
        }

        stage('Install uv with Python 3.11') {
            steps {
                sh """
                    curl -fsSL https://astral.sh/uv/install.sh | sh
                    export PATH="\$HOME/.local/bin:\$PATH"

                    uv python pin 3.11
                    uv sync
                """
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
                      -Dsonar.python.version=3.11 \
                      -Dsonar.host.url=http://sonarqube:9000
                """
            }
        }

        stage('Quality Gate') {
            steps {
                echo "✅ Sonar scan completed successfully!"
            }
        }
    }
}
