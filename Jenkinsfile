stages {

        stage('Install Dependencies') {

            steps {

                sh '''

                    python3 -m venv venv

                    ./venv/bin/python -m pip install --upgrade pip

                    ./venv/bin/python -m pip install -r requirements.txt

                '''

            }

        }

        stage('Run Flask Application') {

            steps {

                sh '''

                    nohup ./venv/bin/python app.py > flask.log 2>&1 &

                    echo $! > flask.pid

                    sleep 5

                    if kill -0 $(cat flask.pid); then

                        echo "Flask application started successfully!"

                    else

                        echo "Flask application failed to start!"

                        cat flask.log

                        exit 1

                    fi

                '''

            }

        }

    }
