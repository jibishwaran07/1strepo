pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh """
                    # Install uv if not present
                    if ! command -v uv &> /dev/null; then
                        curl -fsSL https://astral.sh/uv/install.sh | sh
                        export PATH="\$HOME/.local/bin:\$PATH"
                    fi

                    # Sync environment according to pyproject.toml + uv.lock
                    uv sync
                """
            }
        }

        stage('Run Tests') {
            steps {
                sh """
                    uv run pytest -q
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
                      -Dsonar.python.version=3 \
                      -Dsonar.host.url=http://sonarqube:9000
                """
            }
        }

        stage('Quality Gate') {
            steps {
                echo "SonarQube analysis successfully completed."
            }
        }
    }
}
