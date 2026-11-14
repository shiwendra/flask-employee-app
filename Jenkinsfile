pipeline {


    agent any


    stages {

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask Application') {
            steps {
                sh '''
                    nohup python3 app.py > flask.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            echo 'Flask application started successfully!'
        }


        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
