pipeline {
    agent any

    environment {
        VENV = "venv"
        // Use Jenkins credentials instead of hardcoding
        MONGO_URI = credentials('mongo-uri')
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . $VENV/bin/activate
                pytest --maxfail=1 --disable-warnings -q
                '''
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh '''
                echo "Deploying Flask app to staging..."
                pkill -f "venv/bin/python app.py" || true
                nohup venv/bin/python app.py > app.log 2>&1 &
                '''
            }
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Jenkins Build #${BUILD_NUMBER}",
                body: "The Flask CI/CD pipeline completed successfully.",
                to: "sumansaurabh123@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "FAILURE: Jenkins Build #${BUILD_NUMBER}",
                body: "The Flask CI/CD pipeline failed. Please check Jenkins logs.",
                to: "sumansaurabh123@gmail.com"
            )
        }
    }
}
