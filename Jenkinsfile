pipeline {
    agent any

    environment {
        CONDA_DIR = "${JENKINS_HOME}/miniconda3"
        PATH = "${JENKINS_HOME}/miniconda3/bin:${env.PATH}"
        JENKINS_NODE_COOKIE = 'dontKillMe'
    }

    stages {
        stage('Setup Python') {
            steps {
                sh '''
                    if ! command -v python3 >/dev/null 2>&1; then
                        if [ ! -d "$CONDA_DIR" ]; then
                            curl -sSL https://anaconda.com -o miniconda.sh
                            bash miniconda.sh -b -p "$CONDA_DIR"
                            rm miniconda.sh
                        fi
                    fi
                    python3 --version
                '''
            }
        }

        stage('Create Virtual Environment') {
            steps {
                sh '''
                    rm -rf venv
                    python3 -m venv venv
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    ./venv/bin/python -m pip install --upgrade pip
                    ./venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask Application') {
            steps {
                sh '''
                    pkill -f "python app.py" || true
                    nohup ./venv/bin/python app.py > flask.log 2>&1 &
                    sleep 3
                    curl -f http://localhost:5000/health
                '''
            }
        }
    }

    post {
        success {
            echo 'Flask application started and passed /health check!'
        }
        failure {
            echo 'Pipeline execution failed! Check flask.log if the app failed to boot.'
        }
    }
}
