pipeline {
    agent any

    environment {
        MONGO_URI = credentials('mongo-uri')
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . venv/bin/activate
                pytest
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Deploying Flask app..."
                nohup venv/bin/python app.py > app.log 2>&1 &
                '''
            }
        }
    }
}
